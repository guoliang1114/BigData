## 4.3 Cloudera CDH安装和配置

CDH — Cloudera 分发的 Apache Hadoop 和其他相关开放源代码项目，包括 Impala 和 Cloudera Search。CDH 还提供安全保护以及与许多硬件和软件解决方案的集成。

* Cloudera Manager — 一个复杂的应用程序，用于部署、管理、监控您的 CDH 部署并诊断问题。Cloudera Manager 提供 Admin Console，这是一种基于 Web 的用户界面，使您的企业数据管理简单而直接。它还包括 Cloudera Manager API，可用来获取群集运行状况信息和度量以及配置 Cloudera Manager。
* Cloudera Navigator — CDH 平台的端到端数据管理工具。Cloudera Navigator 使管理员、数据经理和分析师能够了解 Hadoop 中的大量数据。Cloudera Navigator 中强大的审核、数据管理、沿袭管理和生命周期管理使企业能够遵守严格的法规遵从性和法规要求。
* Cloudera Impala — 一种大规模并行处理 SQL 引擎，用于交互式分析和商业智能。其高度优化的体系结构使它非常适合用于具有联接、聚合和子查询的传统 BI 样式的查询。它可以查询来自各种源的 Hadoop 数据文件，包括由 MapReduce 作业生成的数据文件或加载到 Hive 表中的数据文件。YARN 和 Llama 资源管理组件让 Impala 能够共存于使用 Impala SQL 查询并发运行批处理工作负载的群集上。您可以通过 Cloudera Manager 用户界面管理 Impala 及其他 Hadoop 组件，并通过 Sentry 授权框架保护其数据。

安装Cloudera的CDH的方式主要有三种方法：

* PATH A： 通过Cloudera Manager自动安装 \(不适合生产环境\)

* PATH B： Installation Using Cloudera Manager Parcels or Packages

* PATH C： Manual Installation Using Cloudera Manager Tarballs

由于PATH A比较简单，且不适合生产环境，为了学习使用PATH B来安装。

**软件准备**

由于在国内连接cloudera的速度不稳定，因此可以首先准备好一些安装包：

需要下载的软件有下面4个，文件内容：

* cm5.12.0-centos6.tar.gz
* CDH-5.12.0-1.cdh5.12.0.p0.29-el6.parcel
* CDH-5.12.0-1.cdh5.12.0.p0.29-el6.parcel.sha1
* manifest.json

注意区别（**有el6的代表的是centos6的**），我们就下载含有el6的文件和manifest.json文件

本地通过Parcel安装过程与本地通过Package安装过程完全一致，不同的是两者的本地源的配置。 区别如下：

Package本地源：软件包是.rpm格式的，数量通常较多，下载的时候比较麻烦。通过"createrepo ."的命令创建源，并要放到存放源文件主机的web服务器的根目录下，详见创建本地yum软件源，为本地Package安装Cloudera Manager、Cloudera Hadoop及Impala做准备

Parcel本地源：软件包是以.parcel结尾，相当于压缩包格式的，一个系统版本对应一个，下载的时候方便。如centos 6.x使用的CDH版本为CDH-4.3.0-1.cdh4.3.0.p0.22-el6.parcel，而centos 5.x使用的CDH版本为CDH-4.3.0-1.cdh4.3.0.p0.22-el5.parcel。

因此我们使用Parcel的方式来安装。

### 4.3.1 开发环境安装规划

| 名称 | IP | 服务器名 | 配置信息 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| Cloudera \(Master\) - NN + RM + ZK + ISS | 172.24.222.69 | hadoop1.foton.com.cn | 300G |  |
| Cloudera \(Data\)+ZK | 172.24.222.70 | hadoop2.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.71 | hadoop3.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.72 | hadoop4.foton.com.cn | 800G |  |
| Cloudera \(Gateway\) - Flume, Scoop, Hue, Kafka, Spark ，ZK  ，Cloudera \(Cloudera Manager + Database\) | 172.24.222.73 | hadoop5.foton.com.cn | 800G |  |

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

**配置时钟**

计划使用hadoop5作为时钟服务器

```
yum install ntp
service ntpd restart
#设置开机启动
chkconfig ntpd on

#其他服务器设置定时同步任务
crontab -e
00 12 * * * root /usr/sbin/ntpdate hadoop5 >> /root/ntpdate.log 2>&1
#查看任务
crontab -l
```

**修改linux swap空间的swappiness**

Cloudera 建议将 /proc/sys/vm/swappiness 设置为 0,最大不超过10。

修改swappiness的值为零：

```
root@hadoop5:cat /proc/sys/vm/swappiness
root@hadoop5:sysctl vm.swappiness=0
root@hadoop5:echo 0 > /proc/sys/vm/swappiness
```

**大页面压缩设置**

该设置有可能引发重大性能问题

```
echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag
echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
vi /etc/rc.local
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
#1. 本地yum源
加载系统盘，搭建yum源，就不再次赘述了。

#2.cloudera.repo
找到对应的repo文件。

#3.搭建本地cloudera源
#安装apache
#解压cm5.12.0-centos6.tar.gz

#编辑cloudera.repo文件
name = Cloudera Manager, Version 5.12.0
baseurl = http://172.24.222.73/cm5/redhat/6/x86_64/cm/5.12.0/
gpgkey = http://172.24.222.73/cm5/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera
gpgcheck = 1

#4 拷贝parcel文件到目录 /opt/cloudera/parcel-repo
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

![](/assets/4.3_16.png)激活后，进入检查服务器的过程，不建议跳过。

检查结果

![](/assets/4.3_17.png)处理完问题后，

![](/assets/4.3_18.png)

参考以下内容：

![](/assets/4.3_19.png)

![](/assets/4.3_20.png)

![](/assets/4.3_21.png)

执行安装和启动

![](/assets/4.3_22.png)

![](/assets/4.3_25.png)

接着审核安装的内容，配置使用默认即可。

```
 #scm服务重启命令
 service cloudera-scm-server restart
```

异常处理

```
#启动cloudera scm server的时候
3WARN653222119@scm-web-0:com.cloudera.server.web.cmf.csrf.CsrfRefererInterceptor: Rejecting request originating from xx.xx.xx.xxforhttp://xxx.xxxx.lo/cmf/express-wizard/wizard with referrer
http://xxx.xxxx.lo
```

```
#直接注释掉csrf
[root@xx.xx.xx.xx cloudera-scm-server]# cd /usr/share/cmf/
[root@xx.xx.xx.xx cmf]# grep -i -r  csrf  ./
Binary file ./cloudera-navigator-server/libs/cdh5/hadoop-yarn-server-nodemanager-2.6.0-cdh5.5.0.jar matches
Binary file ./common_jars/hadoop-yarn-server-nodemanager-2.6.0-cdh5.5.0.jar matches
Binary file ./common_jars/server-5.6.0.jar matches
Binary file ./common_jars/hadoop-yarn-server-resourcemanager-2.5.0-cdh5.3.2.jar matches
./webapp/WEB-INF/spring/mvc-config.xml:                <bean class="com.cloudera.server.web.cmf.csrf.CsrfRefererInterceptor" />
Binary file ./lib/cdh5-java6/hadoop-yarn-server-resourcemanager-2.5.0-cdh5.3.2.jar matches
Binary file ./lib/server-5.6.0.jar matches
Binary file ./lib/cdh5/hadoop-yarn-server-nodemanager-2.6.0-cdh5.5.0.jar matches
[root@xx.xx.xx.xx cmf]# vi ./webapp/WEB-INF/spring/mvc-config.xml
注释掉这个bean，然后重启server，在访问nginx，整个世界清净了·~~~~~~~~~~~~~~~~~~~~~~~~~~·
              <!--  <bean class="com.cloudera.server.web.cmf.csrf.CsrfRefererInterceptor" /> -->
```

> 参考文档：
>
> [https://www.cloudera.com/documentation/enterprise/latest/topics/cm\_ig\_install\_path\_b.html\#id\_gzv\_zmm\_25\_\_d10688e306](https://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_install_path_b.html#id_gzv_zmm_25__d10688e306)
>
> [http://www.jianshu.com/p/57179e03795f](http://www.jianshu.com/p/57179e03795f)
>
> [http://www.th7.cn/db/nosql/201708/248333.shtml](http://www.th7.cn/db/nosql/201708/248333.shtml)



