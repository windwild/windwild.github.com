---
layout: post
title: "python call python"
description: ""
category: Python
tags: [python, socket, stdout]
---
{% include JB/setup %}

一直想做个东西把PHP和Python打通，今天写了一部分Python根据字符串调用Python函数的Python。
下一步连上socket应该就能用了。现在实现的功能相当于打开一个Python Shell，函数执行的返回值和stdout都写入变量，方便用socket返回。


欢迎指正讨论！[runrunPython](https://github.com/windwild/CodeBox/blob/master/runrunPython.py)
现在用的exec()，还有些人用getattr()，我觉得exec更灵活一点，所以最后采用的是exec。

	import sys

	class stdout2str:
		def __init__(self):
			self.s = ""
		def write(self, buf):
			self.s += buf

	def runFun(function_name):
		tempout = sys.stdout
		out = stdout2str()
		sys.stdout = out
		exec("temp_ret = %s"%(function_name))
		sys.stdout = tempout
		print "function stdout:", out.s
		print "function return", temp_ret

	def fun():
		print "hello windiwld"
		return 2046

	if __name__ == '__main__':
		function_name = "fun()"
		runFun(function_name)