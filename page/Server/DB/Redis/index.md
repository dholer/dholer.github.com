---
layout: page 
title: Redis
---

Redis is an open source, advanced key-value store. It is often referred to as a **data structure server** since keys can contain 
strings, hashes, lists, sets and sorted sets.


- [download](http://redis.io/download)
- [Redis 命令参考](http://redis.readthedocs.org/en/latest/)
- [redis-cli 命令总结](http://slj.me/2011/04/redis-cli-commands/)


###linux install

	[root@localhost download]# wget http://redis.googlecode.com/files/redis-2.4.10.tar.gz
	
安装命令如下

	[root@localhost download]# tar zxvf redis-2.4.10.tar.gz
	[root@localhost services]# cd redis-2.4.10/
	[root@localhost redis-2.4.10]# make 

直接make就行
make命令执行完成后,会再src目录下生成本个可执行文件，分别是

	redis-server、
	redis-cli、
	redis-benchmark、
	redis-stat，
	
它们的作用如下：

- redis-server：Redis服务器的daemon启动程序
- redis-cli：Redis命令行操作工具。当然，你也可以用telnet根据其纯文本协议来操作
- redis-benchmark：Redis性能测试工具，测试Redis在你的系统及你的配置下的读写性能
- redis-stat：Redis状态检测工具，可以检测Redis当前状态参数及延迟状况 


	[root@localhost redis-2.4.10]# vim redis.conf

当前目录redis.conf是redis的配置文件

daemonize no 将no改为yes后台运行

保存退出

	[root@localhost redis-2.4.10]# cd src/

启动脚本redis-server就在src目下

	[root@localhost src]# mv redis-2.4.10 /usr/local/redis
	[root@localhost src]# /usr/local/redis/src/redis-server /usr/local/redis/redis.conf

	echo "/usr/local/redis/src/redis-server /usr/local/redis/redis.conf" >> /etc/rc.local

	[root@localhost src]# ps aux | grep redis
	root      4833  0.0  0.0  39672  1400 ?        Ssl  19:39   0:00 ./redis-server ../redis.conf
	root      4838  0.0  0.0  61148   776 pts/0    R+   19:39   0:00 grep redis

./redis-cli是测试客户端脚本（执行这个脚本就可以和redis交互了）

	[root@localhost src]# /usr/local/redis/src/redis-cli 
	redis 127.0.0.1:6379> set a b
	OK
	redis 127.0.0.1:6379> get a
	"b"

	[root@localhost src]# ./redis-cli shutdown
