---
layout: post
category : MySQL
tags : [mysql,mysql install, windows mysql ,win7 mysql]
title: win7 下安装 mysql 解压版
---
{% include JB/setup %}

###下载MySQL Windows安装包

MySQL Windows安装包说明： 
- mysql-*.*.**-win32.msi Windows 安装包，图形化的下一步下一步的安装。 
- mysql-*.*.** .zip 这个是windows源文件，需要编译，对应的Linux源文件是mysql-5.5.20.tar.gz。
- mysql-*.*.** -win32.zip 这个文件解包后即可使用，是编译好的windows32位Mysql。

到官网下载最新版mysql- *.*.** -win32.zip，然后将mysql解压到任意路径，如：`D:\Dholer\local\MySQL\bin` 将 `D:\Dholer\local\MySQL\bin`添加到系统环境变量

###修改my.ini
在根目录下面有几个已经写好的`my-`开头的ini文件，选一个适合你的，如：`my-small.ini`。复制一份，将文件名修改为`my.ini`，添加以下内容： 

	[mysqld]  
	#设置字符集为utf8  
	default-character-set = utf8  
	basedir = D:/Dholer/local/MySQL/bin
	datadir = D:/Dholer/Data/mysql5.1
	  
	[client]  
	#设置客户端字符集  
	default-character-set = utf8	
	mysqld --install --defaults-file=D:\Dholer\local\MySQL\my.ini


###将mysql安装到windows的服务

	$ mysqld -install
	$ Service successfully installed. 

如果想要卸载服务执行命令：

	$ mysqld -remove

or:	

	$ sc delete mysql   

###启动mysql

	$ net start mysql
	
停止服务输入命令

	$ net stop mysql

如果想设置mysql是否自动启动，可以在` 开始菜单 ` -> ` 运行 ` 中输入` service.msc ` 打开服务管理进行设置。 

第一次登录的时候输入： 

	mysql -h hostname -u username -p   
	mysql -h hostname -u username -p   

命令说明：*mysql命令将调用MySQL监视程序，这是一个可以将我们连接到MySQL服务器端的客户端命令行工具*
选项说明： 

- -h选项：用于指定所希望连接的主机，即运行MySQL服务器的机器。如果在运行MySQL服务器的机器上运行该命令，则可以忽略该选项和hostname参数；如果不是，必须用运行MySQL服务器的主机名称来代替主机名称参数。 
- -u命令：用于指定连接数据库时使用的用户名称。 
- -p命令：用于指定用户输入的密码 

此时我本机安装了MYSQL，可忽略该选项和hostname参数：  

	$ mysql -u root 

修改密码： 

	mysql> update mysql.user set password=PASSWORD('200898') where User='root' 
	mysql> flush privileges 

修改字符集：

	mysql> show variables like '%char%'; 
	
	+--------------------------+---------------------------------------+
	| Variable_name            | Value                                 |
	+--------------------------+---------------------------------------+
	| character_set_client     | utf8                                  |
	| character_set_connection | utf8                                  |
	| character_set_database   | latin1                                |
	| character_set_filesystem | binary                                |
	| character_set_results    | utf8                                  |
	| character_set_server     | utf8                                  |
	| character_set_system     | utf8                                  |
	| character_sets_dir       | D:\Dholer\local\MySQL\share\charsets\ |
	+--------------------------+---------------------------------------+
	8 rows in set (0.00 sec)



##精简MYSQL

- 删除所有的目录，只保留 `data` `share` `bin` 三个文件夹

- 删除`BIN`下面除以下三个文件之外的所有文件：

	`libmysql.dll`(MYSQL5中的文件，在MYSQL5.5中不存在)、
	`mysqladmin.exe`、
	`mysqld.exe`、
	`mysql.exe`

- 删除`Share`目录下除以下目录外的所有目录

	`charsets`、
	`english`
	

- 删除`Data`目录下的除`mysql`之外的所有文件和目录
