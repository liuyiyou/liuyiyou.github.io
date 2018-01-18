title: mysql的limit的用法
date: "2015-11-07"
categories: 
  - mysql
tags:
    - mysql
---

limit主要用在mysql的分页查询中，不过现在很多时候都不会主动写sql了。比如在JPA中提供了Page方法，而Mybatis也有分页插件

### 语法

limit后面可以跟两个数字参数


```sql

#从表中取出5条记录
SELECT * FROM TABLE LIMIT 5

#从第5+1个开始，选取6条记录(6到12行)
SELECT * FROM TBALE LIMIT 5,  6 

```

分页的时候就可以这样传：
假设每页显示50条记录

第一页： 0,50

第二页： 50,10

第三页： 100,50

第I页：  （i-1）*50, 50

