## 4.3 Cloudera CDH安装和配置

安装Cloudera的CDH的方式主要有三种方法：

* PATH A： 通过Cloudera Manager自动安装 \(不适合生产环境\)
* PATH B： Installation Using Cloudera Manager Parcels or Packages
* PATH C： Manual Installation Using Cloudera Manager Tarballs

由于PATH A比较简单，且不适合生产环境，为了学习使用PATH B来安装。

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
service iptables stop （临时关闭）  
chkconfig iptables off （重启后生效）
```

关闭selinux



