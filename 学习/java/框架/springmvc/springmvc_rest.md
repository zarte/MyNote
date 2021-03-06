[TOC]
## Spring MVC Rest
```
第一步：配置Spring MVC 核心Servlet
<!-- spring mvc --><listener><!--  request、session 和  global session web作用域 --><listener-class>
            org.springframework.web.context.request.RequestContextListener
            </listener-class></listener><context-param><param-name>contextConfigLocation</param-name><param-value>
            classpath:applicationContext.xml
        </param-value></context-param><servlet><servlet-name>dispatcher</servlet-name><!-- Spring mvc 核心分发器 --><servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class><init-param><param-name>contextConfigLocation</param-name><param-value>classpath:spring-servlet.xml</param-value></init-param><load-on-startup>1</load-on-startup></servlet><servlet-mapping><servlet-name>dispatcher</servlet-name><!-- 拦截所有请求，静态资源会在spring-servlet中处理 --><url-pattern>/</url-pattern></servlet-mapping><filter><!-- utf-8 编码处理 --><filter-name>Character Encoding</filter-name><filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class><init-param><param-name>encoding</param-name><param-value>UTF-8</param-value></init-param></filter><filter-mapping><filter-name>Character Encoding</filter-name><url-pattern>/</url-pattern></filter-mapping>
第二步：配置spring-servlet.xml
<?xml version="1.0" encoding="UTF-8"?><beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xmlns:context="http://www.springframework.org/schema/context"xmlns:mvc="http://www.springframework.org/schema/mvc"xsi:schemaLocation="http://www.springframework.org/schema/beans 
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context.xsd 
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc.xsd 
        http://www.springframework.org/schema/util 
        http://www.springframework.org/schema/util/spring-util.xsd"><!-- 把标记了@Controller注解的类转换为bean --><context:component-scan base-package="com.hnust.controller" /><!-- 开启MVC注解功能 ，为了使Controller中的参数注解起效，需要如下配置 --><mvc:annotation-driven/><!-- 静态资源获取，不用后台映射 --><mvc:resources mapping="/resource/**" location="/resource/"/><mvc:interceptors><mvc:interceptor><mvc:mapping path="/**"/><!-- 定义在mvc:interceptor下面的表示是对特定的请求才进行拦截的 --><bean class="com.hnust.interceptor.TranslateInterceptor"/></mvc:interceptor></mvc:interceptors><!-- 主要是进行Controller 和 URL 的一些注解绑定，这里可以进行转换器配置:只有配置好了转换器才能进行类与JSON和XML的转换，当然只是针对基于转换器协商资源表述 --><bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter" /><!-- XML 与 Java 数据转换  --><bean id="jaxbMarshaller" class="org.springframework.oxm.jaxb.Jaxb2Marshaller"><property name="classesToBeBound"><list><!--common XML 映射  JavaBean 注册  --><value>com.hnust.bean.Resource</value></list></property></bean><!-- 基于视图渲染进行协商资源表述  --><bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver"><!-- restful 是否采用扩展名的方式确定内容格式，id.json 返回JSON格式 --><property name="favorPathExtension" value="true"></property><!-- restful 是否采用参数支持确定内容格式，id?format=json 返回JSON格式 --><property name="favorParameter" value="true"></property><!-- restful 是否忽略掉accept header，Accept:application/json --><property name="ignoreAcceptHeader" value="false"></property><!-- 基于视图按顺序解析  --><property name="order" value="1" /><!-- 对采用扩展名，参数新式的 URL 进行获取对应的 accept  --><property name="mediaTypes"><map><entry key="json" value="application/json"/><entry key="xml" value="application/xml"/><entry key="html" value="text/html"/></map></property><!-- 如果扩展名，参数甚至header 信息都没有找到对应的accept时  --><property name="defaultContentType" value="text/html"/><!-- 采用对应的视图进行渲染  --><property name="defaultViews"><list ><!-- 转换Java对象为XML格式数据 --><bean class="org.springframework.web.servlet.view.xml.MarshallingView"><constructor-arg ref="jaxbMarshaller" /></bean><!-- 转换Java对象为JSON 格式数据 --><bean class="org.springframework.web.servlet.view.json.MappingJacksonJsonView"/></list></property><!-- 采用对应的视图进行渲染  --><property name="viewResolvers"><list ><!-- 查找在上下文中定义了ID的Bean，并且定位该ID  --><bean class="org.springframework.web.servlet.view.BeanNameViewResolver"/><!-- 对Controller中返回的视图实例进行解析，并且组装URL定位到对应的资源  --><bean id="viewResolver" class="org.springframework.web.servlet.view.UrlBasedViewResolver"><property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/><property name="prefix" value="/WEB-INF/jsp/"/><property name="suffix" value=".jsp"/></bean></list></property></bean></beans>
第三步：编写controller
/**
 *
 *@author：Heweipo
 *@version 1.00
 *
 */@Controller@RequestMapping("/resource")
public class ResourceController {
    @Autowired
    private ResourceService service;
    @RequestMapping(value="/get/{id}" , method=RequestMethod.GET)
    public String get(@PathVariable("id") String id , ModelMap model){
        model.put("resource", service.getResource(id));
        return "resource";
    }
}
第四步：页面请求，获取 Json xml html 三种格式的数据
浏览器请求URL： 
浏览器响应结果：{"resource":{"id":"id_111","name":"maven","number":11}}
浏览器响应结果：
<resource><id>id_111</id><name>maven</name><number>11</number></resource>
```