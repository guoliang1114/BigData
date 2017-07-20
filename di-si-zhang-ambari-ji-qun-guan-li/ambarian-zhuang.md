## 4.2 Ambari安装

**安装准备**

关于 Ambari 的安装，目前网上能找到两个发行版，一个是 Apache 的 Ambari，另一个是 Hortonworks 的，两者区别不大。这里就以 Apache 的 Ambari 2.0.1 作为示例。本文使用三台 Redhat 6.6 作为安装环境（目前测试验证结果为 Ambari 在 Redhat 6.6 的版本上运行比较稳定），三台机器分别为 zwshen37.example.com、zwshen38.example.com、zwshen39.example.com。zwshen37 计划安装为 Ambari 的 Server，另外两台为 Ambari Agent。

