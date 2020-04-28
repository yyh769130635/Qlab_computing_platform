---
description: 作者：杨煜涵           时间：2020-4-22
---

# RDD、Dataframe

![](../.gitbook/assets/image%20%2827%29.png)

RDD \(Spark1.0\) —&gt; Dataframe\(Spark1.3\) —&gt; Dataset\(Spark1.6\)

## RDD

RDD，英文全称叫 Resilient Distributed Datasets。 an RDD is a read-only, partitioned collection of records. 字面意思是只读的分布式数据集。 简单的可以把 RDD 理解为关系数据库 里的一个个操作，比如 map，filter，Join 等。在 Spark 里面实现了许多这样的 RDD 类，即可以看成是操作类。当我们调用一个 map 接口，底层实现是会生成一个 MapPartitionsRDD 对象，当 RDD 真正执行时，会调用 MapPartitionsRDD 对象里面的 compute 方法来执行这个操作的计算逻辑。但是不同的是，RDD 是 lazy 模式，只有像 count，saveasText 这种 action 动作被调用后再会去触发 runJob 动作。

 RDD 分为二类：transformation 和 action。 transformation 是从一个 RDD 转换为一个新的 RDD 或者从数据源生成一个新的 RDD； action 是触发 job 的执行。所有的 transformation 都是 lazy 执行，只有在 action 被提交的时候才触发前面整个 RDD 的执行

### 使用场景

* 你希望可以对你的数据集进行最基本的转换、处理和控制；
*  你的数据是非结构化的，比如流媒体或者字符流； 
* 你想通过函数式编程而不是特定领域内的表达来处理你的数据； 
* 你不希望像进行列式处理一样定义一个模式，通过名字或字段来处理或访问数据属性； 
* 你并不在意通过DataFrame和Dataset进行结构化和半结构化数据处理所能获得的一些优化和性能上的好处；

## DataFrame && DataSet

与RDD类似，DataFrame也是一个分布式数据容器。然而DataFrame更像传统数据库的二维表格，除了数据以外，还记录数据的结构信息，即schema。同时，与Hive类似，DataFrame也支持嵌套数据类型（struct、array和map）。从API易用性的角度上看，DataFrame API提供的是一套高层的关系操作，比函数式的RDD API要更加友好，门槛更低。由于与R和Pandas的DataFrame类似，Spark DataFrame很好地继承了传统单机数据分析的开发体验 

![](../.gitbook/assets/image%20%2835%29.png)

DataFrame是为数据提供了Schema的视图。可以把它当做数据库中的一张表来对待。DataFrame也是懒执行的。**但性能上比RDD要高。**

