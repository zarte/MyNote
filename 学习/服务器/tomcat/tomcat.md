##升级jdk8后系统报错解决：java.lang.RuntimeException: java.io.IOException: invalid constant type: 18 
```

所以最后的解决方法是：删除 cglib, asm 升级到5.0.4, javassist 升级到 3.18.0 以上，测试成功！
相关类库介绍：

ASM 是一个 Java 字节码操控框架。它能够以二进制形式修改已有类或者动态生成类。
ASM 可以直接产生二进制 class 文件，也可以在类被加载入 Java 虚拟机之前动态改变类行为。
ASM 从类文件中读入信息后，能够改变类行为，分析类信息，甚至能够根据用户要求生成新类。
CGlib（Code Generation Library）能够在程序运行的时候动态生成接口的实现类和继承于某个类的子类，它是依赖于ASM的。
Javassist是一个开源的分析、编辑和创建Java字节码的类库，能动态改变类的结构，或者动态生成类。


目前CGlib已经停止更新，Javassist已经取代了CGlib的功能。
```
##JVM致命错误日志（hs_err_pid.log）解读

http://blog.csdn.net/steveguoshao/article/details/12493375

http://blog.csdn.net/aerchi/article/details/7974157

```
查看日志文件：/logs/catalina.2015-08-17.log 发现:
stoping service Catalina
root or context hierarchy
org.quartz.core.QuartzScheduler standby
INFO: Scheduler startInitPmsInfoTask_$NON_CLUSTERED paused.
defining beans [..] root of factory hierarchy
解决方法：
<bean id="userDao" class="com.UserDaoImpl">
        <property name="sessionFactory">
            <ref bean="sessionFactory"/>
        </property>
    </bean>
应该是这部分错了 。 因为没注入到DAO里面去，把<ref bean="" /> 改成<ref local=""/>试试看。
<ref bean="" /> 是寻找全局中到bean
<ref local="" />是寻找本xml文件中到bean。
```
##Spring中ref local与ref bean区别
```
< bean id = "userDAOProxy"
        class = "org.springframework.transaction.interceptor.TransactionProxyFactoryBean" >
        < property name = "transactionManager" >
            < ref bean = "transactionManager" />
        </ property >
        < property name = "target" >
            < ref local = "UserDAO" />
        </ property >
    </ bean >
```
- 用 local 属性指定目标 bean 可以利用 xml 解析器的能力在同一个 XML配置文件中验证 xml id 引用,没有匹配的元素,xml 解析器就会产生一个 error, 所以如果引用的 bean 在同一个 XML配置文件中,那么用 local 形式是最好的选择.
- 可以这么说,<ref bean> 是寻找所有 XML配置文件中的 bean; <ref local> 是寻找本 xml 文件中的 bean. 
- <ref> 提供了如下几方面的属性 :
 1. bean: 在当前Spring XML 配置文件中，或者在同一BeanFactory(ApplicationContext) 中的其他JavaBean中寻找引入的BEAN.
 2. local: 仅在当前 Spring XML 配置文件中寻找引入的BEAN. 
    如果借助于 Spring IDE, 则在编译期可以对其依赖的 JavaBean 进行验证。基于 local 方式，开发者能够使用到 XML 本身提供的                优势，而进行验证。 
 3. parent: 用于指定其依赖的父 JavaBean 定义。


##修改Tomcat内存大小
Windows下，在文件/bin/catalina.bat，

Linux下，在文件/bin/catalina.sh的前面，增加如下设置：
JAVA_OPTS=-Xms【初始化内存大小】 -Xmx【可以使用的最大内存】
JAVA_OPTS 这个是，TOMCAT已经定义好的名，你只需要将JAVA_OPTS=-Xms256m -Xmx512m这句话
添加到
catalina.bat（windows）：set JAVA_OPTS=-Xms256m -Xmx512m
catalina.sh（linux）：JAVA_OPTS=-Xms256m -Xmx512m
或者直接修改start.bat或start.sh文件也行，因为start文件会调用catalina文件，如：
如果是windows环境，在startup.bat中加入set JAVA_OPTS=-Xms256m -Xmx1024m
如果是linux则在startup.sh中加入JAVA_OPTS=-Xms256m -Xmx1024m


## java.lang.ClassNotFoundException: com.mysql.jdbc.Driver
```
方式一：


(1)项目名称(右键)==>>Build path==>>Configure build path...

(2)选择弹出对话框右侧Libraries标签页，选择Add External Jars，

(3)此时导入mysql-connector-java-5.1.27-bin.jar，点击OK即可。

方式二：

直接拷贝 mysql-connector-java-5.1.27-bin.jar到tomcat/libs文件夹下即可。

```

##tomcat配置数据源

```

在Eclipse的Servers的 Tomcat v7.0 Server at localhost-config/context.xml中

< Resource   auth = "Container"   driverClassName = "com.mysql.jdbc.Driver"   maxActive = "5"   maxIdle = "2"   maxWait = "-1"   name = "jdbc/advertisement"   password = ""   type = "javax.sql.DataSource"   url = "jdbc:mysql://192.168.1.44:3306/advertisement?characterEncoding=UTF-8"   username = "root" />

在conf/spring-ref.xml中
<!--  ciss 数据源 -->

     < jee:jndi-lookup   id = "dataSource"   jndi -name = " jdbc /advertisement"   proxy -interface = "javax.sql.DataSource"   lookup-on-startup = "false"   />  

```  


##一个Tomcat支持不同的域名访问各自不同程序的配置方法







##只有指定的主机或IP地址才可以访问部署在Tomcat下的应用

Tomcat提供了两个参数配置：RemoteHostValve 和RemoteAddrValve，前者用于限制主机名，后者用于限制IP地址。

通过配置这两个参数，可以让你过滤来自请求的主机或IP地址，并允许或拒绝哪些主机/IP。

```

全局设置，对Tomcat下所有应用生效server.xml中添加下面一行，重启服务器即可：

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.*" deny=""/>  

此行放在</Host>之前。

（1）只允许192.168.1.10访问：

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.10" eny=""/>

（2）只允许192.168.1.*网段访问：

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.*" deny=""/>

（3）只允许192.168.1.10、192.168.1.30访问：

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.10,192.168.1.30" deny=""/>

（4）根据主机名进行限制：

<Valve className="org.apache.catalina.valves.RemoteHostValve" allow="abc.com" deny=""/>






 局部设置，仅对具体的应用生效根据项目配置情况进行设置：

（1）使用conf目录下xml文件进行配置${tomcat_root}\conf\proj_1.xml

（2）直接在server.xml中进行设置${tomcat_root}\conf\server.xml 在上述文件对应项目的</Context>前增加下面一行：

<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="192.168.1.*" deny=""/>



```

##tomcat启动一闪而过

```

在startup.bat文件的最后敲上“pause”，保存后重新运行startup.bat


```