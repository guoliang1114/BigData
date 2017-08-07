## 5.10 外部表

指向已经在HDFS中存在的数据，可以创建Partition

* 它和内部表在元数据的组织上是相同的，而实际数据的存储则有较大的差异
* 内部表的创建过程和数据加载过程（这两个过程可以在同一个语句中完成），在加载数据的过程中，实际数据会被移动到数据仓库目录中；之后对数据对访问将会直接在数据仓库目录中完成。删除表时，表中的数据和元数据将会被同时删除
* 外部表只有一个过程，加载数据和创建表同时完成，并不会移动到数据仓库目录中，只是与外部数据建立一个链接。当删除一个 外部表时，仅删除该链接。

```
CREATE EXTERNAL TABLE page_view
( viewTime INT,
  userid BIGINT,
  page_url STRING,  
  referrer_url STRING,                                                
  ip STRING COMMENT 'IP Address of the User',
  country STRING COMMENT 'country of origination‘
)
    COMMENT 'This is the staging page view table'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' LINES   TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION 'hdfs://hadoopmaster:9000/user/data/page_view';
```

内部表和外部表转换

```
#可以通过如下语句转换外部表和内部表
alter table tablePartition set TBLPROPERTIES ('EXTERNAL'='TRUE');  //内部表转外部表
alter table tablePartition set TBLPROPERTIES ('EXTERNAL'='FALSE');  //外部表转内部表
```

PS. 如果创建外部分区表，hive并不会自动关联hdfs中指定目录的partitions目录。

需要通过：

```
alter table test1 add partition (visitDate=2017-05-23)；
```

进行分区与分区数据的关联。

