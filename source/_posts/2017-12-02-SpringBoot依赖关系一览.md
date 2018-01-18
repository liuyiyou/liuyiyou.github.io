categories : 
  - spring-boot
tags : 
  - spring-boot
---


官方starter： 在spring-boot-starters中

```xml
<module>spring-boot-starter</module>
<module>spring-boot-starter-activemq</module>
<module>spring-boot-starter-amqp</module>
<module>spring-boot-starter-aop</module>
<module>spring-boot-starter-artemis</module>
<module>spring-boot-starter-batch</module>
<module>spring-boot-starter-cache</module>
<module>spring-boot-starter-cloud-connectors</module>
<module>spring-boot-starter-data-cassandra</module>
<module>spring-boot-starter-data-couchbase</module>
<module>spring-boot-starter-data-elasticsearch</module>
<module>spring-boot-starter-data-gemfire</module>
<module>spring-boot-starter-data-jpa</module>
<module>spring-boot-starter-data-ldap</module>
<module>spring-boot-starter-data-mongodb</module>
<module>spring-boot-starter-data-neo4j</module>
<module>spring-boot-starter-data-redis</module>
<module>spring-boot-starter-data-rest</module>
<module>spring-boot-starter-data-solr</module>
<module>spring-boot-starter-freemarker</module>
<module>spring-boot-starter-groovy-templates</module>
<module>spring-boot-starter-hateoas</module>
<module>spring-boot-starter-integration</module>
<module>spring-boot-starter-jdbc</module>
<module>spring-boot-starter-jersey</module>
<module>spring-boot-starter-jetty</module>
<module>spring-boot-starter-jooq</module>
<module>spring-boot-starter-jta-atomikos</module>
<module>spring-boot-starter-jta-bitronix</module>
<module>spring-boot-starter-jta-narayana</module>
<module>spring-boot-starter-logging</module>
<module>spring-boot-starter-log4j2</module>
<module>spring-boot-starter-mail</module>
<module>spring-boot-starter-mobile</module>
<module>spring-boot-starter-mustache</module>
<module>spring-boot-starter-actuator</module>
<module>spring-boot-starter-parent</module>
<module>spring-boot-starter-security</module>
<module>spring-boot-starter-social-facebook</module>
<module>spring-boot-starter-social-twitter</module>
<module>spring-boot-starter-social-linkedin</module>
<module>spring-boot-starter-remote-shell</module>
<module>spring-boot-starter-test</module>
<module>spring-boot-starter-thymeleaf</module>
<module>spring-boot-starter-tomcat</module>
<module>spring-boot-starter-undertow</module>
<module>spring-boot-starter-validation</module>
<module>spring-boot-starter-web</module>
<module>spring-boot-starter-websocket</module>
<module>spring-boot-starter-web-services</module>
```

所以，比如MyBatis官方是木有提供starter的，需要去MyBatis官网去查



依赖关系：可用用IDE自带的工具，或者运行` mvn dependency:tree`


```java

 +- org.springframework.cloud:spring-cloud-starter-config:jar:1.4.0.RELEASE:compile
 |  +- org.springframework.cloud:spring-cloud-starter:jar:1.3.0.RELEASE:compile
 |  |  +- org.springframework.cloud:spring-cloud-context:jar:1.3.0.RELEASE:compile
 |  |  |  \- org.springframework.security:spring-security-crypto:jar:4.2.3.RELEASE:compile
 |  |  +- org.springframework.cloud:spring-cloud-commons:jar:1.3.0.RELEASE:compile
 |  |  |  \- org.apache.httpcomponents:httpclient:jar:4.5.3:compile
 |  |  |     +- org.apache.httpcomponents:httpcore:jar:4.4.8:compile
 |  |  |     \- commons-codec:commons-codec:jar:1.10:compile
 |  |  \- org.springframework.security:spring-security-rsa:jar:1.0.3.RELEASE:compile
 |  |     \- org.bouncycastle:bcpkix-jdk15on:jar:1.55:compile
 |  |        \- org.bouncycastle:bcprov-jdk15on:jar:1.55:compile
 |  +- org.springframework.cloud:spring-cloud-config-client:jar:1.4.0.RELEASE:compile
 |  |  +- org.springframework.boot:spring-boot-autoconfigure:jar:1.5.8.RELEASE:compile
 |  |  +- org.springframework:spring-web:jar:4.3.12.RELEASE:compile
 |  |  |  \- org.springframework:spring-context:jar:4.3.12.RELEASE:compile
 |  |  |     \- org.springframework:spring-expression:jar:4.3.12.RELEASE:compile
 |  |  \- com.fasterxml.jackson.core:jackson-annotations:jar:2.8.0:compile
 |  \- com.fasterxml.jackson.core:jackson-databind:jar:2.8.10:compile
 |     \- com.fasterxml.jackson.core:jackson-core:jar:2.8.10:compile
 +- org.springframework.cloud:spring-cloud-starter-eureka:jar:1.4.0.RELEASE:compile
 |  \- org.springframework.cloud:spring-cloud-starter-netflix-eureka-client:jar:1.4.0.RELEASE:compile
 |     +- org.springframework.boot:spring-boot-starter-web:jar:1.5.8.RELEASE:compile
 |     |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:1.5.8.RELEASE:compile
 |     |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.5.23:compile
 |     |  |  |  \- org.apache.tomcat:tomcat-annotations-api:jar:8.5.23:compile
 |     |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.5.23:compile
 |     |  \- org.springframework:spring-webmvc:jar:4.3.12.RELEASE:compile
 |     +- org.springframework.cloud:spring-cloud-netflix-core:jar:1.4.0.RELEASE:compile
 |     +- org.springframework.cloud:spring-cloud-netflix-eureka-client:jar:1.4.0.RELEASE:compile
 |     +- com.netflix.eureka:eureka-client:jar:1.7.0:compile
 |     |  +- org.codehaus.jettison:jettison:jar:1.3.7:runtime
 |     |  |  \- stax:stax-api:jar:1.0.1:runtime
 |     |  +- com.netflix.netflix-commons:netflix-eventbus:jar:0.3.0:runtime
 |     |  |  +- com.netflix.netflix-commons:netflix-infix:jar:0.3.0:runtime
 |     |  |  |  +- commons-jxpath:commons-jxpath:jar:1.3:runtime
 |     |  |  |  +- joda-time:joda-time:jar:2.9.9:runtime
 |     |  |  |  +- org.antlr:antlr-runtime:jar:3.4:runtime
 |     |  |  |  |  +- org.antlr:stringtemplate:jar:3.2.1:runtime
 |     |  |  |  |  \- antlr:antlr:jar:2.7.7:runtime
 |     |  |  |  \- com.google.code.gson:gson:jar:2.8.2:runtime
 |     |  |  \- org.apache.commons:commons-math:jar:2.2:runtime
 |     |  +- com.netflix.archaius:archaius-core:jar:0.7.4:compile
 |     |  +- javax.ws.rs:jsr311-api:jar:1.1.1:runtime
 |     |  +- com.netflix.servo:servo-core:jar:0.10.1:runtime
 |     |  |  \- com.netflix.servo:servo-internal:jar:0.10.1:runtime
 |     |  +- com.sun.jersey:jersey-core:jar:1.19.1:runtime
 |     |  +- com.sun.jersey:jersey-client:jar:1.19.1:runtime
 |     |  +- com.sun.jersey.contribs:jersey-apache-client4:jar:1.19.1:runtime
 |     |  \- com.google.inject:guice:jar:4.1.0:runtime
 |     |     \- aopalliance:aopalliance:jar:1.0:runtime
 |     +- com.netflix.eureka:eureka-core:jar:1.7.0:compile
 |     |  \- org.codehaus.woodstox:woodstox-core-asl:jar:4.4.1:runtime
 |     |     +- javax.xml.stream:stax-api:jar:1.0-2:runtime
 |     |     \- org.codehaus.woodstox:stax2-api:jar:3.1.4:runtime
 |     +- org.springframework.cloud:spring-cloud-starter-netflix-archaius:jar:1.4.0.RELEASE:compile
 |     |  +- commons-configuration:commons-configuration:jar:1.8:compile
 |     |  \- com.google.guava:guava:jar:18.0:compile
 |     +- com.netflix.ribbon:ribbon-eureka:jar:2.2.4:compile
 |     \- com.thoughtworks.xstream:xstream:jar:1.4.9:compile
 |        +- xmlpull:xmlpull:jar:1.1.3.1:compile
 |        \- xpp3:xpp3_min:jar:1.1.4c:compile
 +- org.springframework.cloud:spring-cloud-starter-feign:jar:1.4.0.RELEASE:compile
 |  \- org.springframework.cloud:spring-cloud-starter-openfeign:jar:1.4.0.RELEASE:compile
 |     +- io.github.openfeign:feign-core:jar:9.5.0:compile
 |     |  \- org.jvnet:animal-sniffer-annotation:jar:1.0:compile
 |     +- io.github.openfeign:feign-slf4j:jar:9.5.0:compile
 |     \- io.github.openfeign:feign-hystrix:jar:9.5.0:compile
 |        \- com.netflix.hystrix:hystrix-core:jar:1.5.12:compile
 |           \- org.hdrhistogram:HdrHistogram:jar:2.1.9:compile
 +- org.springframework.cloud:spring-cloud-starter-ribbon:jar:1.4.0.RELEASE:compile
 |  \- org.springframework.cloud:spring-cloud-starter-netflix-ribbon:jar:1.4.0.RELEASE:compile
 |     +- com.netflix.ribbon:ribbon:jar:2.2.4:compile
 |     |  +- com.netflix.ribbon:ribbon-transport:jar:2.2.4:runtime
 |     |  |  +- io.reactivex:rxnetty-contexts:jar:0.4.9:runtime
 |     |  |  \- io.reactivex:rxnetty-servo:jar:0.4.9:runtime
 |     |  +- javax.inject:javax.inject:jar:1:runtime
 |     |  \- io.reactivex:rxnetty:jar:0.4.9:runtime
 |     |     +- io.netty:netty-codec-http:jar:4.0.27.Final:runtime
 |     |     |  +- io.netty:netty-codec:jar:4.0.27.Final:runtime
 |     |     |  \- io.netty:netty-handler:jar:4.0.27.Final:runtime
 |     |     \- io.netty:netty-transport-native-epoll:jar:4.0.27.Final:runtime
 |     |        +- io.netty:netty-common:jar:4.0.27.Final:runtime
 |     |        +- io.netty:netty-buffer:jar:4.0.27.Final:runtime
 |     |        \- io.netty:netty-transport:jar:4.0.27.Final:runtime
 |     +- com.netflix.ribbon:ribbon-core:jar:2.2.4:compile
 |     |  \- commons-lang:commons-lang:jar:2.6:compile
 |     +- com.netflix.ribbon:ribbon-httpclient:jar:2.2.4:compile
 |     |  +- commons-collections:commons-collections:jar:3.2.2:runtime
 |     |  \- com.netflix.netflix-commons:netflix-commons-util:jar:0.1.1:runtime
 |     +- com.netflix.ribbon:ribbon-loadbalancer:jar:2.2.4:compile
 |     |  \- com.netflix.netflix-commons:netflix-statistics:jar:0.1.1:runtime
 |     \- io.reactivex:rxjava:jar:1.2.0:compile
 +- org.springframework.boot:spring-boot-starter-data-redis:jar:1.5.8.RELEASE:compile
 |  +- org.springframework.boot:spring-boot-starter:jar:1.5.8.RELEASE:compile
 |  |  +- org.springframework.boot:spring-boot:jar:1.5.8.RELEASE:compile
 |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:1.5.8.RELEASE:compile
 |  |  |  +- ch.qos.logback:logback-classic:jar:1.1.11:compile
 |  |  |  |  \- ch.qos.logback:logback-core:jar:1.1.11:compile
 |  |  |  +- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
 |  |  |  \- org.slf4j:log4j-over-slf4j:jar:1.7.25:compile
 |  |  \- org.yaml:snakeyaml:jar:1.17:runtime
 |  +- org.springframework.data:spring-data-redis:jar:1.8.8.RELEASE:compile
 |  |  +- org.springframework.data:spring-data-keyvalue:jar:1.2.8.RELEASE:compile
 |  |  |  \- org.springframework.data:spring-data-commons:jar:1.13.8.RELEASE:compile
 |  |  +- org.springframework:spring-tx:jar:4.3.12.RELEASE:compile
 |  |  +- org.springframework:spring-oxm:jar:4.3.12.RELEASE:compile
 |  |  +- org.springframework:spring-aop:jar:4.3.12.RELEASE:compile
 |  |  +- org.springframework:spring-context-support:jar:4.3.12.RELEASE:compile
 |  |  +- org.slf4j:slf4j-api:jar:1.7.25:compile
 |  |  \- org.slf4j:jcl-over-slf4j:jar:1.7.25:compile
 |  \- redis.clients:jedis:jar:2.9.0:compile
 |     \- org.apache.commons:commons-pool2:jar:2.4.2:compile
 +- org.springframework.boot:spring-boot-starter-jdbc:jar:1.5.8.RELEASE:compile
 |  +- org.apache.tomcat:tomcat-jdbc:jar:8.5.23:compile
 |  |  \- org.apache.tomcat:tomcat-juli:jar:8.5.23:compile
 |  \- org.springframework:spring-jdbc:jar:4.3.12.RELEASE:compile
 |     \- org.springframework:spring-beans:jar:4.3.12.RELEASE:compile
 +- org.mybatis.spring.boot:mybatis-spring-boot-starter:jar:1.3.1:compile
 |  +- org.mybatis.spring.boot:mybatis-spring-boot-autoconfigure:jar:1.3.1:compile
 |  +- org.mybatis:mybatis:jar:3.4.5:compile
 |  \- org.mybatis:mybatis-spring:jar:1.3.1:compile
 +- org.springframework.boot:spring-boot-starter-validation:jar:1.5.8.RELEASE:compile
 |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.5.23:compile
 |  \- org.hibernate:hibernate-validator:jar:5.3.5.Final:compile
 |     +- javax.validation:validation-api:jar:1.1.0.Final:compile
 |     +- org.jboss.logging:jboss-logging:jar:3.3.1.Final:compile
 |     \- com.fasterxml:classmate:jar:1.3.4:compile
 +- mysql:mysql-connector-java:jar:5.1.44:runtime
 \- org.springframework.boot:spring-boot-starter-test:jar:1.5.8.RELEASE:test
    +- org.springframework.boot:spring-boot-test:jar:1.5.8.RELEASE:test
    +- org.springframework.boot:spring-boot-test-autoconfigure:jar:1.5.8.RELEASE:test
    +- com.jayway.jsonpath:json-path:jar:2.2.0:test
    |  \- net.minidev:json-smart:jar:2.2.1:test
    |     \- net.minidev:accessors-smart:jar:1.1:test
    |        \- org.ow2.asm:asm:jar:5.0.3:test
    +- junit:junit:jar:4.12:test
    +- org.assertj:assertj-core:jar:2.6.0:test
    +- org.mockito:mockito-core:jar:1.10.19:test
    |  \- org.objenesis:objenesis:jar:2.1:test
    +- org.hamcrest:hamcrest-core:jar:1.3:test
    +- org.hamcrest:hamcrest-library:jar:1.3:test
    +- org.skyscreamer:jsonassert:jar:1.4.0:test
    |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
    +- org.springframework:spring-core:jar:4.3.12.RELEASE:compile
    \- org.springframework:spring-test:jar:4.3.12.RELEASE:test

```