##测试运行不了
```
initializationError(org.junit.runner.manipulation.Filter)
java.lang.Exception: No tests found matching [{ExactMatcher:fDisplayName=testAddSkuEvaluatePic], {ExactMatcher:fDisplayName=testAddSkuEvaluatePic(com.bn.b2b.oms.order.evaluate.SkuEvaluatePicServiceImplTest)], {LeadingIdentifierMatcher:fClassName=com.bn.b2b.oms.order.evaluate.SkuEvaluatePicServiceImplTest,fLeadingIdentifier=testAddSkuEvaluatePic]] from org.junit.internal.requests.ClassRequest@68837a77


at org.junit.internal.requests.FilterRequest.getRunner(FilterRequest.java:40)
解决方法：将测试类中的@Before删除即可。
@Before
     public   void  setUp()  throws  Exception {
         areaFreight  =  new  AreaFreightDto();
        System. out .println( areaFreight );     }   

```
##测试用例
```
/oms-service/src/test/java/com/bn/b2b/oms/order/evaluate/SkuEvaluateServiceImplTest.java

import  java.util.List;
import  org.junit.After;
import  org.junit.Before;
import  org.junit.Test;
import  org.junit.runner.RunWith;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.test.context.ContextConfiguration;
import  org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import  com.bn.b2b.oms.order.dto.SkuEvaluateDto;
import  com.bn.b2b.oms.order.service.evaluate.SkuEvaluateService;
import  com.bn.b2b.oms.utils.uuid.DefaultSequenceGenerator;
import  com.bn.framework.page.Pager;
import  com.bn.framework.page.PagerParam;
import  junit.framework.TestCase;
/**
 * 商品评价测试用例
 *  @author  lfz
 *  @version  2.0
 */
@RunWith (SpringJUnit4ClassRunner. class )
@ContextConfiguration (locations = {  "/conf/spring/spring-impl-test.xml" ,  "/conf/spring/spring-res-test.xml"  })
public   class  SkuEvaluateServiceImplTest  extends  TestCase {
     private  SkuEvaluateDto  skuEvaluate  =  null ;
     @Autowired
     private  SkuEvaluateService  skuEvaluateService ;
     @Before
     public   void  setUp()  throws  Exception {
         skuEvaluate  =  new  SkuEvaluateDto();
        System. out .println( skuEvaluate );
    }
     @After
     public   void  tearDown()  throws  Exception {
        System. out .println( "tear Down..." );
    }
     @Test
     public   void  testAddSkuEvaluate() {
         skuEvaluate .setId(DefaultSequenceGenerator. getInstance ().uuid());
         skuEvaluate .setStatus(0);
         assertTrue ( skuEvaluateService .addSkuEvaluate( skuEvaluate ));
    }
     @Test
     public   void  testUpdateSkuEvaluate() {
        String  skuEvaluateId  =  "d75efb8bff5a419e997d0ef167657209" ;
         skuEvaluate  =  skuEvaluateService .getSkuEvaluate( skuEvaluateId );
         skuEvaluate .setStatus(1);
         skuEvaluateService .updateSkuEvaluate( skuEvaluate );
         assertEquals ( new  Integer(1),  skuEvaluateService .getSkuEvaluate( skuEvaluateId ).getStatus());
    }
     @Test
     public   void  testGetSkuEvaluate() {
        String  skuEvaluateId  =  "d75efb8bff5a419e997d0ef167657209" ;
         skuEvaluate  =  skuEvaluateService .getSkuEvaluate( skuEvaluateId );
         assertNotNull ( skuEvaluate );
    }
     @Test
     public   void  testQuerySkuEvaluateList() {
        String  skuEvaluateIds  =  "d75efb8bff5a419e997d0ef167657209,3831ab144a4346919d7ef5131c07306c" ;
        List<SkuEvaluateDto>  skuEvaluateList  =  skuEvaluateService .querySkuEvaluateList( skuEvaluateIds );
         assertEquals (2,  skuEvaluateList .size());
    }
     @Test
     public   void  testQuerySkuEvaluatePage() {
         skuEvaluate  =  new  SkuEvaluateDto();
         skuEvaluate .setStatus(0);
        PagerParam  pagerParam  =  new  PagerParam();
        Pager<SkuEvaluateDto>  evaluatePage  =  skuEvaluateService .querySkuEvaluatePage( skuEvaluate ,  pagerParam );
         assertNotNull ( evaluatePage );
         assertEquals (1,  evaluatePage .getDatas().size());
    }
}  


/oms-service/src/main/resources/conf/spring/spring-impl-test.xml
<? xml   version = "1.0"   encoding = "UTF-8" ?>
< beans   xmlns = "http://www.springframework.org/schema/beans"
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:ss = "http://www.springframework.org/schema/security"
     xmlns:jee = "http://www.springframework.org/schema/jee"   xmlns:aop = "http://www.springframework.org/schema/aop"
     xmlns:context = "http://www.springframework.org/schema/context"   xmlns:tx = "http://www.springframework.org/schema/tx"
     xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee.xsd
       http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd" >
     <!-- 注册相关后置处理器,扫描包路径下的注解配置 -->
      < context:component-scan   base-package = "com.bn.b2b.oms"   >
          < context:exclude-filter   type = "annotation"   expression = "org.springframework.stereotype.Controller" />    
      </ context:component-scan >
     <!-- 初始化logback配置文件 -->
     < bean   id = "loggingInitialization"
         class = "org.springframework.beans.factory.config.MethodInvokingFactoryBean" >
         < property   name = "targetClass"
             value = "com.bn.framework.log.config.LogbackConfigurer"   />
         < property   name = "targetMethod"   value = "initLogging"   />
         < property   name = "arguments" >
             < list >
                 <!--<value>log</value>-->
                 < value > classpath:conf/log/main_${envName}_logging.xml </ value >
             </ list >
         </ property >
     </ bean >
     < bean   id = "redisClient"   class = "com.bn.framework.cache.redis.client.impl.MyShardedClient"   >
         < property   name = "configPath"   value = "redis"   />
         <!-- 定义默认数据源-->
         < property   name = "globalConfig"   value = "false"   />
     </ bean >
     < bean   id = "redisBinaryClient"   class = "com.bn.framework.cache.redis.client.impl.MyShardedBinaryClient"   >
         < property   name = "configPath"   value = "redis"   />
         <!-- 定义默认数据源-->
         < property   name = "globalConfig"   value = "false"   />
     </ bean >
</ beans >


/oms-service/src/main/resources/conf/spring/spring-res-test.xml
<? xml   version = "1.0"   encoding = "UTF-8" ?>
< beans   xmlns = "http://www.springframework.org/schema/beans"
     xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"   xmlns:ss = "http://www.springframework.org/schema/security"
     xmlns:jee = "http://www.springframework.org/schema/jee"   xmlns:aop = "http://www.springframework.org/schema/aop"
     xmlns:context = "http://www.springframework.org/schema/context"   xmlns:tx = "http://www.springframework.org/schema/tx"
     xsi:schemaLocation = "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee.xsd
    http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/security http://www.springframework.org/schema/security/spring-security.xsd
    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd" >
     <!-- 初始化properties文件,变量值可通过系统属性更改 -->
     < bean   id = "propertyConfig"
         class = "org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" >
         < property   name = "location" >
             < value > classpath:conf/main-setting.properties </ value >
         </ property >
         < property   name = "systemPropertiesModeName"   value = "SYSTEM_PROPERTIES_MODE_OVERRIDE"   />
     </ bean >
    <!-- 数据库连接 -->
     < bean   id = "dataSource"   class = "org.apache.commons.dbcp.BasicDataSource"
         destroy-method = "close" >
         < property   name = "driverClassName"   value = "com.mysql.jdbc.Driver"   />
         < property   name = "url"
             value = "jdbc:mysql://192.168.1.44:3306/oms_oms?useUnicode=true&characterEncoding=utf-8"   />
         < property   name = "username"   value = "root"   />
         < property   name = "password"   value = ""   />
     </ bean >
   
    
     <!-- DAL客户端接口实现 -->
     < bean   id = "dalClient"   class = "com.bn.framework.dac.client.support.PaginationDacClient" >
         <!-- SQL的Xml配置路径 -->
         < property   name = "sqlMapConfigLocation"   value = "classpath*:conf/sqlMAP/*.xml"   />
         <!-- 定义默义的数据源 -->
         < property   name = "dataSource"   ref = "dataSource"   />
     </ bean >
     < tx:annotation-driven   transaction-manager = "transactionManager"
         proxy-target-class = "true"   />
     < bean   id = "transactionManager"
         class = "org.springframework.jdbc.datasource.DataSourceTransactionManager" >
         < property   name = "dataSource"   ref = "dataSource" ></ property >
         < qualifier   value = "oms" />
     </ bean >
    
</ beans >


Tomcat v7.0 Server at localhost-config:context.xml:
<? xml   version = "1.0"   encoding = "UTF-8" ?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at
      http://www.apache.org/licenses/LICENSE-2.0
  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
--><!-- The contents of this file will be loaded for each web application --> < Context >
     <!-- Default set of monitored resources -->
     < WatchedResource > WEB-INF/web.xml </ WatchedResource >
     <!-- Uncomment this to disable session persistence across Tomcat restarts -->
     <!--
    <Manager pathname="" />
    -->
     <!-- Uncomment this to enable Comet connection tacking (provides events
         on session expiration as well as webapp lifecycle) -->
     <!--
    <Valve className="org.apache.catalina.valves.CometConnectionManagerValve" />
    -->
     < Resource   auth = "Container"   driverClassName = "com.mysql.jdbc.Driver"   maxActive = "5"   maxIdle = "2"   maxWait = "-1"   name = "jdbc/advertisement"   password = ""   type = "javax.sql.DataSource"   url = "jdbc:mysql://192.168.1.44:3306/advertisement?characterEncoding=UTF-8"   username = "root" />
     < Resource   auth = "Container"   driverClassName = "com.mysql.jdbc.Driver"   maxActive = "5"   maxIdle = "2"   maxWait = "-1"   name = "jdbc/oms"   password = ""   type = "javax.sql.DataSource"   url = "jdbc:mysql://192.168.1.44:3306/oms_oms?characterEncoding=UTF-8"   username = "root" />
     < Resource   auth = "Container"   driverClassName = "com.mysql.jdbc.Driver"   maxActive = "5"   maxIdle = "2"   maxWait = "-1"   name = "jdbc/uaa"   password = ""   type = "javax.sql.DataSource"   url = "jdbc:mysql://192.168.1.44:3306/b2b_uaa_dev?useUnicode=true&characterEncoding=UTF-8"   username = "root" />
</ Context >  

```