## **Vertica集群环境安装和配置**

## 安装规划

操作系统： rhel-server-6.6-x86\_64

Vertica：vertica-8.1.0

| 服务器名称 | IP | 描述 |
| :--- | :--- | :--- |
| db1 | 192.168.44.135 |  |
| db2 | 192.168.44.136 |  |
| db3 | 192.168.44.137 |  |

## **安装Vertica**

准备虚拟机，安装Redhat 6.6操作系统

**网络设置**

`vi /etc/sysconfig/network-scripts/ifcfg-enoXXXX`+

设置ONBOOT=yes+

执行

`service network restart`

然后使用 ifconfig查看服务器IP信息。

**设置hostname**

为了方便后续操作，设置服务器名称

`hostname db1`

以上命令修改会立即生效，但是重启后就会失效，使用以下命令修改：

```
vi /etc/sysconfig/network

NETWORKING=yes
HOSTNAME=db1
```

**安装MC（Management Console）**



