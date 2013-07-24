---
layout: post
title: "Debian PHP 发送邮件"
description: ""
category: 
tags: []
---
{% include JB/setup %}

PHP在发送邮件的时候会调用sendmail函数，但是debian没有默认安装这个，所以

	apt-get install sendmail-bin
	
安装之后就可以发邮件了，但是安装之后，发送邮件的时候总是非常慢，原因是sendmail不能检测到kissexp是否是本机，所以，需要配置一下 `/etc/hosts` 这个。

	112.124.7.165 kissexp localhost kissexp.
	
看好那个kiessexp后边的点号。