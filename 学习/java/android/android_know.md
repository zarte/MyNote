##重新启动adb
```


模拟器在运行一段时间后，adb服务有可能（在Windows进程中可找到这个服务，该服务用来为模拟器或通过USB数据线连接的真机服务）会出现异常。这时需要重新对adb服务关闭和重启。

当然，重启Eclipse可能会解决问题。但那比较麻烦。如果想手工关闭adb服务，可以使用下面的命令。

E:\android\soft\sdk\platform-tools>adb kill-server
  adb kill-server
  在关闭adb服务后，要使用如下的命令启动adb服务。



E:\android\soft\sdk\platform-tools>adb start-server
  adb start-server

来源：  http://blog.csdn.net/alexbxp/article/details/6779005/
``` ##安卓真机调试
1、首先将手机设置为调试模式
方法：设置——应用程序——开发——USB调试，打上√即可
 
 
2、用数据线连接至电脑，在电脑上安装豌豆荚，此时豌豆荚会帮你安装驱动，安装好后豌豆荚就可以连接上手机了


 
3、用adb命令测试是否有装置已连接
命令：adb devices


看到已经有一个装置了，即为我们连接的真机
注意：有的人可能提示找不到这个adb命令，这是因为你没有将其加入到path环境变量中，或者你进入sdk下的tools目录在运行此命令就不会报错，或者将tools路径加入到环境变量中，当然推荐第二种方法了
 
有的时候可能会出现下面的错误：
adb server is out of date.  killing...  
ADB server didn't ACK  * 
failed to start daemon *
 
究其源就是adb server没启动

到stackoverflow上查了一下 经过分析整理如下：

cmd: adb nodaemon server

cannot bing 'tcp:5037' 原来adb server 端口绑定失败

继续查看到底是哪个程序给占用了


C:\Users\xxxxxx>netstat -ano | findstr "5037"
  
      TCP    
127.0.0.1:5037         0.0.0.0:0              LISTENING       4236
  
      TCP    
127.0.0.1:5037         127.0.0.1:49422        ESTABLISHED     4236
  
      TCP    
127.0.0.1:49422        127.0.0.1:5037         ESTABLISHED     3840  
打开任务管理器kill掉PID为4236 的这个进程。ok，至此问题解决了


   
4、开始在真机上调试
在eclipse中选择 Run——Run Configurations，在左边选择好你要调试的工程，然后将右边切换至Target标签下
这有三个选项，如果你想连接至真机调试，可选第一个或第二个，这里我直接选择第一个，点击Run，等待几秒钟出现以下界面

 
在这里就看到了我们的真机装置了，选择上面的真机OK即可在真机上运行程序了

来源：  http://www.cnblogs.com/lanxuezaipiao/archive/2013/03/11/2953564.html ## android工程下不能运行java main程序的解决办法

右击有 main 方法的类
===> Run as
===> Run Configurations
===>双击 java application
===> 单击有 main 方法的类
===>选中 classpath 选项卡
===> remove 掉 Bootstrap Entries 下的 android . jar
===> 然后点击 advanced
===> Add Library
===> JRE System Library
===> next
===>最后 finish
===> Run
--设置好后， debug 时，也可以了。