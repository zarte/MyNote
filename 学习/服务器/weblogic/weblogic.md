几种Web容器的对比


Tomcat
是
Apache
的
Servlet
容器，支持
JSP
,
 
Servlet
和
JDBC
等
J2EE
关键技术，可用
Tomcat
开发基于数据库，
Servlet
和
JSP
页面的
Web
应用。
Tomcat
却不是
EJB
容器；
Tomcat
不支持
EJB
。【
EJB
是分布式应用程序的核心技术】

使用
EJB
组件开发的
Web
应用程序就无法在
Tomcat
下面运行。

凡是需要使用
EJB
来开发的应用（例如，银行、电信等大型的分布式应用系统）就不能用
Tomcat
，这也就是很多公司不选择
Tomcat
的原因。


支持
EJB
的应用服务器，
Weblogic
(
 
Oracle
),
 
WebSphere
（
IBM
)和
JBoss
(
 
Redhat
)都是符合
J2EE
规范的
EJB
容器，所以都可以用来开发大型的分布式应用程序。

原则上来说，只要你要开发基于
EJB
组件的应用，上述三种任选一个都是可以的。唯一的区别是，
Weblogic
和
WebSphere
都是付费的，
JBoss
是开源免费的。

很多公司为了省钱，选择了
JBoss
作为应用服务器，但是，开源免费也就意味着厂商不会为终端用户直接负责；所以，当
JBoss
服务器出现任何问题......元芳，你怎么看？

总的来说，
Weblogic
和
WebSphere
还有
JBoss
都有人用，但是很多公司拿着这些大玩意儿实际上干的也只是
Tomcat
级别的项目，所以如此一来，差别也就不大了。 安装

windows


下载
zip
文件：
http
:
//www.oracle.com/technetwork/middleware/weblogic/downloads/wls-main-097127.html




解压缩后，双击
configure
.
cmd
文件，执行安装，中间会配置一个
domain
(用户名+密码)。

\wls12130\user_projects\domains\mydomain





注意：在安装过程中，程序会自动修改path和classpath的内容：

PATH=;D:\soft\WLS121~1\wls12130\wlserver\server\native\win\32;D:\soft\WLS121~1\wl

ver\bin;D:\soft\WLS121~1\wls12130\oracle_common\modules\org.apache.ant_1.9.2\bin;


CLASSPATH=D:\java\JDK17~1.0_7\lib\tools.jar;D:\soft\WLS121~1\wls12130\wlserver\server\lib\weblogic_s

p.jar;D:\soft\WLS121~1\wls12130\wlserver\server\lib\weblogic.jar;D:\soft\WLS121~1\wls12130\oracle_co

mmon\modules\net.sf.antcontrib_1.1.0.0_1-0b3\lib\ant-contrib.jar;D:\soft\WLS121~1\wls12130\wlserver\

modules\features\oracle.wls.common.nodemanager_2.0.0.0.jar;D:\soft\WLS121~1\wls12130\oracle_common\m

odules\com.oracle.cie.config-wls-online_8.1.0.0.jar;D:\soft\WLS121~1\wls12130\wlserver\common\derby\

lib\derbyclient.jar;D:\soft\WLS121~1\wls12130\wlserver\common\derby\lib\derby.jar;D:\soft\WLS121~1\w

ls12130\wlserver\server\lib\xqrl.jar


访问：
http
:
//localhost:7001/console/  输入之前配置的用户名和密码即可成功访问


linux
部署

windows


准备工作：首先将
web
项目打包为
war
包。

访问：
http
:
//localhost:7001/console/，输入用户名和密码后

点击域结构
 
-》
 
部署
 
-》
 
安装
 
-》选择
 war 
所在的文件目录 -》 下一步 -》 下一步 -》 完成即可。






测试访问：
http
:
//192.168.0.114:7001/Weblogic0727/ 成功!


linux
