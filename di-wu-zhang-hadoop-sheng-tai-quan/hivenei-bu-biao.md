## 5.7 Hive 内部表

Hive内部表与数据库中的Table在概念上是类似。

* 每一个Table在Hive中都有一个相应的目录存储数据。例如，一个表test，它在HDFS中的路径为：$/warehouse/test。warehouse是在hive-site.xml中由${hive.metastore.warehouse.dir}指定的数据仓库的目录
* 所有的Table数据（不包括External Table）都保存在这个目录中。
* 删除表时，元数据与数据都会被删除

**创建内部表**

```
create table t_managed_table(id int);
```

这种方式创建的表就是内部表。由于HIVE是数据仓库工具，因此其并没有插入的命令，只有批量导入数据的命令。

```
hive> create table t_managed_table(id int);
OK
Time taken: 1.483 seconds
hive> LOAD DATA LOCAL INPATH '/home/hadoop/1.txt' INTO TABLE t_managed_table;
Loading data to table default.t_managed_table
OK
Time taken: 2.574 seconds
hive> select *  from t_managed_table;
OK
1
2
3
4
5
6
7
8
9
10
Time taken: 2.97 seconds, Fetched: 10 row(s)
```

在这里，id.txt中的内容会被导入到t\_managed\_table的id列。虽然我们并没有指定数据导入到哪一列，但是因为t\_managed\_table中只有一个列id，所以id.txt中的数据只能导入到这一列中。

此时我们在控制台上，可以看到![](/assets/5.7_1.png)说明;

> 对于命令LOAD DATA LOCAL INPATH '/tmp/root/id.txt' INTO TABLE t\_managed\_table;
>
> 如果包含LOCAL，则意味着我们导入的数据是从本地，因此导入的数据文件要填写本地文件的路径
>
> 如果不包含LOCAL，则意味着我们要从HDFS中导入数据，填写的路径应该是HDFS中的路径。

**多列的内部表**

```
CREATE TABLE multi_clumns_table(id int,name string) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';
```

这个语句的意思是，以制表符"\t"来区分数据文件中不同的列。所以当我们导入数据的时候，我们的数据文件中也需要用制表符。

准备数据文件2.txt

```
1       tianshouzhi
2       wangxiaoxiao
3       wanghanao
4       huhuamin
5       luyang
```

导入数据

```
hive> LOAD DATA LOCAL INPATH '/home/hadoop/2.txt' INTO TABLE multi_clumns_table;
Loading data to table default.multi_clumns_table
OK
Time taken: 8.806 seconds
hive> select *  from multi_clumns_table;
OK
1    tianshouzhi
2    wangxiaoxiao
3    wanghanao
4    huhuamin
5    luyang
Time taken: 2.414 seconds, Fetched: 5 row(s)
```

注意，HIVE在查询的时候，除了select \* from 表名不需要走map/reduce之外，其他的语句都要走map/reduce。因为select \* 表示的是全表扫描，全表扫描实际上就是获取HDFS文件中所有内容。

