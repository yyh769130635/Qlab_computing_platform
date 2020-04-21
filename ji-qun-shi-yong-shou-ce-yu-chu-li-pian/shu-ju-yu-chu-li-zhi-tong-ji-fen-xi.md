---
description: 作者：杨煜涵         时间：2020-4-19
---

# 数据预处理之PySpark

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

6.求最大最小值

方法比较多：

```text
# # Method 1: Use describe()
# float(df.describe("Age").filter("summary = 'max'").select("Age").collect()[0].asDict()['Age'])

# # Method 2: Use SQL
# df.registerTempTable("df_table")
# spark.sql("SELECT MAX(Age) as maxval FROM df_table").collect()[0].asDict()['maxval']

# #Method 3: Use groupby()
# df.groupby().max("Age").collect()[0].asDict()['max(A)']//不太好用

# # Method 4: Convert to RDD
# df.select("Age").rdd.max()[0]

# # Method 5: Convert to RDD
max(df.select("Age").collect())
```

7.删选信息

```text
df.filter(df["Age"]>24).show()
```

8.groupBy+count

```text
df.groupBy("Sex").count().show()
```

9.sort and orderBy

```text
df.sort(df["Fare"], ascengding=True).show()
```

10.null值的填充

```text
df1=df.fillna({"Age": 20})
df1.show()
```

