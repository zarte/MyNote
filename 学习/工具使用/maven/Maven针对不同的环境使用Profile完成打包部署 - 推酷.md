
一、前言：

        一个项目里总会有很多配置文件。而且一般都会有多套环境。开发的、测试的、正式的。而在这些不同的环境这些配置的值都会不一样。比如mail的配置、服务的url配置这些都是很常见的。所以在打包的时候就要根据environment来选不同的值或者配置文件。

      解决办法：

1. 不同的环境建立不同的配置文件目录。在打包的时候用对应的文件目录下的配置文件。

      以前用ant打包的时候处理就比较方便。打包前copy一下对应目录下的配置文件覆盖target下的那些文件，然后再打包就可以了。在刚开始用maven的时候就想要怎么解决，一直没有在Maven中找到ant的这种替代方式。其实主要是按ant这种处理方法去思考了，所以就只去想没有有copy这种target。

2. 其实在maven中用profile可以更简单的解决。

二、使用Maven的profile功能

1.配置pom.xml: 在pom.xml中配置dev, qa, prod三个配置文件为资源源文件，代表着3种环境下的配置文件。观看下面的pom.xml文件

            1） 在resource下指定 <directory> src/main/resources </directory> 和<filtering>true</filtering>，<include>*.*</include>，意思是对src/main/resources目录下的所有文件（本例子中就是上面你看到的settings.properties文件）进行占位符替换。即用dev.properties或者是qa.properties, prod.properties的内容来替换settings.properties中的占位符。换句话说，就是我们在项目中实际上用的资源文件是settings.properties,而这个文件的内容有时是qa.properties,有时是dev.properties,有时是prod.properties。

             2） <activation> ... </activation> 表示的是可以用这样的命令来触发profile，比如用

mvn clean install -DskipTests -Denv=qa, 表示出发qa的profile，也就是会用qa.properties文件来替换settings.properties.

            3) ... <activeByDefault>true</activeByDefault> ... 表示dev是默认的profile

即 mvn clean install -DskipTests -Denv=dev 等价于 mvn clean install -DskipTests

<profiles>
      <profile>
          <id>dev</id>
          <build>
              <filters>
                  <filter>src/main/resources/filters/dev.properties</filter>
              </filters>
              <resources>
                  <resource>
                      <directory>src/main/resources</directory>
                      <filtering>true</filtering>
                     
                      <!-- optional -->
                      <includes>
                          <include>*.*</include>
                      </includes>
                     
                  </resource>
              </resources>
          </build>
         
          <activation>
              <activeByDefault>true</activeByDefault>
              <property>
                  <name>env</name>
                  <value>dev</value>
              </property>
          </activation>
      </profile>
     
      <profile>
          <id>qa</id>
          <build>
              <filters>
                  <filter>src/main/resources/filters/qa.properties</filter>
              </filters>
              <resources>
                  <resource>
                      <directory>src/main/resources</directory>
                      <filtering>true</filtering>
                     
                      <!-- optional -->
                      <includes>
                          <include>*.*</include>
                      </includes>
                     
                  </resource>
              </resources>
          </build>
         
          <activation>
              <property>
                  <name>env</name>
                  <value>qa</value>
              </property>
          </activation>
      </profile>
     
      <profile>
          <id>prod</id>
          <build>
              <filters>
                  <filter>src/main/resources/filters/prod.properties</filter>
              </filters>
              <resources>
                  <resource>
                      <directory>src/main/resources</directory>
                      <filtering>true</filtering>
                     
                      <!-- optional -->
                      <includes>
                          <include>*.*</include>
                      </includes>
                     
                  </resource>
              </resources>
          </build>
         
          <activation>
              <property>
                  <name>env</name>
                  <value>prod</value>
              </property>
          </activation>
      </profile>
     
  </profiles>

 2. 配置qa.properties, dev.properties, prod.properties.以及settings.properties.

1) settings.properteis:

CMSDB_ENDPOINT=${CMSDB_ENDPOINT}

2) qa.properties:

CMSDB_ENDPOINT=http://phx5qa01c-5fca.stratus.phx.qa.xx.com:8080/cms

3) dev.properties:

CMSDB_ENDPOINT=http://localhost:8080/cms

4) prod.properties:

CMSDB_ENDPOINT=http://cms.vip.stratus.xx.com/cms

3. 使用下面的命令，然后查看settings.xml的内容。

mvn clean install -DskipTests

mvn clean install -DskipTests -Denv=qa

mvn clean install -DskipTests -Denv=prod

你会发现settings中的内容会根据你所执行命令的不同而不同，这正是我们所需要的。