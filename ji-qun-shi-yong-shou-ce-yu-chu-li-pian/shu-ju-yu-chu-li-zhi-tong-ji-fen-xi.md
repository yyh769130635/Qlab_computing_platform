---
description: 作者：杨煜涵         时间：2020-4-19
---

# 数据预处理之统计分析

1.打印列索引

```text
df.printSchema()
```

2.对某一列做数据统计

```text
df.describe("Survived").show()
```

3.统计缺失率

```text
import pyspark.sql.functions as fn 
df_miss=df.agg(*[ (1-fn.count(c)/(fn.count('*'))).alias(c ) for c in df.columns ]) 
df_miss.show()
```

4.统一某一列缺失的个数

```text
df_most=df.select("Age").groupBy("Age").count().orderBy("count",ascending=False)
df_most.show()
```

5.求众数

```text
df_most=df.select("Age").groupBy("Age").count().orderBy("count",ascending=False) 
df_most.show()
```

6.

