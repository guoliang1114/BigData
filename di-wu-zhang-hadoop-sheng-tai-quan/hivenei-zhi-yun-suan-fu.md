## 5.5 内置运算符

在Hive有四种类型的运算符：

* 关系运算符

* 算术运算符

* 逻辑运算符

* 复杂运算符

可以在Beeline或者Hive CLI交互窗口中，使用以下命令来查看最新的文档。

```
SHOW FUNCTIONS;
DESCRIBE FUNCTION <function_name>;
DESCRIBE FUNCTION EXTENDED <function_name>;
```

### 5.5.1 关系运算符

以下操作符比较操作数\(operands\)从而产生TRUE/FALSE值。

| 运算符 | 操作数 | 描述 |
| :--- | :--- | :--- |
| A = B | 所有基本类型 | 如果表达A等于表达B，结果TRUE ，否则FALSE。 |
| A != B | 所有基本类型 | 如果A不等于表达式B表达返回TRUE ，否则FALSE。 |
| A &lt; B | 所有基本类型 | TRUE，如果表达式A小于表达式B，否则FALSE。 |
| A &lt;= B | 所有基本类型 | TRUE，如果表达式A小于或等于表达式B，否则FALSE。 |
| A &gt; B | 所有基本类型 | TRUE，如果表达式A大于或等于表达式B，否则FALSE。 |
| A &gt;= B | 所有基本类型 | TRUE，如果表达式A大于或等于表达式B，否则FALSE。 |
| A IS NULL | 所有类型 | TRUE，如果表达式的计算结果为NULL，否则FALSE。 |
| A IS NOT NULL | 所有类型 | FALSE，如果表达式A的计算结果为NULL，否则TRUE。 |
| A LIKE B | 字符串 | TRUE，如果字符串模式A匹配到B，否则FALSE。关系型数据库中的like功能。 |
| A RLIKE B | 字符串 | NULL，如果A或B为NULL；TRUE，如果A任何子字符串匹配Java正则表达式B；否则FALSE。 |
| A REGEXP B | 字符串 | 等同于RLIKE。 |

### 5.5.2 算术运算符

这些运算符支持的操作数各种常见的算术运算。所有这些返回数字类型。下表描述了在Hive中可用的算术运算符：

| 运算符 | 操作 | 描述 |
| :--- | :--- | :--- |
| A + B | 所有数字类型 | A加B的结果 |
| A - B | 所有数字类型 | A减去B的结果 |
| A \* B | 所有数字类型 | A乘以B的结果A |
| A / B | 所有数字类型 | A除以B的结果 |
| A % B | 所有数字类型 | A除以B产生的余数 |
| A & B | 所有数字类型 | A和B的按位与结果 |
| A \| B | 所有数字类型 | A和B的按位或结果 |
| A ^ B | 所有数字类型 | A和B的按位异或结果 |
| ~A | 所有数字类型 | A按位非的结果 |



### 5.5.3 逻辑运算符





