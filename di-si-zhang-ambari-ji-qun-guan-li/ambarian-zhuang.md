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

> 安装Ambari的过程非常痛苦，需要使用maven等各种工具。编译的时候需要联网下载各种软件包。

首先需要获取 Ambari 的公共库文件（public repository）。登录到 Linux 主机并执行下面的命令（也可以自己手工下载）：

```
#安装需要使用的库
yum install rpm-build gcc make gcc-c++ openssl-devel git ant python-devel -y

#由于编译需要安装Python 2.6，而redhat默认的版本是2.7，因此需要安装2.6
wget https://www.python.org/ftp/python/2.6.9/Python-2.6.9.tar.xz //下载python2.6包
tar -xf Python-2.6.9.tar.xz //解压python2.6包
cd Python-2.6.9 //切换路径
./configure //配置python源码
make && make install //编译并安装，安装默认在/usr/local/bin/python2.6

mv /usr/bin/python2  /usr/bin/python2.bak
mv /usr/bin/python2-config  /usr/bin/python2-config.bak
ln -s /usr/local/bin/python2.7 /usr/local/bin/python

由于yum使用的是python2.7版本需要编辑
vi /usr/bin/yum
修改第一行
#!/usr/bin/python2.7

vi /usr/libexec/urlgrabber-ext-down
修改第一行
#!/usr/bin/python2.7



#安装工具
sh setuptools-0.6c11-py2.6.egg

执行JVM参数设置
#export _JAVA_OPTIONS="-Xmx2048m -XX:MaxPermSize=512m -Djava.awt.headless=true"

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

整个过程需要联网下载,如果有VPN，请一定要连接VPN。使用maven进行编译和打包，因此需要花费一定的时间。

执行打包命令

```
mvn -B clean install package rpm:rpm -DnewVersion=2.5.1.0.0 -DskipTests -Dpython.ver="python >= 2.6"
```

如果中途出错，去掉clean继续运行。

![](/assets/4.2.1_1.png)

安装Ambari Server

进入目录

```
cd ambari-server/target/rpm/ambari-server/RPMS/noarch/
export buildNumber=2.5.1.0.0
ambari-server start
```

![](/assets/4.2_3.png)

**安装Ambari Agent\(该部分可以使用Manger安装\)**

拷贝到ambari-agent/target/rpm/ambari-agent/RPMS/x86\_64/ 下的rpm包到agent服务器。

```
cp ambari-agent-2.5.1.0-0.x86_64.rpm hadoopslave1:/home
cp ambari-agent-2.5.1.0-0.x86_64.rpm hadoopslave2:/home

在各个服务器上安装agent
[root@hadoopslave1 home]# yum install ambari-agent-2.5.1.0-0.x86_64.rpm 
[root@hadoopslave2 home]# yum install ambari-agent-2.5.1.0-0.x86_64.rpm

启动agent
export buildNumber=2.5.1.0.0
ambari-agent start
```

**设置和启动Ambari Server**

[http://192.168.44.131:8080/](http://192.168.44.131:8080/)

![](/assets/4.3.4_1.png)

首先选择最新版本。接着填写ambria HOST:hadoopslave1,hadoopslave2。

选择hadoopmaster的root私有文件。目录/root/.ssh/id\_rsa

![](/assets/4.2.3_3.png)

![](/assets/3.2.3_4.png)

等待安装完成。

![](/assets/3.2.4——5.png)

选择安装的组件。选的越多，就会需要越多的机器内存。选择之后就可以继续下一步了。这里需要注意某些 Service 是有依赖关系的。如果您选了一个需要依赖其他 Service 的一个 Service，Ambari 会提醒安装对应依赖的 Service。

![](/assets/3.2.3_5.png)

组件规划设置。系统默认会推荐，但是生产环境需要规划和确认。点击下一步进行安装。

接着分别是选择安装软件所指定的 Master 机器和 Slave 机器，以及 Client 机器。这里使用默认选择即可（真正在生产环境中，需要根据具体的机器配置选择）。

![](/assets/4.2_6.png)

ambari会给一个安装汇总，确认无误后点击部署。

![](/assets/4.2_10.png)

访问仪表盘，可以对所有组件和状态进行查看。也可以启动、停止某些服务。

**异常处理**

```
[ERROR] Failed to execute goal on project ambari-metrics-storm-sink: Could not resolve dependencies for project org.apache.ambari:ambari-metrics-storm-sink:jar:2.4.2.0.0: Failure to find org.apache.storm:storm-core:jar:1.1.0-SNAPSHOT in http://repo.hortonworks.com/content/groups/public/ was cached in the local repository, resolution will not be reattempted until the update interval of apache-hadoop has elapsed or updates are forced -> [Help 1]
```

依赖包的问题，需要找到对应的pom.xml文件修改版本

```
<properties>
     <storm.version>1.1.0-SNAPSHOT</storm.version>
 </properties>
```

ambari server重置

如果在安装的过程中发现ambari有相关问题，可以使用命令进行重置

```
ambari-server stop
ambari-server reset
```

libtirpc-devel找不到。hadoop在安装的时候需要使用该包，在系统盘中并无该包，需要自己下载安装

```
wget ftp://mirror.switch.ch/pool/4/mirror/scientificlinux/7.2/x86_64/os/Packages/libtirpc-devel-0.2.4-0.6.el7.x86_64.rpm
yum install libtirpc-devel*.rpm
```



