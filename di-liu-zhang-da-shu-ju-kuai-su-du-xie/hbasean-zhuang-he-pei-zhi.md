## 6.3 HBase安装和配置

在安装HBase之前，请先安装好Hadoop集群，Hadoop集群具体的安装和配置请参照之前的文档。

整个步骤为，安装和配置Zookeeper，安装和配置HBase。

| HostName | IP | HBase角色 | Zookeeper | Hadoop角色 |
| :--- | :--- | :--- | :--- | :--- |
| hadoopmaster | 192.168.44.131 | master | YES | master |
| hadoopslave1 | 192.168.44.132 | backup master & Region Server | YES | slave1 |
| hadoopslave2 | 192.168.44.133 | Region Server | YES | slave2 |

首先将HBase和Zookeeper安装包上传到服务器端。



