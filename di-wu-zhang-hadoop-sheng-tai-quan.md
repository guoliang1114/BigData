# 第五章 Hadoop生态圈

![](/assets/5.0_1.png)

如图所示，Hadoop的生态圈其实就是一群动物在狂欢。我们来看看一些主要的框架。

（1）HBase

HBase（Hadoop Database）是一个高可靠性、高性能、面向列、可伸缩的分布式存储系统，利用HBase技术可在廉价PC Server上搭建起大规模结构化存储集群。

（2）Hive

Hive是建立在Hadoop上的数据仓库基础构架。它提供了一系列的工具，可以用来进行数据提取转化加载（ETL），这是一种可以存储、查询和分析存储在hadoop中的大规模数据的机制。

（3）Pig

Pig是一个基于Hadoop的大规模数据分析平台，它提供的SQL-LIKE语言叫作Pig Latin。该语言的编译器会把类SQL的数据分析请求转换为一系列经过优化处理的Map-Reduce运算。

（4）Sqoop

Sqoop是一款开源的工具，主要用于在Hadoop（Hive）与传统的数据库（MySQL、post-gresql等）间进行数据的传递，可以将一个关系型数据库中的数据导入Hadoop的HDFS中，也可以将HDFS的数据导入关系型数据库中。

![](/assets/5.0_2.png)

（5）Flume

Flume是Cloudera提供的一个高可用、高可靠、分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据。同时，Flume提供对数据进行简单处理并写到各种数据接受方（可定制）的能力，如下图所示。

![](/assets/5.0_3.png)

（6）Oozie

Oozie是基于Hadoop的调度器，以XML的形式写调度流程，可以调度Mr、Pig、Hive、shell、jar任务等。

主要的功能如下。

1）Workflow：顺序执行流程节点，支持fork（分支多个节点）、join（将多个

节点合并为一个）。

2）Coordinator：定时触发Workflow。

3）Bundle Job：绑定多个Coordinator。

（7）Chukwa

Chukwa是一个开源的、用于监控大型分布式系统的数据收集系统。它构建在Hadoop的HDFS和MapReduce框架上，继承了Hadoop的可伸缩性和鲁棒性。Chukwa还包含了一个强大和灵活的工具集，可用于展示、监控和分析已收集的数据。

（8）ZooKeeper

ZooKeeper是一个开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件，如图2-15所示。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。![](/assets/5.0_5.png)

（10）Mahout

Mahout是Apache Software Foundation（ASF）旗下的一个开源项目，提供一些可扩展的机器学习领域经典算法的实现，旨在帮助开发人员更加方便快捷地创建智能应用程序。Mahout包含许多实现，包括聚类、分类、推荐过滤、频繁子项挖掘。此外，通过使用Apache Hadoop库，可以有效地将Mahout扩展到云中。



