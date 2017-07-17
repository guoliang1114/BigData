## 3.3 Hadoop组件介绍

Hadoop包含以下内容:

**Hadoop Common**: 为Hadoop的其它项目提供一些常用工具，主要包括系统配置工具Configuration、远程过程调用RPC、序列化机制和Hadoop抽象文件系统FieSystem等。

**Hadoop Distributed File System \(HDFS™\)**: Hadoop分布式文件系统，是Hadoop体系中数据存储管理的基础。

**Hadoop YARN**: 一个集群资源管理系统框架。

**Hadoop MapReduce**: 是一种计算模型，用于进行大数据量的计算。

### 3.3.1 Hadoop HDFS 原理介绍

Hadoop分布式文件系统（HDFS）被设计成适合运行在通用硬件（commodity hardware）上的分布式文件系统。它和现有的分布式文件系统有很多共同点，同时，它和其他的分布式文件系统的区别也是很明显的。HDFS是一个高度容错性的系统，适合部署在廉价的机器上。HDFS能提供高吞吐量的数据访问，非常适合大规模数据集上的应用。HDFS放宽了一部分POSIX约束，来实现流式读取文件系统数据的目的。HDFS最开始是作为Apache Nutch搜索引擎项目的基础架构而开发的，HDFS是Apache Hadoop Core项目的一部分。

HDFS有着高容错性（fault-tolerant）的特点，并且设计用来部署在低廉的（low-cost）硬件上。而且它提供高吞吐量（high throughput）来访问应用程序的数据，适合那些有着超大数据集（large data set）的应用程序。HDFS放宽了（relax）POSIX的要求（requirements）这样可以实现以流的形式访问（streaming access）文件系统中的数据。

HDFS采用master/slave架构。一个HDFS集群是由一个NameNode和一定数目的DataNodes组成。NameNode是一个中心服务器，负责管理文件系统的名字空间（namespace）以及客户端对文件的访问。集群中的DataNode一般是一个节点一个，负责管理它所在节点上的存储。HDFS暴露了文件系统的名字空间，用户能够以文件的形式在上面存储数据。从内部看，一个文件其实被分成一个或多个数据块，这些块存储在一组DataNode上。NameNode执行文件系统的名字空间操作，例如打开、关闭、重命名文件或目录。它也负责确定数据块到具体DataNode节点的映射。DataNode负责处理文件系统客户端的读写请求。在NameNode的统一调度下进行数据块的创建、删除和复制。下图所示为HDFS的架构图。

![](/assets/3.3.1_1.png)







