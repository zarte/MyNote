[TOC]
##执行 mvn deploy时报错：java.lang.OutOfMemoryError: Java heap space
解决方法：
执行deploy时指定jre的参数
-Xm s 128M -Xm x 512M
