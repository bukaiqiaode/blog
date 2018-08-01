# Background
Currently, we use Dubbo + Zoo-keeper as the framework for service discovery.
We need different teammates to work on different part of the code, while sharing the same common-library. So we need to investigate the way to manage a project with multiple modules.
To simplify the developing, we also use Spring-boot as the foundation, Maven as the management tool.

# Step 1 Create the parent project
1. Create the folder-structure for the parent-project. The parent-project just works as a container for the modules
```shell
E:\Downloads\documents\STS_projects\justProject>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT
│  pom.xml
│
├─common_api
├─consumer
└─provider
```
2. Create the `pom.xml` for the parent-project, declare it to be a spring-boot project.
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
</project>
```
3. Compile the parent-project for a try. Currently the empty project can also be compiled.
```shell
E:\Downloads\documents\STS_projects\justProject>mvn clean
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------< com.justproject.example:parentproject >----------------
[INFO] Building parentproject 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ parentproject ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.058 s
[INFO] Finished at: 2018-08-01T10:16:50+08:00
[INFO] ------------------------------------------------------------------------



E:\Downloads\documents\STS_projects\justProject>mvn compile
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------< com.justproject.example:parentproject >----------------
[INFO] Building parentproject 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.196 s
[INFO] Finished at: 2018-08-01T10:16:58+08:00
[INFO] ------------------------------------------------------------------------



E:\Downloads\documents\STS_projects\justProject>mvn package
[INFO] Scanning for projects...
[INFO]
[INFO] ---------------< com.justproject.example:parentproject >----------------
[INFO] Building parentproject 1.0-SNAPSHOT
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.189 s
[INFO] Finished at: 2018-08-01T10:17:49+08:00
[INFO] ------------------------------------------------------------------------
```

# Step 2 Create the provider-project
1. Create the folder-structure for the provider-project
```shell
E:\Downloads\documents\STS_projects\justProject>cd provider

E:\Downloads\documents\STS_projects\justProject\provider>mkdir src\main\java\app

E:\Downloads\documents\STS_projects\justProject\provider>tree . /f
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\PROVIDER
└─src
    └─main
        └─java
            └─app
```
2. Create the `pom.xml` for the provider-project.
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
3. In the `pom.xml` of the parent-project, add this provider-project as a module.
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

	<modules>
		<module>provider</module>
	</modules>
</project>
```
4. Create a main-entry for the project
> Content of `provider\src\main\java\app\Provider.java`
```java
package app;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Provider {
    public static void main(String[] args) {
        SpringApplication.run(Provider.class, args);
    }
}
```
5. Try packaging the provider-project now.
```shell

E:\Downloads\documents\STS_projects\justProject\provider>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ provider ---
[INFO] Deleting E:\Downloads\documents\STS_projects\justProject\provider\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\main\resources
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ provider ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to E:\Downloads\documents\STS_projects\justProject\provider\target\classes
[INFO] -------------------------------------------------------------
[ERROR] COMPILATION ERROR :
[INFO] -------------------------------------------------------------
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[3,32] 程序包org.springframework.boot不存在
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[4,46] 程序包org.springframework.boot.autoconfigure不存在
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[6,2] 找不到符号
  符号: 类 SpringBootApplication
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[9,9] 找不到符号
  符号:   变量 SpringApplication
  位置: 类 app.Provider
[INFO] 4 errors
[INFO] -------------------------------------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.919 s
[INFO] Finished at: 2018-08-01T11:02:59+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project provider: Compilation failure: Compilation failure:
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[3,32] 程序包org.springframework.boot不存在
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[4,46] 程序包org.springframework.boot.autoconfigure不存在
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[6,2] 找不到符号
[ERROR]   符号: 类 SpringBootApplication
[ERROR] /E:/Downloads/documents/STS_projects/justProject/provider/src/main/java/app/Provider.java:[9,9] 找不到符号
[ERROR]   符号:   变量 SpringApplication
[ERROR]   位置: 类 app.Provider
[ERROR] -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoFailureException

E:\Downloads\documents\STS_projects\justProject\provider>
```
The build is failed, and the reason is that we didn't declare the dependencies for the provider-project.

6. Since we all base on Spring-book, update the `pom.xml` for the parent-project, to include the common Spring features.
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
	</modules>
</project>
```

7. Build the provider-project again, and see `build success`
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ provider ---
[INFO] Deleting E:\Downloads\documents\STS_projects\justProject\provider\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\main\resources
[INFO] skip non existing resourceDirectory E:\Downloads\documents\STS_projects\justProject\provider\src\main\resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ provider ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to E:\Downloads\documents\STS_projects\justProject\provider\target\classes
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
[INFO] Total time: 8.458 s
[INFO] Finished at: 2018-08-01T11:05:58+08:00
[INFO] ------------------------------------------------------------------------
```

# Step 3 Make provider to register to Zoo-Keeper
1. Add dependency to dubbo & zoo-keeper for provider-project.
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

2. Create a new package `service` to put interfaces & implementations of exposed services.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mkdir src\main\java\mservice

E:\Downloads\documents\STS_projects\justProject\provider>tree .
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\PROVIDER
├─src
│  └─main
│      ├─java
│      │  ├─app
│      │  └─mservice
└─target
    ├─classes
    │  └─app
    ├─generated-sources
    │  └─annotations
    ├─maven-archiver
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
```

3. Add a service-interface
> Content of `provider\src\main\java\mservice\IDemoService.java`
```java
package mservice;

public interface IDemoService{
	String sayHello();
}
```

4. Create a class to implement the interface
> Content of `provider\src\main\java\mservice\DemoServiceImp.java`
```java
package mservice;

public class DemoServiceImp implements IDemoService{
    @Override
    public String sayHello() {
        return "Hello from DemoServiceImp";
    }
}
```
5. Use annotation `@Service` from dubbo, to mark it as a dubbo-service
> Content of `provider\src\main\java\mservice\DemoServiceImp.java`
```java
package mservice;

import com.alibaba.dubbo.config.annotation.Service;

@Service
public class DemoServiceImp implements IDemoService{
    @Override
    public String sayHello() {
        return "Hello from DemoServiceImp";
    }
}
```

6. Create the `resources` folder for provider-project.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mkdir src\main\resources

E:\Downloads\documents\STS_projects\justProject\provider>tree .
卷 Tools 的文件夹 PATH 列表
卷序列号为 E885-5830
E:\DOWNLOADS\DOCUMENTS\STS_PROJECTS\JUSTPROJECT\PROVIDER
├─src
│  └─main
│      ├─java
│      │  ├─app
│      │  └─mservice
│      └─resources
└─target
    ├─classes
    │  └─app
    ├─generated-sources
    │  └─annotations
    ├─maven-archiver
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
```

7. Create the `application.properties` file to save the configuration for provider-project.
**Here, we should define to scan the package of the service-implementation, not the package where we define the service-interface!!!**
> Content of `provider\src\main\resources\application.properties`
```xml
## Configuration for dubbo
# this is a provider
spring.dubbo.application.name=provider
# register to this zoo-keeper
spring.dubbo.registry.address=zookeeper://xxx.xxx.xxx.xxx:2181
# use dubbo-protocol
spring.dubbo.protocol.name=dubbo
# locally, use this port to communicate with zoo-keeper
spring.dubbo.protocol.port=20880
# scan this package to find the service-interfaces
spring.dubbo.scan=mservice
```
8. Build the project again, to see the effect.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>mvn clean package
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< com.justproject.example:provider >------------------
[INFO] Building provider 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.6.1:clean (default-clean) @ provider ---
[INFO] Deleting E:\Downloads\documents\STS_projects\justProject\provider\target
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ provider ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource
[INFO] Copying 0 resource
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ provider ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 3 source files to E:\Downloads\documents\STS_projects\justProject\provider\target\classes
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
[INFO] Total time: 5.965 s
[INFO] Finished at: 2018-08-01T11:43:42+08:00
[INFO] ------------------------------------------------------------------------
```

9. Run the `jar`, from command-line.
```shell
E:\Downloads\documents\STS_projects\justProject\provider>java -jar target\provider-1.0-SNAPSHOT.jar

......
......
2018-08-01 11:48:24.641  INFO 19828 --- [           main] app.Provider                             : Started Provider in 4.749 seconds (JVM running for 5.794)
```

10. Log onto the server where zoo-keeper is running, to check the registered service. Here we can assure that our service has been registered to zoo-keeper.
```shell
[xxx@VM_0_8_centos bin]#  ./zkCli.sh -server 127.0.0.1:2181

[zk: 127.0.0.1:2181(CONNECTED) 3] ls /
[dubbo, zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 5] get /dubbo/mservice.IDemoService
null
cZxid = 0x5
ctime = Wed Aug 01 11:48:26 CST 2018
mZxid = 0x5
mtime = Wed Aug 01 11:48:26 CST 2018
pZxid = 0xa
cversion = 2
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 2
```

# References
- [Blog-post on Dubbo + Spring](https://www.cnblogs.com/jaycekon/p/SpringBootDubbo.html)
- [Official document of zoo-keeper command](https://zookeeper.apache.org/doc/current/zookeeperStarted.html#sc_ConnectingToZooKeeper)
