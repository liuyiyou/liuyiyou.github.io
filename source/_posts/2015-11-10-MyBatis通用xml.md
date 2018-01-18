title: MyBatis的公用xml
date: 2015-11-10
categories : 
  - mybatis
tags : 
  - mybatis
---

比如一些查询条件，比如报表中的一些公用字段，在查询的时候，基本这些条件都是不会动的，但是每个页面多写这样一个东西是很麻烦且不容易维护。

之前想到过通过include解决问题，结果发现行不通，报错：找不到namespace+sqlId，以为是mybatis不支持跨xml调用其他页面的sql，今天又想起来这个问题，重新来了一次，发现不是之前的原因，
而是因为该通用xml没有在mybatis-config.xml中进行引入

记录一下