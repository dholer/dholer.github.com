---
layout: post
title: Win 7 下安装Apache PHP
category : PHP
tags : [PHP]
tagline : 
---
{% include JB/setup %}


###下载PHP5.4

	http://windows.php.net/download/

选择VC9 x86 Thread Safe (2012-Apr-25 23:01:08)

	http://windows.php.net/downloads/releases/php-5.4.1-Win32-VC9-x86.zip


解压到：D:\Dholer\local\PHP\V5.4.1

	copy D:\Dholer\local\PHP\v5.4.1\php.ini-development D:\Dholer\local\PHP\v5.4.1\php.ini

	vi D:\Dholer\local\PHP\v5.4.1\php.ini


修改时区：

	date.timezone ='Asia/Shanghai'

解决：
> Warning: strtotime() [function.strtotime]: It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected 'Asia/Chongqing' for 'CST/8.0/no DST' instead


修改扩展目录

	; Directory in which the loadable extensions (modules) reside.
	; http://php.net/extension-dir
	; extension_dir = "./"
	; On windows:
	extension_dir = "D:\Dholer\local\PHP\V5.4.1\ext"



启用扩展

	extension=php_curl.dll
	extension=php_gd2.dll
	extension=php_gettext.dll
	extension=php_mysql.dll
	extension=php_mysqli.dll
	extension=php_openssl.dll
	extension=php_pdo_mysql.dll
	extension=php_pdo_odbc.dll
	extension=php_pdo_sqlite.dll
	extension=php_soap.dll
	extension=php_sockets.dll
	extension=php_sqlite3.dll
	extension=php_xmlrpc.dll
	extension=php_mbstring.dll

修改上传执行参数：

	max_execution_time = 120
	max_input_time = 60
	post_max_size = 100M
	upload_max_filesize = 20M

修改Session保存目录：

	session.save_path=" D:\Dholer\local\tmp"


添加D:\Dholer\local\PHP\V5.4.1\到环境变量，并成系统立即生效

	$ @set PATH=C:\
	$ exit
重新打开CMD:

	$ echo %PATH%


运行

	$ php -v
	
	PHP 5.4.1 (cli) (built: Apr 25 2012 21:40:16)
	Copyright (c) 1997-2012 The PHP Group
	Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
	修改  vi D:\Dholer\local\Apache2.2\conf\httpd.conf 
	LoadModule rewrite_module modules/mod_rewrite.so

修改APACHE httpd.conf 加入：

	AddHandler application/x-httpd-php .php
	LoadModule php5_module "D:/Dholer/local/PHP/V5.4.1/php5apache2_2.dll"
	PHPIniDir "D:/Dholer/local/PHP/V5.4.1"

重启Apache
	
	$ httpd -k restart

测试

	$ echo Hello World! > D:\Dholer\local\Apache2.2\htdocs\phpinfo.php
	
打开http://127.0.0.1/phpinfo.php

修改D:\Dholer\local\Apache2.2\htdocs\phpinfo.php

添加
	<?php
	echo phpinfo();
	?>

重新打开http://127.0.0.1/phpinfo.php



###安装Pear

pear是PHP的扩展和应用程序库，包含了很多有用的类

	http://pear.php.net/manual/en/installation.php

To update your PEAR installation, request http://pear.php.net/go-pear.phar in your browser and save the output to a local file go-pear.phar. You can then run

	$ cd D:\Dholer\local\PHP\V5.4.1\
	$ wget http://pear.php.net/go-pear.phar
	$ php go-pear.phar
	Are you installing a system-wide PEAR or a local copy?
	(system|local) [system] :
	
	Below is a suggested file layout for your new PEAR installation.  To
	change individual locations, type the number in front of the
	directory.  Type 'all' to change all of them or simply press Enter to
	accept these locations.
	
	 1. Installation base ($prefix)                   : D:\Dholer\local\PHP\V5.4.1
	 2. Temporary directory for processing            : D:\Dholer\local\PHP\V5.4.1\tmp
	 3. Temporary directory for downloads             : D:\Dholer\local\PHP\V5.4.1\tmp
	 4. Binaries directory                            : D:\Dholer\local\PHP\V5.4.1
	 5. PHP code directory ($php_dir)                 : D:\Dholer\local\PHP\V5.4.1\pear
	 6. Documentation directory                       : D:\Dholer\local\PHP\V5.4.1\docs
	 7. Data directory                                : D:\Dholer\local\PHP\V5.4.1\data
	 8. User-modifiable configuration files directory : D:\Dholer\local\PHP\V5.4.1\cfg
	 9. Public Web Files directory                    : D:\Dholer\local\PHP\V5.4.1\www
	10. Tests directory                               : D:\Dholer\local\PHP\V5.4.1\tests
	11. Name of configuration file                    : D:\Dholer\local\PHP\V5.4.1\pear.ini
	12. Path to CLI php.exe                           : D:\Dholer\local\PHP\V5.4.1
	
	1-12, 'all' or Enter to continue:
	Beginning install...
	Configuration written to D:\Dholer\local\PHP\V5.4.1\pear.ini...
	Initialized registry...
	Preparing to install...
	installing phar://D:/Dholer/local/PHP\V5.4.1/go-pear.phar/PEAR/go-pear-tarballs/Archive_Tar-1.3.7.t
	ar...
	installing phar://D:/Dholer/local/PHP\V5.4.1/go-pear.phar/PEAR/go-pear-tarballs/Console_Getopt-1.3.
	0.tar...
	installing phar://D:/Dholer/local/PHP\V5.4.1/go-pear.phar/PEAR/go-pear-tarballs/PEAR-1.9.4.tar...
	installing phar://D:/Dholer/local/PHP\V5.4.1/go-pear.phar/PEAR/go-pear-tarballs/Structures_Graph-1.
	0.4.tar...
	installing phar://D:/Dholer/local/PHP\V5.4.1/go-pear.phar/PEAR/go-pear-tarballs/XML_Util-1.2.1.tar.
	..
	install ok: channel://pear.php.net/Archive_Tar-1.3.7
	install ok: channel://pear.php.net/Console_Getopt-1.3.0
	install ok: channel://pear.php.net/Structures_Graph-1.0.4
	install ok: channel://pear.php.net/XML_Util-1.2.1
	install ok: channel://pear.php.net/PEAR-1.9.4
	PEAR: Optional feature webinstaller available (PEAR's web-based installer)
	PEAR: Optional feature gtkinstaller available (PEAR's PHP-GTK-based installer)
	PEAR: Optional feature gtk2installer available (PEAR's PHP-GTK2-based installer)
	PEAR: To install optional features use "pear install pear/PEAR#featurename"
	
	******************************************************************************
	WARNING!  The include_path defined in the currently used php.ini does not
	contain the PEAR PHP directory you just specified:
	<D:\Dholer\local\PHP\V5.4.1\pear>
	If the specified directory is also not in the include_path used by
	your scripts, you will have problems getting any PEAR packages working.
	
	
	Would you like to alter php.ini <D:\Dholer\local\PHP\V5.4.1\php.ini>? [Y/n] : y
	
	php.ini <D:\Dholer\local\PHP\V5.4.1\php.ini> include_path updated.
	
	Current include path           : .;C:\php\pear
	Configured directory           : D:\Dholer\local\PHP\V5.4.1\pear
	Currently used php.ini (guess) : D:\Dholer\local\PHP\V5.4.1\php.ini
	Press Enter to continue:
	
	** WARNING! Old version found at D:\Dholer\local\PHP\V5.4.1, please remove it or be sure to use the
	 new d:\dholer\local\PHP\V5.4.1\pear.bat command
	
	The 'pear' command is now at your service at d:\dholer\local\PHP\V5.4.1\pear.bat
	
	
	* WINDOWS ENVIRONMENT VARIABLES *
	For convenience, a REG file is available under D:\Dholer\local\PHP\V5.4.1PEAR_ENV.reg .
	This file creates ENV variables for the current user.
	
	Double-click this file to add it to the current user registry.
	
	
	
	Checking if PEAR works
	
	Both pear and pecl tools should be available everywhere on command line. For that to work, pear's binary (bin) directory should be in your PATH variable.
	
	To verify it works, simply type pear. A list of commands should be shown:
	
	$ pear
	Commands:
	build                  Build an Extension From C Source
	bundle                 Unpacks a Pecl Package
	channel-add            Add a Channel
	...
	You should further test that PEAR is up to date:
	
	$ pear version
	PEAR Version: 1.9.4
	PHP Version: 5.401
	Zend Engine Version: 2.4.0
	Running on: Windows...
	
	
	



