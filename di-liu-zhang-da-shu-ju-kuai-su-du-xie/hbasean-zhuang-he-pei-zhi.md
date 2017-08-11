## 6.3 HBase安装和配置

在安装HBase之前，请先安装好Hadoop集群，Hadoop集群具体的安装和配置请参照之前的文档。

整个步骤为，安装和配置Zookeeper，安装和配置HBase。

| HostName | IP | HBase角色 | Zookeeper | Hadoop角色 |
| :--- | :--- | :--- | :--- | :--- |
| hadoopmaster | 192.168.44.131 | master | YES | master |
| hadoopslave1 | 192.168.44.132 | Region Server | YES | slave1 |
| hadoopslave2 | 192.168.44.133 | Region Server | YES | slave2 |

首先将HBase和Zookeeper安装包上传到服务器端：

**解压HBase压缩文件。**

**修改hbase-env.sh文件，修改JAVA\_HOME的地址。**

```
vi hbase-env.conf
export JAVA_HOME=/home/jdk1.8.0_131
```

**修改hbase-site.xml**

```
vi hbase-site.conf

<configuration>
  <property>
    <name>hbase.rootdir</name>
    <value>hdfs://hadoopmaster:9000/hbase</value>
  </property>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>hadoopmaster,hadoopslave1,hadoopslave2</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/hadoop/hbase-1.2.6/zookeeper</value>
  </property>
</configuration>
```

hbase.rootdir指定Hbase数据存储目录

hbase.cluster.distributed 指定是否是完全分布式模式，单机模式和伪分布式模式需要将该值设为false

hbase.zookeeper.quorum 指定zooke的集群，多台机器以逗号分隔

hbase.zookeeper.property.dataDir指定ZooKeeper的zoo.conf中的配置。 快照的存储位置线上配置：/home/hadoop/zookeeperData,默认值：${hbase.tmp.dir}/zookeeper，将会覆盖zookeeper默认的设置

**修改regionservers，将文件内容设置为:**

```
hadoopslave1
hadoopslave2
```

配置环境变量方便未来使用。



