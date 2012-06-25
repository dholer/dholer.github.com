---
layout: post
title: win7 安装RUBY1.9.3
category : ruby
tags : [ruby,rails,win7]
tagline : 
---
{% include JB/setup %}

###安装 ruby 1.9.3 and install

下载地址：

	http://rubyforge.org/projects/rubyinstaller/
	http://ruby.taobao.org/mirrors/ruby/windows/

注意安装上ruby 1.9.3 就不需要再安装rubygems了,这个和以前ruby 1.8.X 的有区别,ruby 1.9.3 已经自带rubygems了,可以

	gem -v 
	
查看.

###安装Development Kit

	https://github.com/oneclick/rubyinstaller/wiki/Development-Kit

###安装rails


由于国内网络原因，导致 rubygems.org 存放在 Amazon S3 上面的资源文件间歇性连接失败。所以你会与遇到 gem install rack 或 bundle install 的时候半天没有响应，具体可以用 gem install rails -V 来查看执行过程。

这是一个完整 rubygems.org 镜像，你可以用此代替官方版本，同步频率目前为30分钟一次以保证尽量与官方服务同步。

	$ gem sources --remove http://rubygems.org/
	$ gem sources -a http://ruby.taobao.org/
	$ gem sources -l

####请确保只有 ruby.taobao.org

	$ gem install rack

如果你是用 Bundle (Rails 项目)
修改你的 Gemfile 将 http://rubygems.org 改为 http://ruby.taobao.org/

	source 'http://ruby.taobao.org/'
	gem 'rails', '3.1.1'


