[TOC]
##ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost' (10061)
```
网上关于此错误的描述有很多,我们知道MySQL的默认端口是3306,当以其他端口启动服务时,使用mysql命令又没有指定对应的端口,当然就无法连接Server啦
附加信息:

MySQL官方文档(5.5) http://dev.mysql.com/doc/refman/5.5/en/index.html

MySQL官方文档(5.6) http://dev.mysql.com/doc/refman/5.6/en/index.html

来源：  http://www.tuicool.com/articles/6ZnYRb
```
##data目录不见了
```
启动   mysql
>mysqld --initialize 
```
##mysqld: Could not create or access the registry key needed for the MySQL application
```
to log to the Windows EventLog. Run the application with sufficient
privileges once to create the key, add the key manually, or turn off
logging for that application.
MySQL启动 遇到上面问题
解决办法: Since the error seems to be related to privileges/permissions, have you tried to "run as administrator"? I'm not too savvy about permissions on Windows machines...but I think "run as administrator" is similar to a linux command running under sudo 

http://stackoverflow.com/questions/33691038/issue-when-running-mysqld 

就是以管理员登录 运行mysqld
启动后登录 
>mysql -u root -p 
找到
“2016-02-12T15:35:00.026880Z 1 [Note] A temporary password is generated for root@localhost: Ux<<lCbrr8&d”
Ux<<lCbrr8&d
这个就是我们要找的密码了
登陆成功：
```
##修改密码的指令
```
果然新版本中修改密码的指令也不是那么好找的。。
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
修改成功。
```
##mysql error:
```
Table 'performance_schema.session_variables' doesn't exist
1.CMD 进入MYSQL的安装目录..Bin 下
执行 D:\soft_study\mysql\bin>mysql_upgrade -u root -p --force
如果报错：Error: Server version (5.7.15) does not match with the version of
the server (5.7.13) with which this program was built/distributed. You can
use --skip-version-check to skip this check.

改为执行：D:\soft_study\mysql\bin>mysql_upgrade -u root -p --force --skip-version-check
2.输入 密码
Upgrade process completed successfully.
Checking if update is needed.即可

注：如果不重启mysql服务，有可能会出现以下错误
java.sql.SQLException: Native table 'performance_schema'.'session_variables' has the wrong structure
```
##[ERROR] Fatal error: Can't open and lock privilege tables: Table 'mysql.user' doesn't exist
```
 [ERROR] Aborting
就是mysql数据库有问题，具体来说就是user表有问题。
解决方案多是linux下的，初始化数据库就ok，即：mysql_install_db --user=mysql
Windows下的这招不能用，
到data\mysql目录下一看，找不到user.frm,user.MYD，user.MYI三个文件，即user表被删了。。
于是从下载的mysql.zip中把三个文件拷过去就行，
当然原来的用户名密码全部重置了，看来这也是个破解mysql密码的方法


ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
2016-09-30T06:06:16.823233Z 0 [ERROR] InnoDB: The innodb_system data file 'ibdat
a1' must be writable
解决方法：
1、打开任务管理器终止mysqld进程；
2、打开mysql安装目录的data文件夹，删除以下2个文件：
ib_logfile0和ib_logfile1
3、重新启动mysql

mysql> select user,host,password from user where user="root";
ERROR 1054 (42S22): Unknown column 'password' in 'field list'
原因：新安装的mysql5.7 password字段改成了authentication_string.
mysql> select user,host,authentication_string from user where user="root";
+------+---------------+-------------------------------------------+
| user | host          | authentication_string                     |
+------+---------------+-------------------------------------------+
| root | localhost     | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 |
| root | 192.168.1.173 | *81F5E21E35407D884A6CD4A731AEBFB6AF209E1B |
+------+---------------+-------------------------------------------+
2 rows in set (0.00 sec)


mysql> flush privileges;
ERROR 1146 (42S02): Table 'mysql.servers' doesn't exist
原因是：新安装的mysql5.7是没有data目录的，data/mysql目录是从之前的版本拷贝过来的，而没有将之前 data/ibdata1文件拷贝过来。
将data/ibdata1文件也拷贝过来就可以了。
```
##客户端登录出现1130 is not allowed to connect to this MySql server”问题，解决方法
```
错误解释：服务器没有授权给你这个ip是不能连接的
你想root用户名使用root密码从任何主机连接到mysql服务器的话。

运行命令：mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
如果你想允许用户root从ip为192.168.1.3的主机连接到mysql服务器，并使用root作为密码

运行命令：mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.3' IDENTIFIED BY 'root' WITH GRANT OPTION;
上面的命令创建一个可以从任意机器以root登录的超级账号，口令是root。这样，就可以使用方便的图形工具(navicat for mysql)进行登录和操作，包括修改root的口令。
```
## ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```
mysql> SET PASSWORD = PASSWORD('123456'); 
```
## Incorrect result size: expected 1, actual 0
```
期望是1，得到的结果是0
意思是用的query方法不对，比如queryMap应该是获取到一个用户的数据，但是却获得了0个或多个用户的数据，应该用list集合盛放，所以应该用queryForList()方法
``` 
##net stop mysql提示发生系统错误5 拒绝访问
```
解决方法：以管理员身份启动cmd，再执行net stop mysqln 和 net start mysql即可。
```
##查询或删除时 提示表不存在，报1146
```
有一个办法，到对应的路径删除文件，但是得停服务。不停服务有什么好办法没？
(新建表的名字与此表不同，建立后，在此表所在数据库目录中 查找此名字，修改为你要删除的表名字试试)
为什么会产生这种情况，是因为我直接复制的数据库吗？存储引擎是innodb
(innodb类型的表是不能直接COPY的)

假设库aaa中的表bbb，出现了你这个情况，那么去aaa这个目录，删除bbb.frm,然后找个已知好的frm，复制成bbb.frm 即可，然后 删除此表，drop aaa.bbb;
当然前提是你不需要此表中的数据了。
```