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

DataFrame与RDD相同之处，都是不可变分布式弹性数据集。不同之处在于，**DataFrame的数据集都是按指定列存储，即结构化数据**。类似于传统数据库中的表。DataFrame的设计是为了让大数据处理起来更容易。DataFrame允许开发者把结构化数据集导入DataFrame，并做了higher-level的抽象；DataFrame提供特定领域的语言\(DSL\)API来操作你的数据集。除了数据以外，还记录数据的结构信息，即schema。同时，与Hive类似，DataFrame也支持嵌套数据类型（struct、array和map）。从API易用性的角度上看，DataFrame API提供的是一套高层的关系操作，比函数式的RDD API要更加友好，门槛更低。由于与R和Pandas的DataFrame类似，Spark DataFrame很好地继承了传统单机数据分析的开发体验。 

![](../.gitbook/assets/image%20%2834%29.png)

上图直观地体现了DataFrame和RDD的区别。左侧的RDD\[Person\]虽然以Person为类型参数，但Spark框架本身不了解Person类的内部结构。而右侧的DataFrame却提供了详细的结构信息，使得Spark SQL可以清楚地知道该数据集中包含哪些列，每列的名称和类型各是什么。DataFrame多了数据的结构信息，即schema。RDD是分布式的Java对象的集合。DataFrame是分布式的Row对象的集合。DataFrame除了提供了比RDD更丰富的算子以外，更重要的特点是提升执行效率、减少数据读取以及执行计划的优化，比如filter下推、裁剪等。**性能上比RDD要高 。**

## **Dataset**

在Spark 2.0中，Dataset具有两个完全不同的API特征：强类型API和弱类型API，见下表。DataFrame是特殊的Dataset，其每行是一个弱类型JVM object。相对应地，Dataset是强类型JVM object的集合，通过Scala的case class或者Java class。

