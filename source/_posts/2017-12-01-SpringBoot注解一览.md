title: SpringBoot注解一览
date: 2017-12-01
categories : 
  - spring-boot
tags : 
  - spring-boot
---

包含Spring和SpringBoot的注解

## Spring


`@ResController`: <==>`@Controller` + `@ResponseBody`

`@Repository`: 一般的dao层

`@Service` : 业务层，一般对于接口和实现

`@Controller`: 控制层，里面有多个连接

`@Component` :  <==> `@Serivce` | `@Controler` |  `@Repository` 

`@ExceptionHandler`: 如果在`@Controller`方法遇到异常，就会调用含有此注解的方法。

`@Configuration`: 标记该class是ApplicationContext的一个bean定义，相当于原来applicationContext.xml文件中的`<bean>`标签。

`@ComponentScan`: 告知Spring去搜索其他组件，配置或者服务。其实就是原来applicationContext.xml文件中的`<mvc:component-scan>`标签.

`@ControllerAdvice` :

`@RestControllerAdvice`:



## SpringBoot


`@EnableAutoConfiguation`: 标记Spring Boot开始加载各种beans（包括自定义的，classpath中的），以及各种属性。

`@SringBootApplication`: <==> `@onfiguration` + `@EnableAutoConfiguation` + `@ComponentScan` 


## SpringColoud相关注解

[SpringCloud注解一览](/2017/12/01/2017-12-01-SpringCloud注解一览/)