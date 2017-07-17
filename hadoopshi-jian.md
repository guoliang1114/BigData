## 3.4 Hadoop实践

### 3.4.1 HDFS命令

**常用介绍下列常用HDFS命令**

本节所有命令，我们都在master虚拟机的“终端”程序中运行。

| 命令 | 说明 |
| :--- | :--- |
|  |  |

**新建文件夹**

```
$hadoop fs -mkdir /u01
$hadoop fs -mkdir /u01/tmp
```

**查看文件夹权限**

```
$hadoop fs -ls -d /u01/tmp
drwxr-xr-x   - hadoop supergroup          0 2017-07-16 02:43 /u01/tmp
```

**上传文件**

hadoop fs –put \[本地地址\] \[hadoop目录\]

> 同时也可以上传文件夹

```
$hadoop fs –put /home/file.txt /user/u01/

#查看文件夹下内容
[hadoop@hadoopmaster home]$ hadoop fs -ls -R /u01/tmp/
-rw-r--r--   2 hadoop supergroup       1659 2017-07-16 02:57 /u01/tmp/1.txt
```

**查看文件内容**

hadoop dfs –cat \[文件目录\]

```
$hadoop fs -cat /u01/tmp/1.txt
```

**复制、移动、删除文件**

```
#复制
$hadoop fs -cp /u01/tmp/1.txt /u01/tmp/2.txt

#移动
$hadoop fs -mv /u01/tmp/2.txt /u01/tmp/3.txt

#删除
$hadoop fs -rm /u01/tmp/3.txt
Deleted /u01/tmp/3.txt
```

**创建新的空文件**

```
$hadoop fs -touchz /u01/tmp/5.txt
```

**下载Hadoop上的文件**

hadoop fs -get \[文件目录\] \[本地目录\]

```
$hadoop fs -get /u01/tmp/2.txt /home/hadoop
```

**删除文件夹**

```
$hadoop fs –rm -r /u01/tmp
```

### 3.4.2 MapReduce



