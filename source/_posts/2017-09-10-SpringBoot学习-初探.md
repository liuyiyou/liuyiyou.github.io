title: SpringBoot学习-初探
date: 2017-09-10
categories :
  - spring-boot
tags :
  - spring-boot
---

## 前言

本系列文章尽可能每章都是一个详细的知识点，浏览到这篇文章的人，能参考该文章对SpringBoot有一个详细的了解

## 内容
- SpringBoot是什么
- SpringBoot快速入门
- 从SpringMvc到SpringBoot


### SpringBoot是什么

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置

### SpringBoot能做什么
spring boot并不是一个全新的框架，它不是spring解决方案的一个替代品，而是spring的一个封装。所以，你以前可以用spring做的事情，现在用spring boot都可以做。

现在流行微服务与分布式系统，springboot就是一个非常好的微服务开发框架，你可以使用它快速的搭建起一个系统。同时，你也可以使用spring cloud（Spring Cloud是一个基于Spring Boot实现的云应用开发工具）来搭建一个分布式的网站。

### SpringBoot优点
- 简化依赖：不再需要在pom中增加各种依赖，不再需要因为不同框架的不同版本而导致各种NoSuchMethod的异常
- 简化配置：不再需要在xml中进行各种配置
- 独立部署：内嵌tomcat和jetty，直接通过main方法即可启动，可打包成jar进行独立部署
- 提供生产就绪型功能，如指标，健康检查和外部配置
- 和SpringCloud等能进行良好的结合


### SpringBoot缺点
- 封装得太彻底，开发人员对底层了解不够
- 出错排查比较麻烦
- 如果不使用约定配置，也会有一堆配置文件

## SpringBoot快速入门

本节参考官网文档：https://projects.spring.io/spring-boot/#quick-start

### 必要条件
- 编辑工具: 推荐使用idea，eclipse也可以，这里使用idea
- jdk版本： 推荐使用1.8
- spring-boot版本： 1.5.9
- 构建工具： maven，也可以使用gradle，这里使用maven

### 用idea构建

1. 选择sdk版本
![新建项目Step1](/images/spring-boot/springboot-start-step1.jpg)

2. 选择jdk版本、项目类型
![新建项目Step2](/images/spring-boot/springboot-start-step2.jpg)

3. 选择相关组件。因为是快速入门，所以这里只选择一个web。截图中没有选择，需要注意,如果没有选择，则只有Spring-Core相关功能，MVC功能会缺失
![新建项目Step3](/images/spring-boot/springboot-start-step3.jpg)

4. 项目构建成功后，会出现如下所示目录
![新建项目Step4](/images/spring-boot/springboot-start-step4.jpg)

5. 修改`HelloApplication.java`, 并使用main方法启动，访问 http://localhost:8080/hello 即可查看效果

```java
package cn.liuyiyou.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController//这个是新加
public class HelloworldApplication {

	public static void main(String[] args) {
		SpringApplication.run(HelloworldApplication.class, args);
	}
  /**
  * 这个是新加
  */
	@RequestMapping("/hello")
	public String sayHello(){
		return "Hello SpringBoot";
	}
}
```


### 用maven构建

如果不想使用idea工具搭建项目，可以使用maven方式，方式和常规maven项目一致，只是入口类和pom需要自己进行配置

具体pom如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>cn.liuyiyou.springboot</groupId>
	<artifactId>ch01-helloworld</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>ch01-helloworld</name>
	<description>SpringBoot快速入门</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
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
## 从SpringMvc到SpringBoot

上面看上去好像有很多，实际操作起来一分钟都不需要，如果已经有相关项目，直接复制pom文件即可。比起常规的mvc来，不需要各种web.xml、spring-application.xml, spring-servlet.xml等配置

这个待续：大概是三个项目 springmvc-xml, springmvc-config, springmvc-boot

## 源码地址

https://github.com/liuyiyou/cn.liuyiyou.springboot/tree/master/ch01-helloworld

## 参考文章
http://tengj.top/2017/02/26/springboot1/
