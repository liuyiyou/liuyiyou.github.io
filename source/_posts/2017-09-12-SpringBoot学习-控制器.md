title: SpringBoot学习-控制器
date: 2017-09-12
categories :
  - spring-boot
tags :
  - spring-boot
---

在上一篇[SpringBoot学习-初探](/2017-09-10-SpringBoot学习-初探/)中搭建了一个最简单SpringBoot程序，后续不再提供项目搭建截图，直接采用maven模式

## 内容
- 返回json
- 返回模板
- Circular view path问题解决方案
- 使用fastjson代替jackson

## 返回json
从spring4.0开始，Spring提供了`@RestController`注解，提供该注解是一个组合注解 `@Controller` && `@ResponseBody`。SpringBoot常见方式是使用@RestController，在控制层加上该注解之后，默认返回json，而不需要额外的配置。

下面的两个方法，如果使用`@Controller`进行注解，会找对应模板下的Hello模板，但是使用`@RestController`进行注解的时候，直接返回内容
```java
@RestController
@RequestMapping("/rest/")
public class HelloRestController {

    /**
     * 返回json
     * @return
     */
    @RequestMapping("hello")
    public String sayHello() {
        return "Hello";
    }
}


```

## 返回模板

想要返回模板，需要加入对应的starter，以thymeleaf为例，首先需要加入如下依赖

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

如果不加该依赖（获取其他依赖）会产生两个主要问题

- 404
- Circular view path 异常：

404原因：如果没有添加对应的依赖，默认使用InterResourceViewResolver和JStlView，导致模板定位不准？


```sh
javax.servlet.ServletException: Circular view path [hello2]: would dispatch back to the current handler URL [/controller/hello2] again. Check your ViewResolver setup! (Hint: This may be the result of an unspecified view, due to default view name generation.)
	at org.springframework.web.servlet.view.InternalResourceView.prepareForRendering(InternalResourceView.java:205) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.view.InternalResourceView.renderMergedOutputModel(InternalResourceView.java:145) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.view.AbstractView.render(AbstractView.java:303) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.render(DispatcherServlet.java:1286) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.processDispatchResult(DispatcherServlet.java:1041) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:984) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:901) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
	at org.springframework.web.servlet.FrameworkServlet.doGet(FrameworkServlet.java:861) ~[spring-webmvc-4.3.13.RELEASE.jar:4.3.13.RELEASE]
```

## Circular view path问题解决方案

Circular view path 异常引发方式：

1. 没有添加thymeleaf
2. 如下所示：

```java
@Controller
@RequestMapping("/controller/")
public class HelloController {
    @RequestMapping("hello2")
    public String hello2(){
        return "hello2";
    }
  }
```

原因：https://stackoverflow.com/questions/18813615/how-to-avoid-the-circular-view-path-exception-with-spring-mvc-test  

当没有声明ViewResolver时，spring会给你注册一个默认的ViewResolver，就是JstlView的实例， 该对象继承自InternalResoureView。
JstlView用来封装JSP或者同一Web应用中的其他资源，它将model对象作为request请求的属性值暴露出来, 并将该请求通过javax.servlet.RequestDispatcher转发到指定的URL.
Spring认为， 这个view的URL是可以用来指定同一web应用中特定资源的，是可以被RequestDispatcher转发的。
也就是说，在页面渲染(render)之前，Spring会试图使用RequestDispatcher来继续转发该请求。如下代码：

```java
if (path.startsWith("/") ? uri.equals(path) : uri.equals(StringUtils.applyRelativePath(uri, path))) {
    throw new ServletException("Circular view path [" + path + "]: would dispatch back " +
                        "to the current handler URL [" + uri + "] again. Check your ViewResolver setup! " +
                        "(Hint: This may be the result of an unspecified view, due to default view name generation.)");
}
```

解决方法

- 添加thymeleaf
- 使用内置的InterResourceViewResolver，这个后续在集成jsp做模板的时候使用

## 使用fastjson代替jackson

原因：

- 可能引发的中文乱码：这个在SpringBoot下很少发生
- 日期格式问题，jackson序列化的日期是long

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.35</version>
</dependency>
```

```java
@SpringBootApplication
public class HelloApplication extends WebMvcConfigurerAdapter {


    public static void main(String[] args) {
        // 程序启动入口
        // 启动嵌入式的 Tomcat 并初始化 Spring 环境及其各 Spring 组件
        SpringApplication.run(HelloApplication.class, args);
    }


    static String dateFormat = "yyyy-MM-dd HH:mm:ss";

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        FastJsonHttpMessageConverter fastConverter =
                new FastJsonHttpMessageConverter();
        FastJsonConfig fastJsonConfig = new FastJsonConfig();
        SerializeConfig serializeConfig = SerializeConfig.globalInstance;
        serializeConfig.put(BigInteger.class, ToStringSerializer.instance);
        serializeConfig.put(Long.class, ToStringSerializer.instance);
        serializeConfig.put(Long.TYPE, ToStringSerializer.instance);
        serializeConfig.put(Date.class, new SimpleDateFormatSerializer(dateFormat));
        fastJsonConfig.setSerializeConfig(serializeConfig);
        fastConverter.setFastJsonConfig(fastJsonConfig);
        converters.add(fastConverter);
    }

}

```
