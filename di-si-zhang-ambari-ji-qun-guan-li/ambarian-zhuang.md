## 4.2 Ambari安装

**安装准备**

关于 Ambari 的安装，目前网上能找到两个发行版，一个是 Apache 的 Ambari，另一个是 Hortonworks 的，两者区别不大。这里就以 Apache 的 Ambari 2.5.1 作为示例。本文使用三台 Redhat 7.2 作为安装环境，三台机器分别为hadoopmaster、hadoopslave1、hadoopslave2。 hadoopmaster计划安装为 Ambari 的 Server，另外两台为 Ambari Agent。

安装 Ambari 最方便的方式就是使用公共的库源（public repository）。有兴趣的朋友可以自己研究一下搭建一个本地库（local repository）进行安装。这个不是重点，所以不在此赘述。在进行具体的安装之前，需要做几个准备工作。

1. SSH 的无密码登录；Ambari 的 Server 会 SSH 到 Agent 的机器，拷贝并执行一些命令。因此我们需要配置 Ambari Server 到 Agent 的 SSH 无密码登录。在这个例子里，zwshen37 可以 SSH 无密码登录 zwshen38 和 zwshen39。
2. 确保 Yum 可以正常工作；通过公共库（public repository），安装 Hadoop 这些软件，背后其实就是应用 Yum 在安装公共库里面的 rpm 包。所以这里需要您的机器都能访问 Internet。
3. 确保 home 目录的写权限。Ambari 会创建一些 OS 用户。
4. 确保机器的 Python 版本大于或等于 2.6.（Redhat6.6，默认就是 2.6 的）。



