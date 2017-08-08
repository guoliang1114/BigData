## 4.3 Cloudera CDH安装和配置

安装Cloudera的CDH的方式主要有三种方法：

* PATH A： 通过Cloudera Manager自动安装 \(不适合生产环境\)

* PATH B： Installation Using Cloudera Manager Parcels or Packages

* PATH C： Manual Installation Using Cloudera Manager Tarballs

由于PATH A比较简单，且不适合生产环境，为了学习使用PATH B来安装。

软件准备

由于在国内连接cloudera的速度不稳定，因此可以首先准备好一些安装包：

需要下载的软件有下面5个，下载地址：

cm5.11.1-centos7.tar.gz   的下载地址：[http://archive.cloudera.com/cm5/repo-as-tarball/5.11.1/cm5.11.1-centos7.tar.gz](http://archive.cloudera.com/cm5/repo-as-tarball/5.11.1/cm5.11.1-centos7.tar.gz)

cloudera-manager-installer.bin 的下载地址：[http://archive.cloudera.com/cm5/installer/5.11.1/cloudera-manager-installer.bin](http://archive.cloudera.com/cm5/installer/5.11.1/cloudera-manager-installer.bin)

CDH-5.11.1-1.cdh5.11.1.p0.4-el7.parcel，CDH-5.11.1-1.cdh5.11.1.p0.4-el7.parcel.sha1，manifest.json这三个文件的下载地址为：[http://archive.cloudera.com/cdh5/parcels/5.11.1/下面](http://archive.cloudera.com/cdh5/parcels/5.11.1/下面)

注意区别（**有el7的代表的是centos7的**），我们就下载含有el7的文件和manifest.json文件

### 4.3.1 开发环境安装规划

| 名称 | IP | 服务器名 | 配置信息 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| Cloudera \(Master\) - NN + RM + ZK + ISS | 172.24.222.69 | hadoop1.foton.com.cn | 300G |  |
| Cloudera \(Data\) | 172.24.222.70 | hadoop2.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.71 | hadoop3.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.72 | hadoop4.foton.com.cn | 800G |  |
| Cloudera \(Gateway\) - Flume, Scoop, Hue, Kafka, Spark   \| Cloudera \(Cloudera Manager + Database\) | 172.24.222.73 | hadoop5.foton.com.cn | 800G |  |

> 以上服务器版本: Red Hat Enterprise Linux Server release 6.6 \(Santiago\)
>
> PS. 用户名是root，密码：Pass1234

**修改各个服务器名称和HOST**

```
172.24.222.69 hadoop1.foton.com.cn hadoop1
172.24.222.70 hadoop2.foton.com.cn hadoop2
172.24.222.71 hadoop3.foton.com.cn hadoop3
172.24.222.72 hadoop4.foton.com.cn hadoop4
172.24.222.73 hadoop5.foton.com.cn hadoop5
```

**关闭selinux和防火墙**

> 需要在所有的节点上执行，因为涉及到的端口太多了，临时关闭防火墙是为了安装起来更方便，安装完毕后可以根据需要设置防火墙策略，保证集群安全。

关闭防火墙

```
#临时关闭
service iptables stop 
#重启后生效 
chkconfig iptables off
```

关闭selinux

```
#临时生效
setenforce 0
#重启后永久生效
#修改 /etc/selinux/config 下的 SELINUX=disabled
```

**配置yum源**

从[https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm\_vd.html下载相应的文件，并拷贝到/etc/yum.repos.d/目录下。](https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm_vd.html下载相应的文件，并拷贝到/etc/yum.repos.d/目录下。)

**安装Cloudera Manager Server **

首先安装JDK

```
yum install oracle-j2sdk1.7
```



