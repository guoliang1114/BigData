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

默认情况下，hive安装完成之后，会有一个`default`库。  


