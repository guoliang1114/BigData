## 10.3 Vertica Lab2

上一节我们安装和配置了示例数据库，并使用数据库设计器对数据进行优化。本节继续举例快速介绍。

本节主要介绍：

1. 数据管理
2. 删除数据
3. 资源管理

### 10.3.1 数据管理

**将数据加载到ROS**

切换到 /opt/vertica/examples/VMart\_schema。运行adminTools

```
su - dbadmin
adminTools
Welcome to vsql, the Vertica Analytic Database interactive terminal.

Type:  \h or \? for help with vsql commands
       \g or terminate with semicolon to execute query
       \q to quit

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
.......
```

```
VMartDB=> select *  from load_streams;
           session_id           |  transaction_id   | statement_id | stream_name | schema_name  |     table_id      |    table_name     |          load_start           | load_duration_ms | is_executing | accepted_row_count | rejected_row_count | read_bytes | input_file_size_bytes | parse_complete_percent | unsorted_row_count | sorted_row_count | sort_complete_percent 
--------------------------------+-------------------+--------------+-------------+--------------+-------------------+-------------------+-------------------------------+------------------+--------------+--------------------+--------------------+------------+-----------------------+------------------------+--------------------+------------------+-----------------------
 v_vmartdb_node0001-27364:0xe5f | 45035996273707042 |            1 |             | store        | 45035996273707684 | store_sales_fact  | 2017-08-14 01:17:48.222679+08 |            29414 | f            |            5000000 |                  0 |  375766803 |             375766803 |                    100 |            5000000 |          5000000 |                   100
 v_vmartdb_node0001-27364:0xe5f | 45035996273707063 |            1 |             | store        | 45035996273707698 | store_orders_fact | 2017-08-14 01:18:17.666192+08 |             1697 | f            |             300000 |                  0 |   31419458 |              31419458 |                    100 |             150264 |           150264 |                   100
 v_vmartdb_node0001-27364:0xe5f | 45035996273707065 |            1 |             | online_sales | 45035996273707716 | online_sales_fact | 2017-08-14 01:18:19.374771+08 |            30249 | f            |            5000000 |                  0 |  379423758 |             379423758 |                    100 |            5000000 |          5000000 |                   100
(3 rows)

VMartDB=>
```

查询load\_streams查看数据加载的过程。脚本中使用DIRECT将数据加载到了ROS。

### 10.3.2 创建分区

```
select *  from partitions;
#查看系统中的分区。
VMartDB=> select *  from partitions;
 partition_key | projection_id | table_schema | projection_name | ros_id | ros_size_bytes | ros_row_count | node_name | deleted_row_count | location_label 
---------------+---------------+--------------+-----------------+--------+----------------+---------------+-----------+-------------------+----------------
(0 rows)


```

VMart默认不创建分区。我们可以手动创建。



