---
layout: post
title: "fuzzywuzzy"
description: ""
category: Python
tags: [fuzzywuzzy]
---
{% include JB/setup %}
Fuzzywuzzy 单从名字上来看就觉得好可爱呀！然后Google的时候发现这是一句绕口令。

	Fuzzy wuzzy was a bear. Fuzzy wuzzy had no hair. Fuzzy wuzzy wasn't very fuzzy, was he?

归正题，Fuzzywuzzy是一个对比字符串的小工具，足够轻量级。一直想自己写一个这样的小工具，今天就发现这货了，可以不用自己写了。
Github地址：[fuzzywuzzy @ github](https://github.com/seatgeek/fuzzywuzzy)

下边一段代码可以很好的介绍Fuzzywuzzy的功能了，自己体会吧！


	from fuzzywuzzy import fuzz
	from fuzzywuzzy import process

	print fuzz.ratio("this is a test", "this is a test!") # 96
	print fuzz.partial_ratio("this is a test", "this is a test!") # 100

	print fuzz.ratio("fuzzy wuzzy was a bear", "wuzzy fuzzy was a bear") # 90
	print fuzz.token_sort_ratio("fuzzy wuzzy was a bear", "wuzzy fuzzy was a bear") # 100

	print fuzz.token_sort_ratio("fuzzy was a bear", "fuzzy fuzzy was a bear") # 84
	print fuzz.token_set_ratio("fuzzy was a bear", "fuzzy fuzzy was a bear") # 100

	choices = ["Atlanta Falcons", "New York Jets", "New York Giants", "Dallas Cowboys"]
	print process.extract("new york jets", choices, limit=2) # [('New York Jets', 100), ('New York Giants', 78)]
	print process.extractOne("cowboys", choices) # ('Dallas Cowboys', 90)
