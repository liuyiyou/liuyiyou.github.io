title: SpringCloud注解一览
date: 2017-12-01
categories : 
  - spring-cloud
tags : 
  - spring-cloud
---


## SpringBoot

SpringBoot和Spring相关配置请参考：[SpringBoot注解一览](/2017/12/01/2017-12-01-SpringBoot注解一览/)  

## SpringCloud


`@SpringCloudApplication`： <==>  `@SpringBootApplication` + `@EnableDiscoveryClient` + `@EnableCircuitBreaker`


### Eureka服务注册

`@EnableEurekaServer` ：开启注册中心服务器的功能。

`@EnableDiscoveryClient` : 开启发现注册中心服务器的功能。Eureka、Consul、Zookeeper、ETCD等服务都可由此来发现

`@EnableEurekaClient` : 开启发现注册中心服务器的功能。注解是基于spring-cloud-netflix依赖，只能为Eureka作用，如果你的classpath中添加了eureka，则它们的作用是一样的




### Config配置中心

`@EnableConfigServer` ：开启Spring Cloud Config 服务端功能


### Ribbon 

`@LoadBalanced`开启客户端负载均衡


### Hystrix 

EnableHystrixDashboard

`@EnableHystrix`表示启动断路器，断路器依赖于服务注册和发现。

## 配置一览


spring.application.name：配置应用名称，在注册中心中显示的服务注册名称。
spring.cloud.client.ipAddress：获取客户端的IP地址。