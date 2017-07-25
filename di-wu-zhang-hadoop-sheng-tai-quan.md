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

Flume是Cloudera提供的一个高可用、高可靠、分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据。同时，Flume提供对数据进行简单处理并写到各种数据接受方（可定制）的能力，如图2-14所示。



