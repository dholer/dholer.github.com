---
layout: post
title: ASIHTTPRequest ARC 无法共用
category : objectiv-c
tags : [objectiv-c,ASIHTTPRequest]
tagline : 
---
{% include JB/setup %}

- 在Build Settings -> Apple LLVM compiler 3.0 - Language -> Objective-C Automatic Referencing Counting里, 默认是YES 设置为NO即可。

- 把ASIHTTPRequest工程做成静态库导进来，这个工程的ARC设为NO

你自己的工程ARC可以设为YES

迁移项目必然要遇到旧的库在新的环境下水土不服的情况，首先遇到的难题是ASIHttpRequest。

Stackoverflow上找到了一个答案，如下：

	It's very easy to use asi-http-request in an ARC environment without changing anything, simply follow these steps:
	
	Simply create a static library target.
	Add asi-http-request files as required to the library target
	Configure the target to build without ARC
	Add that static library as a dependency to your app target
	Add that static library in the build phases link section.
	
可能是我操作有问题没有解决，于是尝试着躲避ARC，就想到了干脆把不想用ARC的文件都加上个flag“-fno-objc-arc”