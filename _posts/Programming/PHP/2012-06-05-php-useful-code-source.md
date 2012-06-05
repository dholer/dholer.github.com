---
layout: post
title: PHP 常用源码
category : PHP
tags : [PHP]
tagline : 
---
{% include JB/setup %}

###Wordpress 排错代码

	ini_set('display_errors','1'); 
	ini_set('display_startup_errors','1'); 
	error_reporting (E_ALL);
	include('index.php');
	
###将代码转化为Html标记

	function tohtml($content) {
		$content = htmlspecialchars($content);
		//转化回车
		$content = str_replace(chr(13), "<br>", $content);
		//转化空格
		$content = str_replace(chr(32), " ", $content);
		return trim($content);
	}
	
	$test = '<b>hello,world!</b>';
	echo tohtml($test);
	echo "\n";
	echo htmlspecialchars("&");
	echo "\n";
	echo htmlspecialchars('"');
	echo "\n";
	echo htmlspecialchars("'");
	echo "\n";
	echo htmlspecialchars("<");
	echo "\n";
	echo htmlspecialchars(">");
	
输出：

	&lt;b&gt;hello,world!&lt;/b&gt;
	&amp;
	&quot;
	'
	&lt;
	&gt;

htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体。

预定义的字符是：

	& （和号） 成为  &amp;
	” （双引号） 成为 &quot;
	‘ （单引号） 成为 ‘
	< （小于） 成为 &lt;
	> （大于） 成为 &gt;
	


###php 截断字符

一个字母是1个字符，一个汉字是两个字符 。当php用substr 截取字符串时，会遇到截取汉字的一半，那么就会出现一个？号。写了一个截取函数可以避免这个问题：
	
	//截断函数  
	function chinasuber($str,$start,$len){
		$strlen=$start+$len;
		for($i=0;$i<$strlen;$i++){
			if(ord(substr($str,$i,1))>0xa0){
				$tempstr.=substr($str,$i,2);
				$i++;
			}
			else
			{
				$tempstr.=substr($str,$i,1);
			}			
		}
			
		if(strlen($str)>$strlen)
		{
			$tempstr.="...";
		}		
		return $tempstr;
	}


###网上资料

- [WordPress 样式标签云探究 Case Study: WordPress Flash Tag Cloud](http://riashanghai.com/zh-hant/node/72)



