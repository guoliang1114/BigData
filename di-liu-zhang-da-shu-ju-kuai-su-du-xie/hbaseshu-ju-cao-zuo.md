### 6.6 HBase数据操作

添加数据

```
语法：put <table>,<rowkey>,<family:column>,<value>,<timestamp>
例如：给表t1的添加一行记录：rowkey是rowkey001，family name：f1，column name：col1，value：value01，timestamp：系统默认
hbase(main)> put 't1','rowkey001','f1:col1','value01'
```

查询数据

```
语法：get <table>,<rowkey>,[<family:column>,....]
例如：查询表t1，rowkey001中的f1下的col1的值
hbase(main)> get 't1','rowkey001', 'f1:col1'
或者：
hbase(main)> get 't1','rowkey001', {COLUMN=>'f1:col1'}
查询表t1，rowke002中的f1下的所有列值
hbase(main)> get 't1','rowkey001'
```

扫描表

```
语法：scan <table>, {COLUMNS => [ <family:column>,.... ], LIMIT => num}
另外，还可以添加STARTROW、TIMERANGE和FITLER等高级功能
例如：扫描表t1的前5条数据
hbase(main)> scan 't1',{LIMIT=>5}
```

查询表中的数据行数

```
语法：count <table>, {INTERVAL => intervalNum, CACHE => cacheNum}
INTERVAL设置多少行显示一次及对应的rowkey，默认1000；CACHE每次去取的缓存区大小，默认是10，调整该参数可提高查询速度
例如，查询表t1中的行数，每100条显示一次，缓存区为500
hbase(main)> count 't1', {INTERVAL => 100, CACHE => 500}
```



