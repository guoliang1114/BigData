## 6.3 HBase安装和配置

在安装HBase之前，请先安装好Hadoop集群，Hadoop集群具体的安装和配置请参照之前的文档。

本次安装我们使用HBase内置的ZooKeeper

| HostName | IP | HBase角色 | Zookeeper | Hadoop角色 |
| :--- | :--- | :--- | :--- | :--- |
| hadoopmaster | 192.168.44.131 | master | YES | master |
| hadoopslave1 | 192.168.44.132 | Region Server | YES | slave1 |
| hadoopslave2 | 192.168.44.133 | Region Server | YES | slave2 |

首先将HBase安装包上传到服务器端：

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

```
 export HBASE_HOME=/hadoop/hbase-1.2.6
 export PATH=$PATH:$HBASE_HOME/bin:$HBASE_HOME/conf
```

**将修改好的安装目录分发到所有节点**，一并修改环境变量。

scp -r /hadoop/hbase-1.2.6  hadoop@hadoopslave1:/hadoop/发送到hadoopslave1上，hadoopslave2的发送都一样，然后分别在slave上做环境变量配置。

**启动HBase**

```
#在主节点运行
./start-hbase.sh
#使用jps查看
[hadoop@hadoopmaster bin]$ jps

16928 SecondaryNameNode
16727 NameNode
29639 HMaster
29754 Jps
17083 ResourceManager
17356 RunJar
```

**验证HBase**

在 master 运行 jps 应该会有HMaster进程。在各个 slave 上运行jps 应该会有HQuorumPeer,HRegionServer两个进程。 在浏览器中输入

[http://hadoopmaster:16010](http://master:16010/)

可以看到 HBase Web UI 。

![](/assets/6.3_1.png)

同时也可以使用hive shell命令查看

```
[hadoop@hadoopslave2 bin]$ ./hbase shell
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/hadoop/hbase-1.2.6/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/hdp/2.6.1.0-129/hadoop/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell; enter 'help<RETURN>' for list of supported commands.
Type "exit<RETURN>" to leave the HBase Shell
Version 1.2.6, rUnknown, Mon May 29 02:25:32 CDT 2017

hbase(main):001:0> status
1 active master, 0 backup masters, 2 servers, 0 dead, 1.0000 average load

hbase(main):002:0> 
```



