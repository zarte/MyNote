mmc-service.impl
```
<bean id="disconfMgrBean" class="com.bn.disconf.client.DisconfMgrBean"
        destroy-method="destroy">
        <!-- 属性支持扫描多包，逗号分隔 -->
        <property name="scanPackage" value="com.bn.o2o" />
        <property name="enableRemoteConf" value="true" />
        <!-- 服务器地址 -->
        <property name="confServerHost" value="${uniconfig.confServerHost}" />
        <!--应用名称 -->
        <property name="app" value="${uniconfig.app}" />
        <!-- 版本 -->
        <property name="version" value="${uniconfig.version}" />
        <!-- 环境 -->
        <property name="env" value="${uniconfig.env}" />
        <!-- 不启用debug模式 -->
        <property name="debug" value="false" />
        <property name="ignore" value="" />
        <property name="confServerUrlRetryTimes" value="1" />
        <property name="confServerUrlRetrySleepSeconds" value="1" />
    </bean>
    
    <bean id="disconfMgrBean2" class="com.bn.disconf.client.DisconfMgrBeanSecond"
        init-method="init" destroy-method="destroy">
    </bean>
    
    <bean id="configproperties_no_reloadable_disconf"
        class="com.bn.disconf.client.addons.properties.ReloadablePropertiesFactoryBean">
        <property name="locations">
            <list>
            </list>
        </property>
    </bean>
    
    <bean id="propertyConfigurerForProject1"
        class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
        <property name="ignoreResourceNotFound" value="true" />
        <property name="ignoreUnresolvablePlaceholders" value="true" />
        <property name="propertiesArray">
            <list>
                <ref bean="configproperties_no_reloadable_disconf" />
            </list>
        </property>     </bean>   

```