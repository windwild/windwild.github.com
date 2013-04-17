---
layout: post
title: "千万不要把python文件名用的和正在调用的包一样"
description: ""
category: Python
tags: [Python]
---
{% include JB/setup %}
清理掉一批作业以后，终于可以继续折腾Python了，可以早上一小段简单的代码就卡住了。。。

	from pattern.web import Twitter, plaintext

	for tweet in Twitter().search('"more important than"', cached=False):
		print plaintext(tweet.description).encode("utf-8")

运行的时候一直找不到pattern下的web，找了10多分钟，终于发现。。。文件名和包名字重复了。。。跪了。。。怪不得找不到，差点去github上开issue