## 10.2 Vertica简单入门

上一节我们安装和配置了数据库，最终把数据启动起来。本节我们使用官方自带的例子快速入门。

**安装VMart示例数据**

切换到dbadmin用户，导航到/opt/vertica/examples目录下。该目录的脚本为示例数据生成器。

```
su - dbadmin
cd /opt/vertica/examples/VMart_Schema
./vmart_gen  #命令执行需要花费一些时间
Using default parameters
datadirectory = ./
numfiles = 1
seed = 20177
null = ''
timefile = Time.txt
numfactsalesrows = 5000000
numfactorderrows = 300000
numprodkeys = 60000
numstorekeys = 250
numpromokeys = 1000
numvendkeys = 50
numcustkeys = 50000
numempkeys = 10000
numwarehousekeys = 100
numshippingkeys = 100
numonlinepagekeys = 1000
numcallcenterkeys = 200
numfactonlinesalesrows = 5000000
numinventoryfactrows = 300000
gen_load_script = false
years = 2003 to 2007
Data Generated successfully !
```

创建示例数据库

```
$adminTools
```

![](/assets-10/10.2_1.png)

选择第6项下的第一项创建数据库

![](/assets-10/10.2_2.png)

点击ok，进入创建数据库界面

![](/assets-10/10.2_3.png)

数据库名称为VMartDB,点击ok

![](/assets-10/10.2_5.png)

输入密码后，点击ok

![](/assets-10/10.2_6.png)

选择要部署到的服务器，我们全选。



