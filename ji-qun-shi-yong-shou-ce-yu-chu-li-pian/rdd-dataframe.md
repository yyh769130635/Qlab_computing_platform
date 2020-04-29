---
description: 作者：杨煜涵           时间：2020-4-22
---

# spark数据结构

![RDD \(Spark1.0\) &#x2014;&amp;gt; Dataframe\(Spark1.3\) &#x2014;&amp;gt; Dataset\(Spark1.6\)](../.gitbook/assets/image%20%2814%29.png)

## RDD（Resilient Distributed Datasets）

an RDD is a read-only, partitioned collection of records. 字面意思是只读的分布式数据集。类似于关系数据库 里的一个个操作，比如 map，filter，Join 等。在 Spark 里面实现了许多这样的 RDD 类，即可以看成是操作类。当我们调用一个 map 接口，底层实现是会生成一个 MapPartitionsRDD 对象，当 RDD 真正执行时，会调用 MapPartitionsRDD 对象里面的 compute 方法来执行这个操作的计算逻辑。但是不同的是，RDD 是 lazy 模式，只有像 count，saveasText 这种 action 动作被调用后再会去触发 runJob 动作。

RDD 分为二类：transformation 和 action。

* transformation 是从一个 RDD 转换为一个新的 RDD 或者从数据源生成一个新的 RDD；
* action 是触发 job 的执行。所有的 transformation 都是 lazy 执行，只有在 action 被提交的时候才触发前面整个 RDD 的执行

### 适用场景

* 你需要使用low-level的transformation和action来控制你的数据集； 
* 你的数据集非结构化，比如：流媒体或者文本流； 
* 你想使用函数式编程来操作你的数据，而不是用特定领域语言\(DSL\)表达； 
* 你不在乎schema，比如，当通过名字或者列处理\(或访问\)数据属性不在意列式存储格式； 
* 你放弃使用DataFrame和Dataset来优化结构化和半结构化数据集

## DataFrame

DataFrame与RDD相同之处，都是不可变分布式弹性数据集。不同之处在于，**DataFrame的数据集都是按指定列存储，即结构化数据**。类似于传统数据库中的表。DataFrame的设计是为了让大数据处理起来更容易。DataFrame允许开发者把结构化数据集导入DataFrame，并做了higher-level的抽象；DataFrame提供特定领域的语言\(DSL\)API来操作你的数据集。

