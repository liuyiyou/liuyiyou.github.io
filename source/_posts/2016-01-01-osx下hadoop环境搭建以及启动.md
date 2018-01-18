date: "2016-01-01"
categories: 
  - 大数据
title: osx下hadoop环境搭建以及启动
tags : hadoop
---

## 参考：

http://hadoop.apache.org/docs/r2.6.0/hadoop-project-dist/hadoop-common/SingleCluster.html#Standalone_Operation


新年好！

新年的第一天，开始大数据。

以下文章只是为了自己记录，如果被各位搜到，参考就行了，如果参考后还是没有得到想要的结果，请自行谷歌，反正我也是谷歌出来的

系统：osx

参考：慕课网的相关视频（这个可以百度）

下载的hadoop版本：[下载地址](http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz )  


## 步骤

1. 将下载的文件解压到 /User/liuyiyou 下，解压后的路径如下：/Users/liuyiyou/hadoop-2.6.0

2. 配置环境变量:我是放在用户目录下，所以，每次都需要source .bash_profile以下。	

	```sh	

       JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_65.jdk/Contents/Home
       M2_HOME=/Users/liuyiyou/TechSoft/apache-maven-3.0.5
       MYSQL_HOME=/usr/local/mysql
       HADOOP_HOME=/Users/liuyiyou/hadoop-2.6.0
       HIVE_HOME=/usr/local/hive/apache-hive-1.2.1-bin
       PATH=$JAVA_HOME/bin:$M2_HOME/bin:$MYSQL_HOME/bin:$HIVE_HOME/bin:$HADOOP_HOME/bin:$PATH
       export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
       export CLASSPATH=$CLASSPATH:$HVIE_HOME/lib/*:.
       export PATH=$JAVA_HOME/bin:$PATH
       export HADOOP_CLASSPATH=$JAVA_HOME/lib/tools.jar	

	```


	2.1. hadoop-env.sh 中将修改：export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_65.jdk/Contents/Home

	2.2. 修改etc/hdfs-site.xml：如下

	```xml

	<configuration>

     <?xml version="1.0" encoding="UTF-8"?>
     <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
     <!--hdfs-site.xml 文件中包含的信息，如复制数据的值，名称节点的路径，本地文件系统的数据节点的路径。-->
     <!-- Put site-specific property overrides in this file. -->     
     <configuration>
     	<property>
             <name>dfs.replication</name>
             <value>1</value>
         </property>
         <property> 
           <name>dfs.name.dir</name> 
           <value>/Users/liuyiyou/hadoop-2.6.0/hadoopinfra/hdfs/namenode </value> 
        </property> 
        <property> 
           <name>dfs.data.dir</name>
           <value>/Users/liuyiyou/hadoop-2.6.0/hadoopinfra/hdfs/datanode </value > 
        </property>
     </configuration>

	```

	2.3. 修改core-site.xml 如下

	```xml

	<?xml version="1.0" encoding="UTF-8"?>
	<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
	<!--core-site.xml文件中包含的信息，如使用Hadoop实例分配给文件系统的存储器，用于存储数据的内存限制的端口号，以及读/写缓冲器的大小。-->
	<!-- Put site-specific property overrides in this file. -->
	<configuration>
		<property>
	        <name>fs.defaultFS</name>
	        <value>hdfs://localhost:9000</value>
	    </property>
	</configuration>


	```

	2.4. 修改mapred-site.xml如下

	```xml

	<?xml version="1.0"?>
	<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
	<!--
	  此文件用于指定我们正在使用的MapReduce框架。缺省情况下，包含 yarn-site.xml模板。首先，需要将文件从mapred-site.xml复制。模板mapred-site.xml文件使用以下命令。
	-->	
	<!-- Put site-specific property overrides in this file. -->	
	<configuration>	
	   <property> 
	      <name>mapreduce.framework.name</name> 
	      <value>yarn</value> 
	   </property>	

	</configuration>


	```

3. 注意：因为.bash_profile在终端关闭之后会失效，所以每次使用的时候需要重复source，不然会报错：找不到hadoop命令

4. ssh配置，在设置中将远程登录打开

 ![console](/images/hadoop-remote.png)

 成功标志是：ssh localhost 不报异常

5. 在bin目录下运行  hadoop namenode -format 

如果之前成功，则不需要按照上面的命令。因为format后，hadoop的数据会清除

 如果在运行过程中与权限有关的错误，可以参考： [Hadoop上路_04-启动Hadoop](http://my.oschina.net/vigiles/blog/132255?fromerr=aLe76Uag,"Hadoop上路_04-启动Hadoop") 

如果报错找不到文件异常：可以在相应地方创建一个文件夹：文件名可参考配置


6. 启动

hadoop2.x版本会有两个启动hadoop的文件夹，一个是bin一个是sbin。其中bin是老版本的，sbin是新版本的


```sh

hadoop-2.6.0 sbin/start-dfs.sh 

```

这个时候可以访问：http://localhost:50070/dfshealth.jsp ，如果不出错，表示启动正常

```sh

hadoop-2.6.0 sbin/start-yarn.sh 

```

这个时候可以访问：http://localhost:8088/cluster 可以看到当前的

http://localhost:50030/jobtracker.jsp



几个坑：

1. 环境变量配置问题，因为配置在用户环境变量中，导致hadoop找不到

2. 权限问题，当时是放在/opt下，发现权限有问题，后来移动到用户目录下，还是没有解决，最终是参考了上面博客才解决

3. ssh登录问题，这个因系统而异

4. 需要自己创建一个





