##安装 centos 出错 此主机支持 Intel VT-x，但 Intel VT-x 处于禁用状态
```
1、关机，开机，在品牌商的logo 出现时候按 BIOS 的启动键（一般在logo 的下面有），进入 BIOS 设置页面；

2、选择 configuration ，再选择intel virtual technology ，此时该选项应该是disabled（关闭）的；

3、将disabled（关闭）改为 enabled（开启）；

4、保存设置，重启即可。
```
##鼠标退出 vmware
ctrl+alt即可。
##上传、下载文件
##安装 centos minimal
```
bash:ifconfig: command not found
yum install net-tools
```
##解決 centos -bash: vim: command not found
```
i. 那么如何安裝 vim 呢?
输入rpm -qa|grep vim 命令, 如果 vim 已经正确安裝,会返回下面的三行代码:

root@server1 [~]# rpm -qa|grep vim
vim-enhanced-7.0.109-7.el5
vim-minimal-7.0.109-7.el5
vim-common-7.0.109-7.el5
如果少了其中的某一条,比如 vim-enhanced 的,就用命令 yum -y install vim-enhanced 来安裝:


yum -y install vim-enhanced
如果上面的三条一条都沒有返回, 可以直接用 yum -y install vim* 命令


yum -y install vim*
```
##修改主机名
vim /etc/hostname

##解压缩hadoop


命令格式：tar  -zxvf   压缩文件名.tar.gz

解压缩后的文件只能放在当前的目录。

tar -zxvf hadoop-2.7.3.tar.gz

##linux下重命名文件或文件夹的命令mv既可以重命名，又可以移动文件或文件夹.
```
例子：将目录A重命名为B
mv A B

例子：将/a目录移动到/b下，并重命名为c
mv /a /b/c
```
##vmware虚拟机与主机通信，同时又可以上网
1.NAT方式的网络联结
2.打开虚拟网络编辑器，设置VMnet8的 子网ip 与 子网掩码与主机 的一样。
3.可在虚拟上开启tomcat ，主机访问即可看到效果。
[root@localhost bin]# ./startup.sh 
[root@localhost bin]# ps -ef|grep java
