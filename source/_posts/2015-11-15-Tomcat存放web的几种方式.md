title: Tomcat存放web的几种方式
date: 2015-11-15
categories : 
  - tomcat
tags : 
  - tomcat
---

第一种方法：在tomcat中的conf目录中，在server.xml中的，节点中添加：


至于Context 节点属性，可详细见相关文档。

第二种方法：将web项目文件件拷贝到webapps 目录中。

第三种方法：很灵活，在conf目录中，新建 Catalina（注意大小写）＼localhost目录，在该目录中新建一个xml文件，名字可以随意取，只要和当前文件中的文件名不重复就行了，该xml文件的内容为：

第3个方法有个优点，可以定义别名。服务器端运行的项目名称为path，外部访问的URL则使用XML的文件名。这个方法很方便的隐藏了项目的名称，对一些项目名称被固定不能更换，但外部访问时又想换个路径，非常有效。
第2、3还有优点，可以定义一些个性配置，如数据源的配置等。
还有一篇 详细的
此处主要讲述Tomcat部署发布JSP应用程序的三种方法
    1、直接放到Webapps目录下
     Tomcat的Webapps目录是Tomcat默认的应用目录，当服务器启动时，会加载所有这个目录下的应用。也可以将JSP程序打包成一个 war包放在目录下，服务器会自动解开这个war包，并在这个目录下生成一个同名的文件夹。一个war包就是有特性格式的jar包，它是将一个Web程序的所有内容进行压缩得到。具体如何打包，可以使用许多开发工具的IDE环境，如Eclipse、NetBeans、ant、JBuilder等。也可以用 cmd 命令：jar -cvf applicationname.war package.*；
甚至可以在程序执行中打包：
try{ 
string strjavahome = system.getproperty("java.home"); 
strjavahome = strjavahome.substring(0,strjavahome.lastindexof(\\))+"\\bin\\"; 
runtime.getruntime().exec("cmd /c start "+strjavahome+"jar cvf hello.war c:\\tomcat5.0\\webapps\\root\\*"); 
}  
catch(exception e){system.out.println(e);}

     webapps这个默认的应用目录也是可以改变。打开Tomcat的conf目录下的server.xml文件，找到下面内容：


   2、在server.xml中指定
     在Tomcat的配置文件中，一个Web应用就是一个特定的Context，可以通过在server.xml中新建Context里部署一个JSP应用程序。打开server.xml文件，在Host标签内建一个Context，内容如下。

     其中path是虚拟路径，docBase是JSP应用程序的物理路径，workDir是这个应用的工作目录，存放运行是生成的于这个应用相关的文件。

   3、创建一个Context文件
     以上两种方法，Web应用被服务器加载后都会在Tomcat的conf\catalina\localhost目录下生成一个XML文件，其内容如下：

可以看出，文件中描述一个应用程序的Context信息，其内容和server.xml中的Context信息格式是一致的，文件名便是虚拟目录名。您可以直接建立这样的一个xml文件，放在Tomcat的conf\catalina\localhost目录下。例子如下：
注意：删除一个Web应用同时也要删除webapps下相应的文件夹祸server.xml中相应的Context，还要将Tomcat的conf
\catalina\localhost目录下相应的xml文件删除。否则Tomcat仍会岸配置去加载。。。
