## **Vertica集群环境安装和配置**

## 安装规划

操作系统： rhel-server-6.6-x86\_64

Vertica：vertica-8.1.0

| 服务器名称 | IP | 描述 |
| :--- | :--- | :--- |
| db1 | 192.168.44.135 |  |
| db2 | 192.168.44.136 |  |
| db3 | 192.168.44.137 |  |

## 安装过程

* 安装和配置MC
* 准备服务器
* 免密码访问
* 运行集群创建导航
* 验证服务器并创建集群
* 基于集群创建新的数据库

**安装Vertica**

准备虚拟机，安装Redhat 6.6操作系统

**网络设置**

`vi /etc/sysconfig/network-scripts/ifcfg-enoXXXX`+

设置ONBOOT=yes+

执行

`service network restart`

然后使用 ifconfig查看服务器IP信息。

关闭防火墙

```
service iptables stop #立即关闭
chkconfig iptables off #重启后关闭
```

关闭

```
setenforce 0
/etc/selinux/config 文件中的 SELINUX="" 为 disabled
```

**设置时钟同步**

```
/sbin/service ntpd restart
/sbin/chkconfig ntpd on
```

**修改kerne参数**

```
#1
vi /etc/sysctl.conf
#添加参数
fs.file-max = 65536
#重新载入
sysctl -p


#2
vi /etc/security/limits.conf
dbadmin - nice 0

#3 min_free_kbytes 设置
/sbin/sysctl vm.min_free_kbytes
vm.min_free_kbytes = 45056
#以上参数需要大于4096

#4 
ulimit -n
dbadmin - nofile 65536

#5
vi /etc/pam.d/su
session required pam_limits.so


#6
sysctl -w kernel.pid_max=524288

#7
vi /etc/security/limits.conf
dbadmin - as unlimited
dbadmin - fsize unlimited
dbadmin - nproc 4096

#8
vi /etc/sysctl.conf
vm.max_map_count=65536
sysctl -p


#9
vi /etc/rc.local
echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
#重启才会生效哦

#10
vi /etc/sysctl.conf
#添加
vm.swappiness = 1
cat /proc/sys/vm/swappiness
echo 1 > /proc/sys/vm/swappiness

#11
sysctl -w kernel.pid_max=524288

#12
/sbin/blockdev --setra 2048 /dev/sda
echo '/sbin/blockdev --setra 2048 /dev/sda' >> /etc/rc.local
```

**设置hostname**

为了方便后续操作，设置服务器名称

`hostname db1`

以上命令修改会立即生效，但是重启后就会失效，使用以下命令修改：

```
vi /etc/sysconfig/network

NETWORKING=yes
HOSTNAME=db1
```

**安装依赖包**

```
#检查操作系统的安装包
yum install pstack 
yum install mcelog
yum install sysstat
yum install dialog
```

**安装MC（Management Console）**

```
[root@db1 ~]# rpm -Uvh vertica-console-8.1.0-3.x86_64.RHEL6.rpm 
Preparing...                ########################################### [100%]
[preinstall] clean up older drivers
[preinstall] Starting installation....
   1:vertica-console        ########################################### [100%]
[postinstall] copy vertica-consoled
[postinstall] configure the daemon service
Cleaning up temp folder...
Starting the vertica management console....
Vertica Console: 2017-08-13 18:29:24.808:INFO:cv.Startup:Attempting to load properties from /opt/vconsole/config/console.properties
2017-08-13 18:29:24.810:INFO:cv.Startup:Starting Server...
2017-08-13 18:29:24.841:WARN:oejs.AbstractConnector:Acceptors should be <=2*availableProcessors: SslSelectChannelConnector@0.0.0.0:5450 STOPPED
2017-08-13 18:29:24.924:INFO:cv.Startup:starting monitor thread
2017-08-13 18:29:24.932:INFO:oejs.Server:jetty-7.x.y-SNAPSHOT
2017-08-13 18:29:24.969:INFO:oejw.WebInfConfiguration:Extract jar:file:/opt/vconsole/lib/webui.war!/ to /opt/vconsole/temp/webapp
2017-08-13 18:29:30.050:INFO:/webui:Set web app root system property: 'webapp.root' = [/opt/vconsole/temp/webapp]
2017-08-13 18:29:30.113:INFO:/webui:Initializing log4j from [classpath:log4j.xml]
2017-08-13 18:29:30.150:INFO:/webui:Initializing Spring root WebApplicationContext
---- Upgrading /opt/vconsole/config/console.properties ----


************************************************************************************************************

Please open the Vertica Management Console at https://db1:5450/webui

************************************************************************************************************


2017-08-13 18:29:58.333:INFO:oejsh.ContextHandler:started o.e.j.w.WebAppContext{/webui,file:/opt/vconsole/temp/webapp/},file:/opt/vconsole/lib/webui.war
2017-08-13 18:29:58.425:INFO:/webui:Initializing Spring FrameworkServlet 'appServlet'
2017-08-13 18:30:02.377:INFO:oejdp.ScanningAppProvider:Deployment monitor /opt/vconsole/webapps at interval 2
2017-08-13 18:30:02.493:INFO:oejhs.SslContextFactory:Enabled Protocols [SSLv2Hello, TLSv1, TLSv1.1, TLSv1.2] of [SSLv2Hello, SSLv3, TLSv1, TLSv1.1, TLSv1.2]
2017-08-13 18:30:02.546:INFO:oejs.AbstractConnector:Started SslSelectChannelConnector@0.0.0.0:5450 STARTING
start OK
[postinstall] Changing permissions of /opt/vconsole
```

安装完毕后，如果需要手动重启MC

```
/etc/init.d/verticad start
```

**设置MC**

打开浏览器,输入地址: [https://192.168.44.135:5450/webui/login](https://192.168.44.135:5450/webui/login)

![](/assets-10/10.1_1.png)

点击接受协议。

![](/assets-10/10.1_2.png)

![](/assets-10/10.1_3.png)

保持默认。

![](/assets-10/10.1_4.png)

点击完成后，稍微等待。

![](/assets-10/10.1_5.png)

使用刚刚设置的密码登录。登录成功即可。

**免密码访问**

在开始安装集群之前，MC必须能够访问所有的服务器，MC使用SSH访问各个服务器。

```
cd ~
mkdir .ssh
ssh-keygen -q -t rsa -f ~/.ssh/vid_rsa -N ''
chmod 700 /root/.ssh
cd ~/.ssh
cat vid_rsa.pub >> vauthorized_keys2
cat vid_rsa.pub >> authorized_keys2

ssh db2 "mkdir /root/.ssh"
ssh db3 "mkdir /root/.ssh"

scp -r /root/.ssh/vauthorized_keys2 db2:/root/.ssh/.
scp -r /root/.ssh/vauthorized_keys2 db3:/root/.ssh/.

ssh db2 "cd /root/.ssh/;cat vauthorized_keys2 >> authorized_keys2; chmod 600 /root/.ssh/authorized_keys2"
ssh db3 "cd /root/.ssh/;cat vauthorized_keys2 >> authorized_keys2; chmod 600 /root/.ssh/authorized_keys2"

 ssh -i /root/.ssh/vid_rsa db2 "rm /root/.ssh/vauthorized_keys2"
 ssh -i /root/.ssh/vid_rsa db3 "rm /root/.ssh/vauthorized_keys2"

 rm ~/.ssh/vauthorized_keys2
```

生成的文件vid\_rsa，需要在MC用到，可以先保存下来。

**运行MC的集群导航**

登录MC，点击Provisioning -  Createa new cluster

![](/assets-10/10.1_7.png)

点击下一步

![](/assets-10/10.1_8.png)执行安装文件

![](/assets-10/10.1_9.png)点击预览，会打开新页面，在页面中输入IP地址

![](/assets-10/10.1_10.png)

**验证和创建集群**

![](/assets-10/10.1_12.png)

针对每台服务器进行校验。

校验成功后，县级创建集群。

![](/assets-10/10.1_15.png)

![](/assets-10/10.1_16.png)

**创建数据库**

![](/assets-10/10.1_20.png)填写数据库名称以及密码点击下一步。

![](/assets-10/10.1_21.png)由于电脑磁盘有限，先创建两个节点吧。

![](/assets-10/10.1_22.png)

创建成功。

![](/assets-10/10.1_23.png)

![](/assets-10/10.1_24.png)

