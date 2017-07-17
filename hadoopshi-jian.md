## 3.4 Hadoop实践

### 3.4.1 HDFS命令

新建文件夹

```
$hadoop fs -mkdir /u01
$hadoop fs -mkdir /u01/tmp
```

查看文件夹权限

```
$hadoop fs -ls -d /u01/tmp
drwxr-xr-x   - hadoop supergroup          0 2017-07-16 02:43 /u01/tmp
```



