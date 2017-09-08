## 7.1 Spark 集群安装和配置

Spark的集群架构图

![](/assets7/7.1.1_0.png)

* Driver Program就是程序员所设计的Spark程序，在Spark程序中必须定义SparkContext，它是开发Spark应用程序的入口。
* SparkContext通过Cluster Manager管理整个集群，集群中包含多个worker Node，在每个worker node上都有Executor负责执行任务。

本文的Spark安装基于YARN安装。

安装环境:

> 虚拟机: VMWare Fusion 8.5.7
> 操作系统: RedHat Linux Server 7.2
> JDK环境: JDK-8u131-linux-x64 Hadoop+YARN: Apache Hadoop 2.8
> scala-2.10.6 Spark: spark-2.2.0

安装规划:

| 服务器名称 | 服务器IP | 备注 |
| :--- | :--- | :--- |
| hadoopmaster | 192.168.44.131 | Spark master |
| hadoopslave1 | 192.168.44.132 | Spark Work1 |
| hadoopslave2 | 192.168.44.133 | Spark Work2 |

安装步骤：

1. 操作系统准备
2. 安装和配置Hadoop和YARN
3. 安装Scala
4. 安装和配置Spark

**安装过程：**

由于操作系统、Hadoop、YARN已经在之前的文档的安装过了，本节不做叙述了

**安装Scala**



