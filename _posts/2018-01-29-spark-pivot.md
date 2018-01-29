---
title: spark pivot 小记
layout: post
tags: 计算机
---

# 说明

使用 spark 时有需要将一个窄表转换为宽表（根据某列或某几列的**<u>内容</u>**生成新的列）。

例如有这样一张表 T，其中有三列 A, B, C，假设其内容如下所示

| A    | B    | C    |
| ---- | ---- | ---- |
| a1   | k2   | c12  |
| a2   | k2   | c21  |
| a2   | k1   | c22  |
| a2   | k3   | c23  |

需要将之生成为

| A    | k1   | k2   | k3   |
| ---- | ---- | ---- | ---- |
| a1   | c11  | c12  | null |
| a2   | c22  | c12  | c23  |

# spark pivot 

这种操作在 spark 中术语叫作 pivot（旋转）。在 spark 中需要进行 `df.groupBy(...).pivot(...).agg(...)` 生成新的 `DataFrame`

`pivot` 方法在 `org.apache.spark.sql.RelationalGroupedDataset` 中定义，返回类型仍是 `GroupedData` 类型。看得出 `pivot` 本质上与 `group` 相似，都是在进行聚合。

如上面的例子在 spark 中可以写作:

```scala
df.groupBy($"a").pivot("b").agg(expr("first(c)"))
```

此外，需要注意到，因为 pivot 是对指定列的内容进行聚合，实际上操作分为两步
1. 获取 pivot 指定列的可能取值
2. 对每种可能取值进行聚合

相当于以下 spark 方法

```scala
df.groupBy("a").agg(
    expr("first(case when b = 'k1' then c end, true)").as("k1"),
    expr("first(case when b = 'k2' then c end, true)").as("k2"),
    expr("first(case when b = 'k3' then c end, true)").as("k3")
  ).show
```

> 其中 first 第二个参数表示是否忽略 null 值

为了提高处理性能，可以在 `pivot` 加入第二个参数表示可选取值，即

```scala
def pivot(pivotColumn: String, values: Seq[Any]): RelationalGroupedDataset
```

其作用有二：
1. 减少对 pivot 列可能聚合的统计步骤
2. 限制可能取值范围，只处理我们关心的，减少聚合的开销

另外，`pivot` 只支持对一列进行旋转。如果要对多列（实际上要对多列内容进行合并，生成新的列名）进行操作，则可以先用 `concat` 方法生成一个新的列用于 `pivot`

# 参考资料

* Reshaping Data with Pivot in Apache Spark，by Andrew Ray (文中还比较了 panda(python) 与 reshape2(R) 与 spark(scala) 的 pivot 语法设计)
* spark doc
* stack overflow, How to pivot DataFrame? 