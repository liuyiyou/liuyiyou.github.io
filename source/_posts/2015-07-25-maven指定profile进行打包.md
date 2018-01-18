title: maven指定pom文件进行打包
date: "2015-07-25"
categories : 
  - tools
tags : 
  - maven
---

因为使用分布式服务框架，其中对服务端分为两部分，接口和实现。其中接口和实体类对外开放，服务实现是不对外开放的。但是两者却是放在同一个项目中的，对外公布的接口通过服务器上的iWork进行打包。本地则使用常规的打包方式，但是这样造成一个不便就是：如果不上传到iWork上打包，再依赖，而是直接依赖本地的话就比较麻烦。。。。。"


第一种笨办法：都写在pom.xml中，打包的时候分开注释，第一次打包的时候通用打包，第二次打包的时候，通过maven-assembly-plugin进行打包（只打包实体类和服务接口），这样虽然能完成，但是非常麻烦。如果修改频繁，需要来回进行增加和去掉注释。


> pom.xml：服务端打包

```xml

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>cn.liuyiyou.service</groupId>
	<!--针对服务端的打包方式 -->
	<!-- <artifactId>cn.liuyiyou.service.basicservice</artifactId> -->
	<!--针对客户端的打包方式 -->
	<artifactId>ccn.liuyiyou.service.basicservice.contract</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>cn.liuyiyou.service.basicservice</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<build>
	<plugins>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-compiler-plugin</artifactId>
			<configuration>
				<source>1.6</source>
				<target>1.6</target>
				<encoding>UTF-8</encoding>
			</configuration>
		</plugin>
		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-surefire-plugin</artifactId>
			<configuration>
				<skip>true</skip>
			</configuration>
		</plugin>
		<plugin>
			<artifactId>maven-source-plugin</artifactId>
			<version>2.2.1</version>
			<configuration>
				<attach>true</attach>
				<includes>
					<include>com/bj58/daojia/service/basicservice/contract/model/*.java</include>
					<include>com/bj58/daojia/service/basicservice/contract/model/shared/*.java</include>
					<include>com/bj58/daojia/service/basicservice/contract/service/*.java</include>
				</includes>
			</configuration>
			<executions>
				<execution>
					<phase>compile</phase>
					<goals>
						<goal>jar</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
		<plugin>
			<artifactId>maven-assembly-plugin</artifactId>
			<configuration>
				<appendAssemblyId>false</appendAssemblyId>
				<descriptors>
					<descriptor>deploy.xml</descriptor>
				</descriptors>
			</configuration>
			<executions>
				<execution>
					<id>make-assembly</id>
					<phase>package</phase>
					<goals>
						<goal>single</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
	</plugins>
</build>
</project>

```


> deploy.xml，只打包客户端包

```xml

<?xml version="1.0" encoding="UTF-8"?>
<assembly
	xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
	
	<id>package</id>
	<formats>
		<format>jar</format>
	</formats>
	<includeBaseDirectory>false</includeBaseDirectory>
	<fileSets>
		<fileSet>
			<directory>target/classes/cn.liuyiyouservice/basicservice/contract/model</directory>
			<outputDirectory>/cn.liuyiyouservice/basicservice/contract/model</outputDirectory>
		</fileSet>
		<fileSet>
			<directory>target/classes/cn.liuyiyouservice/basicservice/contract/model/shared</directory>
			<outputDirectory>/cn.liuyiyouservice/basicservice/contract/model/shared</outputDirectory>
		</fileSet>
		<fileSet>
			<directory>target/classes/cn.liuyiyouservice/basicservice/contract/service/</directory>
			<outputDirectory>/cn.liuyiyouservice/basicservice/contract/service</outputDirectory>
		</fileSet>
		<fileSet>
			<directory>target/classes/cn.liuyiyouservice/basicservice/contract/service/</directory>
			<outputDirectory>/cn.liuyiyouservice/basicservice/contract/service</outputDirectory>
		</fileSet>
	</fileSets>
</assembly>

```




第二种办法： 通过mvn package -f  指定pom文件

比如只打包服务端： mvn package -f pom.xml
打包客户端端： mvn package -f deploy_pom.xml

在这种情况下，target目录中出现了两个jar包。


第三种方法：


##参考资料

https://www.zybuluo.com/haokuixi/note/25985

http://agiledon.github.io/blog/2013/11/10/create-two-jars-from-one-project-using-maven/