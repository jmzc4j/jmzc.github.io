---
title: Maven入门
date: 2019-08-28 16:40:39
tags: maven
categories: 学习笔记
---
### what is
- maven 是 apache软件基金会的一个产品；
- maven 是一个将模型应用于工程的管理系统；
- maven 是 一个Java项目的构建和管理工具，包括项目生命周期的管理和项目依赖的管理；

### why use it
- 简化项目构建，缩短了开发周期；
- 将代码与架包分离，仅仅在POM中提供依赖的引用即可；
- 统一的目录结构和约定，有利于开发者理解开发原则；

### how to config it
- 配置本地仓库路径
```
<settings>
  ...
  <localRepository>D:\Repository\mvnRepo</localRepository>
  ...
</settings>
```
- 配置远程镜像仓库
```
<settings>
  ...
  <mirrors>
    <mirror>
		<id>alimaven</id>
		<name>aliyun maven</name>
		<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
		<mirrorOf>central</mirrorOf>
    </mirror>
  </mirrors>
...
</settings>
```
- 配置JDK版本（也可以使用编译插件）
```
<settings>
  ...
  <profiles>
		<!--定义工程的JDK版本-->
		<profile>
			<id>jdk-1.8</id>
			<activation>
				<activeteByDefault>true</activeteByDefault>
				<jdk>1.8</jdk>
			</activation>
			<properties>
				<maven.compiler.source>1.8</maven.compiler.source>
				<maven.compiler.target>1.8</maven.compiler.target>
				<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
			</properties>
		</profile>
  </profiles>
  ...
</settings>
```

### how to use it
- maven的基本工作单元就是POM，POM中包含着项目的部分描述信息，所有的maven操作都是从POM开始的；
- maven的指令代表着生命周期中的不同阶段，这些指令仅仅是一个抽象层的定义，真实的操作都有由相应的插件来处理的；
- maven项目创建,使用原型（模板）创建不同的项目，过程中默认选择模板7（快速版）和默认1.0-snapshot版本；
```
mvn archetype:generate  
  -DarchetypeGroupId=org.apache.maven.archetypes   
  -DgroupId=com.mycompany.app  
  -DartifactId=my-app  
```
- 创建结果如下：
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <!-- 指定模型的版本，一般不会变，统一版本可以确保稳定性 -->
  <modelVersion>4.0.0</modelVersion>

  <!--
    指定坐标和打包方式：
      groupId：组织和项目唯一标识；
      artifactId：工程基础名称；
      version：版本号和类型；
      packaging：打包方式，默认jar，同时约束了生命周期的最终阶段；
   -->
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <!--
    Maven生成的文档相关：
      name:项目的显示名称;
      url:项目站点的位置;
      description:项目的基本描述;
   -->
  <name>my-app</name>
  <url>http://maven.apache.org</url>

  <！-- 自定义的属性，EL方式引用 -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```
- 目录结构如下：(使用IDE创建的还会有resource目录)
```
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
```
- 其他maven指令：   
  1. `mvn compile`：编译源代码；   
  2. `mvn test`：编译测试源代码并指定命名约定的单元测试；   
  3. `mvn test-compile`：仅编译吃源代码；   
  4. `mvn package`：打包到target目录中；   
  5. `mvn install`：打包到本地仓库中；   
  6. `mvn site`：生成项目的web站点；   
  7. `mvn clean`：清理target的目录；   
  8. `mvn eclipse:eclipse`：生成eclispe项目；   
  9. `mvn idea:idea`：生成idea项目；   
  10. `mvn eclipse:clean `：清楚eclispe配置；   
  11. `mvn dependency:list`：列出所有依赖；   
  12. `mvn deploy`：上传到私服；   
  13. `mvn test -skipping compile -skipping test-compile`：不编译仅测试；   
  14. `mvn -version/-v`：查看maven版本；   
  15. `mvn jetty:run`：jetty上运行；   
  16. `mvn -e`：显示错误信息；   
  17. `mvn validate`：验证工程是否正确，所有需要的资源是否可用；   
  18. `mvn verify`：运行任何检查，验证包是否有效且达到质量标准；   
  19. `mvn dependency:tree`：输出依赖树；   
  20. `mvn tomcat:run`：在tomcat6上运行；   
  21. `mvn help:describe -Dplugin=pluginName -Dgoal(或-Dmojo)=goalName`：列出某个插件的goal信息；   
  22. `mvn tomcat7:run -Dmaven.test.skip=true`：跳过测试；   
  23. `mvn eclipse:eclipse -DskipTests`：生成导入Eclipse中的项目列表。一般在项目导入之前使用；   
  24. `mvn install:install-file -DgroupId=com -DartifactId=client -Dversion=0.1.0 -Dpackaging=jar -Dfile=d:\client-0.1.0.jar`：打包jar到本地库；

### 其他配置
- 如何发布jar到远程仓库
```
<distributionManagement>
    <repository>
      <id>mycompany-repository</id>
      <name>MyCompany Repository</name>
      <url>scp://repository.mycompany.com/repository/maven2</url>
    </repository>
</distributionManagement>
```
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
  ...
  <servers>
    <server>
      <id>mycompany-repository</id>
      <username>jvanzyl</username>
      <!-- Default value is ~/.ssh/id_dsa -->
      <privateKey>/path/to/identity</privateKey> (default is ~/.ssh/id_dsa)
      <passphrase>my_key_passphrase</passphrase>
    </server>
  </servers>
  ...
</settings>
```
- 编译插件
```
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.7.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>
</build>
```
- 测试插件:
```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>2.18.1</version>
    <!-- 同样实现跳过生命周期中的测试阶段 -->
	<configuration>
        <skipTests>true</skipTests>
	</configuration>
</plugin>
```
- 资源文件插件
```
<project>  
  ...  
  <build>  
    ...  
    <resources>  
      <!--
          资源插件默认行为只是将项目主资源文件复制到主代码编译输出目录中，将测试资源文件复制到测试代码编译输出目录中。
      -->
      <resource>  
        <!-- 指定资源文件目录 -->
        <directory>src/main/resources</directory>  
        <!--
          开启资源过滤 ：（默认false）
            filtering：作用是用环境变量，pom文件里定义的属性和指定文件里的属性替换属性文件的占位符。
        -->
        <filtering>true</filtering>
        <!-- includes之中的也会被过滤 -->  
        <includes>  
          <include>**/*.xml</include>
        </includes>
      </resource>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>false</filtering>
        <!-- excludes之外的不会被过滤 -->
        <excludes>
          <exclude>**/*.xml</exclude>  
        </excludes>  
      </resource>  
      ...  
    </resources>  
    ...  
  </build>  
  ...  
</project>
```
- mybatis逆向工程插件
```
<plugin>
	<groupId>org.mybatis.generator</groupId>
	<artifactId>mybatis-generator-maven-plugin</artifactId>
	<version>1.3.7</version>
	<configuration>
		<configurationFile>${basedir}/src/main/resources/generatorConfig.xml</configurationFile>
		<overwrite>true</overwrite>
	</configuration>
</plugin>

goal：mybatis-generator:generate
```

### Maven中的属性
- 内置属性：
  1. `${basedir}`：项目的根目录(包含pom.xml文件的目录；
  2. `${version}`：项目版本；
- POM属性：
  1. `${project.build.sourceDirectory}`：项目的主源码目录，默认为src/main/java；
  2. `${project.build.testSourceDirectory}`：项目的测试源码目录，默认为src/test/java；
  3. `${project.build.directory}`：项目构件输出目录，默认为target/；
  4. `${project.outputDirectory}`：项目主代码编译输出目录，默认为target/classes/；
  5. `${project.testOutputDirectory}`：项目测试代码编译输出目录，默认为target/test-classes/；
  6. `${project.groupId}`：项目的groupId；
  7. `${project.artifactId}`：项目的artifactId；
  8. `${project.version}`：项目的version，与${version}等价；
  9. `${project.build.fianlName}`：项目打包输出文件的名称，默认为${project.artifactId}-${project.version}；
- 自定义属性：用户可以在POM的<properties>元素下自定义Maven属性；
- Settings属性：用户使用settings.开头的属性引用settings.xml文件中XML元素的值；
- Java系统属性：所有Java系统属性都可以使用Maven属性引用；
- 环境变量属性：所有环境变量都可以使用以env.开头的Maven属性引用；

### 聚合工程
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.mycompany.app</groupId>
  <artifactId>app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <modules>
    <module>my-app</module>
    <module>my-webapp</module>
  </modules>
</project>
```
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <parent>
    <groupId>com.mycompany.app</groupId>
    <artifactId>app</artifactId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  ...
```

### 参考文献
- [Apache官方-快速入门](https://maven.apache.org/guides/getting-started/index.html)
- [博客园-Maven属性、profile和资源过滤](https://www.cnblogs.com/forerver-elf/p/6257395.html)
- [博客园-Maven常用命令](https://www.cnblogs.com/wkrbky/p/6352188.html)
- [CSDN-30个常用的 Maven 命令](https://blog.csdn.net/benhuo931115/article/details/80674760)
- [CSDN-maven-surefire-plugin简介](https://blog.csdn.net/en_joker/article/details/84067071)

### 附件
{% asset_link settings.xml maven-settings %}