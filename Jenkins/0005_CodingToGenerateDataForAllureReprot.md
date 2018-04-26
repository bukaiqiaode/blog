## Step 1 Identify the information needed

For each test case execution, we need to collect below information:
- start-time & end-time of entire execution
- start-time, end-time & duration of each test case
- name of the test script
- execution status. 111=pass, 100=Fail, 000=Error, 101=Block
- comment
- location of log-files
- feature name, can be abstracted from script name

## Step 2 Decide the format of the information
- start-time, end-time, duration, in seconds since 1970-01-01
- script name, of type string
- execution status, of type string
- comment, location of logs, feature, of type string

## Step 3 Collect the information
- List all the sub-folders of current folder
```python
    import os
    #list all the folders in this folder
    target_folder = "./logs"
    
    arr_dir = []
    for root, dirs, files in os.walk(target_folder):
        for name in dirs:
            arr_dir.append(name)
```

- Build path to some log-file, then read contents of the file
```python
    stdout_path = os.path.join(target_folder, dir_name, "stdout.txt")
    stdout_f = open(stdout_path, 'r')
    stdout_contents = stdout_f.readlines() 
```
- Get the first/last line of the log
```python
    first_line = stdout_contents[0].strip()
    last_line = stdout_contents[-1].strip()
```

- Get the first/last line of the log
```python
    first_line = stdout_contents[0].strip()
    last_line = stdout_contents[-1].strip()
```
- Convert time-string like `2018-04-18 07-33-08` into seconds since 1970-01-01
```python
    import time

    #convert string to time
    time_str = "2018-04-18 07-33-08"
    the_tick = time.strptime(the_tick, '%Y-%m-%d %H-%M-%S')

    #get time-stamp
    the_tick = time.mktime(the_tick)
```
- Decide the execution result of the test case, using log content
```python
    def abstract_script_result_from_log_line(the_line):    
        if "Result = Pass" in the_line:
            return "Pass"
        if "Result = Fail" in the_line:
            return "Fail"
        else:
            return "Error"
```
## Step 4 Write information into XML format
```python
    from xml.etree.ElementTree import Element, SubElement
    from xml.etree.ElementTree import Comment, tostring, ElementTree

    #build xml-element        
    result = Element('result')
    start_time = SubElement(result, 'startTime')
    start_time.text = str(script_start_time)
        
    end_time = SubElement(result, 'endTime')
    end_time.text = str(script_end_time)
        
    duration = SubElement(result, 'duration')
    duration.text = str(script_duration)[0:6]
        
    name = SubElement(result, 'name')
    name.text = script_name.strip()
        
    status = SubElement(result, 'status')
    status.text = script_status_code
        
    comment = SubElement(result, 'comment')
    comment.text = script_comment
        
    log_file = SubElement(result, 'logFile')
    log_file.text = script_log_folder
        
    class_name = SubElement(result, 'featureName')
    class_name.text = script_feature_name
    
    #write to result xml file
    meta_data = "results.xml"
    results_xml = open(meta_data, 'w')
    
    #global start-time
    results_xml.write('<results>')
    results_xml.write('<startTime>')
    results_xml.write(str(global_start_time))
    results_xml.write('</startTime>')

    results_xml.write(tostring(result))
    results_xml.write('\n')

    #global end-time
    results_xml.write('<endTime>')
    results_xml.write(str(global_end_time))
    results_xml.write('</endTime>')
    results_xml.write('</results>')
```

## Step 5 Define a script file to convert the XML content into J-Unit format
- Define the schema of the XML file(xlst.xsl)
```xml
<xsl:stylesheet version="1.0"
	xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
	<xsl:output method="xml" version="1.0" encoding="UTF-8"
		indent="yes" />
	<xsl:strip-space elements="*" />
	<xsl:template match="results">
		<testsuites>
			<testsuite tests="{count(result/status)}" passed="{count(result[status='111'])}"
				failure="{count(result[status='100'])}" skipped="{count(result[status='101'])}"
				error="{count(result[status='000'])}" time="{endTime - startTime}">
				<xsl:for-each select="result[status='111']">
					<testcase name="{name}" classname="{featureName}" result="{status}"
						comments="{comment}" time="{duration}">
						<passed />
					</testcase>
				</xsl:for-each>
				<xsl:for-each select="result[status='000']">
					<testcase name="{name}" classname="{featureName}" result="{status}"
						comments="{comment}" time="{duration}">
						<failure message="{failReason}"></failure>
						<error />
					</testcase>

				</xsl:for-each>
				<xsl:for-each select="result[status='100']">
					<testcase name="{name}" classname="{featureName}" result="{status}"
						comments="{comment}" time="{duration}">
						<failure message="{failReason}"></failure>
						<failure />
					</testcase>
				</xsl:for-each>
				<xsl:for-each select="result[status='101']">
					<testcase name="{name}" result="{status}" comments="{comment}">
						<skipped message="Test never ran"></skipped>
						<skipped />
					</testcase>
				</xsl:for-each>
			</testsuite>
		</testsuites>
	</xsl:template>
</xsl:stylesheet>
```
- Use Python to read and convert the XML based on the schema
```python
from lxml import etree, objectify
import os.path
import sys

def generateJUnit(metadata):
        parser = etree.XMLParser(remove_blank_text=True)
        tree = etree.parse(metadata, parser)
        root = tree.getroot()
        for elem in root.getiterator():
            if not hasattr(elem.tag, 'find'): continue  # (1)
            i = elem.tag.find('}')
            if i >= 0:
                elem.tag = elem.tag[i + 1:]
        objectify.deannotate(root, cleanup_namespaces=True)
        tree.write(metadata,pretty_print=True, xml_declaration=True, encoding='UTF-8')
        dom = etree.parse(metadata)
        xslt = etree.parse("XLST.xsl")
        transform = etree.XSLT(xslt)
        newdom = transform(dom)
        str = etree.tostring(newdom, pretty_print=True)
        print("Generating TestResults.xml under TestEnvironment")
        with open("TestEnvironment\TestResults.xml","w")as fp:
            fp.write(str)

if __name__ == '__main__':
    metadata = sys.argv[1]
    generateJUnit(metadata)
```
- Invoke the python script file
```shell
    python JUnitResultGenerator.py
```
## Step 6 Use Allure command-line to generate the HTML report
```shell
allure-commandline\bin\allure.bat generate TestEnvironment -c -o allure-results
```


## References:
- XML format used by J-Unit: http://llg.cubic.org/docs/junit/
- J-Unit data accepted by Allure: https://github.com/allure-framework/allure2/blob/a145f2933bb4c81b5bb0a3f24ae94e4023b71b83/allure-generator/src/test/resources/junitdata/TEST-test.SampleTest.xml

## Back to [index](./index.md)
