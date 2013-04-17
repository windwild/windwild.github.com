---
layout: post
title: "Hadoop with Python"
description: ""
category: Hadoop
tags: [Python, Hadoop]
---
{% include JB/setup %}

<div id="contentleft">
	
	
	<p>由于数据量的疯狂增长，现在的实验或者是比赛都不得不用并行的算法来实现，而hadoop中的map/reduce框架正是多种并行框架中被广泛使用的一种。下面总结一下python+hadoop的几种方法：</p>
<p>1、hadoop流</p>
<p>hadoop为我们提供了一个计算平台和一个并行计算框架，Hadoop流提供的api允许用户使用任何脚本来编写map/reduce函数，因为它使用unix标准流作为程序与hadoop之间的接口，所以任何程序只要可以从标准输入流中读取数据，并且可以写入标准输出流，就可以实现MapReduce。</p>
<p>具体的实现example可以参见这篇blog：http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/</p>
<p>中文版的在此：http://blog.c114.net/html/71/482871-63885.html</p>
<p>2、dumbo（详见https://github.com/klbostee/dumbo/wiki）</p>
<p>--<span style="color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">Dumbo is a project that allows you to easily write and run Hadoop programs in Python (it’s named after Disney’s flying circus elephant, since the&nbsp;</span><a href="http://hadoop.apache.org/core/images/hadoop-logo.jpg" style="margin:0px;padding:0px;border:0px;color:#4183c4;text-decoration:none;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">logo of Hadoop</a><span style="color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">&nbsp;is an elephant and Python was named after the&nbsp;</span><span class="caps" style="margin:0px;padding:0px;border:0px;color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">BBC</span><span style="color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">&nbsp;series “Monty Python’s Flying Circus”). More generally, Dumbo can be considered to be a convenient Python&nbsp;</span><span class="caps" style="margin:0px;padding:0px;border:0px;color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">API</span><span style="color:#333333;font-family:Helvetica, arial, freesans, clean, sans-serif;font-size:13.63636302947998px;line-height:20px;">&nbsp;for writing MapReduce programs.</span></p>
<p>3、hadoopy（详见https://github.com/bwhite/hadoopy）</p>
<p>--<a href="https://github.com/bwhite/hadoopy" target="_blank" style="box-sizing:border-box;text-decoration:none;color:#0392b2;cursor:pointer;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">hadoopy</a><span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">&nbsp;is another Streaming wrapper that is compatible with dumbo. Similarly, it focuses on typedbytes serialization of data, and directly writes typedbytes to HDFS.</span></p>
<p>4、pydoop(详见<b style="color:#009933;font-family:arial, sans-serif;font-size:14.399999618530273px;line-height:16.799999237060547px;">pydoop</b><span style="font-family:arial, sans-serif;font-size:14.399999618530273px;line-height:16.799999237060547px;">.sourceforge.net</span><span style="line-height:1.5;">)</span></p>
<p>--<span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">n contrast to the other frameworks,&nbsp;</span><a href="http://pydoop.sourceforge.net/docs/" target="_blank" style="box-sizing:border-box;text-decoration:none;color:#0392b2;cursor:pointer;font-weight:bold;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">pydoop</a><span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">&nbsp;wraps Hadoop Pipes, which is a C++ API into Hadoop. The project claims that they can provide a richer interface with Hadoop and HDFS because of this, as well as better performance, but this is not clear to me. However, one advantage is the ability to implement a Python&nbsp;</span><code style="box-sizing:border-box;margin:0px;padding:0px;color:#505050;font-size:13px;line-height:19px;">Partitioner</code><span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">,</span><code style="box-sizing:border-box;margin:0px;padding:0px;color:#505050;font-size:13px;line-height:19px;">RecordReader</code><span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">, and&nbsp;</span><code style="box-sizing:border-box;margin:0px;padding:0px;color:#505050;font-size:13px;line-height:19px;">RecordWriter</code><span style="color:#505050;font-family:'Helvetica Neue', Helvetica, Arial, sans-serif;font-size:13px;line-height:19px;">. All input/output must be strings.</span></p>
<p>几种python实现或者是结合hadoop的方法都各有利弊，但都有一个共同的好处，就是你可以用简短的几行python代码就实现以前很多用c++或者是c实现的并行算法，加上hdfs，就可以很轻松地处理各种大数据了。</p>

</div>
[zz]<http://somemory.com/myblog/?post=56>
