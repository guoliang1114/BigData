## 5.2 Hive环境准备

安装Hive的过程可以概括为以下几步:

* 验证JAVA的安装
* 验证Hadoop的安装
* 准备MySQL环境
* 安装Hive
* 配置Hive

> 默认情况下Hive将元数据保存在Derby数据库中，但是仅支持一个会话连接，适合简单测试。为了支持多用户多会话，则需要一个独立的元数据库，我们使用 MySQL 作为元数据库，Hive 内部对 MySQL 提供了很好的支持。

5.2.1 验证JAVA的安装

```
[root@hadoopmaster ~]# java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

java安装的版本是64位的JDK 1.8。

5.2.2 验证Hadoop的安装

```
[root@hadoopmaster ~]# hadoop version
Hadoop 2.8.0
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 91f2b7a13d1e97be65db92ddabc627cc29ac0009
Compiled by jdu on 2017-03-17T04:12Z
Compiled with protoc 2.5.0
From source with checksum 60125541c2b3e266cbf3becc5bda666
This command was run using /hadoop/hadoop-2.8.0/share/hadoop/common/hadoop-common-2.8.0.jar
```

默认安装的Hadoop的版本是 2.8.0

