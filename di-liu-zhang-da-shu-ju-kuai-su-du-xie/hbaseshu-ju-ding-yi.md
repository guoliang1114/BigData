## 6.5 数据定义

### 6.5.1 创建表

可以使用命令创建一个表，在这里必须指定表名和列族名。在HBase shell中创建表的语法如下所示。

```
create ‘<table name>’,’<column family>’
```

示例

下面给出的是一个表名为emp的样本模式。它有两个列族：“personal data”和“professional data”。

```
hbase(main):002:0> create 'emp', 'personal data', ’professional data’
```

| Row  Key | personal data | professinonal data |
| :--- | :--- | :--- |
|  |  |  |

如上表所示。

