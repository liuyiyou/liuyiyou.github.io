title: SpringBoot学习-异常处理
date: 2017-09-13
categories :
  - spring-boot
tags :
  - spring-boot
---


## 常规异常

## 统一异常


需要注意的是, ExceptionHandler 的优先级比 ControllerAdvice 高, 即 Controller 抛出的异常如果既可以让 ExceptionHandler 标注的方法处理, 又可以让 ControllerAdvice 标注的类中的方法处理, 则优先让 ExceptionHandler 标注的方法处理.



## 处理404

在上述过程中，如果发送异常，则会由统一异常进行管理，但是如果是404，SpringBoot不会抛出异常，而是跳转到error页面，如果没有error页面，则会返回whitepage，将错误信息打印出来。
如果是做接口，返回的话不是很友好。

解决方案：

```java
spring.mvc.throw-exception-if-no-handler-found=true
spring.resources.add-mappings=false
```


## 参考文章
https://segmentfault.com/a/1190000006749441
