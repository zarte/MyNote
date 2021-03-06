## Freemarker自定义指令
```


在项目应用中，遇到这样一个问题，当文本过长时，需要将前面的文本省略一部分，用…代替，而使用css只能在文本最后加…

 



我们可以通过freemarker自定义指令的方式实现上述功能。

freemarker自定义指令需要继承TemplateDirectiveModel接口，

package com.nexusy.freemarker.directive;  

   

import java.io.IOException;  

import java.util.Map;  

   

import freemarker.core.Environment;  

import freemarker.template.SimpleScalar;  

import freemarker.template.TemplateDirectiveBody;  

import freemarker.template.TemplateDirectiveModel;  

import freemarker.template.TemplateException;  

import freemarker.template.TemplateModel;  

   

public class EllipsisDirective implements TemplateDirectiveModel {  

   

    @Override  

    public void execute(Environment env, @SuppressWarnings("rawtypes") Map params, TemplateModel[] loopVars, TemplateDirectiveBody body)  

            throws TemplateException, IOException {  

        String text = "";  

        int length = 0;  

        if(params.get("text") != null){  

            text = ((SimpleScalar) params.get("text")).getAsString();  

        }  

        if(params.get("length") != null){  

            length = Integer.valueOf(((SimpleScalar) params.get("length")).getAsString());  

        }  

        if(length < text.length()){  

            text = "..." + text.substring(text.length() - length);  

        }  

        env.getOut().write(text);  

    }  

}  


 

 然后在springmvc配置文件中配置该指令

 <bean id="ellipsis" class="com.nexusy.freemarker.directive.EllipsisDirective" />  

<bean id="freemarkerConfig"  

          class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">  

    <property name="templateLoaderPath" value="/" />  

    <property name="freemarkerSettings">  

        <props>  

            <prop key="datetime_format">yyyy-MM-dd</prop>  

            <prop key="number_format">0.##</prop>  

            <prop key="url_escaping_charset">UTF-8</prop>  

            <prop key="output_encoding">UTF-8</prop>  

            <prop key="template_exception_handler">ignore</prop>  

        </props>  

    </property>  

    <property name="freemarkerVariables">  

        <map>  

            <entry key="xml_escape" value-ref="fmXmlEscape" />  

            <entry key="ellipsis" value-ref="ellipsis" />  

        </map>  

    </property>  

    <property name="defaultEncoding" value="UTF-8" />  

</bean>

  

在模版中使用 

 html代码

< @ellipsis  text = "1234567"   length = "6" > </ @ellipsis >   

 



 来源：  http://lanhuidong.iteye.com/blog/1762572

```