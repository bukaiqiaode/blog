> Since the service-interfaces are registered onto zoo-keeper, we can now try to consume them.

# Step 6 Prepare the consumer-project
1. Create the folder-structure for the consumer-project.
```shell
e:\Downloads\documents\STS_projects\justProject>cd consumer

e:\Downloads\documents\STS_projects\justProject\consumer>mkdir src\main\java\mconsumer

e:\Downloads\documents\STS_projects\justProject\consumer>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\CONSUMER
└─src
    └─main
        └─java
            └─mconsumer
```
2. Create `pom.xml` for consumer-project, it also depends on the library-project.
> Content of `consumer\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>consumer</artifactId>
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
3. Add the module definition in parent-project.
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
		<module>common_api</module>
		<module>consumer</module>
		<module>provider</module>
	</modules>
</project>
```
4. Add an entry-point for the consumer-project.
> Content of `consumer\src\main\java\mconsumer\Consumer.java`
```java
package mconsumer;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Consumer {
    public static void main(String[] args) {
        SpringApplication.run(Consumer.class, args);
    }
}
```
5. Try to package this empty project.
```shell

e:\Downloads\documents\STS_projects\justProject\consumer>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\CONSUMER
│  .classpath
│  .project
│  pom.xml
│
├─.settings
│      org.eclipse.core.resources.prefs
│      org.eclipse.jdt.core.prefs
│      org.eclipse.wst.common.project.facet.core.xml
│
└─src
    └─main
        └─java
            └─mconsumer
                    Consumer.java


e:\Downloads\documents\STS_projects\justProject\consumer>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:consumer >------------------
[INFO] Building consumer 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ consumer ---
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\consumer\src\main\resources
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\consumer\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ consumer ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to e:\Downloads\documents\STS_projects\justProject\consumer\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\consumer\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ consumer ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ consumer ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ consumer ---
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\consumer\target\consumer-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.5.7.RELEASE:repackage (default) @ consumer ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.170 s
[INFO] Finished at: 2018-08-01T14:07:58+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\consumer>
```

# Step 7 Add a controller for the consumer-project
1. To use annotation @RestController, add dependency in `pom.xml` of the consumer-project.
> Content of `consumer\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>consumer</artifactId>
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
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
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
2. Write a controller to handle the request.
> Content of `consumer\src\main\java\mconsumer\SampleController.java`
```java
package mconsumer;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class SampleController {
	@RequestMapping("/test")
	public String test() {
		return "hello from test.";
	}
}
```

3. Create the `resources` folder for the consumer-project.
```shell
e:\Downloads\documents\STS_projects\justProject\consumer>mkdir src\main\resources

e:\Downloads\documents\STS_projects\justProject\consumer>mvn clean
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:consumer >------------------
[INFO] Building consumer 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ consumer ---
[INFO] Deleting e:\Downloads\documents\STS_projects\justProject\consumer\target
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.098 s
[INFO] Finished at: 2018-08-01T14:18:29+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\consumer>tree .
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\CONSUMER
├─.settings
└─src
    └─main
        ├─java
        │  └─mconsumer
        └─resources
```
4. Create the `application.properties` file to save the configuration for consumer-project
> Content of `consumer\src\main\resources\application.properties`
```
## Use a different port
server.port=6666
```
5. Package and run the consumer-project to see.
```shell
e:\Downloads\documents\STS_projects\justProject\consumer>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:consumer >------------------
[INFO] Building consumer 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ consumer ---
[INFO] Deleting e:\Downloads\documents\STS_projects\justProject\consumer\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ consumer ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to e:\Downloads\documents\STS_projects\justProject\consumer\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\consumer\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ consumer ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ consumer ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ consumer ---
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\consumer\target\consumer-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.5.7.RELEASE:repackage (default) @ consumer ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 5.545 s
[INFO] Finished at: 2018-08-01T14:23:42+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\consumer>java -jar target\consumer-1.0-SNAPSHOT.jar
```
6. Access `http://localhost:666/test` or use `curl` to see that the consumer is working.
```shell
C:\Users\home>curl http://localhost:6666/test
hello from test.
```

# Step 8 Change consumer-project to consume from zoo-keeper
1. Add dependency to dubbo & zoo-keeper
> Content of `consumer\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.justproject.example</groupId>
	<artifactId>consumer</artifactId>
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
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!-- dubbo & zoo-keeper -->
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
2. Add a service-class, which refers to the service-interface.
> Content of `consumer\src\main\java\mconsumer\AnotherServiceConsumer.java`
```java
package mconsumer;

import org.springframework.stereotype.Component;
import com.alibaba.dubbo.config.annotation.Reference;
import mapi.IAnotherService;

@Component
public class AnotherServiceConsumer {
	@Reference
	IAnotherService anotherService;
	
	public String printSomething() {
		return anotherService.sayHello();
	}
}
```

3. Update the controller to use the service-class.
> Content of `consumer\src\main\java\mconsumer\SampleController.java`
```java
package mconsumer;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class SampleController {
	
	@Autowired
	AnotherServiceConsumer consumer;
	
	@RequestMapping("/test")
	public String test() {
		return consumer.printSomething();
	}
}
```
4. Edit `application.properties`, to use zoo-keeper. 
**Here, we should define to scan the package of the service-implementation, not the package where we define the service-interface!!!**
> Content of `consumer\src\main\resources\application.properties`
```python
## Use a different port
server.port=6666
## Configuration as a dubbo-consumer
spring.dubbo.application.name=provider
spring.dubbo.registry.address=zookeeper://xxx.xxx.xxx.xxx:2181
## package to scan for the service
spring.dubbo.scan=mconsumer
```
5. Package the consumer-project & run it.
```shell
e:\Downloads\documents\STS_projects\justProject\consumer>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:consumer >------------------
[INFO] Building consumer 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ consumer ---
[INFO] Deleting e:\Downloads\documents\STS_projects\justProject\consumer\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ consumer ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 3 source files to e:\Downloads\documents\STS_projects\justProject\consumer\target\classes
[INFO]
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ consumer ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory e:\Downloads\documents\STS_projects\justProject\consumer\src\test\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ consumer ---
[INFO] No sources to compile
[INFO]
[INFO] --- maven-surefire-plugin:2.18.1:test (default-test) @ consumer ---
[INFO] No tests to run.
[INFO]
[INFO] --- maven-jar-plugin:2.6:jar (default-jar) @ consumer ---
[INFO] Building jar: e:\Downloads\documents\STS_projects\justProject\consumer\target\consumer-1.0-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:1.5.7.RELEASE:repackage (default) @ consumer ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 6.022 s
[INFO] Finished at: 2018-08-01T16:31:42+08:00
[INFO] ------------------------------------------------------------------------

e:\Downloads\documents\STS_projects\justProject\consumer>java -jar target\consumer-1.0-SNAPSHOT.jar
```
6. Access `http://localhost:666/test` or use `curl` to see that the consumer is working.
```shell
C:\Users\home>curl http://localhost:6666/test
Hello from AnotherServiceImp
```

# More
We can configure to ask Spring to scan multiple-packages, in case we defined different services in the practice. Example of `application.properties` as below.
```python
## Configuration as a dubbo-provider
spring.dubbo.application.name=provider
spring.dubbo.registry.address=zookeeper://aaa.bbb.ccc.ddd:2181
spring.dubbo.protocol.name=dubbo
spring.dubbo.protocol.port=20880
## package to scan for the service
spring.dubbo.scan=mservice,moreservice
```

# References
- [Blog post that resolved the key-point](https://blog.csdn.net/VcStrong/article/details/78712647)
- [Blog post explains spring.dubbo.scan](https://blog.csdn.net/jeffli1993/article/details/71480627)
