##Session 0x0 for server null, unexpected error, closing socket connection and attempting reconnect
```
原因是没有设置myid,在/conf/zoo.cfg添加 server.1=192.168.1.36:2888:3888即可。


设置好后，重新测试连接 .zkClient.sh 
2016-05-25 15:51:17,495 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@975] - Opening socket connection to server localhost/0:0:0:0:0:0:0:1:2181. Will not attempt to authenticate using SASL (unknown error)
2016-05-25 15:51:17,630 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@852] - Socket connection established to localhost/0:0:0:0:0:0:0:1:2181, initiating session
[zk: localhost:2181(CONNECTING) 0] 2016-05-25 15:51:17,790 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@1235] - Session establishment complete on server localhost/0:0:0:0:0:0:0:1:2181, sessionid = 0x154e6e527940000, negotiated timeout = 30000


```