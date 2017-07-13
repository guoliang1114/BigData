#  {#hadoop-272完全分布式的搭建}

## 3.2 Hadoop 完全分布式的搭建

### 3.2.1 系统环境

* VMWare Fusion 8.5.7
* RedHat Linux Server 7.2
* JDK 7U79
* Apache Hadoop 2.8

### 3.2.2 网络环境

* hadoopmaster 192.168.1.159
* hadoopslave1 192.168.1.76
* hadoopslave2 192.168.1.166

### 3.2.2 RedHat Linux Server 7.2 安装和准备

使用VMWare Fusion安装虚拟机，使用最小安装。安装完毕后拷贝2份副本。

| 名称 | 配置信息 | IP | 备注 |
| :---: | :---: | :---: | :---: |
| hadoopmaster | 1C/1G/30G | 192.168.44.131 |  |
| hadoopslave1 | 1C/1G/30G | 192.168.44.132 |  |
| hadoopslave2 | 1C/1G.30G | 192.168.44.133 |  |



> 备注:由于使用MacBook Pro搭建环境，因此配置信息都不是太大，大家在学习中可以根据自身情况调整。



### 3.2.3 Linux服务器



