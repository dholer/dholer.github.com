---
layout: page 
title: Apache
---


###资料整理

- [使用.htaccess提升Apache Webserver网站性能](http://www.oschina.net/code/snippet_99526_4382)
- [利用.htaccess绑定域名到子目录](http://www.ccvita.com/category/apache/)
- [Apache download](http://www.apachelounge.com/download/)
- [Apache docs](http://httpd.apache.org/docs/2.2/platform/windows.html)



###httpd-vhosts.conf

	NameVirtualHost *:89
	
	<VirtualHost *:89>
	   DocumentRoot "D:/Dholer/home/htdocs"
	   ServerName localhost
	   <Directory "D:/Dholer/home/htdocs">
	        Options Indexes MultiViews FollowSymLinks
			DirectoryIndex index.html index.php
	        AllowOverride All
	        Order allow,deny
	        Allow from all
	   </Directory>
	   
	   
	    Alias /phpmyadmin "D:/Dholer/local/phpMyAdmin/V3.5.1-all-languages"
		<Directory "D:/Dholer/local/phpMyAdmin/V3.5.1-all-languages">
		 AllowOverride None
		 Options Indexes FollowSymLinks Includes
		 DirectoryIndex index.html index.php
		 Order allow,deny
		 Allow from all
		</Directory>
	   
	</VirtualHost>
	
	
	<VirtualHost *:89>
	   DocumentRoot "D:/Dholer/home/workspace"
	   ServerName www.dev.org
	   <Directory "D:/Dholer/home/workspace">
	        Options Indexes MultiViews FollowSymLinks
			DirectoryIndex index.html index.php
	        AllowOverride All
	        Order allow,deny
	        Allow from all
	   </Directory>
	</VirtualHost>
	


###mod_wsgi python apathe

	LoadModule wsgi_module modules/mod_wsgi-win32-ap22py27-3.3.so
	
	
	##python django
	#D:\Dholer\home\workspace\ShowcaseCloud\ShowcaseCloud
	#<VirtualHost *:80>
	#   DocumentRoot "D:/Dholer/home/workspace/ShowcaseCloud/ShowcaseCloud"
	#   ServerName www.showcase.com#
		
		#Alias /static  D:/Dholer/home/workspace/ShowcaseCloud/ShowcaseCloud/static
	
		#<Directory "D:/Dholer/home/workspace/ShowcaseCloud/ShowcaseCloud/static">
	#		Order allow,deny
			#Options Indexes
			#Allow from all
		#</Directory>
		
		
		#WSGIScriptAlias / D:/Dholer/home/workspace/ShowcaseCloud/ShowcaseCloud/index.wsgi
	   #<Directory "D:/Dholer/home/workspace/ShowcaseCloud/ShowcaseCloud">
	    #    Order deny,allow
	#		Allow from all
	#   </Directory>
	   
	#</VirtualHost>

###mod_rewrite
通常利用Apache的rewrite模块对 URL 进行重写的时候， rewrite规则会写在 .htaccess 文件里。但要使 apache 能够正常的读取.htaccess 文件的内容，就必须对.htaccess 所在目录进行配置。从安全性考虑，根目录的AllowOverride属性一般都配置成不允许任何Override ，即
	
	<Directory />
		AllowOverride None
	</Directory>

在 AllowOverride 设置为 None 时， .htaccess 文件将被完全忽略。当此指令设置为 All 时，所有具有 ".htaccess" 作用域的指令都允许出现在 .htaccess 文件中。

而对于 URL rewrite 来说，至少需要把目录设置为

	<Directory /myblogroot/>
		AllowOverride FileInfo
	</Directory>

AllowOverride的参数：

	AuthConfig 允许使用与认证授权相关的指令(AuthDBMGroupFile, AuthDBMUserFile, AuthGroupFile, AuthName, AuthType, AuthUserFile, Require, 等)。
	FileInfo 允许使用控制文档类型的指令(DefaultType, ErrorDocument, ForceType, LanguagePriority, SetHandler, SetInputFilter, SetOutputFilter, mod_mime中的 Add* 和 Remove* 指令等等)、控制文档元数据的指令(Header, RequestHeader, SetEnvIf, SetEnvIfNoCase, BrowserMatch, CookieExpires, CookieDomain, CookieStyle, CookieTracking, CookieName)、mod_rewrite中的指令(RewriteEngine, RewriteOptions, RewriteBase, RewriteCond, RewriteRule)和mod_actions中的Action指令。
	Indexes 允许使用控制目录索引的指令(AddDescription, AddIcon, AddIconByEncoding, AddIconByType, DefaultIcon, DirectoryIndex, FancyIndexing, HeaderName, IndexIgnore, IndexOptions, ReadmeName, 等)。Limit 允许使用控制主机访问的指令(Allow, Deny, Order)。
	Options[=Option,...] 允许使用控制指定目录功能的指令(Options和XBitHack)。可以在等号后面附加一个逗号分隔的(无空格的)Options选项列表，用来控制允许Options指令使用哪些选项。


###apache 处理PHP FLOW

> HTTP请求过来，apache fork子进程，启动php解释器，装载源文件，创建数据库连接，生成orm对象，操纵orm对象，执行映射的sql语句，销毁对象，断开数据库连接，释放资源，关闭http 

