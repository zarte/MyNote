
一.创建项目

1.Eclipse中用Maven创建项目



上图中Next

 

2.继续Next



 

3.选maven-archetype-webapp后，next



 

4.填写相应的信息，Packaged是默认创建一个包，不写也可以



 

5.创建好项目后，目录如下：



至此，项目已经创建完毕，下边可是配置。

二.项目配置

1.添加 Source Folder

Maven规定，必须创建以下几个Source Folder

src/main/resources

src/main/java

src/test/resources

src/test/java

添加以上的Source Folder





创建好后的目录如下：



2.配置 Build Path



 

3.设定4个文件夹的输出Output folder，双击修改



分别修改输出路径为

src/main/resources　　对应　　target/classes

src/main/java　　对应　　target/classes

src/test/resources　　对应　　target/test-classes

src/test/java　　对应　　target/test-classes

4.修改后如下图



 

5.设定Libraries





 

6.配置完Build Path后目录如下：



7.将项目转换成Dynamic Web Project

在项目上右键Properties

在左侧选择 Project Facets,单击右侧的 ”Convert faceted from “



 

8.修改Java为你当前项目的JDK，并添加Dynamic Web Module ，最后单击”Further Configuration available“ 链接：



 

9.修改Content directory 为 src/main/webapp ,单击OK:



 

10.设置完Content directory，ok后再次点击前一界面ok，完成转换成Dynamic Web Project项目



 

11.设置部署程序集(Web Deployment Assembly)

在项目上右键单击，选择Properties，在左侧选择Deployment Assembly



 

12.设置部署时的文件发布路径

　　1，我们删除test的两项，因为test是测试使用，并不需要部署。
　　2，设置将Maven的jar包发布到lib下。
　　　　Add -> Java Build Path Entries -> Maven Dependencies -> Finish

设置完成后如图



 

ok后，web项目就创建完毕了，目录机构如图



运行后访问工程成功！



下一章将测试一个servlet

 

 

友情赞助

如果您喜欢此文，感觉对您工作有帮助，预期领导会给您涨工资，不妨小额赞助一下，让我有动力继续努力。

赞助方式： 打开支付宝App，使用“扫一扫”付款，付款码见下，别忘了付款留言哦！



 

 