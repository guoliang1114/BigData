# 3.1 Hadoop简介 {#hadoop-272完全分布式的搭建}

### Hadoop是什么？

  Hadoop是一个开源的框架，可编写和运行分布式应用处理大规模数据，是专为离线和大规模数据分析而设计的，并不适合那种对几个记录随机读写的在线事务处理模式。Hadoop=HDFS（文件系统，数据存储技术相关）+ Mapreduce（数据处理），Hadoop的数据来源可以是任何形式，在处理半结构化和非结构化数据上与关系型数据库相比有更好的性能，具有更灵活的处理能力，不管任何数据形式最终会转化为key/value，key/value是基本数据单元。用函数式变成Mapreduce代替SQL，SQL是查询语句，而Mapreduce则是使用脚本和代码，而对于适用于关系型数据库，习惯SQL的Hadoop有开源工具hive代替。

Hadoop就是一个分布式计算的解决方案。

### Hadoop能做什么？

    hadoop擅长日志分析，facebook就用Hive来进行日志分析，2009年时facebook就有非编程人员的30%的人使用HiveQL进行数据分析；淘宝搜索中的自定义筛选也使用的Hive；利用Pig还可以做高级的数据处理，包括Twitter、LinkedIn上用于发现您可能认识的人，可以实现类似Amazon.com的协同过滤的推荐效果。淘宝的商品推荐也是！在Yahoo！的40%的Hadoop作业是用pig运行的，包括垃圾邮件的识别和过滤，还有用户特征建模。（2012年8月25新更新，天猫的推荐系统是hive，少量尝试mahout！）

> 从现在企业的使用趋势来看,Pig慢慢有点从企业的视野中淡化了.+

### Hadoop的版本演进

当前Hadoop有两大版本：Hadoop 1.0和Hadoop 2.0，如下图所示。

![](/assets/import3.1-1.png)

Hadoop版本演进图+

Hadoop1.0被称为第一代Hadoop，由分布式文件系统HDFS和分布式计算框架MapReduce组成，其中，HDFS由一个NameNode和多个DataNode组成，MapReduce由一个JobTracker和多个TaskTracker组成，对应Hadoop版本为0.20.x、0.21.X，0.22.x和Hadoop 1.x。其中0.20.x是比较稳定的版本，最后演化为1. x，变成稳定版本。0.21.x和0.22.x则增加了NameNode HA等新特性。

   第二代Hadoop被称为Hadoop2.0，是为克服Hadoop 1.0中HDFS和MapReduce存在的各种问题而提出的，对应Hadoop版本为Hadoop 0.23.x和2.x。

    针对Hadoop1.0中NameNode HA不支持自动切换且切换时间过长的风险，Hadoop2.0提出了基于共享存储的HA方式，支持失败自动切换切回。

   针对Hadoop 1.0中的单NameNode制约HDFS的扩展性问题，提出了HDFS Federation机制，它允许多个NameNode各自分管不同的命名空间进而实现数据访问隔离和集群横向扩展。

   针对Hadoop 1.0中的MapReduce在扩展性和多框架支持方面的不足，提出了全新的资源管理框架YARN，它将JobTracker中的资源管理和作业控制功能分开，分别由组件ResourceManager和ApplicationMaster实现。其中，ResourceManager负责所有应用程序的资源分配，而ApplicationMaster仅负责管理一个应用程序。相比于 Hadoop 1.0，Hadoop 2.0框架具有更好的扩展性、可用性、可靠性、向后兼容性和更高的资源利用率以及能支持除了MapReduce计算框架外的更多的计算框架，Hadoop 2.0目前是业界主流使用的Hadoop版本。

