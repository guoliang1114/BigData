## 5.2 Hive环境准备

Hive的安装模式也有三种：

1. **内嵌模式**。将元数据保存在本地内嵌的 Derby 数据库中，这是使用 Hive 最简单的方式。但是这种方式缺点也比较明显，因为一个内嵌的 Derby 数据库每次只能访问一个数据文件，这也就意味着它不支持多会话连接。
2. **本地模式**。这种模式是将元数据保存在本地独立的数据库中（一般是 MySQL），这用就可以支持多会话和多用户连接了。
3. **远程模式**。此模式应用于 Hive 客户端较多的情况。把 MySQL 数据库独立出来，将元数据保存在远端独立的 MySQL 服务中，避免了在每个客户端都安装 MySQL 服务从而造成冗余浪费的情况。

安装Hive的过程可以概括为以下几步:

* 验证JAVA的安装
* 验证Hadoop的安装
* 准备MySQL环境
* 安装Hive
* 配置Hive



### 5.2.1 验证JAVA的安装

```
[root@hadoopmaster ~]# java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

java安装的版本是64位的JDK 1.8。

### 5.2.2 验证Hadoop的安装

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

### 5.2.3 准备MySQL环境



