##使用MyBatis Generator自动创建代码
```
这两天需要用到MyBatis的代码自动生成的功能，由于MyBatis属于一种半自动的ORM框架，所以主要的工作就是配置Mapping映射文件，但是由于手写映射文件很容易出错，所以可利用MyBatis生成器自动生成实体类、DAO接口和Mapping映射文件。这样可以省去很多的功夫，将生成的代码copy到项目工程中即可。
           使用自动生成有很多方式，可以在eclipse中安装插件，但是以下将要介绍的这种方式我认为很轻松，最简单，不需要装插件，只需要下几个jar包即可，把它们放在一个目录下面。
       生成代码需要的文件和jar包：
（上图文件下载地址：http://download.csdn.net/detail/u012909091/7206091）
       其中有mybatis框架的jar包，数据库驱动程序jar包以及MyBatis生成器jar包。其中的generatorConfig.xml是需要我们来配置的文件，配置如下：
<?xml version="1.0" encoding="UTF-8"?>    
<!DOCTYPE generatorConfiguration    
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"    
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">    
<generatorConfiguration>    
<!-- 数据库驱动-->    
    <classPathEntry  location="mysql-connector-java-5.1.25-bin.jar"/>    
    <context id="DB2Tables"  targetRuntime="MyBatis3">    
        <commentGenerator>    
            <property name="suppressDate" value="true"/>    
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->    
            <property name="suppressAllComments" value="true"/>    
        </commentGenerator>    
        <!--数据库链接URL，用户名、密码 -->    
        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://125.221.1.1/db_124" userId="dem" password="dem">    
        </jdbcConnection>    
        <javaTypeResolver>    
            <property name="forceBigDecimals" value="false"/>    
        </javaTypeResolver>    
        <!-- 生成模型的包名和位置-->    
        <javaModelGenerator targetPackage="test.domain" targetProject="src">    
            <property name="enableSubPackages" value="true"/>    
            <property name="trimStrings" value="true"/>    
        </javaModelGenerator>    
        <!-- 生成映射文件的包名和位置-->    
        <sqlMapGenerator targetPackage="test.mapping" targetProject="src">    
            <property name="enableSubPackages" value="true"/>    
        </sqlMapGenerator>    
        <!-- 生成DAO的包名和位置-->    
        <javaClientGenerator type="XMLMAPPER" targetPackage="test.IDao" targetProject="src">    
            <property name="enableSubPackages" value="true"/>    
        </javaClientGenerator>    
        <!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名-->    
        <table tableName="user_info_t" domainObjectName="User" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>  
    </context>    
</generatorConfiguration>    
       当以上这些完成之后，只需要打开控制台，进入lib目录下，执行脚本：
java -jar mybatis-generator-core-1.3.2.jar -configfile generatorConfig.xml -overwrite即可。
      这样在生成之后，就可以在src目录下找到相应的文件夹，每个表格都会对应三个文件（实体类、接口、配置文件）。
```