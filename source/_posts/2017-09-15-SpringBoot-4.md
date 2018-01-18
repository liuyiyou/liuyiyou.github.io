title: SpringBoot(4)-集成JDBC
date: 2017-09-15
categories : 
  - spring-boot
tags : 
  - spring-boot
---


## 最简配置

## pom


```xml

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

```


## aplication.properties


这个必须配置，不然会报错



```properties

spring.datasource.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&zeroDateTimeBehavior=convertToNull
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driverClassName=com.mysql.jdbc.Driver

```





## 分布式事务


### application.properties

```xml

spring.jta.enabled=true
spring.jta.atomikos.service=com.atomikos.icatch.standalone.UserTransactionServiceFactory



spring.jta.atomikos.datasource.one.xa-data-source-class-name=com.mysql.jdbc.jdbc2.optional.MysqlXADataSource
spring.jta.atomikos.datasource.one.unique-resource-name=dataSourceOne
spring.jta.atomikos.datasource.one.max-pool-size=5
spring.jta.atomikos.datasource.one.xa-properties.user=root
spring.jta.atomikos.datasource.one.xa-properties.password=123456
spring.jta.atomikos.datasource.one.xa-properties.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&zeroDateTimeBehavior=convertToNull


spring.jta.atomikos.datasource.two.unique-resource-name=dataSourceTwo
spring.jta.atomikos.datasource.two.max-pool-size=5
spring.jta.atomikos.datasource.two.xa-properties.user=root
spring.jta.atomikos.datasource.two.xa-properties.password=123456
spring.jta.atomikos.datasource.two.xa-properties.url=jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&zeroDateTimeBehavior=convertToNull
spring.jta.atomikos.datasource.two.xa-data-source-class-name=com.mysql.jdbc.jdbc2.optional.MysqlXADataSource


spring.jta.atomikos.datasource.three.unique-resource-name=dataSourceThree
spring.jta.atomikos.datasource.three.max-pool-size=5
spring.jta.atomikos.datasource.three.xa-properties.user=ibalife
spring.jta.atomikos.datasource.three.xa-properties.password=ibalife
spring.jta.atomikos.datasource.three.xa-properties.url=jdbc:mysql://192.168.0.200:3306/ibalife_user?useUnicode=true&characterEncoding=utf-8&rewriteBatchedStatements=true&zeroDateTimeBehavior=convertToNull
spring.jta.atomikos.datasource.three.xa-data-source-class-name=com.mysql.jdbc.jdbc2.optional.MysqlXADataSource


```


### java

```java

/**
     * 数据源1 ,这样写是为了每个DataSource能依赖他自己的sqlsessionFactory
     */
    @Configuration
    @MapperScan(basePackages = BlogDataSourceConfig.PACKAGE, sqlSessionFactoryRef = "blogSqlSessionFactory")
    class BlogDataSourceConfig {
        static final String PACKAGE = "com.ibalife.message.mapper.blog";

        @Bean
        @Primary
        @ConfigurationProperties(prefix = "spring.jta.atomikos.datasource.one")
        public DataSource dataSourceOne() {
            return new AtomikosDataSourceBean();
        }

        @Bean(name = "blogSqlSessionFactory")
        @Primary
        public SqlSessionFactory clusterSqlSessionFactory(@Qualifier("dataSourceOne") DataSource clusterDataSource)
                throws Exception {
            final SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
            sessionFactory.setDataSource(clusterDataSource);
            sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver()
                    .getResources("classpath:mapper/blog/*.xml"));
            return sessionFactory.getObject();
        }

    }

    /**
     * 数据源2
     */
    @Configuration
    @MapperScan(basePackages = TestDataSourceConfig.PACKAGE, sqlSessionFactoryRef = "testSqlSessionFactory")
    public class TestDataSourceConfig {

        static final String PACKAGE = "com.ibalife.message.mapper.test";

        @Bean
        @ConfigurationProperties(prefix = "spring.jta.atomikos.datasource.two")
        public DataSource dataSourceTwo() {
            return new AtomikosDataSourceBean();
        }


        @Bean(name = "testSqlSessionFactory")
        public SqlSessionFactory testSqlSessionFactory(@Qualifier("dataSourceTwo") DataSource clusterDataSource)
                throws Exception {
            final SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
            sessionFactory.setDataSource(clusterDataSource);
            sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver()
                    .getResources("classpath:mapper/test/*.xml"));
            return sessionFactory.getObject();
        }

    }


    /**
     * 数据源3 ,在另外一台机器上
     */
    @Configuration
    @MapperScan(basePackages = IbaDataSourceConfig.PACKAGE, sqlSessionFactoryRef = "ibaSqlSessionFactory")
    public class IbaDataSourceConfig {

        static final String PACKAGE = "com.ibalife.message.mapper.iba";

        @Bean
        @ConfigurationProperties(prefix = "spring.jta.atomikos.datasource.three")
        public DataSource dataSourceThree() {
            return new AtomikosDataSourceBean();
        }


        @Bean(name = "ibaSqlSessionFactory")
        public SqlSessionFactory testSqlSessionFactory(@Qualifier("dataSourceThree") DataSource clusterDataSource)
                throws Exception {
            final SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
            sessionFactory.setDataSource(clusterDataSource);
            sessionFactory.setMapperLocations(new PathMatchingResourcePatternResolver()
                    .getResources("classpath:mapper/ibalife/*.xml"));
            return sessionFactory.getObject();
        }

    }


```    