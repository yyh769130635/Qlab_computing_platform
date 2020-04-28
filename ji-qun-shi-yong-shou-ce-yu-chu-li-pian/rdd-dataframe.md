---
description: 作者：杨煜涵           时间：2020-4-22
---

# spark数据结构

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

## DataFrame

与RDD类似，DataFrame也是一个分布式数据容器。然而DataFrame更像传统数据库的二维表格，除了数据以外，还记录数据的结构信息，即schema。同时，与Hive类似，DataFrame也支持嵌套数据类型（struct、array和map）。从API易用性的角度上看，DataFrame API提供的是一套高层的关系操作，比函数式的RDD API要更加友好，门槛更低。由于与R和Pandas的DataFrame类似，Spark DataFrame很好地继承了传统单机数据分析的开发体验 

![](../.gitbook/assets/image%20%2836%29.png)

DataFrame是为数据提供了Schema的视图。可以把它当做数据库中的一张表来对待。DataFrame也是懒执行的。**但性能上比RDD要高。**

### 使用场景

* 如果你需要丰富的语义、高级抽象和特定领域专用的API
* 如果你的处理需要对半结构化数据进行高级处理，如filter、map、aggregation、average、sum、SQL查询、列式访问或使用lambda函数 
* 如果你想在编译时就有高度的类型安全，想要有类型的JVM对象，用上Catalyst优化，并得益于Tungsten生成的高效代码，那就使用Dataset； 
* 如果你想在不同的Spark库之间使用一致和简化的API，那就使用DataFrame或Dataset； 
* 如果你是R语言使用者，就用DataFrame； 如果你是Python语言使用者，就用DataFrame，
* 在需要更细致的控制时就退回去使用RDD；

## DataSet

从Spark 2.0开始，Dataset开始具有两种不同类型的API特征：有明确类型的API和无类型的API。从概念上来说，你可以把DataFrame当作一些通用对象Dataset\[Row\]的集合的一个别名，而一行就是一个通用的无类型的JVM对象。与之形成对比，Dataset就是一些有明确类型定义的JVM对象的集合，通过你在Scala中定义的Case Class或者Java中的Class来指定。

![](../.gitbook/assets/image%20%2831%29.png)

注意：因为Python和R没有编译时类型安全，所以我们只有称之为DataFrame的无类型API。

## 三者区别总结

### RDD：

1. **RDD一般和spark mlib同时使用**
2. RDD不支持spark sql操作

### DataFrame

1. 与RDD和Dataset不同，DataFrame每一行的类型固定为Row，只有通过解析才能获取各个字段的值
2. **DataFrame与Dataset一般不与spark ml同时使用**
3. DataFrame与Dataset均支持sparksql的操作，比如select，groupby之类，还能注册临时表/视窗，进行sql语句操作
4. DataFrame与Dataset支持一些特别方便的保存方式，比如保存成csv，可以带上表头，这样每一列的字段名一目了然

### DataSet

1. Dataset和DataFrame拥有完全相同的成员函数，区别只是每一行的数据类型不同
2. DataFrame也可以叫Dataset\[Row\],每一行的类型是Row，不解析，每一行究竟有哪些字段，各个字段又是什么类型都无从得知。而Dataset中，每一行是什么类型是不一定的，在自定义了case class之后可以很自由的获得每一行的信息
3. 如果要写一些适配性很强的函数时，如果使用Dataset，行的类型又不确定，可能是各种case class，无法实现适配，这时候用DataFrame即Dataset\[Row\]



