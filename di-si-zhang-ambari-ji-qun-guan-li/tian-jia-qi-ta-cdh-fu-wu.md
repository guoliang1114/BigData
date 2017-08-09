## 4.4 添加其他CDH服务

安装完CDH后，有可能还需要其他的服务，可以登录CM管理，选择集群-添加服务。

### 4.4.1 **添加Sqoop2**

在页面中选择Sqoop 2，并选择要安装到的节点。

![](/assets/4.4_1.png)

设置密码

![](/assets/4.4_2_1.png)

密码为Pass1234

![](/assets/4.4_4.png)其他组件也是一样，可以会直接选择安装。

### 4.4.2 添加Kafka服务

在CDH官网中关于Kafka的安装和升级中已经说到，在CDH中，Kafka作为一个分布式的parcel，单独出来作为parcel分发安装包。只要我们把分离开的kafka的服务描述jar包和服务parcel包下载了，就可以实现完美集成了。

在CM中点击Parcel中点击，查看

![](/assets/4.4_5.png)可以下载再分配。

当然可以手动下载到目录：/opt/cloudera/parcel-repo

然后启动cm服务，检查更新parcel，分配并激活percel包，注意此处一定要激活才能使用。

添加服务的过程和上面一致的![](/assets/4.4_6.png)下一步：两个属性必填：

Destination Broker List： hadoop5:9092

Source Broker List: hadoop5:9092

下一步启动，会遇到以下错误：

**Java Heap Size不够**

点击继续，出错后，Kafka服务未启动。返回集群配置，打卡Kafka服务配置页，查找“Java Heap Size of Broker”项，将对大小从50MB修改为256MB。

**配置Topic Whitelist**

项为正则表达式：

```
(?!x)x
```

，保存更改。然后添加角色实例，重新配置即可

