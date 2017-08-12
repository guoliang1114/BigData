## 6.4 HBase Shell

HBase包含可以与HBase进行通信的Shell。 HBase使用Hadoop文件系统来存储数据。它拥有一个主服务器和区域服务器。数据存储将在区域\(表\)的形式。这些区域被分割并存储在区域服务器。

主服务器管理这些区域服务器，所有这些任务发生在HDFS。下面给出的是一些由HBase Shell支持的命令。

**通用命令**

* **status:**提供HBase的状态，例如，服务器的数量。
* **version:**提供正在使用HBase版本。
* **table\_help:**表引用命令提供帮助。
* **whoami:**提供有关用户的信息。



