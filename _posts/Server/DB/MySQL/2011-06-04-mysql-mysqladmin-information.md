---
layout: post
category : MySQL
tags : [mysql,mysqladmin]
title: mysqladmin 详解
---
{% include JB/setup %}


###mysqladmin 使用格式：

	mysqladmin [option] command [command option] command ......

**option 选项：**

- -c  number 自动运行次数统计，必须和 -i 一起使用
- -i   number 间隔多长时间重复执行

每个两秒查看一次服务器的状态，总共重复5次。

	$ mysqladmin -uroot -p -i 2 -c 5 status
	
- -#, --debug\[=name\]  Output debug log. Often this is 'd:t:o,filename'.
- -f, --force         Don't ask for confirmation on drop database; with
                    multiple commands, continue even if an error occurs. 禁用错误，drop 数据库时不提示，执行多条命令时出错继续执行
- -C, --compress      Use compression in server/client protocol.
- --character-sets-dir=name  字符集所在目录
                    Directory where character sets are.
- --default-character-set=name 设置默认字符集
                    Set the default character set.
- -?, --help          Display this help and exit. 显示帮助 

- -h, --host=name     Connect to host.  连接的主机名或iP
- -p, --password\[=name\]  登录密码，如果不写于参数后，则会提示输入
                    Password to use when connecting to server. If password is
                    not given it's asked from the tty.
- -P, --port=#        Port number to use for connection. 指定数据库端口
- --protocol=name     The protocol of connection (tcp,socket,pipe,memory). 指定连接协议
- -r, --relative      Show difference between current and previous values when
                    used with -i. Currently works only with extended-status. 显示前后变化的值，必须结合- i


--

	$ mysqladmin -uroot -p -i 2 -r extended-status
	
	

假如此处显示的uptime 将永远是2，因为与之前取的结果比只相差2.
 
- -O, --set-variable=name 
                      Change the value of a variable. Please note that this
                      option is deprecated; you can set variables directly with
                      --variable-name=value.修改变量的值，该选项已经不再使用，请使用--variable-name=value 的方式修改变量值
- -s, --silent        Silently exit if one can't connect to server. 
- -S, --socket=name   Socket file to use for connection. 指定socket file
- -i, --sleep=#       Execute commands again and again with a sleep between. 间隔一段时间执行一次
- -u, --user=name     User for login if not current user.登录数据库用户名
- -v, --verbose       Write more information. 写更多的信息
- -V, --version       Output version information and exit. 显示版本

--
	$ mysql -uroot -p -V

- -E, --vertical      Print output vertically. Is similar to --relative, but
                      prints output vertically. 
- -w, --wait\[=#\]      Wait and retry if connection is down. 如果连接断开，等待w 指定的时间后重试
- --connect_timeout=# 
- --shutdown_timeout=#

####查看服务器的状况：status

	$ mysql -uroot -p status
	
	Uptime: 4883162  Threads: 1  Questions: 86  Slow queries: 0  Opens: 0  Flush tables: 1  Open tables: 18  Queries per second avg: 0.000

####修改root 密码：

	$ mysqladmin -u root -poldpassword password 'newpassword'

####检查mysqlserver是否可用：

	$ mysqladmin -uroot -p ping
	mysqld is alive

####查询服务器的版本

	$ mysqladmin -uroot -p version

####查看服务器状态的当前值：

	$ mysqladmin -uroot -p extended-status



####查询服务器系统变量值：

	$ mysqladmin -uroot -p variables


####显示服务器所有运行的进程：

	$ mysqladmin -uroot -p processlist
	
每秒刷新一次:

	$ mysqladmin -uroot -p -i 1 processlist 
	

####创建数据库

	$ mysqladmin -uroot -p create daba-test

验证：

	$ mysql -uroot -p  -e "show databases;"


####删除数据库 daba-test

	$ mysqladmin -uroot -p drop daba-test

####重载权限信息

	$ mysqladmin -uroot -p reload

####刷新所有表缓存，并关闭和打开log

	$ mysqladmin -uroot -p refresh

####使用安全模式关闭数据库

	$ mysqladmin -uroot -p shutdown
	
	You can also use “/etc/rc.d/init.d/mysqld stop” to shutdown the server. To start the server, execute “/etc/rc.d/init.d/mysql start”--如果不是以服务来运行则这两条命令无效

####mysqladmin flush commands

	$ mysqladmin -u root -ptmppassword flush-hosts
	$ mysqladmin -u root -ptmppassword flush-logs
	$ mysqladmin -u root -ptmppassword flush-privileges
	$ mysqladmin -u root -ptmppassword flush-status
	$ mysqladmin -u root -ptmppassword flush-tables
	$ mysqladmin -u root -ptmppassword flush-threads

	flush-hosts: Flush all information in the host cache.
	flush-privileges: Reload the grant tables (same as reload).
	flush-status: Clear status variables.
	flush-threads: Flush the thread cache.
	
####mysqladmin 执行kill 进程：

	$ mysqladmin -uroot -p processlist
	
or:

	$ mysqladmin -uroot -p kill idnum

####停止和启动MySQL replication on a slave server

	$ mysqladmin  -u root -p stop-slave
	
or:

	$ mysqladmin  -u root -p start-slave

####同时执行多个命令

	$ mysqladmin  -u root -p process status version 
	

<hr>
	
##MySQL mysqlshow
	
####显示服务器上的所有数据库

	$ mysqlshow -uroot -p

####显示数据库daba-test下有些什么表：

	$ mysqlshow -uroot -p daba-test

####统计daba-test 下数据库表列的汇总

	$ mysqlshow -uroot -p daba-test -v

####统计daba-test 下数据库表的列数和行数

	$ mysqlshow -uroot -p daba-test -v -v
	
