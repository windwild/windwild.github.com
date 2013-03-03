---
layout: post
title: "Hadoop读书笔记"
description: "Hadoop读书笔记"
category: Data
tags: [BigData,MapReduce,Hadoop]
---
{% include JB/setup %}

## 读书笔记 Hadoop: The Definitive Guide

### 第一章 Meet Hadoop

#### MapReduce 优点

* 错误处理
* 优秀的消息处理机制，摆脱从Socket搞起的旧套路
* 本地化存储，which大大的加速了数据分析速度，节约了带宽
* 虽然感觉上约束很多，但实际上解决了很多的问题，如：图像分析，图算法，机器学习算法等等。

#### Hadoop 生态圈

* Common - 一系列的components和interfaces用来处理文件系统和一般的I/O,例如serialization，Java RPC， Persistent data structures。
* Avro - 一个Serialization System用来进行高效，跨语言的RPC，和persistent data storage。
* MapReduce - 分布式数据处理模型，也是在普通计算机上运行大规模Cluster的运行环境。
* HDFS - 运行在普通计算机上的分布式文件系统
* Pig - Data Flow Language（数据交换语言？）以及处理大规模数据的运行环境，基于HDFS和MapReduce。
* Hive - 分布式数据仓库，管理在HDFS上存储的数据，并提供了基于SQL的查询。
* HBase - 分布式Column-Oriented数据库，采用HDFS存储数据，支持Match-Style计算和点查询（SQL like？）
* ZooKeeyper - 分布式，高可靠的Coordination Service。提供了Primitives，比如分布式锁机制。
* Sqoop - 从关系数据库迁移数据到HDFS的工具。

### 第二章 Meet Hadoop
#### Map Reduce Example

在这一章里，首先介绍了一个使用MapReduce的例子，根据传感器信息分析每年的最高气温。还介绍了一下Hadoop MapReduce的新老API和区别，当然新的更好用一些。

这部分觉得没什么太多重点，最主要的就是能看看怎么写一个Map-Reduce吧。

#### Scaling Out
##### Data Flow

![Hadoop data Flow](http://autofei.files.wordpress.com/2010/06/2-3.png)
这个图很好的诠释了Hadoop中Mapreduce部分的Data FLow。

先介绍了一些术语。在MR中，Job是最大的单元，包含输入数据、MR程序和配置信息。Hadoop将Job分成不同的`tasks`然后执行。所以就有了两种nodes，一种是`jobtracker`，另一种是`tasktracker`。

关于input数据的划分，建议大小是64M，因为64M是HDFS一个block的大小。过多的划分会造成overhead（就是执行工作的时间少，执行调度的时间长），过少的划分会造成负载不均衡现象。有强调了一下Hadoop强大的 __data locality optimization__ 策略。尽量减少网络通讯，以节约贷款并提升处理速度。如果不能在本地调动数据，也一定要争取在一个rake（机架）内调动数据。

回到上边的那个图，Map nodes从 HDFS 中读取数据，然后将处理过的数据保存在本地（因为是临时数据，没必要大费周折）。再将数据传输到制定的Reduce node上（关于Reduce node的选择可以采用Hash（key）的方法，同一个key尽量在一个node上处理，一个node上可以处理多个key）。当数据到达reduce node的时候通常需要做一个merge操作，可以是将数据排序或者分类。reduce节点处理过数据后将数据存在HDFS上。

再然后介绍了一下__Combiner Functions__，这个Combiner Functions在Map结束后执行，是对Map产生的数据进行的优化。最给力的是，你还可以省掉Reduce Function，通过Combiner Functions直接得到结果。（这样的结构不科学啊）

#### Hadoop Streaming(也就是说可以和多语言通用)
看到这部分的时候真是心潮澎湃了啊！`stdin`，`stdout`真是太可爱了。因为本人不喜欢Java，没有为什么，就是不喜欢。这一部分就是说可以通过`stdin`和`stdout`与多种语言通用Hadoop框架。比如Ruby，__Python__甚至C++（学名Hadoop Piples）。

### 第三章 HDFS
HDFS全称是Hadoop Distributed Filesystem，为满足Hadoop对文件系统要求设计而成的文件系统。功能类似于Google的GFS。

__优点__：
HDFS适合存储__大文件__，这和淘宝的TFS形成了比较大的对比，因为需求不一样，这两个东西其实不能相提并论。HDFS同时支持以__数据流__的形式读取，数据__写一次读多次__。当然，HDFS并不需要昂贵的硬件，就可以实现高可靠性。

__缺点__：
HDFS当然也有很多缺点，首先__不适合低延迟数据访问__，如果想获得低延迟的数据访问可以参考`HBase`。HDFS__不适合存储大量小文件__，因为HDFS存在namenode负责管理文件列表。HDFS同样__不适用于多用户同时写入修改__。

HDFS中存在两种节点，`name node`和`datanode`。`name node`管理HDFS中的文件，管理`filesystem tree`和`metadata`。这些信息以两种形式永久的存储在本地，分别是`namespace image`和`edit log`。这样的话，如果`name node`挂了，整个HDFS也就挂了，因为只有`name node`知道哪个文件的哪个块儿在哪儿。为此Hadoop提供了两种防错机制。首先是备份，`name node`原子的同步数据到其他位置，比如NFS。另一种策略是`secondary namenode`。具体疗效还没大看懂。书上讲是合并`namespace image`和`edit log`以防`edit log`过大

HDFS可以通过增加`name node`来进行扩展。应该叫HDFS Federation。

当HDFS的name node挂掉的时候可以迅速的切换name node。新节点可用必须满足：加载namespace image到内存，重做edit log，从datanodes收到足够的块报告。

再然后就是怎么用Java什么的调用HDFS了。不细说了，看书上的sample就好，很容易。

未完

