---
layout: page 
title: MangoDB
---


[MongoDB](http://www.mongodb.org) (from "humongous") is a scalable, high-performance, open source NoSQL database. Written in C++

- [Download](http://www.mongodb.org/downloads)



###YUM 安装

	vi /etc/yum.repos.d/10gen.repo
	
64-bit

	[10gen]
	name=10gen Repository
	baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64
	gpgcheck=0

32-bit

	[10gen]
	name=10gen Repository
	baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/i686
	gpgcheck=0


注：如果已经安装过mongodb，需要升级到新版本，最好先删除已安装的"mongo-stable", "mongo-stable-server", "mongo-unstable" 或 "mongo-unstable-server" 安装包，然后再安装新版本。

OK，下面就可以直接安装啦：

	yum install mongo-10gen mongo-10gen-server
	
安装成功后，可以通过脚本来控制启动/停止/重新启动

	/etc/init.d/mongodb start/stop/restart
	
###GIT安装

	yum -y install git-core scons gcc-c++ glibc-devel	
	git clone git://github.com/mongodb/mongo.git	
	cd mongo
	git tag -l
	git checkout r2.0.4	
	#yum -y install boost*	
	yum -y install boost-devel boost-devel-static	
	scons all
	scons --prefix=/usr/local/mongo  install
	
###源码安装

	curl http://fastdl.mongodb.org/linux/mongodb-linux-i686-2.0.4.tgz > mongo.tgz
	tar zxf mongo.tgz 
	mv mongodb-linux-i686-2.0.4 /usr/local/mogodb

	mkdir -p /data/db  #新建mongodb数据文件存放目录
	mkdir -p /data/logs #新建log文件存放目录

	cd /usr/local/mongodb/bin
	vi mongodb.conf
	
修改为：

	dbpath = /data/db #数据文件存放目录
	logpath = /data/logs/mongodb.log #日志文件存放目录
	port = 27017  #端口
	fork = true  #以守护程序的方式启用，即在后台运行
	
启动

	/usr/local/mongodb/bin/mongod --config /usr/local/mongodb/bin/mongodb.conf

- 关闭Http访问端口，mongodb安装完之后，默认是启用了Http的访问端口，比mongodb监听的端口大1000，即28017。
- 通过访问http://gevin.me:28017/可以查看到mongodb启动的一些信息，同时也对mongodb运行的统计情况进行监控。在使用mongodb过程中，我们可以使用参数将该功能禁用掉。
- 修改配置文件mongodb.conf，增加参数选项：nohttpinterface = true 即可。
- 如果处于连接状态，可以直接通过在admin库中发送db.shutdownServer()指令停止MongoDB实例，如下面的代码所示：

//用shutdownServer()停止服务  

	use admin
	db.shutdownServer()  
	
	ps aux|grep mongod  
	kill -2 19269 
	
> 注意　不要用kill -9 pid来杀死MongoDB进程，这样可能会导致MongoDB的数据损坏。


