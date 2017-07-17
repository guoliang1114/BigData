## 3.3 Hadoop组件介绍

Hadoop包含以下内容:

**Hadoop Common**: 为Hadoop的其它项目提供一些常用工具，主要包括系统配置工具Configuration、远程过程调用RPC、序列化机制和Hadoop抽象文件系统FieSystem等。

**Hadoop Distributed File System \(HDFS™\)**: Hadoop分布式文件系统，是Hadoop体系中数据存储管理的基础。

**Hadoop YARN**: 一个集群资源管理系统框架。

**Hadoop MapReduce**: 是一种计算模型，用于进行大数据量的计算。



### 3.3.1 Hadoop HDFS 原理介绍

     Hadoop分布式文件系统（HDFS）被设计成适合运行在通用硬件（commodity hardware）上的分布式文件系统。它和现有的分布式文件系统有很多共同点，同时，它和其他的分布式文件系统的区别也是很明显的。HDFS是一个高度容错性的系统，适合部署在廉价的机器上。HDFS能提供高吞吐量的数据访问，非常适合大规模数据集上的应用。HDFS放宽了一部分POSIX约束，来实现流式读取文件系统数据的目的。HDFS最开始是作为Apache Nutch搜索引擎项目的基础架构而开发的，HDFS是Apache Hadoop Core项目的一部分。



