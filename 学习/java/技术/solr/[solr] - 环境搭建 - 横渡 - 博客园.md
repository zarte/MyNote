
这里忽略java安装和tomcat安装，这里使用的是solr-4.10.0

 

1、到apache下载solr，地址：

http://mirrors.hust.edu.cn/apache/lucene/solr/

 

2、解压出solr-4.10.0

 

3、复制solr-4.10.0\example\webapps中的solr.war文件到tomcat安装目录中的webapps文件夹下

 

4、运行tomcat。（忽略怎么运行tomcat），tomcat会自动解压solr.war文件。

 

5、删除solr.war文件。（不然每次启动tomcat都会发布一次）

 

6、回到tomcat的webapps目录下，记事本打开solr\WEB-INF\web.xml文件。

加入如下代码：在<web-app />节点内的最后。

<
env-entry
>
 
   
<
env-entry-name
>
solr/home
</
env-entry-name
>
 
   
<
env-entry-value
>
E:\solrhome
</
env-entry-value
>
 
   
<
env-entry-type
>
java.lang.String
</
env-entry-type
>
 

</
env-entry
>

如上代码，需要在E盘新建一个文件夹：solrhome

 

7、回到解压的solr-4.10.0目录，打开文件夹：solr-4.10.0\example\solr，复制所有内容到E:\solrhome

 

8、打开文件夹：solr-4.10.0\example\lib\ext，复制所有jar包到tomcat的webapps\solr\WEB-INF\lib下。

 

9、运行web：http://localhost:8899/solr，将看到如下画面：



 

10、在E:\solrhome目录下，新建一个mycore文件夹。

 

11、在解压的solr-4.10.0\example\multicore目录中，复制core0文件夹到E:\solrhome\mycore中。

 

12、在E:\solrhome中新建一个文件夹：mydocs

 

13、复制解压的solr-4.10.0\example\exampledocs下的post.jar到E:\solrhome\mydocs中

 

14、复制解压的solr-4.10.0\example\multicore\exampledocs下的ipod_other.xml文件到E:\solrhome\mydocs中

 

15、在solr web page中新建core:



 

16、重启tomcat。（如何重启，忽略）

 

17、打开CMD，运行下面语句：（怎么在命令行下运行java就不说了）

java -Durl=http://localhost:8899/solr/mycore/update -Ddata=files -jar post.jar ipod_other.xml



 

18、在solr web中选择core：



 

19、查询测试：



 

20、也可以直接使用URL查询：

http:
//
localhost:8899/solr/mycore/select?q=name%3AB*&wt=json&indent=true&_=1410949535746

返回JSON：

{
  
"responseHeader"
:{
    
"status":0
,
    
"QTime":0
},
  
"response":{"numFound":1,"start":0,"docs"
:[
      {
        
"id":"F8V7067-APL-KIT"
,
        
"name":"Belkin Mobile Power Cord for iPod w/ Dock"
,
        
"_version_":1479481822989516800
}]
  }}


 