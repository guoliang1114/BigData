## 10.2 Vertica简单入门

上一节我们安装和配置了数据库，最终把数据启动起来。本节我们使用官方自带的例子快速入门。

### 10.2.1 **安装VMart示例数据**

切换到dbadmin用户，导航到/opt/vertica/examples目录下。该目录的脚本为示例数据生成器。

```
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

### 10.2.2 **创建示例数据库**

```
./adminTools
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

![](/assets-10/10.2_9.png)

设置数据库的目录，使用/home/dbadmin/class\_files

![](/assets-10/10.2_10.png)

点击Yes创建数据库。

一会儿就看到创建成功了。

![](/assets-10/10.2_12.png)

### 10.2.3 定义架构和表

打开adminTools，选择“Main Menu”，然后选择“连接到数据库”，会看到以下内容：

```
Welcome to vsql, the Vertica Analytic Database interactive terminal.

Type:  \h or \? for help with vsql commands
       \g or terminate with semicolon to execute query
       \q to quit

VMartDB=>
```

执行schme和表定义

```
VMartDB=> \i vmart_define_schema.sql
CREATE SCHEMA
CREATE SCHEMA
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
CREATE TABLE
CREATE TABLE
ALTER TABLE
```

使用\dt 查看创建的表

```
VMartDB=> \dt
                          List of tables
    Schema    |         Name          | Kind  |  Owner  | Comment 
--------------+-----------------------+-------+---------+---------
 online_sales | call_center_dimension | table | dbadmin | 
 online_sales | online_page_dimension | table | dbadmin | 
 online_sales | online_sales_fact     | table | dbadmin | 
 public       | customer_dimension    | table | dbadmin | 
 public       | date_dimension        | table | dbadmin | 
 public       | employee_dimension    | table | dbadmin | 
 public       | inventory_fact        | table | dbadmin | 
 public       | product_dimension     | table | dbadmin | 
 public       | promotion_dimension   | table | dbadmin | 
 public       | shipping_dimension    | table | dbadmin | 
 public       | vendor_dimension      | table | dbadmin | 
 public       | warehouse_dimension   | table | dbadmin | 
 store        | store_dimension       | table | dbadmin | 
 store        | store_orders_fact     | table | dbadmin | 
 store        | store_sales_fact      | table | dbadmin | 
(15 rows)

VMartDB=>
```

### 10.2.4 加载示例数据

同样使用\i 执行sql

```
VMartDB=> \i vmart_load_data.sql
 Rows Loaded 
-------------
        1826
(1 row)

 Rows Loaded 
-------------
       60000
(1 row)

 Rows Loaded 
-------------
         250
...............
```

```
VMartDB=> select projection_schema,projection_name,anchor_table_name from projections;
 projection_schema |       projection_name       |   anchor_table_name   
-------------------+-----------------------------+-----------------------
 public            | date_dimension_super        | date_dimension
 public            | product_dimension_super     | product_dimension
 store             | store_dimension_super       | store_dimension
 public            | promotion_dimension_super   | promotion_dimension
 public            | vendor_dimension_super      | vendor_dimension
 public            | customer_dimension_super    | customer_dimension
 public            | employee_dimension_super    | employee_dimension
 public            | warehouse_dimension_super   | warehouse_dimension
 public            | shipping_dimension_super    | shipping_dimension
 online_sales      | online_page_dimension_super | online_page_dimension
 online_sales      | call_center_dimension_super | call_center_dimension
 store             | store_sales_fact_super      | store_sales_fact
 store             | store_orders_fact_super     | store_orders_fact
 online_sales      | online_sales_fact_super     | online_sales_fact
 public            | inventory_fact_super        | inventory_fact
(15 rows)
```

查询projections。当然也可以使用\timing 来执行SQL，在查询后可以返回使用的时间。

\q 可以退出adminTools。



### 10.2.5 数据库设计器



```
cd /opt/vertica/examples/VMart_Schema
adminTools
```

在“主菜单”中选择“设置菜单”,选择运行数据库设计器。

![](/assets-10/10.2.5_1.png)

选择数据库，点击ok。





