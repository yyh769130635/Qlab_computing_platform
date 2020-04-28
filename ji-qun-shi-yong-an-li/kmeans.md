---
description: 作者：杨煜涵         时间：2020-4-19
---

# Kmeans

{% tabs %}
{% tab title="方法1" %}
```python
%pyspark
from pyspark.ml.clustering import KMeans
# from pyspark.ml.evaluation import ClusteringEvaluator
dataset = spark.read.format("libsvm")\
.load("hdfs://10.129.2.155:50090/123/data/spark-data/mllib/sample_kmeans_data.txt")
kmeans = KMeans().setK(2).setSeed(1)
model = kmeans.fit(dataset)
wssse = model.computeCost(dataset)
print("Within Set Sum of Squared Errors = " + str(wssse))
centers = model.clusterCenters()
print("Cluster Centers: ")
for center in centers:
    print(center)
```
{% endtab %}

{% tab title="方法2" %}
```python
%pyspark
from pyspark.sql import Row
from pyspark.ml.clustering import KMeans,KMeansModel
from pyspark.ml.linalg import Vectors
rawData = sc.textFile("hdfs://10.129.2.155:50090/123/data/iris.data.txt")
def f(x):
    rel = {}
    rel['features'] = Vectors.dense(float(x[0]),float(x[1]),float(x[2]),float(x[3]))
    return rel
df = sc.textFile("hdfs://10.129.2.155:50090/123/data/iris.data.txt").map(lambda line: line.split(',')).map(lambda p: Row(**f(p))).toDF()
kmeansmodel = KMeans().setK(3).setFeaturesCol('features').setPredictionCol('prediction').fit(df)
results=kmeansmodel.transform(df).collect()
```
{% endtab %}
{% endtabs %}

