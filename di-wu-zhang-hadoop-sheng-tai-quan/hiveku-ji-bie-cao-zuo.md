## 5.6 库级别操作

### 5.6.1 查看库

使用Show DATABASES;

```
0: jdbc:hive2://localhost:10000/test> SHOW DATABASES;
+----------------+
| database_name  |
+----------------+
| default        |
| test           |
+----------------+
2 rows selected (1.283 seconds)
```

默认情况下，hive安装完成之后，会有一个default库。

创建一个数据库testdb;

```
0: jdbc:hive2://localhost:10000/test> CREATE DATABASE IF NOT EXISTS testdb;
No rows affected (0.562 seconds)
0: jdbc:hive2://localhost:10000> show databases;
 +----------------+--+
 | database_name  |
 +----------------+--+
 | default        |
 | testdb         |
+----------------+--+
```

hive中的库的概念对应于在hive-site.xml中配置项hive.metastore.warehouse.dir指定目录的一个子目录。

例如刚才创建的testdb在hdfs中对应的目录为：![](/assets/5.6.1_1.png)

目录的名称就是"库名.db"

在库中创建的表实际上对应的"库名.db "下的子目录。

需要注意的是，在default库中创建的表会直接出现在/user/hive/warehouse目录下，因此/user/hive/warehouse下可能会同时存在表和库。如果是表的话，就表示的是default库中的表，如果是库的话，则目录以.db结尾。

  


