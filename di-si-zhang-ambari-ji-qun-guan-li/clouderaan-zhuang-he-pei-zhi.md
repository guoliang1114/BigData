## 4.3 Cloudera CDH安装和配置

### 4.3.1 开发环境安装规划

| 名称 | IP | 服务器名 | 配置信息 | 备注 |
| :--- | :--- | :--- | :--- | :--- |
| Cloudera \(Master\) - NN + RM + ZK + ISS | 172.24.222.69 | hadoop1.foton.com.cn | 300G |  |
| Cloudera \(Data\) | 172.24.222.70 | hadoop2.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.71 | hadoop3.foton.com.cn | 800G |  |
| Cloudera \(Data\) | 172.24.222.72 | hadoop4.foton.com.cn | 800G |  |
| Cloudera \(Gateway\) - Flume, Scoop, Hue, Kafka, Spark   \| Cloudera \(Cloudera Manager + Database\) | 172.24.222.73 | hadoop5.foton.com.cn | 800G |  |

> 以上服务器版本: Red Hat Enterprise Linux Server release 6.6 \(Santiago\)
>
> PS. 用户名是root，密码：Pass1234

首先更各个服务器名称和HOST

```
172.24.222.69 hadoop1.foton.com.cn hadoop1
172.24.222.70 hadoop2.foton.com.cn hadoop2
172.24.222.71 hadoop3.foton.com.cn hadoop3
172.24.222.72 hadoop4.foton.com.cn hadoop4
172.24.222.73 hadoop5.foton.com.cn hadoop5
```



