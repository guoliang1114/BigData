## 4.2 Ambari安装

**安装准备**

关于 Ambari 的安装，目前网上能找到两个发行版，一个是 Apache 的 Ambari，另一个是 Hortonworks 的，两者区别不大。这里就以 Apache 的 Ambari 2.5.1 作为示例。本文使用三台 Redhat 7.2 作为安装环境，三台机器分别为hadoopmaster、hadoopslave1、hadoopslave2。 hadoopmaster计划安装为 Ambari 的 Server，另外两台为 Ambari Agent。

安装 Ambari 最方便的方式就是使用公共的库源（public repository）。有兴趣的朋友可以自己研究一下搭建一个本地库（local repository）进行安装。这个不是重点，所以不在此赘述。在进行具体的安装之前，需要做几个准备工作。

1. SSH 的无密码登录；Ambari 的 Server 会 SSH 到 Agent 的机器，拷贝并执行一些命令。因此我们需要配置 Ambari Server 到 Agent 的 SSH 无密码登录。在这个例子里， hadoopmaster可以 SSH 无密码登录 hadoopslave1 和 hadoopslave2。在之前安装hadoop的时候已经准备过来了。
2. 确保 Yum 可以正常工作；通过公共库（public repository），安装 Hadoop 这些软件，背后其实就是应用 Yum 在安装公共库里面的 rpm 包。所以这里需要您的机器都能访问 Internet。
3. 确保 home 目录的写权限。Ambari 会创建一些 OS 用户。
4. 确保机器的 Python 版本大于或等于 2.7.5.（Redhat7.2，默认就是 2.7.5 的）。
5. 确保机器安装了Maven 3.5.0。

> 以上的检查点，如果之前在hadoop安装的时候已经有详细的说明。
>
> 如果未安装wget，请使用yum install wget
>
> 所有的安装过程都可以参考官方文档：[https://cwiki.apache.org/confluence/display/AMBARI/Installation+Guide+for+Ambari+2.5.1](https://cwiki.apache.org/confluence/display/AMBARI/Installation+Guide+for+Ambari+2.5.1)
>
> 安装maven
>
> ```
> #下载
> cd /home/apache-maven
> wget https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.5.0/binaries/apache-maven-3.5.0-bin.tar.gz
> tar -xvf apache-maven-3.5.0-bin.tar.gz
>
> #配置环境变量
> vi /etc/profile
> export M2_HOME=/home/apache-maven/apache-maven-3.5.0
> export M2=$M2_HOME/bin
> export PATH=$M2:$PATH
> ```

**安装过程**

安装过程概述:

**Step 1**: 下载及编译Ambari 2.5.1源码。

**Step 2**: 安装Ambari Server

**Step 3**: 设置和启动Ambari Server

**Step 4**: 在hadoopslave1、hadoopslave2上安装和启动Ambari Agent

**Step 5**: 使用Ambari Web UI部署集群



首先需要获取 Ambari 的公共库文件（public repository）。登录到 Linux 主机并执行下面的命令（也可以自己手工下载）：

```
#安装需要使用的库
yum install -y rpm-build

#也可以使用其他镜像
$wget http://www.apache.org/dist/ambari/ambari-2.5.1/apache-ambari-2.5.1-src.tar.gz 
$tar xfvz apache-ambari-2.5.1-src.tar.gz
$cd apache-ambari-2.5.1-src
#设置主目录版本号
$mvn versions:set -DnewVersion=2.5.1.0.0

#ambari-metrics打上版本
$pushd ambari-metrics
$mvn versions:set -DnewVersion=2.5.1.0.0
$popd
```

整个过程需要联网下载，使用maven进行编译和打包，因此需要花费一定的时间。

继续执行打包命令

```
mvn -B clean install package rpm:rpm -DnewVersion=2.5.1.0.0 -DskipTests -Dpython.ver="python >= 2.6"
```



