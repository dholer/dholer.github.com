---
layout: post
title: php linux下用phpize 安装扩展
category : PHP
tags : [PHP]
tagline : 
---
{% include JB/setup %}

安装完php后最好保留当时安装的文件,比如我的径/opt

	cd php-5.1.6/ext/ldap
	/usr/local/php/bin/phpize
	./configure --with-php-config=/usr/local/php/bin/php-config --enable-ldap  --enable-gettext
	make
	make install
	
编译后的ldap.so文件保存在了/usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/目录下
修改php.ini文件
手工修改：查找/usr/local/webserver/php/etc/php.ini中的extension_dir = "./"
修改为

	extension_dir = "/usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/"
并在此行后增加如下,然后保存：

	extension = "ldap.so"

重新启动apache,ok 我们就已经能看到扩展的soap模块了.
如果还要扩展别的模块可以一次类推,这里还有点要说明,如果做了zend,php.ini文件是在/usr/local/php/etc下的,但是我们这边重新编译后,它回放到/usr/local/php/lib下.这里要注意一下.
