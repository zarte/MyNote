##eclipse maven war包
```
配置 pom.xml文件，在overview视窗里 配置 packaging为 war 然后
然后点击 pom.xml右键，run  as 选择 install 或是 package
如果项目没问题，配置没问题
就会在项目的target 的目录里生成 war文件.
```
##打不同环境（test）的包
mvn clean package -Dmaven.test.skip=true -Ptest
