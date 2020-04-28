---
description: 作者：杨煜涵         时间：2020-4-19
---

# Zeppelin+sql+pyspark使用

## 资源性能对比

### zeppelin资源配置

zeppelin这个工具非常方便的嵌入了多种编译器，具有数据可视化功能，以及编译器的资源配置功能。相较之前计算资源的通过shell命令的分配，zeppelin可以直接通过web页面指定资源的配置：

![](../.gitbook/assets/image%20%2824%29.png)

### 多核与单核性能对比

实验中，我们选取了10G的数据展示一下多核并行处理计算速度的优势。（需要注意的是，并不是所有计算都能达到加速的效果，对于一些小规模数据集，或者是算法中存在着非常复杂的迭代递归计算，spark并不一定适合），**本次实验主要对比的是cpu核数对计算速度的影响**：

| code | core 1, memory 50G | core 20, memory 50G |
| :--- | :--- | :--- |
| 计算缺失率 | 4 min 50 sec | 34 sec |
| df.select\("City"\).filter\("City is null"\).count\(\) | 2 min 21 sec | 22 sec |
| df.select\("City"\).count\(\) | 2 min 17 sec | 15 sec |
| df.select\("City"\).distinct\(\).count\(\) | 2 min 24 sec | 13 sec |
| 统计缺失个数 | 2 min 22 sec | 14 sec |
| 统计数量并排序 | 2 min 28 sec | 11 sec |
| df.groupBy\("Complaint Type"\).count\(\).show\(\) | 2 min 26 sec | 12 sec |

## zeppelin可视化

