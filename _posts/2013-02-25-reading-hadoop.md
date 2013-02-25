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
