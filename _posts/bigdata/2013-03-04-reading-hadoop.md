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
不同于普通文件系统的是，HDFS中，如果一个文件小于一个数据块的大小，并不占用整个数据块存储空间。

HDFS中存在两种节点，`name node`和`datanode`。`name node`管理HDFS中的文件，管理`filesystem tree`和`metadata`。这些信息以两种形式永久的存储在本地，分别是`namespace image`和`edit log`。这样的话，如果`name node`挂了，整个HDFS也就挂了，因为只有`name node`知道哪个文件的哪个块儿在哪儿。为此Hadoop提供了两种防错机制。首先是备份，`name node`原子的同步数据到其他位置，比如NFS。另一种策略是`secondary namenode`。具体疗效还没大看懂。书上讲是合并`namespace image`和`edit log`以防`edit log`过大。

HDFS可以通过增加`name node`来进行扩展。应该叫HDFS Federation。

当HDFS的name node挂掉的时候可以迅速的切换name node。新节点可用必须满足：加载namespace image到内存，重做edit log，从datanodes收到足够的块报告。

再然后就是怎么用Java什么的调用HDFS了。不细说了，看书上的sample就好，很容易。

#### 元数据节点(Namenode)和数据节点(datanode)

	元数据节点用来管理文件系统的命名空间
		其将所有的文件和文件夹的元数据保存在一个文件系统树中。
		这些信息也会在硬盘上保存成以下文件：命名空间镜像(namespace image)及修改日志(edit log)
		其还保存了一个文件包括哪些数据块，分布在哪些数据节点上。然而这些信息并不存储在硬盘上，而是在系统启动的时候从数据节点收集而成的。
	数据节点是文件系统中真正存储数据的地方。
		客户端(client)或者元数据信息(namenode)可以向数据节点请求写入或者读出数据块。
		其周期性的向元数据节点回报其存储的数据块信息。
	从元数据节点(secondary namenode)
		从元数据节点并不是元数据节点出现问题时候的备用节点，它和元数据节点负责不同的事情。
		其主要功能就是周期性将元数据节点的命名空间镜像文件和修改日志合并，以防日志文件过大。这点在下面会相信叙述。
		合并过后的命名空间镜像文件也在从元数据节点保存了一份，以防元数据节点失败的时候，可以恢复。

#### 文件系统命名空间映像文件及修改日志

	当文件系统客户端(client)进行写操作时，首先把它记录在修改日志中(edit log)
	元数据节点在内存中保存了文件系统的元数据信息。在记录了修改日志后，元数据节点则修改内存中的数据结构。
	每次的写操作成功之前，修改日志都会同步(sync)到文件系统。
	fsimage文件，也即命名空间映像文件，是内存中的元数据在硬盘上的checkpoint，它是一种序列化的格式，并不能够在硬盘上直接修改。
	同数据的机制相似，当元数据节点失败时，则最新checkpoint的元数据信息从fsimage加载到内存中，然后逐一重新执行修改日志中的操作。
	从元数据节点就是用来帮助元数据节点将内存中的元数据信息checkpoint到硬盘上的
	checkpoint的过程如下：
		从元数据节点通知元数据节点生成新的日志文件，以后的日志都写到新的日志文件中。
		从元数据节点用http get从元数据节点获得fsimage文件及旧的日志文件。
		从元数据节点将fsimage文件加载到内存中，并执行日志文件中的操作，然后生成新的fsimage文件。
		从元数据节点奖新的fsimage文件用http post传回元数据节点
		元数据节点可以将旧的fsimage文件及旧的日志文件，换为新的fsimage文件和新的日志文件(第一步生成的)，然后更新fstime文件，写入此次checkpoint的时间。
		这样元数据节点中的fsimage文件保存了最新的checkpoint的元数据信息，日志文件也重新开始，不会变的很大了。


#### 数据流
__读文件：__

![reading from HDFS](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/Hadoop_10C71/image_8.png)

* 客户端(client)用FileSystem的open()函数打开文件
* DistributedFileSystem用RPC调用元数据节点，得到文件的数据块信息。
* 对于每一个数据块，元数据节点返回保存数据块的数据节点的地址。
* DistributedFileSystem返回FSDataInputStream给客户端，用来读取数据。
* 客户端调用stream的read()函数开始读取数据。
* DFSInputStream连接保存此文件第一个数据块的最近的数据节点。
* Data从数据节点读到客户端(client)
* 当此数据块读取完毕时，DFSInputStream关闭和此数据节点的连接，然后连接此文件下一个数据块的最近的数据节点。
* 当客户端读取完毕数据的时候，调用FSDataInputStream的close函数。
* 在读取数据的过程中，如果客户端在与数据节点通信出现错误，则尝试连接包含此数据块的下一个数据节点。
* 失败的数据节点将被记录，以后不再连接。

__写文件：__

![writing to HDFS](http://images.cnblogs.com/cnblogs_com/forfuture1978/WindowsLiveWriter/Hadoop_10C71/image_10.png)

	客户端调用create()来创建文件
	DistributedFileSystem用RPC调用元数据节点，在文件系统的命名空间中创建一个新的文件。
	元数据节点首先确定文件原来不存在，并且客户端有创建文件的权限，然后创建新文件。
	DistributedFileSystem返回DFSOutputStream，客户端用于写数据。
	客户端开始写入数据，DFSOutputStream将数据分成块，写入data queue。
	Data queue由Data Streamer读取，并通知元数据节点分配数据节点，用来存储数据块(每块默认复制3块)。分配的数据节点放在一个pipeline里。
	Data Streamer将数据块写入pipeline中的第一个数据节点。第一个数据节点将数据块发送给第二个数据节点。第二个数据节点将数据发送给第三个数据节点。
	DFSOutputStream为发出去的数据块保存了ack queue，等待pipeline中的数据节点告知数据已经写入成功。
	如果数据节点在写入的过程中失败：
		关闭pipeline，将ack queue中的数据块放入data queue的开始。
		当前的数据块在已经写入的数据节点中被元数据节点赋予新的标示，则错误节点重启后能够察觉其数据块是过时的，会被删除。
		失败的数据节点从pipeline中移除，另外的数据块则写入pipeline中的另外两个数据节点。
		元数据节点则被通知此数据块是复制块数不足，将来会再创建第三份备份。
	当客户端结束写入数据，则调用stream的close函数。此操作将所有的数据块写入pipeline中的数据节点，并等待ack queue返回成功。最后通知元数据节点写入完毕。

__数据的距离：__
在同一台机器上距离为0，在同机架上的距离为2，再同cluster不同机架上位4，不同cluster为6.

__Block可见性：__
正在写的Block是对读的用户不可见的。

#### 辅助工具
* Flume 是一个讲大的流数据移动到HDFS的工具。
* Sqoop 是迁移大数据到HDFS的，如Relational databases。
* distcp 并行复制工具。但可能破坏HDFS的balance。 __balancer__
* Hadoop Archives 打包小文件
