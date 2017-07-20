## 4.2 Ambari安装

**安装准备**

关于 Ambari 的安装，目前网上能找到两个发行版，一个是 Apache 的 Ambari，另一个是 Hortonworks 的，两者区别不大。这里就以 Apache 的 Ambari 2.5.1 作为示例。本文使用三台 Redhat 7.2 作为安装环境，三台机器分别为hadoopmaster、hadoopslave1、hadoopslave2。 hadoopmaster计划安装为 Ambari 的 Server，另外两台为 Ambari Agent。

安装 Ambari 最方便的方式就是使用公共的库源（public repository）。有兴趣的朋友可以自己研究一下搭建一个本地库（local repository）进行安装。这个不是重点，所以不在此赘述。在进行具体的安装之前，需要做几个准备工作。

1. 


