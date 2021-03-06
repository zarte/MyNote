##context:property-placeholder属性
```
test.database.properties:

## partition1
partition1.driverClassName=com.mysql.jdbc.Driver
partition1.ip=192.168.24.87
partition1.port=3306
partition1.database=demo
partition1.url=jdbc:mysql://localhost:3306/demo?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
partition1.username=root
partition1.password=root
partition1.maxActive=50
partition1.maxWait=1800
partition1.initialSize=2
partition1.minIdle=2
partition1.testWhileIdle=true
partition1.testOnReturn=false
partition1.testOnBorrow=true
partition1.validationQuery=select now()
partition1.timeBetweenEvictionRunsMillis=60000
partition1.minEvictableIdleTimeMillis=180000
## partition2
partition2.driverClassName=com.mysql.jdbc.Driver
partition2.ip=192.168.24.87
partition2.port=3306
partition2.database=ssm
partition2.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
partition2.username=root
partition2.password=root
partition2.maxActive=50
partition2.maxWait=1800
partition2.initialSize=2
partition2.minIdle=2
partition2.testWhileIdle=true
partition2.testOnReturn=false
partition2.testOnBorrow=true
partition2.validationQuery=select now()
partition2.timeBetweenEvictionRunsMillis=60000
partition2.minEvictableIdleTimeMillis=180000

1.有些参数在某些阶段中是常量
    比如 ：a、在开发阶段我们连接数据库时的连接url，username，password，driverClass等 
                 b、分布式应用中client端访问server端所用的server地址，port，service等  
                  c、配置文件的位置
2.而这些参数在不同阶段之间又往往需要改变
    比如：在项目开发阶段和交付阶段数据库的连接信息往往是不同的，分布式应用也是同样的情况。
期望：能不能有一种解决方案可以方便我们在一个阶段内不需要频繁书写一个参数的值，而在不同阶段间又可以方便的切换参数配置信息
解决：spring3中提供了一种简便的方式就是context:property-placeholder/元素

只需要在spring的配置文件里添加一句：<context:property-placeholder location="classpath:jdbc.properties"/> 即可，这里location值为参数配置文件的位置，参数配置文件通常放在src目录下，而参数配置文件的格式跟java通用的参数配置文件相同，即键值对的形式，例如：
#jdbc配置
test.jdbc.driverClassName=com.mysql.jdbc.Driver
test.jdbc.url=jdbc:mysql://localhost:3306/test
test.jdbc.username=root
test.jdbc.password=root

```
##spring中 context:property-placeholder 导入多个独立的 .properties配置文件
```
Spring容器采用反射扫描的发现机制，在探测到Spring容器中有一个 org.springframework.beans.factory.config.PropertyPlaceholderConfigurer的 Bean就会停止对剩余PropertyPlaceholderConfigurer的扫描（Spring 3.1已经使用PropertySourcesPlaceholderConfigurer替代 PropertyPlaceholderConfigurer了）。


换句话说，即Spring容器仅允许最多定义一个PropertyPlaceholderConfigurer(或<context:property-placeholder/>)，其余的会被Spring忽略掉（其实Spring如果提供一个警告就好了）。


拿上来的例子来说，如果A和B模块是单独运行的，由于Spring容器都只有一个PropertyPlaceholderConfigurer， 因此属性文件会被正常加载并替换掉。如果A和B两模块集成后运行，Spring容器中就有两个 PropertyPlaceholderConfigurer Bean了，这时就看谁先谁后了， 先的保留，后的忽略！因此，只加载到了一个属性文件，因而造成无法正确进行属性替换的问题。

咋解决呢？
通配符解决
<context:property-placeholder location="classpath*:conf/conf*.properties"/>  
```
## 自动扫描 context:component-scan
```
<context:component-scan base-package="com.adanac.ssm"/>  
```

## typeAliases 元素
```
别名是一个较短的Java 类型的名称。这只是与XML 配置文件相关联，减少输入多余的完整类
名。例如：
 <typeAliases>
<typeAlias alias="Author" type="domain.blog.Author"/>
<typeAlias alias="Blog" type="domain.blog.Blog"/>
<typeAlias alias="Comment" type="domain.blog.Comment"/>
<typeAlias alias="Post" type="domain.blog.Post"/>
<typeAlias alias="Section" type="domain.blog.Section"/>
<typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
在这个配置中，您就可以在想要使用"domain.blog.Blog"的地方使用别名“Blog”了。
对常用的java 类型，已经内置了一些别名支持。这些别名都是不区分大小写的。注意java
的基本数据类型，它们进行了特别处理，加了“_”前缀。
```
