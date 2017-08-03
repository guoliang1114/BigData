## 5.3 Hive数据类型

### 5.3.1 基本类型

Integers（整型）

* TINYINT －1位的整型
* SMALLINT －2位的整型
* INT －4位的整型
* BIGINT －8位的整型

布尔类型

* BOOLEAN －TRUE/FALSE

浮点数

* FLOAT －单精度
* DOUBLE －双精度

定点数

* DECIMAL －用户可以指定范围和小数点位数

字符串 

* STRING －在特定的字符集中的一个字符串序列 
* VARCHAR －在特定的字符集中的一个有最大长度限制的字符串序列 
* CHAR －在特定的字符集中的一个指定长度的字符串序列

日期和时间 

* TIMESTAMP －一个特定的时间点，精确到纳秒。 
* DATE －一个日期

二进制 

* BINARY －一个二进制位序列

具体信息如下表所示：

| 类型 | 所占字节 | 数值范围 | 后缀 | 示例 |
| :--- | :--- | :--- | :--- | :--- |
| TINYINT | 1字节有符号整数 | -128 ~ 127 | Y | 10Y |
| SMALLINT | 2字节有符号整数 | -32768 ~ 32767 | S | 10S |
| INT | 4字节有符号整数 | -2147483648~2147483647 | - | 10 |
| BIGINT | 8字节有符号整数 | -9223372036854775808~9223372036854775807 | L | 10L |
| FLOAT | 4字节单精度浮点数 |  |  |  |
| DOUBLE | 8字节双精度浮点数 |  |  |  |
| DECIMAL | 自定义精度 | 默认是DECIMAL\(10,0\)可以自定义例如DECIMAL\(9,7\) |  |  |
| TIMESTAMP |  | 支持到纳秒，YYYY-MM-DD HH:MM:SS.fffffffff |  |  |
| DATE |  | 日期YYYY-MM-DD |  |  |
| STRING |  |  |  |  |
| VARCHAR |  | 1 ~ 65355 |  |  |
| CHAR |  | 255 |  |  |
| BOOLEAN |  |  |  |  |
| BINARY |  |  |  |  |

**基本类型转换**

Hive支持基本类型的转换，低字节基本类型可转化为高字节的类型，例如TINYINT,SMALLINT,INT可以转化为FLOAT。而所有的整数类型、FLOAT以及STRING类型可以转化为DOUBLE类型。

### 5.3.2 复杂类型

HIve中的复杂类型见下表：

| 类型 | 说明 | 实例和访问方式 |
| :--- | :--- | :--- |
| ARRAY | 数组。有序的类型相同的元素集合，如Array&lt;T&gt; | Array\(1,2\)、Array\("abc","d"\)。访问方式：Array\[n\],下标n从0开始 |
| MAP | 映射。无序的键值对。键的类型必须相同，值的类型也必须相同。例如Map&lt;STRING,INT&gt; | Map\("abc",1,"d",4\)。其中逗号分隔的元素分别为key1,value1,key2,value2。访问方式为M\[key\],其中key为map中的key值 |
| STRUCT | 结构体。有名称的元素集合。如STRUCT&lt;name:STRING,age:INT&gt; | Struct\("John",31\)。访问当时为S.item,item是Struct中的元素。 |

### 5.3.3 文件的存储结构

Hive常见的存储结构是TEXTFILE。在Hive建表时，通过Stored AS FILE\_FORMAT来指定文件存储结构。比如Stored As TextFile。

