##conf目录没有
hadoop 2 版本相关配置文件都在 $HADOOP_HOME/etc/hadoop/ 下面
## 上传hadoop，配置hadoop
```

1.修改环境变量，将hadoop加进去（最后四个linux都操作一次）
vim ~/.bashrc
export HADOOP_HOME = /usr/local/hadoop
export PATH = $JAVA_HOme/bin:$HADOOP_HOME/bin:$PATH
修改完后，用source ~/.bashrc让配置文件生效。


2.修改/usr/local/hadoop/conf下配置文件 hadoop-env.sh，

# The java implementation to use.
export JAVA_HOME=${JAVA_HOME}
export HADOOP_HOME=${HADOOP_HOME}
export PATH=$HADOOP_HOME/bin:$PATH


3.修改 core-site.xml
<configuration>
        <property>
                <name>fs.default.name</name>
                <value>hdfs://master:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/usr/local/hadoop/tmp</value>
        </property>
</configuration>


4.修改hdfs-site.xml
<configuration>
        <property>      
                <name>dfs.replication</name>
                <value>2</value>
        </property>
        <property>      
                <name>heartbeat.recheckinterval</name>
                <value>10</value>
        </property>
        <property>      
                <name>dfs.name.dir</name>
                <value>/usr/local/hadoop/hdfs/name</value>
        </property>
        <property>
                <name>dfs.data.dir</name>
                <value>/usr/local/hadoop/hdfs/data</value>
        </property>
</configuration>


5.修改mapred-site.xml
<configuration>

        <property>
                <name>mapred.job.tracker</name>
                <value>master:9001</value>
        </property>
</configuration>


```
##建立文件夹
```
在/usr/local/hadoop下建立 conf 将

[root@master hadoop]# mv hadoop-env.sh core-site.xml hdfs-site.xml mapred-site.xml /usr/local/hadoop/conf/

[root@master hadoop]# vi masters

master
[root@master hadoop]# vi slaves

host1
host2
host3
```
##新建用户
```


新建用户同时增加工作组
useradd -g test phpq //新建phpq用户并增加到test工作组
注：：-g 所属组 -d 家目录 -s 所用的SHELL
```

