##常用cmd命令
```
arp -a  查看局域网中所有ip
nbtstat -a 192.168.1.17 查看192.168.1.17的mac信息


```
##定时关机
```

五分钟后关机：shutdown -s -t 300
at 22:50 shutdown -s
```
##查看某个端口,以及杀掉某个端口
```
netstat -an|find "61616" 

C:\Users\Administrator>netstat -ano | findstr "61616"
  TCP    127.0.0.1:62861        127.0.0.1:61616        SYN_SENT        5116 （可以看到当前进程所占用的进程号）



如:查询占用了8080端口的进程：netstat -ano | findstr "8080"



使用命令杀死进程
1>首先找到进程号对应的进程名称
tasklist|findstr 进程号


如:tasklist | findstr 3112


2>然后根据进程名称杀死进程
/f 强行终止这个进程
taskkill /F /PID 3112

```
##删除某文件夹下指定的文件
```
del /s /q nj\EMS\*.dt  --删除nj\EMS文件夹下的所有.dt的文件。
DEL [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names
ERASE [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names

  names         指定一个或数个文件或目录列表。通配符可被用来
                删除多个文件。如果指定了一个目录，目录中的所
                有文件都会被删除。

  /P            删除每一个文件之前提示确认。
  /F            强制删除只读文件。
  /S            从所有子目录删除指定文件。
  /Q            安静模式。删除全局通配符时，不要求确认。
  /A            根据属性选择要删除的文件。
  attributes      R  只读文件                     S  系统文件
                  H  隐藏文件                     A  存档文件
                  -  表示“否”的前缀
```

