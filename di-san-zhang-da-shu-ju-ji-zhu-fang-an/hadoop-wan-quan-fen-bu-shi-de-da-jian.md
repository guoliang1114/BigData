#  {#hadoop-272完全分布式的搭建}

## 3.2 Hadoop 完全分布式的搭建

### 3.2.1 系统环境

* VMWare Fusion 8.5.7
* RedHat Linux Server 7.2
* JDK-8u131-linux-x64
* Apache Hadoop 2.8

### 3.2.2 网络环境

* hadoopmaster 192.168.1.159
* hadoopslave1 192.168.1.76
* hadoopslave2 192.168.1.166

### 3.2.3 RedHat Linux Server 7.2 安装和准备

使用VMWare Fusion安装虚拟机，使用最小安装。安装完毕后拷贝2份副本。

| 名称 | 配置信息 | IP | 备注 |
| :---: | :---: | :---: | :---: |
| hadoopmaster | 1C/1G/30G | 192.168.44.131 |  |
| hadoopslave1 | 1C/1G/30G | 192.168.44.132 |  |
| hadoopslave2 | 1C/1G.30G | 192.168.44.133 |  |

> 备注:由于使用MacBook Pro搭建环境，因此配置信息都不是太大，大家在学习中可以根据自身情况调整。

### 3.2.4 Linux 7.2 服务器配置

由于使用了最小安装，因此需要进行以下设置：

#### 网络设置

`vi /etc/sysconfig/network-scripts/ifcfg-enoXXXX`

设置ONBOOT=yes

执行

`service network restart`

然后使用 ip addr show查看服务器IP信息。

> Redhat 7.2不推荐使用ifconfig命令，也建议使用ip命令来查看和设置网络信息

#### 设置hostname

为了方便后续操作，设置服务器名称

`hostname hadoopmaster`

以上命令修改会立即生效，但是重启后就会失效，使用以下命令修改：

`hostnamectl set-hostname hadoopmaster`

### 3.2.5 JDK安装和配置

首先解压jdk  
`tar xvfz jdk-8u131-linux-x64.tar.gz`

设置JAVA环境变量

`vi /etc/profile        
export JAVA_HOME=/home/jdk1.8.0_131`

`export JRE_HOME=${JAVA_HOME}/jre`

`export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib`

`export PATH=${JAVA_HOME}/bin:$PATH`

执行source /etc/profile立即生效。

### 3.2.6 Hadoop安装和配置

1. 创建用户和用户组。

`groupadd hadoop`

`adduser -g hadoop hadoop`

### 3.2.7 无密码登陆

ssh-key-gen在hadoopmaster主机上创建公钥与密钥。需要注意一下,一定要用hadoop用户生成公钥,因为我们是免密钥登录用的是hadoop。

  
`[hadoop@hadoopmaster /]$ ssh-keygen -t rsa`

`Generating public/private rsa key pair.`

`Enter file in which to save the key (/home/hadoop/.ssh/id_rsa):`

`Created directory '/home/hadoop/.ssh'.`

`Enter passphrase (empty for no passphrase):`

`Enter same passphrase again:`

`Your identification has been saved in /home/hadoop/.ssh/id_rsa.`

`Your public key has been saved in /home/hadoop/.ssh/id_rsa.pub.`

`The key fingerprint is:`

`fd:63:be:4e:cc:29:0f:d3:64:c4:e1:eb:ed:f1:de:72 hadoop@hadoopmaster`

`The key's randomart image is:`

`+--[ RSA 2048]----+`

`| . |`

`|  o .  |`

`| + |`

`|  . . .  |`

`| S . + |`

`|  O o  |`

`| + @ o |`

`|  O o.oE|`

`|  .=..++|`

`+-----------------+`





