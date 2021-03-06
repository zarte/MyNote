## MySQL数据库迁移
```
关键点：拷贝对应的数据库文件以外，还要拷贝data/ibdata1文件。


MySQL数据库迁移(数据文件直接迁移)

在今年10月下旬的时候，公司的服务器需要迁移，其中涉及到了MySQL数据库迁移。查看了一下MySQL数据文件的大小，接近60G的大小(实际数据并没用那么多)。由于服务器上业务需要，要尽量减少服务器迁移时的损失。所以迁移时间选在了晚上零点开始，而且要尽量减少迁移所用的时间。

在迁移之前有三种方案：
数据库直接导出，拷贝文件到新服务器，在新服务器上导入。
使用【MySQL GUI Tools】中的 MySQLMigrationTool。


数据文件和库表结构文件直接拷贝到新服务器，挂载到同样配置的MySQL服务下。

我在我的电脑上用虚拟机测试后，选中了占用时间最少的第三种方案。下面是三种方案的对比：

 

    第一种方案的优点:会重建数据文件，减少数据文件的占用空间。
    第一种方案的缺点:时间占用长。(导入导出都需要很长的时间，并且导出后的文件还要经过网络传输，也要占用一定的时间。)

    第二种方案的优点：设置完成后传输无人值守
    第二种方案的缺点：
设置繁琐。
传输中网络出现异常，不能及时的被发现，并且会一直停留在数据传输的状态不能被停止，如不仔细观察不会被发现异常。 
传输相对其他fang时间长。 
异常后很难从异常的位置继续传输。

    第三种方案的优点：时间占用短，文件可断点传输。操作步骤少。（绝大部分时间都是在文件的网络传输） 
    第三种方案的缺点：可能引起未知问题，暂时未发现。

下面介绍一下第三种方案d迁移步骤：
保证Mysql版本一致，安装配置基本一致（注意：这里的数据文件和库表结构文件都指定在同一目录data下）
停止两边的Mysql服务（A服务器--迁移-->B服务器)
删除B服务器Mysql的data目录下所有文件
拷贝A服务器Mysql的data目录下除了ib_logfile和.err之外的文件到B服务器data下
启动B服务器的Mysql服务，检测是否发生异常

迁移完成后，服务启动正常，未发现其他异常问题。

```

data文件夹文件列表如下：



  

备注：经测试，源mysql的安装目录及数据文件目录 可以与 目标Mysql的安装目录及数据文件目录 不一致。

此时，只需要拷贝您所需移动的dbname（如上：pa、testdb）及'mysql'和'ibdata1'，即可。

来源：  http://www.cnblogs.com/advocate/archive/2013/11/19/3431606.html
## mysql数据库恢复(*frm)文件 WorkBench
```


在使用虚拟服务器时，服务器提供商一般不会像我们使用本地数据库一样：使用导入导出（这样的文件后缀是*.sql）。大部分时候提供的是一个文件夹，里面包括：数据库名文件夹，文件夹里包括，*.frm,*.MYI,*.MYD,并且包含一个db.opt文件。分别介绍一下：
    *.frm----描述了表的结构
    *.MYI----表的索引
    *.myd----保存了表的数据记录
    db.opt----用文本编辑器打开，可以看到里面保存的是编码信息

要把上述的数据库导入进mysql：
安装mysql数据库：我安装的数据库是MySQL Server 5.5，安装目录选择:D:\program files\MySQL   （注意：路径中不要包含中文）   
在C:\Documents and Settings\All Users\Application Data\  里找到 MySQL\MySQL Server 5.5文件夹，该文件夹下有个文件： my.ini 
在my.ini文件里找到一个datadir的key如：datadir="C:\Documents and Settings\All Users\Application Data\MySQL\MySQL Server 5.5\data\“   
在3找到的一个data文件夹下，拷贝服务商提供备份时提供的文件（包括*.frm,*.MYI,*.MYD,db.opt）   
一般重启mysql服务，在管理界面就可以看到表的结构及数据了

来源：  http://www.cnblogs.com/dupeng0811/archive/2013/07/04/3171524.html
```