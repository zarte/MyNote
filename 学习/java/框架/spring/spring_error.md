[TOC]
##Spring NoSuchBeanDefinitionException原因分析
```
如果BeanFactory在Spring Context中没有找到bean的实例，就会抛出这个常见的异常。

Cause: No qualifying bean of type […] found for dependency

这个异常的出现一般是因为需要注入的bean未定义 
有一个类BeanA.Java

package com.csdn.training.model;
@Component
public class BeanA {
    @Autowired
    private BeanB beanB;
}


有一个类BeanB.java

package com.csdn.training.service;

@Component
public class BeanB {

}


配置文件applicationContext.xml

<context:component-scan base-package="com.csdn.training.model"></context:component-scan>

用一个测试类去启动拉起Spring容器：

package com.csdn.test;

public class AppTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
        BeanA beanA = (BeanA) context.getBean("beanA");
    }
}

自动扫描包路径缺少了BeanB，它和BeanA 不在同一路径下 
如果依赖 IBusinessService 在Spring 上下文中没有定义，引导进程报错：No Such Bean Definition Exception.

nested exception is org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type [com.csdn.training.service.BeanB] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency.


Spring会提示：”Expected at least 1 bean which qualifies as autowire candidate for this dependency“(依赖至少有一个备选的bean能被自动注入) 
出现异常的原因是IBusinessService 在上下文中不存在：如果bean是通过classpath自动扫描来装配，并且IBusinessService已经正确的加上了注解（@Component，@Repository，@Service，@Controller等），也许是你没有把正确的包路径告诉Spring。

配置文件可以如下配置：

<context:component-scan base-package="com.csdn.training"></context:component-scan>

如果bean不能自动被扫描到，而手动定义却可以识别，那Bean就没有在Spring上下文中定义。

Cause: No qualifying bean of type […] is defined

造成这一异常的原因可能是Spring上下文中存在两个或以上该bean的定义。如果接口IService 有两个实现类 ServiceImplA 和ServiceImplB 
接口：IService.java

package com.csdn.training.service;

public interface IService {

}


两个实现类：ServiceImplA.java

package com.csdn.training.service;

import org.springframework.stereotype.Service;
@Service
public class ServiceImplA implements IService {

}


ServiceImplB.java
package com.csdn.training.service;
import org.springframework.stereotype.Service;
@Service
public class ServiceImplB implements IService {

}

如果BeanA 自动注入这一接口，Spring就无法分辨到底注入哪一个实现类：

package com.csdn.training.model;

import com.csdn.training.service.IService;
@Component
public class BeanA {    
    @Autowired
    private IService serviceImpl;
}


然后，BeanFactory就抛出异常NoSuchBeanDefinitionException 
Spring会提示：”expected single matching bean but found 2“（只应该匹配一个bean但是找到了多个）

Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: 
No qualifying bean of type [com.csdn.training.service.IService] is defined: 
expected single matching bean but found 2: serviceImplA,serviceImplB


上例中，有时你看到的异常信息是NoUniqueBeanDefinitionException，它是NoSuchBeanDefinitionException 它的子类，在Spring 3.2.1中，修正了这一异常，为的是和bean未定义这一异常区分开。


nested exception is org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean of type [com.csdn.training.service.IService] is defined: expected single matching bean but found 2: serviceImplA,serviceImplB
1
1
解决这一异常可以用注解@Qualifier 来指定想要注入的bean的名字。


package com.csdn.training.model;


import com.csdn.training.service.IService;
@Component
public class BeanA {
    @Autowired
    @Qualifier("serviceImplA")
    private IService serviceImpl;
}


修改以后，Spring就可以区分出应该注入那个bean的实例，需要注意的是ServiceImplA的默认实例名称是serviceImplA


Cause: No Bean Named […] is defined


当你通过具体的bean的名字去得到一个bean的实例的时候，如果Spring 没有在上下文中找到这个bean，就会抛出这个异常。


package com.csdn.test;


public class AppTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
        context.getBean("beanX");
    }
}


这个例子中，没有一个bean被定义成 “beanX”,就会抛出如下异常： 
Spring会提示：”No bean named XXX is defined” (没有找到一个名叫XXX的bean)


org.springframework.beans.factory.NoSuchBeanDefinitionException: No bean named 'beanX' is defined at org.springframework.beans.factory.support.DefaultListableBeanFactory.getBeanDefinition


Cause: Proxied Beans


如果bean由JDK的动态代理机制所管理，那么代理将不会继承该bean，它只会实现与其相同的接口。因此，如果bean是通过接口注入的，就可以成功注入。如果通过其实现类注入，Spring就无法将bean实例与类关联，因为代理并不真正的继承于类。 
出现这一原因，很有可能是因为你使用了Spring的事物，在bean上使用了注解@Transactional 
如下，ServiceA注入了ServiceB，这两个service都使用了事物，通过实现类注入bean就不起作用了。 
借口IService.java无变化，其实现类加上事物的注解 
ServiceImplA.java


package com.csdn.training.service;


@Service
@Transactional
public class ServiceImplA implements IService {
    @Autowired
    private ServiceImplB serviceImplB;
}


ServiceImplB.java


package com.csdn.training.service;


@Service
@Transactional
public class ServiceImplB implements IService {


}


如果改成通过接口注入，就可以：


ServiceImpl.java


package com.csdn.training.service;


@Service
@Transactional
public class ServiceImplA implements IService {
    @Autowired
    @Qualifier("serviceImplB")
    private IService serviceImplB;
}
```
##Caused by: org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type  com.adanac.framework.mq.activemq.consumer.MQConsumer
```
原因：有可能是service中的配置文件中配置了相关信息，但web.xml中没有引用。
比如：
/rageon-service/src/main/resources/conf/spring/spring-config.xml

<!--ActiveMQ-->
     <!-- MQConfig -->
     < bean   id = "mqConfig"   class = "com.adanac.framework.mq.activemq.MQConfig" >
         < property   name = "brokerURL"   value = "${mq.server}" />
         < property   name = "userName"   value = "admin" />
         < property   name = "password"   value = "admin" />
     </ bean >
     <!-- MQTemplate -->
        < bean   id = "activeMQTemplate"   class = "com.adanac.framework.mq.activemq.MQTemplate" >
            < constructor-arg    ref = "mqConfig" ></ constructor-arg >
        </ bean >
       
        <!-- <bean id="activeMQTemplate1" class="com.adanac.framework.mq.activemq.MQTemplate">
           <constructor-arg value="test/ActiveMQConfig.properties"></constructor-arg>
       </bean> -->
       
        <!-- MQConsumer -->
        < bean   id = "consumer"   class = "com.adanac.framework.mq.activemq.consumer.MQConsumer" >
            < constructor-arg   ref = "activeMQTemplate" ></ constructor-arg >
        </ bean >
       
        <!-- MQProducer -->
         < bean   id = "producer"   class = "com.adanac.framework.mq.activemq.producer.MQProducer" >
            < constructor-arg   ref = "activeMQTemplate" ></ constructor-arg >
        </ bean >
             <!--ActiveMQ-->  


web.xml中引入spring-config.xml
< context-param >
         < param-name > contextConfigLocation </ param-name >
         < param-value >
        classpath:conf/spring/spring-impl.xml,
        classpath:conf/spring/spring-res.xml,
        classpath:conf/spring/spring-config.xml
         <!-- ,
        classpath:conf/spring/spring-dubbo.xml,       
        classpath:conf/spring/spring-user.xml,
        classpath:conf/spring/spring-cache.xml -->
         </ param-value >
     </ context-param >
``` ## Caused by:  java.lang.ClassNotFoundException : org.springframework.expression.PropertyAccessor
```
原因:缺少jar包， 加入  org.springframework.expression 包 即可解决.
```
## Caused by:  java.lang.IllegalStateException : Ambiguous mapping found. Cannot map 'loginControllerUrl' bean method 
public org.springframework.web.servlet.ModelAndView com.adanac.login.controller.LoginController.loginAction(java.lang.String,java.lang.String,javax.servlet.http.HttpSession,javax.servlet.http.HttpServletResponse,java.lang.String) to {[/loginAction.do],methods=[POST],params=[],headers=[],consumes=[],produces=[],custom=[]}: There is already 'loginController' bean method
```

原因:配置的请求有冲突，在beans.xml文件中配置了.action,而使用注解配置了.do.
@RequestMapping (value =  "loginAction.do" , method = RequestMethod. POST )  
`使用bean的name属性配置url和handler的映射关系`
`<bean name="/login.action" id="loginControllerUrl" class="com.adanac.login.controller.LoginController"/>`
解决：二选一即可。
```
##The type org.springframework.dao.DataAccessException cannot be resolved. It is indirectly referenced from required .class files
```
原因：org.springframework.transaction-3.1.1.RELEASE.jar未加入这个包，（spring-tx-3.2.0.RELEASE.jar加入这个包也可以）
```
## Caused by:  java.lang.ClassNotFoundException : org.apache.commons.dbcp.BasicDataSource
```
原因：没有加入关于dpcp的包（加入 commons-dbcp-1.4.jar，commons-pool-1.5.6.jar ）
```
## Caused by:  org.springframework.beans.NotWritablePropertyException : Invalid property 'userDao' of bean class  [com.adanac.login.service.UserServiceImpl]: Bean property 'userDao' is not writable or has an invalid setter method. Does the parameter type of the setter match the return type of the getter?
```
原因：在controller层引入了service,而没有对引入的service字段设置setter/getter方法，同理在service也是一样没有设置引入的dao的setter/getter方法。
```
##java.lang.ClassNotFoundException: org.springframework.web.context.ContextLoaderListener
```
在web.xml中配置了spring的上下文监听器：
<listener>  
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
</listener>

随后启动tomcat服务器，控制台提示如下错误：
java.lang.ClassNotFoundException: org.springframework.web.context.ContextLoaderListener
找不到“ org.springframework.web.context.ContextLoaderListener”这个类，ContextLoaderListener这个类是在spring-web.jar包下，我仔细检查了项目jar环境，发现该jar包确实存在，而且也能找到编译后的ContextLoaderListener.class文件。

Java虚拟机是根据Java ClassLoader(类加载器)决定如何加载Class。  
系统默认提供了3个ClassLoader    
Root ClassLoader，ClassPath Loader，Ext ClassLoader  
我们也可以编写自己的ClassLoader，去加载特定环境下的Jar文件。    
能不能加载Jar，加载哪里的Jar，是由ClassLoader决定的。    
  
楼主的问题可能是 导入的仅仅是jar包的引用，例如在eclipse中通过build path加进user lib……（类似快捷方式）  
这种在Java Application中没问题，但在web Application中可能会出现找不到类的异常。  
在WEB Application中jar包最好放在webroot或webcontent下的lib文件夹内，特别是xml中用到的jar包。
```
##java.lang.NumberFormatException: For input string: "${partition1.maxWait}"
项目启动时报此错，估计是配置文件不对。
E:\adanac_demo\zTree\zTree-web\src\main\resources\spring-mvc.xml 引入文件：
<import resource="config/spring-config.xml" />

E:\adanac_demo\zTree\zTree-intf\src\main\resources\config\spring-config.xml 引入文件
<import resource="classpath:config/spring-database-config.xml"/>

E:\adanac_demo\zTree\zTree-intf\src\main\resources\config\spring-database-config.xml
 <bean id="dataSource1" class="org.apache.tomcat.jdbc.pool.DataSource">
<property name="driverClassName" value="${partition1.driverClassName}"/>
<property name="url" value="${partition1.url}"/>
<property name="username" value="${partition1.username}"/>
<property name="password" value="${partition1.password}"/>
</bean>


E:\adanac_demo\zTree\zTree-intf\src\main\resources\config\database.properties
partition1.url=jdbc:mysql://localhost:3306/demo?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
partition1.username=root
partition1.password=root

原因是：在spring配置文件中没有引入数据库配置 database.properties文件
解决一：在E:\adanac_demo\zTree\zTree-web\src\main\resources\spring-config.xml 添加bean:
<bean id="propertyConfigurer"
          class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="locations">
        <list>
            <value>classpath:database.properties</value>
        </list>
    </property>
</bean>
解决二：在config/spring-database-config.xml中添加
<context:property-placeholder ignore-resource-not-found="true"
                                  location="classpath*:/config/test.database.properties"/>
                                  
    <!-- 数据源1 (kwms数据库)-->
    <bean id="dataSource1" class="org.apache.tomcat.jdbc.pool.DataSource">
        <property name="driverClassName" value="${partition1.driverClassName}"/>
        <property name="url" value="${partition1.url}"/>
        <property name="username" value="${partition1.username}"/>
        <property name="password" value="${partition1.password}"/>
        <property name="maxActive" value="${partition1.maxActive}"/>
        <property name="maxWait" value="${partition1.maxWait}"/>
        <property name="initialSize" value="${partition1.initialSize}"/>
        <property name="maxIdle" value="${partition1.maxActive}"/>
        <property name="minIdle" value="${partition1.minIdle}"/>
        <property name="testWhileIdle" value="${partition1.testWhileIdle}"/>
        <property name="testOnReturn" value="${partition1.testOnReturn}"/>
        <property name="testOnBorrow" value="${partition1.testOnBorrow}"/>
        <property name="validationQuery" value="${partition1.validationQuery}"/>
        <property name="validationInterval" value="30000" />
        <property name="timeBetweenEvictionRunsMillis" value="${partition1.timeBetweenEvictionRunsMillis}"/>
        <property name="minEvictableIdleTimeMillis" value="${partition1.minEvictableIdleTimeMillis}"/>
    </bean>
##java.lang.ClassNotFoundException: com.mysql.jdbc.Driver
原因：没有引入jar包
解决: 在web工程的pom.xml添加：
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.35</version>
</dependency>
##class path resource [spring.xml] cannot be opened because it does not exist
spring.xml中没有任何东西，但没有这个文件却报错。
spring.xml文件内容如下：
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
</beans>
##The namespace attribute, 'http://www.springframework.org/schema/tool', of an <import> element information item must be identical to the targetNamespace attribute, 'http://www.springframework.org/schema/context', of the imported document.
解决：引入jstl包即可。
<!--Jstl标签-->
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>${jstl.version}</version>
</dependency>
##Caused by: java.lang.ClassNotFoundException: org.springframework.beans.factory.config.EmbeddedValueResolver
解决：引入 spring-beans包
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>${spring.version}</version>
</dependency>
