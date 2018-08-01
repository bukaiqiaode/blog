>We know that both provider & consumer need to have the same definition of service-interface, in order to co-work through zoo-keeper and dubbo.
It will be good, if we can split the definition of service-interface into a library-project, which can be shared by provider & consumer.

# Step 4 Separate the service-interface into a library-project
1. Create the folder-structure for the library-project.
```shell
e:\Downloads\documents\STS_projects\justProject>cd common_api

e:\Downloads\documents\STS_projects\justProject\common_api>mkdir src\main\java\mapi

e:\Downloads\documents\STS_projects\justProject\common_api>tree .
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\COMMON_API
└─src
    └─main
        └─java
            └─mapi
```
2. Create the `pom.xml` for the library-project
> Content of `common_api\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>common_api</artifactId>
	<packaging>jar</packaging>
	<version>1.0-SNAPSHOT</version>

	<parent>
		<groupId>com.justproject.example</groupId>
		<artifactId>parentproject</artifactId>
		<version>1.0-SNAPSHOT</version>
	</parent>

	<dependencies>
	</dependencies>
</project>
```
3. Try to build this empty project
```shell

e:\Downloads\documents\STS_projects\justProject\common_api>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\COMMON_API
│  pom.xml
│
└─src
    └─main
        └─java
            └─mapi

e:\Downloads\documents\STS_projects\justProject\common_api>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< com.justproject.example:common_api >-----------------
[INFO] Building common_api 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ common_api ---
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ common_api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ common_api ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ common_api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ common_api ---
[WARNING] JAR will be empty - no content was marked for inclusion!
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.925 s
[INFO] Finished at: 2018-08-01T12:29:09+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\common_api>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\COMMON_API
│  pom.xml
│
├─src
│  └─main
│      └─java
│          └─mapi
└─target
    │  common_api-1.0-SNAPSHOT.jar
    │
    ├─maven-archiver
    │      pom.properties
    │
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
                        inputFiles.lst


e:\Downloads\documents\STS_projects\justProject\common_api>
```
4. Add a module in parent-project.
> `pom.xml` of parent-project
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>parentproject</artifactId>
	<packaging>pom</packaging>
	<version>1.0-SNAPSHOT</version>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.7.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-autoconfigure</artifactId>
		</dependency>
	</dependencies>

	<modules>
		<module>provider</module>
		<module>common_api</module>
	</modules>
</project>
```

5. Define another service-interface in the library-project
> Content of `common_api\src\main\java\mapi\IAnotherService.java`
```java
package mapi;

public interface IAnotherService{
	String sayHello();
}
```
6. Package the library-project again.
```shell
e:\Downloads\documents\STS_projects\justProject\common_api>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\COMMON_API
│  pom.xml
│
├─src
│  └─main
│      └─java
│          └─mapi
│                  IAnotherService.java
│
└─target
    │  common_api-1.0-SNAPSHOT.jar
    │
    ├─maven-archiver
    │      pom.properties
    │
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
                        inputFiles.lst


e:\Downloads\documents\STS_projects\justProject\common_api>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< com.justproject.example:common_api >-----------------
[INFO] Building common_api 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ common_api ---
[INFO] Deleting e:\Downloads\documents\STS_projects\justProject\common_api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ common_api ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to e:\Downloads\documents\STS_projects\justProject\common_api\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ common_api ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ common_api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ common_api ---
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.000 s
[INFO] Finished at: 2018-08-01T13:27:28+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\common_api>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\COMMON_API
│  pom.xml
│
├─src
│  └─main
│      └─java
│          └─mapi
│                  IAnotherService.java
│
└─target
    │  common_api-1.0-SNAPSHOT.jar
    │
    ├─classes
    │  └─mapi
    │          IAnotherService.class
    │
    ├─generated-sources
    │  └─annotations
    ├─maven-archiver
    │      pom.properties
    │
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
                        createdFiles.lst
                        inputFiles.lst


e:\Downloads\documents\STS_projects\justProject\common_api>jar tvf target\common_api-1.0-SNAPSHOT.jar
     0 Wed Aug 01 13:27:28 CST 2018 META-INF/
   387 Wed Aug 01 13:27:28 CST 2018 META-INF/MANIFEST.MF
     0 Wed Aug 01 13:27:26 CST 2018 mapi/
   158 Wed Aug 01 13:27:26 CST 2018 mapi/IAnotherService.class
     0 Wed Aug 01 13:27:28 CST 2018 META-INF/maven/
     0 Wed Aug 01 13:27:28 CST 2018 META-INF/maven/com.justproject.example/
     0 Wed Aug 01 13:27:28 CST 2018 META-INF/maven/com.justproject.example/common_api/
   613 Wed Aug 01 12:27:38 CST 2018 META-INF/maven/com.justproject.example/common_api/pom.xml
   137 Wed Aug 01 13:27:28 CST 2018 META-INF/maven/com.justproject.example/common_api/pom.properties

e:\Downloads\documents\STS_projects\justProject\common_api>
```

# Step 5 Make provider-project depend on library-project
1. Define the dependency in `pom.xml` of the provider-project.
> Content of `provider\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>provider</artifactId>
	<packaging>jar</packaging>
	<version>1.0-SNAPSHOT</version>


	<parent>
		<groupId>com.justproject.example</groupId>
		<artifactId>parentproject</artifactId>
		<version>1.0-SNAPSHOT</version>
	</parent>

	<dependencies>
		<dependency>
			<groupId>com.justproject.example</groupId>
			<artifactId>common_api</artifactId>
			<version>1.0-SNAPSHOT</version>
		</dependency>
		<dependency>
			<groupId>io.dubbo.springboot</groupId>
			<artifactId>spring-boot-starter-dubbo</artifactId>
			<version>1.0.0</version>
		</dependency>
		<dependency>
			<groupId>org.apache.zookeeper</groupId>
			<artifactId>zookeeper</artifactId>
			<version>3.4.6</version>
			<exclusions>
				<exclusion>
					<groupId>org.slf4j</groupId>
					<artifactId>slf4j-log4j12</artifactId>
				</exclusion>
				<exclusion>
					<groupId>log4j</groupId>
					<artifactId>log4j</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>


	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```
2. Add a service in provider-project, to implement the service-interface in library-project. Also use the dubbo-annotation `@Service` here.
> Content of `provider\src\main\java\mservice\AnotherServiceImp.java`
```java
package mservice;

import com.alibaba.dubbo.config.annotation.Service;

import mapi.IAnotherService;

@Service
public class AnotherServiceImp implements IAnotherService {

	@Override
	public String sayHello() {
		return "Hello from AnotherServiceImp";
	}

}
```
3. Try to package the provider-project now.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[WARNING] The POM for com.justproject.example:common_api:jar:1.0-SNAPSHOT is missing, no dependency information available
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.301 s
[INFO] Finished at: 2018-08-01T13:37:04+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal on project provider: Could not resolve dependencies for project com.justproject.example:provider:jar:1.0-SNAPSHOT: Could not find artifact com.justproject.example:common
_api:jar:1.0-SNAPSHOT -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/DependencyResolutionException
```
The reason of the failure is that, Maven cannot find the expected `jar` package of the library-project.

4. Package & Install the library-project into local-repository, so that it could be included by the provider-project.
```shell

e:\Downloads\documents\STS_projects\justProject\common_api>mvn clean package install
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< com.justproject.example:common_api >-----------------
[INFO] Building common_api 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ common_api ---
[INFO] Deleting e:\Downloads\documents\STS_projects\justProject\common_api\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ common_api ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to e:\Downloads\documents\STS_projects\justProject\common_api\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ common_api ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ common_api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ common_api ---
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ common_api ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\common_api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ common_api ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ common_api ---
[INFO] No tests to run.
[INFO] Skipping execution of surefire because it has already been run for this configuration
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ common_api ---
[INFO]
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ common_api ---
[INFO] Installing e:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar to C:\Users\home\.m2\repository\com\justproject\example\common_api\1.0-SNAPSHOT\common_a
pi-1.0-SNAPSHOT.jar
[INFO] Installing e:\Downloads\documents\STS_projects\justProject\common_api\pom.xml to C:\Users\home\.m2\repository\com\justproject\example\common_api\1.0-SNAPSHOT\common_api-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.555 s
[INFO] Finished at: 2018-08-01T13:38:47+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\common_api>
```
5. Install the parent-project.
```shell

E:\Downloads\documents\STS_projects\justProject\provider>cd ..

E:\Downloads\documents\STS_projects\justProject>mvn install
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] parentproject                                                      [pom]
[INFO] common_api                                                         [jar]
[INFO] provider                                                           [jar]
[INFO]
[INFO] ---------------< com.justproject.example:parentproject >----------------
[INFO] Building parentproject 1.0-SNAPSHOT                                [1/3]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ parentproject ---
[INFO] Installing E:\Downloads\documents\STS_projects\justProject\pom.xml to C:\Users\home\.m2\repository\com\justproject\example\parentproject\1.0-SNAPSHOT\parentproject-1.0-SNAPSHOT.pom
[INFO]
[INFO] -----------------< com.justproject.example:common_api >-----------------
[INFO] Building common_api 1.0-SNAPSHOT                                   [2/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\common_api\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ common_api ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to E:\Downloads\documents\STS_projects\justProject\common_api\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common_api ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\common_api\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ common_api ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ common_api ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ common_api ---
[INFO] Building jar: E:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ common_api ---
[INFO] Installing E:\Downloads\documents\STS_projects\justProject\common_api\target\common_api-1.0-SNAPSHOT.jar to C:\Users\home\.m2\repository\com\justproject\example\common_api\1.0-SNAPSHOT\common_a
pi-1.0-SNAPSHOT.jar
[INFO] Installing E:\Downloads\documents\STS_projects\justProject\common_api\pom.xml to C:\Users\home\.m2\repository\com\justproject\example\common_api\1.0-SNAPSHOT\common_api-1.0-SNAPSHOT.pom
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT                                     [3/3]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ provider ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 4 source files to E:\Downloads\documents\STS_projects\justProject\provider\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ provider ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ provider ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ provider ---
[INFO] Building jar: E:\Downloads\documents\STS_projects\justProject\provider\target\provider-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.5.7.RELEASE:repackage (default) @ provider ---
[INFO]
[INFO] --- maven-install-plugin:2.5.2:install (default-install) @ provider ---
[INFO] Installing E:\Downloads\documents\STS_projects\justProject\provider\target\provider-1.0-SNAPSHOT.jar to C:\Users\home\.m2\repository\com\justproject\example\provider\1.0-SNAPSHOT\provider-1.0-S
NAPSHOT.jar
[INFO] Installing E:\Downloads\documents\STS_projects\justProject\provider\pom.xml to C:\Users\home\.m2\repository\com\justproject\example\provider\1.0-SNAPSHOT\provider-1.0-SNAPSHOT.pom
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] parentproject 1.0-SNAPSHOT ......................... SUCCESS [  0.498 s]
[INFO] common_api ......................................... SUCCESS [  2.738 s]
[INFO] provider 1.0-SNAPSHOT .............................. SUCCESS [  1.873 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.872 s
[INFO] Finished at: 2018-08-01T13:41:42+08:00
[INFO] ------------------------------------------------------------------------

E:\Downloads\documents\STS_projects\justProject>
```

6. Now try to package the provider-project again.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ provider ---
[INFO] Nothing to compile - all classes are up to date
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ provider ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ provider ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ provider ---
[INFO] Building jar: E:\Downloads\documents\STS_projects\justProject\provider\target\provider-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.5.7.RELEASE:repackage (default) @ provider ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.366 s
[INFO] Finished at: 2018-08-01T13:42:42+08:00
[INFO] ------------------------------------------------------------------------
```

7. Run the provider.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>java -jar target\provider-1.0-SNAPSHOT.jar
```
8. Check at zoo-keeper, to see that the two services are registered.
```shell
[zk: 127.0.0.1:2181(CONNECTED) 34] ls /
[dubbo, zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 35] ls /dubbo
[mapi.IAnotherService, mservice.IDemoService]
```
