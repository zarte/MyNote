##恢复已删除的文件
鼠标右键点击项目名Test，选择Restore from Local history.

##eclipse导入myeclipse的项目报错、中文乱码

myeclipse导出项目：右击项目-》export-》File System-》确定。

eclipse导入项目：右击空白-》import-》Existing projects into workspace.

##关于报错：
1.右击项目-》Build Path-》Config Build path，修改jar所在的路径。

以及添加运行时的jar包：D:\java\tomcat7.63\lib\jsp-api.jar 和 D:\java\tomcat7.63\lib\servlet-api.jar
2.打开widnow>show view>nagitgor>找到你的导入项目>.settings>org
.eclipse.wst.common.componet>找到source-path(注意：这里eclipse默认是webcontent
,而myeclipse是webroot)所以这里改成webRoot,就ok了。

##关于中文乱码
项目右键 properties -》Resource》改变编码为GBK或UTF-8
(和项目原来在myeclipse中的编码格式保持一致即可) eclipse插件安装与卸载
方式一：
1.把plugins中的所有jar拷贝到eclipse的plugins文件夹之中
2. 把features中的所有文件夹拷贝到eclipse的features文件夹之中
3. 重启eclipse，ok


方式二：延伸方法（比较好用）：

##Eclipse插件使用links目录的方法
假设把插件安装在d:\\myplugin\\mybatis-generator目录中，则myplugin
的目录结构一定要是这样的：d:\\myplugin\\mybatis-generator\\eclipse\\plugins\\
插件及 d:\\myplugin\\mybatis-generator\\eclipse\\features\\插件

在eclipse安装目录下，在d:\eclipse目录下的links文件夹中，建立一个
link文件，如mybatis-generator.link，该文件内容为path=d:\\myplugin\\mybatis-
generator。

重新启动eclipse，插件即安装上了。

卸载方法
如果想暂时不启动插件，只需把
mybatis-generator如果想暂时不启动插件，只需把mybatis-generator.link
文件删除即可。

##将Java转换为Html的工具,高亮语法语法不会丢失的工具。
Java2Html 能够的把java源代码转换为高亮有序的HTML, RTF, TeX与 XHTML格式。
一个没有解决的问题:就是
Java2Html转换中文注释后出现乱码情况。


##安装与操作
解压缩java2html_50.zip到一个文件夹java2html_50，在DOS命令行窗口进入此目录下，执行命令：
java -jar java2html.jar如：
E:\soft_assist\javatools\java2html_50>java -jar java2html.jar

分两种转换方式：文件转换与直接转换。

##Eclipse+Maven创建webapp项目

1、开启eclipse，右键new——》other，如下图找到maven project
2、选择maven project，显示创建maven项目的窗口，勾选如图所示，Create a simple project
3、输入maven项目的基本信息，如下图所示：
4、完成maven项目的创建，生成相应的maven项目结果，如下所示，此处有部分结构是项目不需要的，我们需要去掉：

5、选择项目，右键选择Properties，进入属性页面，选择到Maven菜单下，如下图所示：

6、选择java版本为1.7，并去掉其他两项，如下图：
7、点击ok之后，再次回到项目结构，此时项目结构比较清晰，符合我们想要创建的maven项目
8、此时webapp下的结果还没有显示出来，因为此时我们还没有配置此的项目为web项目，再次进去Properties配置，如下图所示：

9、点击Further configuration available...，如下：

10、配置src/main/webapp，并勾选生成web.xml的选项，如下：
11、确定之后，返回到maven菜单下去掉Dynamic Web Module的勾选，点击ok，如下所示，webapp目录结构显示出来了：

12、此时还需要配置，src/main/webapp为“/”项目的根目录，如下所示：

13、完成如上配置后，最后完成maven webapp项目结构如下图所示：

来源： < http://www.cnblogs.com/candle806/p/3439469.html >  

##eclipse theme
http://www.eclipsecolorthemes.org/
