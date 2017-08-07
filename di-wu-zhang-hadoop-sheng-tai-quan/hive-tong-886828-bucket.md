## 5.9 桶表\(bucket\)

桶表和分区表有类似的地方，都是将数据文件划分到不同的地方存储。不同的是，分区表是由用户指定数据处处的位置。而桶表是根据数据表的某个字段去hash值，然后模桶的数量，最终放到其中一个桶中，但是最终放到哪一个桶中，用户是不知道的。

**创建桶表**

```
create table bucket_table(id string) clustered by(id) into 4 buckets;
```

**往桶表中加载数据**

往桶表中加载数据不能再使用LOAD DATA 的方式，要采取以下的方式：

```
set hive.enforce.bucketing = true;
#有可能花些时间
insert into table bucket_table select name from multi_clumns_table;
insert overwrite table bucket_table select name from stu;

```

在HDFS的查看文件,如命令行所示，创建了4个bucket。

![](/assets/5.9_2.png)

抽样检查

```
#抽样检查
select * from bucket_table tablesample(bucket 1 out of 4 on id);
```



