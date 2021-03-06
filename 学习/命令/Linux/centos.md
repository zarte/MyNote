[TOC]
##忘记root登录密码
- 重启linux系统
-  在第二行最后边输入 single，有一个空格:
- 最后按“b”启动，启动后就进入了单用户模式了
此时已经进入到单用户模式了，你可以更改root密码了。更密码的命令为 passwd

## linux登录情况下修改root并添加新的用户
- linux下root密码修改(修改root的密码为allen111)
root身份登录后，执行
`passwd root allen111`
 
- 添加用户（添加allen用户，并设置其密码为allen111）
root身份登录后，
`useradd allen`
`passwd  allen111`

## 建立用户和组
- 建立用户
```
adduser hadoop //新建用户hadoop
passwd hadoop //给hadoop用户设置密码
```
- 建立工作组
`groupadd bigdata //新建bigdata工作组`
- 建立用户同时增加工作组
`useradd -g bigdata hadoop //新建hadoop用户并增加到bigdata组中`
- 给已有的用户增加工作组
```
usermod -G groupname username
或者：
gpasswd -a user group
```
- 临时关闭
```
在/etc/shadow文件中属于该用户的行的第二个字段（密码）前面加上*就可以了。
想恢复该用户，去掉*即可。
或者使用如下命令关闭用户账号：passwd peter –l
重新释放（恢复使用账号）：passwd peter –u
```
- 永久性删除用户账号
```
userdel peter
groupdel peter
usermod –G peter peter   （强制删除该用户的主目录和主目录下的所有文件和子目录）
```
- 从组中删除用户
编辑/etc/group 找到GROUP1那一行，删除 A 或者用命令 `gpasswd -d A GROUP`
- 显示用户信息
`id adanac //查看用户adanac的信息`
`cat /etc/passwd`

## 查看centos中的用户和用户组
用户列表文件：/etc/passwd
用户组列表文件：/etc/group
查看系统中有哪些用户：cut -d : -f 1 /etc/passwd
查看可以登录系统的用户：cat /etc/passwd | grep -v /sbin/nologin | cut -d : -f 1
centos下修改hostname,ip,netmask,gateway,dns
--修改host name
  即时生效: # hostname centos 重启后失效
  启动（重启计算机）生效：　修改/etc/sysconfig/network
  HOSTNAME=tank #修改此处主机名
  NETWORKING=yes
--修改ip/netmask/gateway
  配置文件：/etc/sysconfig/network-scripts/ifcfg-eth0
  DEVICE=eth0 #描述网卡对应的设备别名，例如ifcfg-eth0的文件中它为eth0
  BOOTPROTO=static #设置网卡获得ip地址的方式，可能的选项为static,dhcp或bootp,分别对应静态指定的ip地址，
  通过dhcp协议获得的ip地址，通过bootp协议获得的ip地址
  BROADCAST=192.168.0.255 #对应的子网广播地址
  HWADDR=00:07:E9:05:E8:B4 #对应的网卡物理地址
  IPADDR=12.168.1.2 #如果设置网卡获得 ip地址的方式为静态指定，此字段就指定了网卡对应的ip地址
  IPV6INIT=no
  IPV6_AUTOCONF=no
  NETMASK=255.255.255.0 #网卡对应的网络掩码
  NETWORK=192.168.1.0 #网卡对应的网络地址
  ONBOOT=yes #系统启动时是否设置此网络接口，设置为yes时，系统启动时激活此设备
--修改dns
  配置文件：/etc/resolv.conf
  nameserver 8.8.8.8 #google域名服务器
  nameserver 8.8.4.4 #google域名服务器
--重新启动网络配置
  service network restart
  或
  /etc/init.d/network restart
--修改 IP 地址
  即时生效：ifconfig eth0 192.168.0.2 netmask 255.255.255.0
  启动生效：/etc/sysconfig/network-scripts/ifcfg-eth0
--修改网关 Default Gateway
  即时生效：route add default gw 192.168.0.1 dev eth0
  启动生效：/etc/sysconfig/network
--修改 DNS
 　修改/etc/resolv.conf, 修改后可即时生效，启动同样有效
常用命令
--查看文件内容
cat file1 从第一个字节开始正向查看文件的内容
tac file1 从最后一行开始反向查看一个文件的内容
more file1 查看一个长文件的内容
less file1 类似于 'more' 命令，但是它允许在文件中和正向操作一样的反向操作
head -2 file1 查看一个文件的前两行
tail -2 file1 查看一个文件的最后两行
tail -f /var/log/messages 实时查看被添加到一个文件中的内容
--文件搜索
find / -name file1 从 '/' 开始进入根文件系统搜索文件和目录
find / -user user1 搜索属于用户 'user1' 的文件和目录
find /home/user1 -name \*.bin 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件
find /usr/bin -type f -atime +100 搜索在过去100天内未被使用过的执行文件
find /usr/bin -type f -mtime -10 搜索在10天内被创建或者修改过的文件
find / -name \*.rpm -exec chmod 755 '{}' \; 搜索以 '.rpm' 结尾的文件并定义其权限
find / -xdev -name \*.rpm 搜索以 '.rpm' 结尾的文件，忽略光驱、捷盘等可移动设备
locate \*.ps 寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令
whereis halt 显示一个二进制文件、源码或man的位置
which halt 显示一个二进制文件或可执行文件的完整路径
--命令find
1.作用
find命令的作用是在目录中搜索文件，它的使用权限是所有用户。
2.格式
find [path][options][expression]
path指定目录路径，系统从这里开始沿着目录树向下查找文件。它是一个路径列表，相互用空格分离，如果不写path，那么默认为当前目录。
3.主要参数
[options]参数：
－depth：使用深度级别的查找过程方式，在某层指定目录中优先查找文件内容。
－maxdepth levels：表示至多查找到开始目录的第level层子目录。level是一个非负数，如果level是0的话表示仅在当前目录中查找。
－mindepth levels：表示至少查找到开始目录的第level层子目录。
－mount：不在其它文件系统（如Msdos、Vfat等）的目录和文件中查找。
－version：打印版本。
[expression]是匹配表达式，是find命令接受的表达式，find命令的所有操作都是针对表达式的。它的参数非常多，这里只介绍一些常用的参数。
—name：支持统配符*和?。
－atime n：搜索在过去n天读取过的文件。
－ctime n：搜索在过去n天修改过的文件。
－group grpoupname：搜索所有组为grpoupname的文件。
－user 用户名：搜索所有文件属主为用户名（ID或名称）的文件。
－size n：搜索文件大小是n个block的文件。
－print：输出搜索结果，并且打印。
4.应用技巧
find命令查找文件的几种方法：
(1)根据文件名查找
find / －name lilo.conf 在整个硬盘上（/）搜索文件名是lilo.conf的文件
(2)快速查找文件
find /etc －name smb.conf 在/etc目录内搜索配置文件smb.conf
(3)根据部分文件名查找方法
find / －name '*abvd*' 在整个硬盘上搜索文件名中含有abvd的文件
(4) 使用混合查找方式查找文件
find /etc -size +500000c -and -mtime +1  
在/etc目录中查找大于500000字节，并且在24小时内修改的某个文件
--命令mkdir
1.作用
mkdir命令的作用是建立名称为dirname的子目录，与MS DOS下的md命令类似，它的使用权限是所有用户。
2.格式
mkdir [options] 目录名
3.[options]主要参数
－m, －－mode=模式：设定权限<模式>，与chmod类似。
－p, －－parents：需要时创建上层目录；如果目录早已存在，则不当作错误。
－v, －－verbose：每次创建新目录都显示信息。
－－version：显示版本信息后离开。
4.应用实例
在进行目录创建时可以设置目录的权限，此时使用的参数是“－m”。假设要创建的目录名是“tsk”，让所有用户都有rwx(即读、写、执行的权限)，那么可以使用以下命令：
$ mkdir －m 777 tsk  
--命令grep
1.作用
grep命令可以指定文件中搜索特定的内容，并将含有这些内容的行标准输出。
grep全称是Global Regular Expression Print，表示全局正则表达式版本，它的使用权限是所有用户。
2.格式
grep [options]
3.主要参数
[options]主要参数：
－c：只输出匹配行的计数。
－I：不区分大小写（只适用于单字符）。
－h：查询多文件时不显示文件名。
－l：查询多文件时只输出包含匹配字符的文件名。
－n：显示匹配行及行号。
－s：不显示不存在或无匹配文本的错误信息。
－v：显示不包含匹配文本的所有行。
pattern正则表达式主要参数：
\：忽略正则表达式中特殊字符的原有含义。
^：匹配正则表达式的开始行。
$: 匹配正则表达式的结束行。
\<：从匹配正则表达式的行开始。
\>：到匹配正则表达式的行结束。
[ ]：单个字符，如[A]即A符合要求 。
[ - ]：范围，如[A-Z]，即A、B、C一直到Z都符合要求 。
. ：所有的单个字符。
* ：有字符，长度可以为0。
在Linux系统上，正则表达式通常被用来查找文本的模式，以及对文本执行“搜索－替换”操作和其它功能。
4.应用实例
如果要查看nnn.nnn网络地址，但是却忘了第二部分中的其余部分，只知到有两个句点，
例如nnn nn..。要抽取其中所有nnn.nnn IP地址，使用[0－9 ]\{3 \}\.[0－0\{3\}\。含义是任意数字出现3次，后跟句点，接着是任意数字出现3次，后跟句点。
$ grep '[0－9 ]\{3 \}\.[0－0\{3\}\' ipfile
补充说明，grep家族还包括fgrep和egrep。
fgrep是fix grep，允许查找字符串而不是一个模式；egrep是扩展grep，支持基本及扩展的正则表达式，但不支持\q模式范围的应用及与之相对应的一些更加规范的模式。
--命令dd
1.作用
dd命令用来复制文件，并根据参数将数据转换和格式化。
2.格式
dd [options]
3.[opitions]主要参数
bs=字节：强迫 ibs=<字节>及obs=<字节>。
cbs=字节：每次转换指定的<字节>。
conv=关键字：根据以逗号分隔的关键字表示的方式来转换文件。
count=块数目：只复制指定<块数目>的输入数据。
ibs=字节：每次读取指定的<字节>。
if=文件：读取<文件>内容，而非标准输入的数据。
obs=字节：每次写入指定的<字节>。
of=文件：将数据写入<文件>，而不在标准输出显示。
seek=块数目：先略过以obs为单位的指定<块数目>的输出数据。
skip=块数目：先略过以ibs为单位的指定<块数目>的输入数据。
4.应用实例
dd命令常常用来制作Linux启动盘。先找一个可引导内核，令它的根设备指向正确的根分区，然后使用dd命令将其写入软盘：
$ rdev vmlinuz /dev/hda
$ dd if＝vmlinuz of＝/dev/fd0
上面代码说明，使用rdev命令将可引导内核vmlinuz中的根设备指向/dev/hda，请把“hda”换成自己的根分区，接下来用dd命令将该内核写入软盘。
## rpm包
RPM包 - （Fedora, Redhat及类似系统）
rpm -ivh package.rpm 安装一个rpm包
rpm -ivh --nodeeps package.rpm 安装一个rpm包而忽略依赖关系警告
rpm -U package.rpm 更新一个rpm包但不改变其配置文件
rpm -F package.rpm 更新一个确定已经安装的rpm包
rpm -e package_name.rpm 删除一个rpm包
rpm -qa 显示系统中所有已经安装的rpm包
rpm -qa | grep httpd 显示所有名称中包含 "httpd" 字样的rpm包
rpm -qi package_name 获取一个已安装包的特殊信息
rpm -qg "System Environment/Daemons" 显示一个组件的rpm包
rpm -ql package_name 显示一个已经安装的rpm包提供的文件列表
rpm -qc package_name 显示一个已经安装的rpm包提供的配置文件列表
rpm -q package_name --whatrequires 显示与一个rpm包存在依赖关系的列表
rpm -q package_name --whatprovides 显示一个rpm包所占的体积
rpm -q package_name --scripts 显示在安装/删除期间所执行的脚本l
rpm -q package_name --changelog 显示一个rpm包的修改历史
rpm -qf /etc/httpd/conf/httpd.conf 确认所给的文件由哪个rpm包所提供
rpm -qp package.rpm -l 显示由一个尚未安装的rpm包提供的文件列表
rpm --import /media/cdrom/RPM-GPG-KEY 导入公钥数字证书
rpm --checksig package.rpm 确认一个rpm包的完整性
rpm -qa gpg-pubkey 确认已安装的所有rpm包的完整性
rpm -V package_name 检查文件尺寸、 许可、类型、所有者、群组、MD5检查以及最后修改时间
rpm -Va 检查系统中所有已安装的rpm包- 小心使用
rpm -Vp package.rpm 确认一个rpm包还未安装
rpm2cpio package.rpm | cpio --extract --make-directories *bin* 从一个rpm包运行可执行文件
rpm -ivh /usr/src/redhat/RPMS/`arch`/package.rpm 从一个rpm源码安装一个构建好的包
rpmbuild --rebuild package_name.src.rpm 从一个rpm源码构建一个 rpm 包

## 常用指令
- 系统管理命令
top                 动态显示当前耗费资源最多进程信息
ps                  显示瞬间进程状态 ps -aux
du                  查看目录大小 du -h /home带有单位显示目录信息
df                  查看磁盘大小 df -h 带有单位显示磁盘信息
alias               对命令重命名 如：alias showmeit="ps -aux" ，另外解除使用unaliax showmeit
kill                 杀死进程，可以先用ps 或 top命令查看进程的id，然后再用kill命令杀死进程。 
- 打包压缩相关命令
gzip：
bzip2：
tar:                打包压缩
     -c              归档文件
     -x              压缩文件
     -z              gzip压缩文件
     -j              bzip2压缩文件
     -v              显示压缩或解压缩过程 v(view)
     -f              使用档名
例：
tar -cvf /home/abc.tar /home/abc              只打包，不压缩
tar -zcvf /home/abc.tar.gz /home/abc        打包，并用gzip压缩
tar -jcvf /home/abc.tar.bz2 /home/abc      打包，并用bzip2压缩
当然，如果想解压缩，就直接替换上面的命令  tar -cvf  / tar -zcvf  / tar -jcvf 中的“c” 换成“x” 就可以了。
 
- 关机/重启机器
shutdown
     -r             关机重启
     -h             关机不重启
     now          立刻关机
halt               关机
reboot          重启 
- Linux管道
将一个命令的标准输出作为另一个命令的标准输入。也就是把几个命令组合起来使用，后一个命令除以前一个命令的结果。
例：grep -r "close" /home/* | more       在home目录下所有文件中查找，包括close的文件，并分页输出。 
- Linux软件包管理
dpkg (Debian Package)管理工具，软件包名以.deb后缀。这种方法适合系统不能联网的情况下。
比如安装tree命令的安装包，先将tree.deb传到Linux系统中。再使用如下命令安装。
sudo dpkg -i tree_1.5.3-1_i386.deb         安装软件
sudo dpkg -r tree                                     卸载软件
注：将tree.deb传到Linux系统中，有多种方式。VMwareTool，使用挂载方式；使用winSCP工具等；
APT（Advanced Packaging Tool）高级软件工具。这种方法适合系统能够连接互联网的情况。
依然以tree为例
sudo apt-get install tree                         安装tree
sudo apt-get remove tree                          卸载tree
sudo apt-get update                               更新软件
sudo apt-get upgrade        

> 将.rpm文件转为.deb文件.rpm为RedHat使用的软件格式。在Ubuntu下不能直接使用，所以需要转换一下。
sudo alien abc.rpm 

- 用户及用户组管理
/etc/passwd    存储用户账号
/etc/group       存储组账号
/etc/shadow    存储用户账号的密码
/etc/gshadow  存储用户组账号的密码
useradd 用户名
userdel 用户名
adduser 用户名
groupadd 组名
groupdel 组名
passwd root     给root设置密码
su root
su - root 
/etc/profile     系统环境变量
bash_profile     用户环境变量
.bashrc          用户环境变量
su user              切换用户，加载配置文件.bashrc
su - user            切换用户，加载配置文件/etc/profile ，加载bash_profile
- 更改文件的用户及用户组
sudo chown [-R] owner[:group] {File|Directory}
例如：还以jdk-7u21-linux-i586.tar.gz为例。属于用户hadoop，组hadoop
要想切换此文件所属的用户及组。可以使用命令。
sudo chown root:root jdk-7u21-linux-i586.tar.gz 
- 文件权限管理
三种基本权限
R          读         数值表示为4
W          写         数值表示为2
X          可执行     数值表示为1
如jdk-7u21-linux-i586.tar.gz文件的权限为-rw-rw-r--
-rw-rw-r--一共十个字符，分成四段。
第一个字符“-”表示普通文件；这个位置还可能会出现“l”链接；“d”表示目录
第二三四个字符“rw-”表示当前所属用户的权限。   所以用数值表示为4+2=6
第五六七个字符“rw-”表示当前所属组的权限。      所以用数值表示为4+2=6
第八九十个字符“r--”表示其他用户权限。              所以用数值表示为2
所以操作此文件的权限用数值表示为662 
- 更改权限
sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名 
例如：有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765
sudo chmod u+x g+w o+r  filename
上面的例子可以用数值表示
sudo chmod 765 filename