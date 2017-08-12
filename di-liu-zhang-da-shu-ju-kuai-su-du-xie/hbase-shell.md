## 6.4 HBase Shell

HBase包含可以与HBase进行通信的Shell。 HBase使用Hadoop文件系统来存储数据。它拥有一个主服务器和区域服务器。数据存储将在区域\(表\)的形式。这些区域被分割并存储在区域服务器。

主服务器管理这些区域服务器，所有这些任务发生在HDFS。下面给出的是一些由HBase Shell支持的命令。

**通用命令**

* **status:**提供HBase的状态，例如，服务器的数量。
* **version:**提供正在使用HBase版本。
* **table\_help:**表引用命令提供帮助。
* **whoami:**提供有关用户的信息。

在安装和配置里我们已经使用过了status命令。

**数据定义命令**

这些是关于HBase在表中操作的命令。

* **create:**

  创建一个表。

* **list:**  
  列出HBase的所有表。

* **disable:**  
  禁用表。

* **is\_disabled:**  
  验证表是否被禁用。

* **enable:**  
  启用一个表。

* **is\_enabled:**
  验证表是否已启用。
* **describe:**
  提供了一个表的描述。
* **alter:**
  改变一个表。
* **exists:**
  验证表是否存在。
* **drop:**
  从HBase中删除表。
* **drop\_all:**
  丢弃在命令中给出匹配“regex”的表。
* **Java Admin API:**
  在此之前所有的上述命令，Java提供了一个通过API编程来管理实现DDL功能。在这个org.apache.hadoop.hbase.client包中有HBaseAdmin和HTableDescriptor 这两个重要的类提供DDL功能。

**数据操作命令**

* **put:**

  把指定列在指定的行中单元格的值在一个特定的表。

* **get:**  
  取行或单元格的内容。

* **delete:**  
  删除表中的单元格值。

* **deleteall:**
  删除给定行的所有单元格。
* **scan:**
  扫描并返回表数据。
* **count:**
  计数并返回表中的行的数目。
* **truncate:**
  禁用，删除和重新创建一个指定的表。
* **Java client API:**
  在此之前所有上述命令，Java提供了一个客户端API来实现DML功能，CRUD（创建检索更新删除）操作更多的是通过编程，在org.apache.hadoop.hbase.client包下。 在此包HTable 的 Put和Get是重要的类。

要退出交互shell命令，在任何时候键入 exit 或使用&lt;Ctrl + C&gt;

。进一步处理检查shell功能之前，使用 list 命令用于列出所有可用命令。list是用来获取所有HBase 表的列表。首先，验证安装HBase在系统中使用如下所示。

