---
layout: post
title: HBase split 策略备忘
categories: hbase
---

# 概述

从物理上，HBase 表将 KeyValue 数据以 HFile 存储并分布于多个节点（RegionServer）上。

从逻辑上看，HBase 表中的数据严格按照 rowkey 排序，不同片段的 rowkey 被分配到一个 region，并由一个 RegionServer 托管。写入操作时，向相应的 region 插入数据，由 region server 负责缓存和 region 操作。

物理与逻辑上的结合在于，region 中可以包含多个 HFile（StoreFile），region server 直接管理 region。

常见的 region 操作包含两种：合并（Compact）和分片（Split）：
* Compact: 将 region 中的多个 hfile 合并成一个更大的 hfile，并清理里面的过期和删除数据
* Split: 如果 region 中的某个 hfile 过大，则分裂为两个 region，由单独的 region server 管理

HBase 中影响性能最大的莫过于合并和分裂：前者提高查询性能，但进行时占用大量资源，并且阻塞 put 等修改操作；后者提高并发入库性能，但牵扯数据迁移，并会增大 region 个数，增大 region server 处理压力。

本文主要针对 split 方面。我们希望将 split 控制在这样一种效果：对于增长的数据量能够有效的分区，并将分区总数控制在一定范围。

compact 部分将在后续文章中加以分析说明。
