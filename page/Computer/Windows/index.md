---
layout: page 
title: Windows
---




- [wmic教程 命令收集](http://qlj.sh.cn/system/20090430/wmic-command/)
- [80端口被NT kernel & System 占用pid 4](http://www.2cto.com/os/201111/111269.html)



##常用dos 命令

###添加环境变量，并成系统立即生效

	@set PATH=C:\
	exit
	
重新打开CMD

	echo %PATH%
	
###打开services

	%windir%\system32\services.msc
	
###查看指定端口号

	netstat -ano|findstr ":80"
###查看指定进程号进程

	tasklist|find "4"
	tasklist /FI "PID eq 4"



##文件操作

###创建文件

**方法一：**

重定向输出，此时创建文本文件A.txt

	echo ...... >  A.txt       

向A.txt文件中追加信息.....;

	echo ...... >> A.txt       


**方法二：**
创建A.txt文本文件

	copy con A.txt    
	 
输入内容....;

	.......^Z 　
	　　　　 　 
按CTRL+Z键，之后再回车；　　
　             

**方法三：**
编辑A.txt文本文件，调用command.com中创建文本文件的命令，如果A.txt文件不存在，则创建文件，输入内容，保存。

	edit A.txt                   
                                   

**方法四：**
可建立一个空文件中，在批处理中经常用。

	type nul>filename   
	
可建立一个空文件中，在批处理中经常用，也可清空一个文件。

	copy nul A.txt         



###创建目录：

	mkdir 目录名

###删除目录：

	rmdir 目录名
