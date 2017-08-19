## 6.5 数据定义

### 6.5.1 创建表

可以使用命令创建一个表，在这里必须指定表名和列族名。在HBase shell中创建表的语法如下所示。

```
create ‘<table name>’,’<column family>’
```

示例

下面给出的是一个表名为emp的样本模式。它有两个列族：“personal data”和“professional data”。

```
hbase(main):002:0> create 'emp', 'personal data', 'professional data'
```

| Row  Key | personal data | professinonal data |
| :--- | :--- | :--- |
|  |  |  |

如上表所示。

接着我们使用list验证创建

```
hbase(main):003:0> list
TABLE                                                                                                                                                                                
emp                                                                                                                                                                                  
1 row(s) in 0.0590 seconds

=> ["emp"]
```

### 6.5.2 禁用表

可以使用list查看有哪些表。

要删除表或改变其设置，首先需要使用 disable 命令关闭表。使用 enable 命令，可以重新启用它。

```
disable ‘emp’
hbase(main):006:0> desc 'emp'
Table emp is DISABLED                                                                                                                                                                
emp                                                                                                                                                                                  
COLUMN FAMILIES DESCRIPTION                                                                                                                                                          
{NAME => 'personal data', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION =>
 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                                                                                  
{NAME => 'professional data', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSIO
N => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                                                                              
2 row(s) in 0.0630 seconds
```

同时也可以使用is\_disabled来查看表是否被禁用

```
hbase(main):010:0> is_disabled 'emp'
true                                                                                                                                                                                 
0 row(s) in 0.0260 seconds
```

### 6.5.3 启用表

启用表的语法：

```
hbase(main):011:0> enable 'emp'
0 row(s) in 1.3130 seconds
```

也可以使用is\_enabled来判断表是否为启用状态。

```
hbase(main):001:0> is_enabled 'emp'
true                                                                                                                                                                                 
0 row(s) in 0.5310 seconds

hbase(main):002:0>
```

### 6.5.4 表描述和修改

describe返回对表的说明。

```
hbase(main):002:0> desc 'emp'
Table emp is ENABLED                                                                                                                                                                 
emp                                                                                                                                                                                  
COLUMN FAMILIES DESCRIPTION                                                                                                                                                          
{NAME => 'personal data', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSION =>
 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                                                                                  
{NAME => 'professional data', BLOOMFILTER => 'ROW', VERSIONS => '1', IN_MEMORY => 'false', KEEP_DELETED_CELLS => 'FALSE', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', COMPRESSIO
N => 'NONE', MIN_VERSIONS => '0', BLOCKCACHE => 'true', BLOCKSIZE => '65536', REPLICATION_SCOPE => '0'}                                                                              
2 row(s) in 0.1550 seconds
```

alter用于更改现有表的命令。使用此命令可以更改列族的单元，设定最大数量和删除表范围运算符，并从表中删除列家族。

### 6.5.5 删除表

删除表分两步，首先禁用表，再删除表。

```
disable 'emp'
drop 'emp'
```

### 6.5.6 修改表

修改表首先也要禁用表，再修改表

```
语法: alter 't1', {NAME => 'f1'}, {NAME => 'f2', METHOD => 'delete'}
例如：修改表test1的cf的TTL为180天
hbase(main)> disable 'test1'
hbase(main)> alter 'test1',{NAME=>'body',TTL=>'15552000'},{NAME=>'meta', TTL=>'15552000'}
hbase(main)> enable 'test1'
```

### 6.5.7 权限管理

分配权限

```
语法 : grant <user> <permissions> <table> <column family> <column qualifier> 
参数后面用逗号分隔权限用五个字母表示： "RWXCA".
READ('R'), WRITE('W'), EXEC('X'), CREATE('C'), ADMIN('A')
例如，给用户‘test'分配对表t1有读写的权限，
hbase(main)> grant 'test','RW','t1'
```

使用以下命令查看权限

```
语法：user_permission <table>
例如，查看表t1的权限列表
hbase(main)> user_permission 't1'
```

回收权限

```
与分配权限类似，语法：revoke <user> <table> <column family> <column qualifier>
例如，收回test用户在表t1上的权限
hbase(main)> revoke 'test','t1'
```

### 6.5.8 数据操作

添加数据

```
语法：put <table>,<rowkey>,<family:column>,<value>,<timestamp>
例如：给表t1的添加一行记录：rowkey是rowkey001，family name：f1，column name：col1，value：value01，timestamp：系统默认
hbase(main)> put 't1','rowkey001','f1:col1','value01'
```



