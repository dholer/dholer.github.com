---
layout: page 
title: PHP
---




###下载PHP

PHP的 windows 版本已经分离出来了，见 [下载地址](http://windows.php.net/download) ，但是上面有很多不同的版本，包括 :VC9, VC6, x86 Non Thread Safe, x86 Thread Safe

- VC6 版本是使用 Visual Studio 6 编译器编译的
- VC9 版本是使用 Visual Studio 2008 编译器编译的
- Non Thread Safe 就是非线程安全，在执行时不进行线程（ Thread ）安全检查； 
- Thread Safe 是线程安全，执行时会进行线程（ Thread ）安全检查，以防止有新要求就启动新线程的 CGI 执行方式而耗尽系统资源；

Windows 下的 PHP 主要有两种执行方式： **ISAPI** 和 **FastCGI** 。

- ISAPI 执行方式是以 DLL 动态库的形式使用，可以在被用户请求后执行，在处理完一个用户请求后不会马上消失，所以需要进行线程安全检查，这样来提高程序的执行效率，所以如果是以 ISAPI 来执行 PHP，建议选择 Thread Safe 版本；
- FastCGI 执行方式是以单一线程来执行操作，所以不需要进行线程的安全检查，除去线程安全检查的防护反而可以提高执行效率，所以，如果是以 FastCGI 来执行 PHP ，建议选择 Non Thread Safe 版本。官方并不建议你将 Non Thread Safe 应用于生产环境。

####知识补充：
- ISAPI 缩写词为 Internet Server Application Programming Interface 为 Microsoft 所提的 Internet server 的 API 。 ISAPI 服务器扩展是可以被 HTTP 服务器加载和调用的 DLL 。 Internet 服务器扩展也称为 Internet 服务器应用程序 (ISA) ，用于增强符合 Internet 服务器 API (ISAPI) 的服务器的功能。 ISA 通过浏览器应用程序调用，并且将相似的功能提供给通用网关接口(CGI) 应用程序。
- FastCGI 是一个程序接口，它能加速公共网关接口（ CGI ）， CGI 是一种用最常见的方式使 Web 服务器调用应用程序的 Web 应用程序。按一个 FastCGI 工具来看，用户要求进入一个网站并使用一个专门的应用软件的话，使用 FastCGI 能够快 3 到 30 倍。 FastCGI 是 Web 服务器的一种 插件。为了获得良好的性能，它要求对现有服务器应用程序（比如 Perl、 Tcl 脚本和 C 、 C++ 程序）做细小的改动。　　

基本上， FastCGI 是一个在单一步骤中管理多重 CGI 请求的程序，为每个请求减少了许多程序指令。没有 FastCGI 的话，每当用户请求某一服务时都会导致 Web 服务器打开 一个新的能控制和执行这项服务的程序，然后关闭它。有了 FastCGI的话，一个步骤的耗费会被所有当前正处理的请求所分担。与 CGI 不同，有了 FastCGI 的话，每个步骤是独立于 Web 服务器运行的， 这样就提供了更多的安全。 FastCGI 是独立代码的。它的版权属于 Open Market 公司，该公司提供 FastCGI的免费使用并且将其作为一个公开标准。 FastCGI 提供了唯一一个可以跨平台和在任何 Web 服务器上使用的 无知识产权的方法。





###空间资源

- [PHP Cloud](https://www.phpfog.com/)

###集成开发环境

- [CoreAMP](http://code.google.com/p/coreamp/)


