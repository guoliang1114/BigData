# **第六章 大数据快速读写HBase**

作为NoSQL家庭的一员，HBase的出现弥补了Hadoop只能离线批处理的不足，同时能够存储小文件，提供海量数据的随机检索，并保证一定的性能。而这些特性也完善了整个Hadoop生态系统，泛化其大数据的处理能力，结合其高性能、稳定、扩展性好的特行，给使用大数据的企业带来了福音。

**HBase基本概念**

HBase是基于列设计的，那什么是基于列呢？

首先看下关系型数据库的表结构，第一行是table的所有列，第二行开始就是各个列对应的值：

| ID | name | age | birthday |
| :--- | :--- | :--- | :--- |
| 1 | George1 | 12 | 1990-01-01 |
| 2 | George2 | 13 | 1990-01-02 |
| 3 | George3 | 15 | 1990-01-03 |

HBase表结构是这样的

| Row Key | Time Stamp | ColumnFamily contents | ColumnFamily names |
| :--- | :--- | :--- | :--- |
| me.format.hbase | t1 | contents:format='George1' |  |
| me.format.hbase | t2 | contents:title='title1' |  |
| me.fotmat.hbase | t3 |  | names:gogog='data1' |

从上面这个HBase表的例子来说明HBase的存储结构。

Row Key：行的键值，其实就相当于这一行的标识符。上面的数据其实只有1行，因为他们的标识符是一样的。

TimeStamp：时间戳，创建数据的时间戳，hbase默认会自动生成

ColumnFamily：列的前缀，一列可以存储多条数据，具体存储什么类型的数据还需要另外一个标示符qualify，上面那个例子中，contents和names就是两个Column Family

ColumnFamily qualify：列前缀后的标识符，一个ColumnFamily可以有多个qualify。上面那个例子中format和title就是contents这个ColumnFamily的qualify。gogogo是names这个ColumnFamily的qualify

