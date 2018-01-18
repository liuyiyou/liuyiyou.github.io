title: SpringBoot(5)-集成MyBatis
date: 2017-09-16
categories : 
  - spring-boot
tags : 
  - spring-boot
---



之前在Spring集成MyBatis的时候需要将相关依赖加到pom中，然后在spring-config.xml中配置``SqlSessionFactoryBean``， 里面需要指定mybatis配置文件位置等，还需要配置扫描接口``MapperScannerConfigurer``
现在只需要一行代码搞定

``@MapperScan("cn.liuyiyou.sprongboot.mapper")``


之前的配置

```xml

    <!-- ======================================================================== -->
    <!-- MyBatis SQL定义。管理MyBatis的Sqlsession -->
    <!-- ======================================================================== -->
    <!-- 创建SqlSessionFactory，同时指定数据源 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!-- 别名，如果在同一个基础包下有同名的，会报错。所以在这里需要精确一点 -->
        <property name="typeAliasesPackage" value="cn.liuyiyou.mybatis.domain.chapter11" />
        <!-- mybatis配置文件位置 -->
        <property name="configLocation" value="classpath:chapter11/mybatis-config.xml" />
        <!-- 映射文件位置 -->
        <property name="mapperLocations" value="classpath*:mapper/chapter11/**/*.xml" />
        <property name="typeAliases"
            value="org.springframework.util.LinkedCaseInsensitiveMap" />
    </bean>

    <!-- ======================================================================== -->
    <!-- 扫描Mybatis接口包定义。 -->
    <!-- ======================================================================== -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="cn.liuyiyou.mybatis.mapper.chapter11" />
    </bean>

```



## 最简配置

``@MapperScan("cn.liuyiyou.sprongboot.mapper")``一行代码搞定

需要注意的是，该注释并不是``mybatis-spring-boot-starter``提供的，而是mybatis-spring来提供的，也就是说，哪怕不用springboot，也是有该功能的，只是在spring-boot之前，一般都是基于xml配置的。

另外，如果不是基于spring-boot，需要定义数据源、事务管理器等，而集成spring-boot之后，不在需要自己管理这些，而是由``MybatisAutoConfiguration``来进行自动配置


```java

@Configuration
   @MapperScan("org.mybatis.spring.sample.mapper")
   public class AppConfig {
  
     @Bean
     public DataSource dataSource() {
       return new EmbeddedDatabaseBuilder()
                .addScript("schema.sql")
                .build();
     }
  
     @Bean
     public DataSourceTransactionManager transactionManager() {
       return new DataSourceTransactionManager(dataSource());
     }
  
     @Bean
     public SqlSessionFactory sqlSessionFactory() throws Exception {
       SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
       sessionFactory.setDataSource(dataSource());
       return sessionFactory.getObject();
     }
   }
   

```   

## pom


```xml

<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>

```


## aplication.properties

这个虽然与mybatis无关，但是不能去掉，毕竟mybatis就是一个orm框架

```properties

spring.datasource.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&zeroDateTimeBehavior=convertToNull
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driverClassName=com.mysql.jdbc.Driver

```



