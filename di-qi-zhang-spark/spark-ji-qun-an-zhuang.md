## 7.1 Spark 集群安装和配置

Spark的集群架构图

![](/assets7/7.1.1_0.png)

Driver Program就是程序员所设计的Spark程序，在Spark程序中必须定义SparkContext，它是开发Spark应用程序的入口。

SparkContext通过Cluster Manager管理整个集群，集群中包含多个worker Node，在每个worker node上都有Executor负责执行任务。

