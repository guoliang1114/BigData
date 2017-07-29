## 5.2 Hive环境准备

Hive的安装模式也有三种：

1. **内嵌模式**。将元数据保存在本地内嵌的 Derby 数据库中，这是使用 Hive 最简单的方式。但是这种方式缺点也比较明显，因为一个内嵌的 Derby 数据库每次只能访问一个数据文件，这也就意味着它不支持多会话连接。
2. **本地模式**。这种模式是将元数据保存在本地独立的数据库中（一般是 MySQL），这用就可以支持多会话和多用户连接了。
3. **远程模式**。此模式应用于 Hive 客户端较多的情况。把 MySQL 数据库独立出来，将元数据保存在远端独立的 MySQL 服务中，避免了在每个客户端都安装 MySQL 服务从而造成冗余浪费的情况。

本次安装Hive的使用本地模式，过程可以概括为以下几步:

* 验证JAVA的安装
* 验证Hadoop的安装
* 准备MySQL环境
* 安装Hive
* 配置Hive

### 5.2.1 验证JAVA的安装

```
[root@hadoopmaster ~]# java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
```

java安装的版本是64位的JDK 1.8。

### 5.2.2 验证Hadoop的安装

```
[root@hadoopmaster ~]# hadoop version
Hadoop 2.8.0
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r 91f2b7a13d1e97be65db92ddabc627cc29ac0009
Compiled by jdu on 2017-03-17T04:12Z
Compiled with protoc 2.5.0
From source with checksum 60125541c2b3e266cbf3becc5bda666
This command was run using /hadoop/hadoop-2.8.0/share/hadoop/common/hadoop-common-2.8.0.jar
```

默认安装的Hadoop的版本是 2.8.0

### 5.2.3 准备MySQL环境

下载地址：

[https://downloads.mysql.com/archives/community/](https://downloads.mysql.com/archives/community/)

找到合适的版本进行安装：

```
yum install mysql-community-*.rpm
#安装完毕后启动
service mysqld start
#用户的密码保存在/var/log/mysqld.log文件中，找到如下行：
#2017-07-27T08:55:10.532965Z 1 [Note] A temporary password is generated for root@localhost: HleLF(Ou4=QV

#如果需要重置Mysql账户密码，请执行以下语句
mysql_secure_installation

#访问mysql
[root@hadoopmaster /]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 15
Server version: 5.7.18 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

#创建Hive用户并分配权限
mysql>CREATE USER 'hive' IDENTIFIED BY '1qaz@WSX';
mysql>GRANT ALL PRIVILEGES ON *.* TO 'hive'@'%' IDENTIFIED BY '1qaz@WSX' WITH GRANT OPTION;
mysql>flush privileges;

#创建数据库
mysql>create database hive;
```

### 5.2.4 安装Hive

准备下载Hive，在官网上找到如下说明:

25 July 2017 : release 2.3.0 available This release works with Hadoop 2.x.y

选择2.3.0版本。

```
#使用hadoop用户
cd /hadoop
tar -zxvf apache-hive-2.3.0-bin.tar.gz

#修改环境变量
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:/usr/local/hive/lib
$source /etc/profile

#修改配置文件
#hive-env.sh,hive-default.xml 

cd /apache-hive-2.3.0-bin/conf
#指定hadoop的目录
cp hive-env.sh.template hive-env.sh
vi hive-env.sh
HADOOP_HOME=/hadoop/hadoop-2.8.0

#修改hive-default.xml 文件
#指定MySQL数据库驱动、数据库名、用户名及密码
cp hive-default.xml.template hive-default.xml
vi hive-default.xml
#修改以下内容
#javax.jdo.option.ConnectionURL参数指定的是Hive连接数据库的连接字符串；
#javax.jdo.option.ConnectionDriverName参数指定的是驱动的类入口名称；
#javax.jdo.option.ConnectionUserName参数指定了数据库的用户名；
#javax.jdo.option.ConnectionPassword参数指定了数据库的密码。
```



