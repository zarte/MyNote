



创建100G表空间，提示ORA-01144: File size (13107200 blocks) exceeds maximum of 4194303 blocks 最大4194303 blo




并不是
100g
的表空间，是
100g
的数据文件。一般情况下，单个数据文件的最大为
32g
。

解决方法：

1
、创建多个数据文件，都不能超过
32g

2
、创建大表空间。
create bigfile tablespace  
他的上限是
32t
，不过，
oracle10
及以后版本才能用。
these query results are not updateable







解决方法：
select 
*
 from table_name 
for
 update
;
 
(
table_name
为要编辑的表)
ORA-01031: insufficient privileges





原因：由普通帐号尝试以管理员身份登录导致，如：

sqlplus scott
/
tiger@192
.
168.164
.
101
:
1521
/
orcl as sysdba
;

解决方法:

sqlplus scott
/
tiger@192
.
168.164
.
101
:
1521
/
orcl as sysdba
;
ORA-12514:tns:监听程序当前无法识别连接描述符中请求的服务





问题描述：在
cmd
下，输入
sqlplus 
,
 scott 
,
tiger
可以正常登录
 
。但在
sqldevelper
中却不可以登录。

# tnsnames.ora Network Configuration File: E:\oracle\product\10.2.0\db_1\network\admin\tnsnames.ora

ORCL 
=

  
(
DESCRIPTION 
=

    
(
ADDRESS 
=
 
(
PROTOCOL 
=
 TCP
)(
HOST 
=
 
127.0
.
0.1
)(
PORT 
=
 
1521
)
)

    
(
CONNECT_DATA 
=

      
(
SERVER 
=
 DEDICATED
)

      
(
SERVICE_NAME 
=
 orcl
)

    
)

  
)

EXTPROC_CONNECTION_DATA 
=

  
(
DESCRIPTION 
=

    
(
ADDRESS_LIST 
=

      
(
ADDRESS 
=
 
(
PROTOCOL 
=
 IPC
)(
KEY 
=
 EXTPROC1
))

    
)

    
(
CONNECT_DATA 
=

      
(
SID 
=
 
PLSExtProc
)

      
(
PRESENTATION 
=
 RO
)

    
)

  
)


# listener.ora Network Configuration File: E:\oracle\product\10.2.0\db_1\network\admin\listener.ora

SID_LIST_LISTENER 
=

  
(
SID_LIST 
=

    
(
SID_DESC 
=

      
(
SID_NAME 
=
 
PLSExtProc
)

      
(
ORACLE_HOME 
=
 E
:
\oracle\product\1
0.2
.
0
\db_1
)

      
(
PROGRAM 
=
 extproc
)

    
)

    
(
SID_DESC 
=

    
(
GLOBAL_DBNAME 
=
 orcl
)

    
(
ORACLE_HOME 
=
 E
:
\oracle\product\1
0.2
.
0
\db_1
)

    
(
SID_NAME 
=
 orcl
)

    
)

  
)


LISTENER 
=

  
(
DESCRIPTION_LIST 
=

    
(
DESCRIPTION 
=

      
(
ADDRESS 
=
 
(
PROTOCOL 
=
 IPC
)(
KEY 
=
 EXTPROC1
))

      
(
ADDRESS 
=
 
(
PROTOCOL 
=
 TCP
)(
HOST 
=
 
127.0
.
0.1
)(
PORT 
=
 
1521
))

    
)

  
)

解决方法：在
tnsnames
.
ora
中添加监听
orcl
，并重启服务
OracleOraDb10g_home1TNSListener
。即可重新连上。

Oracle增加新实例



Oracle锁表


insert into table1 select 
*
 from table2

表
2
数据量很大，导致长时间这条


java.sql.sqlexception 关闭的连接




代码中报错：
java
.
sql
.
sqlexception 
关闭的连接

多个方法中用到了
OpraterOracle
.
getConnectionForJDBC
();
 
//读取配置文件，返回数据库连接

原因：多个不同的方法用到了数据库的连接和关闭。

解决方法：在不同的方法中，分别开启数据库连接，用完后，并及时关闭连接库。下次不同的方法调用时再建立新的连接，并关闭。
连接不上数据库



首先确保oracle相关的几个服务是开启的，






pl
/
sql
连接不上数据库，修改数据库的
tnsname
.
ora
文件，
E
:
\oracle\product\1
0.2
.
0
\db_1\NETWORK\ADMIN\tnsname
.
ora
文件：

ORCL 
=

  
(
DESCRIPTION 
=

    
(
ADDRESS 
=
 
(
PROTOCOL 
=
 TCP
)(
HOST 
=
 vanceinfo
)(
PORT 
=
 
1521
))

    
(
CONNECT_DATA 
=

        
(
SERVER 
=
 DEDICATED
)

        
(
SERVICE_NAME 
=
 orcl
)

    
)

  
)


cmd 
连接
oracle
数据库报错：

SP2
-
1503
:
 
无法初始化
 
Oracle
 
调用界面，

SP2
-
1503
:
 
无法初始化
Oracle
 
调
SP2
-
0152

 

解决办法：

 
在
oracle\product\1
0.2
.
0
\db_2\BIN 
目录下找到
 sqlplus
.
exe  

 

右键属性---兼容性---
 
选上已兼容模式运行这个程序---确定

试着打开一下，右键
 
--
 
以管理员的身份运行---
 
然后会打开一个黑窗口，输入用户名：
system  
密码：（你的密码）,
OK
!
结果集已耗尽


解决：

在取
ResultSet
里面的东西.判断
ResultSet
.
next
()每次用完,
Connection
和
ResultSet
都要
close

注意：尽量不要嵌套
Connection
，比如从
ResultSet
中取值后，还没关闭时，接着用这个值再去数据库里做一次操作。
sqlexception 超出打开游标的最大数



实际上，这个错误的原因，主要还是代码问题引起的。
 

ora
-
01000
:
 maximum open cursors exceeded
:表示已经达到一个进程打开的最大游标数。
 

 

这样的错误很容易出现在
Java
代码中的主要原因是：
Java
代码在执行
conn
.
createStatement
()和
conn
.
prepareStatement
()的时候，实际上都是相当与在数据库中打开了一个
cursor
。尤其是，如果你的
createStatement
和
prepareStatement
是在一个循环里面的话，就会非常容易出现这个问题。因为游标一直在不停的打开，而且没有关闭。
 

 

一般来说，我们在写
Java
代码的时候，
createStatement
和
prepareStatement
都应该要放在循环外面，而且使用了这些
Statment
后，及时关闭。最好是在执行了一次
executeQuery
、
executeUpdate
等之后，如果不需要使用结果集（
ResultSet
）的数据，就马上将
Statment
关闭。
 

 

对于出现
ORA
-
01000
错误这种情况，单纯的加大
open_cursors
并不是好办法，那只是治标不治本。实际上，代码中的隐患并没有解除。
 
而且，绝大部分情况下，
open_cursors
只需要设置一个比较小的值，就足够使用了，除非有非常特别的要求。
 

 

下面是一些查看当前
open cursors
的
sql
命令：
 

 

比如在生成记录的时候，有一个自增序列，一次需要增加
153
个
id
，结果报错
java
.
sql
.
SQLException
:
 ORA
-
01000
:
 
超出打开游标的最大数.
 

以管理员身份登录，
cmd
>
 sqlplus system
/
password@192
.
164.1
.
101
:
1521
/
orcl
;

--加大游标数：
SQL
>
 alter system 
set
 open_cursors
=
1000
;
   

--查看当前打开的游标数：
SQL
>
 select count
(*)
 from v$open_cursor
;
 

 

查看数据库连接数：
sql
>
 select count from v$session 
where
 status 
=
 
'INACTIVE'
 and username is not null
;
 

 

--在数据库文件中查找打开游标的最大数：
\Oracle\product\1
0.2
\admin\orcl\profile\init
.
ora
.
91xxxxxx
文件中找到
key
=
open_cursors
的属性，发现
open_cursor 
=
 
300
,即最大游标数为
300
。

 

--在
sqlplus
中查看
 open_cursors 
的数值:
 

SELECT v
.
name
,
 v
.
value as
"最大游标数"
 value FROM V$PARAMETER v WHERE name 
=
 
'open_cursors'
;
 

--使用下面的命令可以修改它的大小:
 alter system 
set
 open_cursors
=
30000
;
   

  
如果可以就直接生效了；

  
如果不行可以使用下面的语句：
alter system  
set
 open_cursors
=
30000
 scope
=
spfile
;
   

然后重启数据库生效。

 

 

如果上面的设置在启动是不生效，那么在管理器使用用户
sys
登录，在服务器名字点右键，启动，你会发现有一个启动加载的文件，把它指定到你修改的
init
.
ora
文件，然后使用语句
 

sqlplus 

用户名：
sys as sysdba 

密码：
 

sql
>
shutdown immediate
；
 

sql
>
startup
;
   

sql
>
create spfile from pfile
;
版本问题


同事执行这段语句没问题，我却不行。原因是数据库版本不统一。我这个数据库必须分组才行。

SELECT 

    
--
 COUNT
(
DISTINCT 
(
r
.
cust_id
)),

    r
.`
cust_id
`,

    r
.`
manager_id
`,

    IF
(

     ISNULL
(
m
.
floatCommission
),

     
0
,

     m
.
floatCommission

   
)

   

   FROM

   ccfp_cust_manager r 

   INNER JOIN

   ccfp_manager m

   ON r
.
manager_id 
=
 m
.
id

   WHERE r
.
manager_id 
=
 
'20150320000004'

   AND r
.
is_valid
=
'T'

   AND r
.
is_default
=
'T'

   AND r
.
user_level
=
'1'
; java.sql.SQLException: ORA-00060   等待资源时检测到死锁


原因分析：

    
首先死锁是怎么发生的:

   
简单说，两个或多个并发事务相互等待，互补想让，没有外力就无法继续下去，这就制造了死锁。数据库检测到死锁时，就会将死锁的各个事务回滚，并抛出
ORA
-
00060
异常。所以上面报错出现的情况极少，将死锁解除后又可以正常运行。

 

解决思路：

    
死锁是无法根除的，特别在高并发的系统中。只有尽可能优化速度，减少互相等待的机会。原则为：执行速度越快越好，访问资源时锁的范围越小越好。根据这个原则就可以优化我们的
sql
，将负责的
sql
拆分，若果业务允许的情况下。还有事务越小越好。

 

解决技巧：

        
1
,出现死锁异常后，手工将死锁解开。

        
2
，找出造成死锁的
sql
：

            a
，直接看日志：程序中日志做的很详细的话，是能够找到具体哪个
sql
报的错，操作的哪个表，还有别的模块也操作这个表，线程，并发的程序也会引起。

            b
,通过
oracle
的后台
v$session
表
 
和
 v$sql 
的分析
 
找到。
  

        
3
,对
sql
进行优化。
java程序中如何使用oracle的事务机制



public
 
static
 
void
 main
(
String
[]
 args
)
 
{

		
// 设置全局变量，可以在catch中使用

		
Connection
 ct 
=
 
null
;

		
try
 
{

			
// 加载驱动

			
Class
.
forName
(
"oracle.jdbc.driver.OracleDriver"
);

			
// 得到连接

			ct 
=
 
DriverManager
.
getConnection
(

					
"jdbc:oracle:thin:@127.0.0.1:1521:simlink"
,
 
"scott"
,

					
"tiger"
);

 

			
// 加入事务处理,设置不能自动提交

			ct
.
setAutoCommit
(
false
);

 

			
Statement
 sm 
=
 ct
.
createStatement
();

			
// 从ALLEN工资中减去100元

			sm
.
executeUpdate
(
"update emp set sal=sal-100 where ename='ALLEN'"
);

			
// 故意是程序在此处跳出，抛出异常

			
// 注：如果程序中不适用事务机制，则allen会减去100元，但smith不会增加100元，使用后因为程
序报错，所以allen和smith的工资都不会变化

			
int
 i 
=
 
9
 
/
 
0
;

			
// 在SMITH工资中加上100元

			sm
.
executeUpdate
(
"update emp set sal=sal+100 where ename='SMITH'"
);

 

			
// 事务提交

			ct
.
commit
();

			sm
.
close
();

			ct
.
close
();

 

		
}
 
catch
 
(
Exception
 e
)
 
{

			
// 如果程序发生异常，就回滚

			
try
 
{

				ct
.
rollback
();

			
}

			
// 捕捉回滚事务异常

			
catch
 
(
Exception
 ex
)
 
{

				ex
.
printStackTrace
();

			
}

			e
.
printStackTrace
();

		
}

	
}
update表时，提示无效的数字


表
user
(
id
,
name
,
age
)
  id
的类型为
varchar2
(
64
)

update user set age 
=
 age
+
1
 where id 
=
 
113143232
 
,有可能会提示无效的数字。

修改为
 update user set age 
=
 age 
+
 
1
 where id 
=
 
'113143232 '
  
就正常。

原因分析：

如果要更新的
user
中
id
为
113143232
的记录数唯一的话，
id
列加不加单引号都是可以的。

但如果
user
中
id
为
113143232
的记录不唯一，则必须要加单引号。

 
ORA-01732: 此视图的数据操纵非法


视图只是一个虚拟的对象，只有物化视图才能被更改，普通的视图只不过是存储了你对表的访问语句。

 

带
union
 all
的视图不是可更新的视图。

可更新视图：

1
）没有使用连接函数、集合运算函数和组函数

2
）创建视图的
select
语句中没有聚合函数且没有
GROUP BY
,
ONNECT BY
,
START WITH
子句以及
DISTINCT
关键字

3
）
select
语句中不包含从基表红通过计算得到的列

4
）创建视图没包含只读属性
Oracle之主键的创建、添加、删除操作

创建表的同时创建主键约束

  
--无命名

     SQL
>
 create table jack 
(
id 
int
 primary key not 
null
,
name varchar2
(
20
));

     SQL
>
 select table_name
,
index_name from user_indexes where table_name
=
'JACK'
;

  
--有命名

     SQL
>
 create table jack 
(
id 
int
 
,
name varchar2
(
20
),
constraint ixd_id primary key
(
id
));

 

向表中添加主键约束

  SQL
>
 create table jack as select 
*
 from dba_objects
;

  SQL
>
 desc jack
;

  SQL
>
 alter table jack add constraint pk_id primary key
(
object_id
);

  
--另外当索引创建好以后再添加主键的效果：

     SQL
>
 create table jack as select 
*
 from dba_objects
;

     SQL
>
 create index ind_object_id on jack
(
object_id
);

     SQL
>
 select table_name
,
index_name from user_indexes where table_name
=
'JACK'
;

     SQL
>
 desc jack
;

     SQL
>
 alter table jack add constraint pk_id primary key
(
object_id
);

 

修改主键约束

   
--禁用/启用主键

      SQL
>
 alter table jack disable primary key
;

      SQL
>
 alter table jack enable primary key
;

   
--重命名主键

      SQL
>
 alter table jack rename constraint pk_id to pk_jack_id
;

删除表中已有的主键约束

   
--无命名

     
先利用
user_cons_columns
表查得主键名：
SQL
>
 select owner
,
constraint_name
,
table_name
,
column_name from user_cons_columns where table_name 
=
 
'JACK'
;

      SQL
>
 select table_name
,
index_name from user_indexes where table_name
=
'JACK'
;

      SQL
>
 alter table jack drop constraint SYS_C0011105
;

   
--有命名

      SQL
>
 select owner
,
constraint_name
,
table_name
,
column_name from user_cons_columns where table_name 
=
 
'JACK'
;

      SQL
>
 alter table jack drop constraint IXD_ID
;

删除表的主键约束时，ORA-00054:资源正忙，但指定以 NOWAIT 方式获取资源，或者超时失效



很明显这张表被锁了,可以查到是被谁锁定的。

select s
.
username
,
 s
.
sid
,
 s
.
serial
#
 

from gv$session s
,
 gv$lock l
,
 dba_objects o

where l
.
sid 
=
 s
.
sid 

      and l
.
id1 
=
 o
.
object_id
(+)

      and s
.
username is not 
null

      and o
.
owner 
=
 
'REPORT'

执行结果：

USERNAME    SID    SERIAL
#

---------
  
-----
   
----------

REPORT      
140
      
417

果然是这个表被另一个同事锁住了，而这个人又不在工位上。断开了他的
session
后，就可以成功执行。

SQL
>
 alter system kill session   
'140,417'
;

System
 altered


快速解决方法二：

解决方法如下：

 

=========================================================

SQL
>
 select session_id from v$locked_object
;

 

SESSION_ID

----------

       
56

 

SQL
>
 SELECT sid
,
 serial
#,
 username
,
 osuser FROM v$session where sid 
=
 
56
;

 

       SID    SERIAL
#
 USERNAME                       OSUSER

----------
 
----------
 
------------------------------
 
------------------------------

       
56
       
2088
      ghb                          fy

 

SQL
>
 ALTER SYSTEM KILL SESSION 
'56,2088'
;

 

System
 altered



ORA-01861: 文字与格式字符串不匹配




sql
>
 select 
*
 from user_common 
where
 t
.
updatetime 
=
 
'2015-7-29 18:09:32'

原因：
sql
语句要用
to
-
date
转换时间
  to_date
(
'2011-04-01 00:11;11'
,
'yyyy-mm-dd hh24:mi:ss'
)

即修改为：

sql
>
 select 
*
 from user_common 
where
 t
.
updatetime 
=
 to_date
(
'2015-7-29 18:09:32'
,
'yyyy-mm-ss hh24:mi:ss'
)
ORA-00936：缺失表达式


最有可能的原因是：

子查询中查询出来的结果含有多条记录导致。

sql
>
 select 
*
 from user_common t 
where
 t
.
id
=
'178312'
 and t
.
updatetime 
=
 
(
select max
(
t2
.
updatetime
)
 from user_common t2
)

由于
 
子查询
 
没有
where
过滤，所以查询出来的是整张表里的最新时间，而并非是当前
id
下的最新时间。

修改后的
sql
.

sql
>
 select 
*
 from user_common t 
where
 t
.
id
=
'178312'
 and t
.
updatetime 
=
 
(
select max
(
t2
.
updatetime
)
 from user_common t2 
where
 t2
.
id
=
'178312'
)



