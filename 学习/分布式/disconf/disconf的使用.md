##disconf基础
```
1.rageon-pom工程pom文件：
#uniconfig
uniconfig.confServerHost= 192.168.1.28:8080/disconf-web
uniconfig.app= hh
uniconfig.version= 0.0.1 uniconfig.env= rd  
 2.rageon-web工程/mmcApp-web/src/main/resources/conf/main-setting-web.properties

#\u914D\u7F6E\u6587\u4EF6\u670D\u52A1\u5668 HOST
conf_server_host= ${uniconfig.confServerHost}
#app
app= ${uniconfig.app}
#\u7248\u672C
version= ${uniconfig.version}
#\u90E8\u7F72\u73AF\u5883
env= ${uniconfig.env}
enable.remote.conf= true
debug= false
ignore=
# \u83B7\u53D6\u8FDC\u7A0B\u914D\u7F6E \u91CD\u8BD5\u6B21\u6570
conf_server_url_retry_times= 1
#\u83B7\u53D6\u8FDC\u7A0B\u914D\u7F6E \u91CD\u8BD5\u65F6\u4F11\u7720\u65F6\u95F4 conf_server_url_retry_sleep_seconds= 1   

3.rageon-service工程/rageon-service/src/main/resources/conf/spring/spring-impl.xml
<!-- 统一配置 -->
     < bean   id = "disconfMgrBean"   class = "com.bn.disconf.client.DisconfMgrBean"
         destroy-method = "destroy" >
         <!-- 属性支持扫描多包，逗号分隔 -->
         < property   name = "scanPackage"   value = "com.adanac.tool"   />
         < property   name = "configLocation"   value = "conf/main-setting-web.properties"   />
     </ bean >
    
     < bean   id = "disconfMgrBean2"   class = "com.bn.disconf.client.DisconfMgrBeanSecond"
         init-method = "init"   destroy-method = "destroy" >
     </ bean >
    
     < bean   id = "configproperties_no_reloadable_disconf"
         class = "com.bn.disconf.client.addons.properties.ReloadablePropertiesFactoryBean" >
         < property   name = "locations" >
             < list >
                 < value > mmcApp_dubbo.properties </ value >
             </ list >
         </ property >
     </ bean >
    
     < bean   id = "propertyConfigurerForProject1"
         class = "org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" >
         < property   name = "ignoreResourceNotFound"   value = "true"   />
         < property   name = "ignoreUnresolvablePlaceholders"   value = "true"   />
         < property   name = "propertiesArray" >
             < list >
                 < ref   bean = "configproperties_no_reloadable_disconf"   />
             </ list >
         </ property >      </ bean >  
4.使用
rageon-service/com.adanac.tool.rageon.service.common.RageonUniConfig
/**
 * 统一配置服务
 */
@Service
public   class  RageonUniConfig {
     public  MyLogger  log  = MyLoggerFactory. getLogger (RageonUniConfig. class );
     private  String  secretKey ; // app server secrect key
     private  String  solrUrl ; // solr url
     private  String  companyCode ; // company code
     @DisconfItem (key =  "mmcApp_secretKey" )
     public  String getSecretKey() {
         return   secretKey ;
    }
     public   void  setSecretKey(String  secretKey ) {
         log .info( "secretKey updated: old={}, new={}." ,  this . secretKey ,  secretKey );
         this . secretKey  =  secretKey ;
    }
     @DisconfItem (key =  "mmcApp_solrUrl" )
     public  String getSolrUrl() {
         return   solrUrl ;
    }
     public   void  setSolrUrl(String  solrUrl ) {
         log .info( "solrUrl updated: old={}, new={}." ,  this . solrUrl ,  solrUrl );
         this . solrUrl  =  solrUrl ;
    }
     @DisconfItem (key =  "mmcApp_companyCode" )
     public  String getCompanyCode() {
         return   companyCode ;
    }
     public   void  setCompanyCode(String  companyCode ) {
         log .info( "companyCode updated: old={}, new={}." ,  this . companyCode ,  companyCode );
         this . companyCode  =  companyCode ;
    }
}  

```