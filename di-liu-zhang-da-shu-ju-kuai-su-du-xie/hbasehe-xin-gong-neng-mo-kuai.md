## 6.2 HBase核心模块

Hadoop框架包含两个核心组件：HDFS和MapReduce，其中HDFS是文件存储系统，负责数据存储；MapReduce是计算框架，负责数据计算。它们之间分工明确、低度耦合、相关关联。对于HBase数据库的核心组件，即核心功能模块共有4个，它们分别是：客户端Client、协调服务模块ZooKeeper、主节点HMaster和Region节点RegionServer，这些组件的描述和相互之间的关联关系如图所示。

![](/assets/6.2_1.png)

