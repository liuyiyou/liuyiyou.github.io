date: 2014/11/28
categories: 
  - java
title: isNotEmpty和isNotBlank的区别以及Map和JSONObject的toString方法
tags : 
  - java
---

### commons-lang.jar中isNotEmpty和isNotBlank的区别

主要是，针对空字符串的处理不同

```java

public static void main(String[] args) {
  String nullStr = null;
  String emptyStr =  "";
  String blankStr = " ";
  System.out.println(StringUtils.isBlank(nullStr));  //true
  System.out.println(StringUtils.isBlank(emptyStr));//true
  System.out.println(StringUtils.isBlank(blankStr));//true
  System.out.println(StringUtils.isNotBlank(nullStr));  //false
  System.out.println(StringUtils.isNotBlank(emptyStr)); //false
  System.out.println(StringUtils.isNotBlank(blankStr));//false
 
  System.out.println(StringUtils.isEmpty(nullStr));//true
  System.out.println(StringUtils.isEmpty(emptyStr));//true
  System.out.println(StringUtils.isEmpty(blankStr)); //false
  System.out.println(StringUtils.isNotEmpty(nullStr));//false
  System.out.println(StringUtils.isNotEmpty(emptyStr));//false
  System.out.println(StringUtils.isNotEmpty(blankStr));//true
 }

```

### Map和JSONObject的toString方法

前者的toString方法，只是单纯的返回数据，而后者返回的数据是json格式的


```java

public static void main(String[] args) {
        Map<String,Object> map1 =new HashMap<String, Object>();
        map1.put("result","success");
        map1.put("message","这是使用Map封装的数据");
        System.out.println(map1.toString());

        JSONObject map2 = new JSONObject();
        try {
            map2.put("result","success");
            map2.put("message","这是使用Map封装的数据");
        } catch (JSONException e) {
        }

        System.out.println(map2.toString());


        /**
         * {message=这是使用Map封装的数据, result=success}
         *{"message":"这是使用Map封装的数据","result":"success"}
         * */
    }


```


顺便说一句，有了JSONObect并不是在以下包中：


```xml

 <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-asl</artifactId>
            <version>1.9.3</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-asl</artifactId>
            <version>1.9.3</version>
            <type>jar</type>
            <scope>compile</scope>
        </dependency>

  ```


  而是在：

  ```xml

  <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20080701</version>
        </dependency>

  ```

 
  顺便提问一个问题：idea中的maven artifact search的快捷键在哪里？我让他出现的方式是：直接写类名，然后使用option+enter键，在弹出的菜单中选择add Maven Dependncy，这样太麻烦了，有没有快捷方式
