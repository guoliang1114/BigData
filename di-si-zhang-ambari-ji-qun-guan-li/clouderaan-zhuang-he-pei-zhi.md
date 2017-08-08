## 4.3 Cloudera CDH安装和配置

安装Cloudera的CDH的方式主要有三种方法：

* PATH A： 通过Cloudera Manager自动安装 \(不适合生产环境\)

* PATH B： Installation Using Cloudera Manager Parcels or Packages

* PATH C： Manual Installation Using Cloudera Manager Tarballs

由于PATH A比较简单，且不适合生产环境，为了学习使用PATH B来安装。

软件准备

由于在国内连接cloudera的速度不稳定，因此可以首先准备好一些安装包：

需要下载的软件有下面5个，下载地址：

注意区别（**有el6的代表的是centos7的**），我们就下载含有el6的文件和manifest.json文件

本地通过Parcel安装过程与本地通过Package安装过程完全一致，不同的是两者的本地源的配置。 区别如下：

Package本地源：软件包是.rpm格式的，数量通常较多，下载的时候比较麻烦。通过"createrepo ."的命令创建源，并要放到存放源文件主机的web服务器的根目录下，详见创建本地yum软件源，为本地Package安装Cloudera Manager、Cloudera Hadoop及Impala做准备

Parcel本地源：软件包是以.parcel结尾，相当于压缩包格式的，一个系统版本对应一个，下载的时候方便。如centos 6.x使用的CDH版本为CDH-4.3.0-1.cdh4.3.0.p0.22-el6.parcel，而centos 5.x使用的CDH版本为CDH-4.3.0-1.cdh4.3.0.p0.22-el5.parcel。

因此我们使用Parcel的方式来安装。

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

配置cloudera的源

从[https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm\_vd.html下载相应的文件，并拷贝到/etc/yum.repos.d/目录下。](https://www.cloudera.com/documentation/enterprise/release-notes/topics/cm_vd.html下载相应的文件，并拷贝到/etc/yum.repos.d/目录下。)

由于网络原因，我们使用离线安装，需要首先准备好相关软件。

* CM
* cloudera-manager-installer
* CDH.parcel

配置系统的yum源

```
#检查系统使用的yum包
rpm -qa |grep yum
#删除redhat自带的yum包
rpm -qa|grep yum|xargs rpm -e --nodeps
#再次检查
rpm -qa |grep yum

#版本冲突处理
rpm -e python-urlgrabber-3.9.1-9.el6.noarch
rpm -ivh python-urlgrabber-3.9.1-11.el6.noarch.rpm

#安装
rpm -ivh yum-metadata-parser-1.1.2-16.el6.x86_64.rpm  yum-plugin-fastestmirror-1.1.30-40.el6.noarch.rpm yum-3.2.29-81.el6.centos.noarch.rpm

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS6-Base-163.repo
```

**安装Cloudera Manager Server **

首先安装JDK

```
yum install oracle-j2sdk1.7
```

安装CM

```
 chmod 777 ./cloudera-manager-installer.bin
 ./cloudera-manager-installer.bin
```

![](/assets/4.3_1.png)

点击下一步。

![](/assets/4.3_2.png)

剩下全部ok安装。

![](/assets/4.3_4.png)根据提示打开浏览器吧。

使用admin/admin登陆。

![](/assets/4.3_7.png)同意条款。

选择评估版本，有60天的使用期限，过期后，会自动降级到cloudera express。

![](/assets/4.3_8.png)指定安装CDH的服务器

![](/assets/4.3_9.png)点击继续，设置存储库

![](/assets/4.3_10.png)此处需要注意设置存储库的位置：

[http://172.24.222.73/cm5/redhat/6/x86\_64/cm/5.12.0/](http://172.24.222.73/cm5/redhat/6/x86_64/cm/5.12.0/)

[http://172.24.222.73/cm5/redhat/6/x86\_64/cm/RPM-GPG-KEY-cloudera](http://172.24.222.73/cm5/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera)

使用本地的存储库

jdk安装，由于之前已经安装了jdk，跳过

![](/assets/3.4_11.png)不启用单用户模式

![](/assets/4.3_12.png)提供SSH登陆凭据

由于CDH会自动管理所有主机间的SSH通讯，所以我们之前并没有手动配置各个节点间的SSH免密登录。在这里统一设置就行了，设置好密码点继续。![](/assets/4.3_13.png)执行安装过程

![](/assets/4.3_14.png)安装成功后，点击继续。

![](/assets/4.3_15.png)

![](/assets/4.3_16.png)

```
 #scm服务重启命令
 service cloudera-scm-server restart
```



