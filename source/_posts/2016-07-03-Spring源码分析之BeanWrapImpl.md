date: "2016-07-0"
categories: 
  - spring
title: Spring源码分析之BeanWrap
tags : 
 - spring
---


基于Spring1.0

## BeanWrapper
BeanWrapper是对Bean的包装，其接口中所定义的功能很简单包括设置获取被包装的对象，获取被包装bean的属性描述器，由于BeanWrapper接口是PropertyAccessor的子接口，因此其也可以设置以及访问被包装对象的属性值。BeanWrapper大部分情况下是在spring ioc内部进行使用，通过BeanWrapper,spring ioc容器可以用统一的方式来访问bean的属性。用户很少需要直接使用BeanWrapper进行编程

## BeanWrapperImpl
BeanWrapperImpl类是对BeanWrapper接口的默认实现，它包装了一个bean对象，缓存了bean的内省结果，并可以访问bean的属性、设置bean的属性值。BeanWrapperImpl类提供了许多默认属性编辑器，支持多种不同类型的类型转换，可以将数组、集合类型的属性转换成指定特殊类型的数组或集合。用户也可以注册自定义的属性编辑器在BeanWrapperImpl中。