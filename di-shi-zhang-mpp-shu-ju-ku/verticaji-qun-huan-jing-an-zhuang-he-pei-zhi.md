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

关闭防火墙

```
service iptables stop #立即关闭
chkconfig iptables off #重启后关闭
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

打开浏览器,输入地址: https://192.168.44.135:5450/webui/login

![](/assets-10/10.1_1.png)



