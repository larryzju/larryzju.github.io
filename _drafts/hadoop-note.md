---
layout: post
title: hadoop 基础学习笔记
categories: bigdata
---

# 前言

面对大数据造成的存储和处理的挑战，基本解决思路如下：

1. 分布式的存储：提高读写速度
2. 硬件故障容错：冗余副本
3. 计算框架易用：避免过多的底层细节干扰

Hadoop 包括 HDFS、YARN 及 MapReduce，也泛指大数据生态环境总称，常用的其它组件有：

* Zookeeper
* Kafka
* Impala, Hive, Tez
* Spark
* Spark Streaming, Storm, Samza
* Solr
* Oozie, Azkaban

# 概述

MapReduce 框架试用于一次写入多次使用（write once and read many times）的情况。它与 HDFS 是密切相关的，关键在于数据本地化

> 不同于简单的共享存储加分布式计算，单纯的共享存储将在网络IO处产生瓶颈，如何让 **计算靠近数据** 才是 Hadoop 真正解决的问题

# MapReduce

MapReduce 计算框架将任务简化为 map 和 reducer 阶段，前者处理并行任务中独立不相关的每个部分，后者用于聚合 map 产生的结果。

以 hdfs 文件处理为例，对文件的每一个块（block）发起一个 map 任务（task），以计算靠近数据的方式进行计算，结果保存在本地存储上；reduce 任务输入来源于一个或多个 map 的结果的全部或一部分，以 map 生成的 key 作为分区键（shuffle/partition），生成最终结果，保存于 hdfs。

> Shuffle 是整个流程性能优化的关键

另外，在一些计算中，map 的结果可以进行一步 combiner，以减少输出量。

除了使用 java API 编写 MapReduce，Hadoop 还提供了 Streaming 方式，通过 UNIX 管道进行输入输出流的分布式，可以使用任何语言进行处理。


# HDFS

分布式的文件系统，用于处理大文件、一次写入多次使用、批量处理、容错存储的场景。

HDFS 的关键特性如下：

1. 块存储

   HDFS 中的文件默认以 128MB 的块大小来存储，分块存储的优势在于：
   
   1. 可以支持超大文件（超过了单个硬盘存储大小）
   2. 管理简单
   3. 支持简单的副本机制
   
2. Master & Slave 结构

   HDFS 的节点分为两类：NameNode 和 DataNode。
   
   * NameNode 负责管理文件系统（目录结构）及块存储元信息
   * DataNode 负责进行具体的块存储和管理
   
3. HDFS Fedoration

   类似于 Linux/Unix 中将不同的文件系统 mount 到同一个目录树中，存在多个 NameNode，分别管理一个子文件系统和目录结构。主要用于支持更大文件数量场景。
   
   
4. HDFS Secondary NameNode

   NameNode 角色在 HDFS 中特殊且重要，它将文件系统元信息 metadata 存储于内存之中，以 namespace image 和 edit log 存在。
   
   引入 Secondary NameNode，作为恢复点（CheckPoint），如果 NameNode 出现故障，可以从 Secondary NameNode 同步的最近一次记录中恢复
   
   > 注意，从 Secondary NameNode 恢复将 **丢失最近的修改**，且因为需要重放 edit log 并等待各 DataNode 块信息收集，恢复需要相当长的时间
   > 要避免这类问题，可以使用 HA 策略
   
5. HDFS High Availability

   为了解决 Secondary NameNode 恢复慢及丢失最近修改的情况，引入更复杂的机制进行多 NameNode 热切换，主要的机制如下：
   
   1. DataNode 同时向 Active 和 Standby NameNode 汇报块信息
   2. NameNode 之间通过 QJM ( Quorum Journal Manager ) 或 NFS 共享 edit log 内容，以进行快速的编辑日志重放
   3. 通过 ZooKeeper 实现 Active 和 Standby NameNode 的热切换
   4. 客户端通过一个虚拟的 NameNode 名称访问整个 NameNode 组中正在作用的一个
   
   
Hadoop 文件系统并非只有 HDFS，常见的还有 local, ftp, har 等，类似于 Linux 文件系统中可以有 ext4 或 ntfs 一样。HDFS 类似于 ext4，而 Hadoop 文件系统则是相当于 Linux 的虚拟文件系统。

通常，我们操作 HDFS 文件，实际上是在通过 Hadoop 文件系统接口（FileSystem Interface）访问 DistributedFileSystem。同样，通过 URI scheme 可以访问不同类型的文件系统。

> 在真实计算中，HDFS 配合 MapReduce 可以实现数据本地优化，实现高并发且有效的处理过程

HDFS 的副本存储考虑到了带宽占用和安全性，其策略可归纳为：

* 主块位于写入客户端所在 DataNode
* 第二副本位于另一机架（rack）保证可靠性
* 第三副本从第二副本复制，位于第二副本所在机架另一节点
* 其它副本随机分配，但同时兼顾平衡性
   
# YARN

原始的 MapReduce 是一套计算框架加分布式调度平台。YARN 把计算框架独立出去（如 MapReduce 和 Spark），仅关注其中的资源管理与应用调度部分。

YARN 由两部分构成：

1. ResourceManager
   a. Scheduler
   b. ApplicationManager
2. NodeManager

Scheduler 负责对各节点的资源进行管理和维护，而 ApplicationManager 负责对各 ApplicationMaster 进行管理和维护。

NodeManager 将每个机器的计算资源（如 CPU 核、内存）进行虚拟化，形成一个最小虚拟单元 container。

当一个应用提交到 YARN 时流程如下：

1. 通过 ApplicationManager 申请一个 Container 作为 ApplicationMaster
2. ApplicationMaster 通过 Scheduler 申请具体的计算资源，进行计算



# TODO

* hdfs namenode image + edit log
* remode procedure call

