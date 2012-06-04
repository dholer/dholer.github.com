---
layout: page 
title: Vim
---

##常用命令
http://baike.baidu.com/view/113188.htm

##\_vimrc

[我的配置文件](https://raw.github.com/dholer/dholer.github.com/master/page/Vim/_vimrc)<br>
[配置文件1](http://amix.dk/vim/vimrc.html)<br>
[国内网友改进版] (http://blog.csdn.net/redguardtoo/article/details/1172136)<br>


##相关资料
- [Vim 常用命令总结](http://www.pizn.me/2012/03/03/vim-commonly-used-command.html)



##实现html文档快速插入

	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

终端下

	sudo vi /etc/vim/vimrc

在最下面添加：

	"html 插入头部申明
	
	function AddTitle()
		call setline(1,'<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">')
	endf
	map html :call AddTitle()<CR>

保存；
