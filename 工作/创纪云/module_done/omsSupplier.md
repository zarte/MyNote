##首页面的生成
1.引入页面原型，supplier及RES资源包，freemarker/common/common.ftl
2.配置web.xml，对.htm和.do的请求拦截，所以配置欢迎页面index.htm
```
<welcome-file-list>
        <welcome-file>index.htm</welcome-file>     </welcome-file-list>  

```
```
<servlet>
        <servlet-name>action</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:conf/spring/spring-dubbo.xml;classpath:conf/spring/spring-servlet.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>action</servlet-name>
        <url-pattern>*.htm</url-pattern>
        <url-pattern>*.do</url-pattern>     </servlet-mapping> 
```
3.写Controller


##web项目启动发现找不到 web 所依赖的 service中的xml文件

原因：是maven更改的jar包，`要选中的工程Maven update 后，再 重新编译（Project - clean）;`
##页面中没有引入样式成功。
1.原因：发现引入路径没有解析，在配置文件中写入


添加resRoot即可。
2.base配置错误：base=$[app-root]改为base=${app-root}


配置app-root:




##使用ajax异步加载实现无刷新删除


一、目前的效果是点击删除后，数据删除成功，但是却跳转到了首页。
删除运费模板的链接，在js文件中：
```
<a href='${base}/supplier/delTemplateById.do?id=" +data.row.templateId +  "'>删除</a>
```
删除分区域模板的链接：
```
<a href="${base}/supplier/deleteAreaTemplateById.do?id=${tempList.atId }">删除</a>

```
二、使用ajax后


ftl文件，删除运费模板的链接：



删除分区域模板的链接：

备注：如果没有加@ResponseBody注解，会报错：





##关于@ResponseBody注解：


##freemarker渲染radio





##freemarker渲染checkbox




##实现查看









###实现新增、修改保存
新增、修改均为同一个页面，


如果是新增，为了页面不报错，也要添加一个空的addressDto.

修改的话，查询出来此对象，填充到页面。


页面只需要添加value属性即可实现回显数据





