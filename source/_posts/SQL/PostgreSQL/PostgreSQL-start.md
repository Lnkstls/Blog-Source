---
title: PostgreSQL
date: 2022-10-04 12:05:07
categories: 
    - SQL
    - PostgreSQL
tags:
    - PostgreSQL
---

# PostgreSQL

参考：

> https://www.runoob.com/postgresql/postgresql-syntax.html 菜鸟教程 

## 数值类型

数值类型由 2 字节、4 字节或 8 字节的整数以及 4 字节或 8 字节的浮点数和可选精度的十进制数组成。

下表列出了可用的数值类型。

| 名字 | 存储长度 | 描述 | 范围 |
| --- | --- | --- | --- |
| smallint | 2 字节 | 小范围整数 | \-32768 到 +32767 |
| integer | 4 字节 | 常用的整数 | \-2147483648 到 +2147483647 |
| bigint | 8 字节 | 大范围整数 | \-9223372036854775808 到 +9223372036854775807 |
| decimal | 可变长 | 用户指定的精度，精确 | 小数点前 131072 位；小数点后 16383 位 |
| numeric | 可变长 | 用户指定的精度，精确 | 小数点前 131072 位；小数点后 16383 位 |
| real | 4 字节 | 可变精度，不精确 | 6 位十进制数字精度 |
| double precision | 8 字节 | 可变精度，不精确 | 15 位十进制数字精度 |
| smallserial | 2 字节 | 自增的小范围整数 | 1 到 32767 |
| serial | 4 字节 | 自增整数 | 1 到 2147483647 |
| bigserial | 8 字节 | 自增的大范围整数 | 1 到 9223372036854775807 |



### 货币类型

money 类型存储带有固定小数精度的货币金额。

numeric、int 和 bigint 类型的值可以转换为 money，不建议使用浮点数来处理处理货币类型，因为存在舍入错误的可能性。

| 名字 | 存储容量 | 描述 | 范围 |
| --- | --- | --- | --- |
| money | 8 字节 | 货币金额 | \-92233720368547758.08 到 +92233720368547758.07 |



### 字符类型

| 名字 | 描述 |
| --- | --- |
| varchar | 变长，有长度限制 |
| char | 定长,不足补空白 |
| text | 变长，无长度限制 |

### 日期/时间类型

| 名字 | 存储空间 | 描述 | 最低值 | 最高值 | 分辨率 |
| --- | --- | --- | --- | --- | --- |
| timestamp \[ (_p_) \] \[ without time zone \] | 8 字节 | 日期和时间(无时区) | 4713 BC | 294276 AD | 1 毫秒 / 14 位 |
| timestamp \[ (_p_) \] with time zone | 8 字节 | 日期和时间，有时区 | 4713 BC | 294276 AD | 1 毫秒 / 14 位 |
| date | 4 字节 | 只用于日期 | 4713 BC | 5874897 AD | 1 天 |
| time \[ (_p_) \] \[ without time zone \] | 8 字节 | 只用于一日内时间 | 00:00:00 | 24:00:00 | 1 毫秒 / 14 位 |
| time \[ (_p_) \] with time zone | 12 字节 | 只用于一日内时间，带时区 | 00:00:00+1459 | 24:00:00-1459 | 1 毫秒 / 14 位 |
| interval \[ _fields_ \] \[ (_p_) \] | 12 字节 | 时间间隔 | \-178000000 年 | 178000000 年 | 1 毫秒 / 14 位 |



### 布尔类型

PostgreSQL 支持标准的 boolean 数据类型。

boolean 有"true"(真)或"false"(假)两个状态， 第三种"unknown"(未知)状态，用 NULL 表示。

| 名称 | 存储格式 | 描述 |
| --- | --- | --- |
| bool | 1 字节 | true/false |

### 枚举类型

枚举类型是一个包含静态和值的有序集合的数据类型。

PostgtesSQL中的枚举类型类似于 C 语言中的 enum 类型。

与其他类型不同的是枚举类型需要使用 CREATE TYPE 命令创建。

```
CREATE TYPE mood AS ENUM ('sad',  'ok',  'happy');
```

创建一周中的几天，如下所示:

```
CREATE TYPE week AS ENUM ('Mon',  'Tue',  'Wed',  'Thu',  'Fri',  'Sat',  'Sun');
```

就像其他类型一样，一旦创建，枚举类型可以用于表和函数定义。

```
CREATE TYPE mood AS ENUM ('sad',  'ok',  'happy'); CREATE TABLE person ( name text, current\_mood mood ); INSERT INTO person VALUES ('Moe',  'happy'); SELECT \* FROM person WHERE current\_mood \=  'happy'; name | current\_mood \------+--------------  Moe  | happy (1 row)
```



### 几何类型

几何数据类型表示二维的平面物体。

下表列出了 PostgreSQL 支持的几何类型。

最基本的类型：点。它是其它类型的基础。

| 名字 | 存储空间 | 说明 | 表现形式 |
| --- | --- | --- | --- |
| point | 16 字节 | 平面中的点 | (x,y) |
| line | 32 字节 | (无穷)直线(未完全实现) | ((x1,y1),(x2,y2)) |
| lseg | 32 字节 | (有限)线段 | ((x1,y1),(x2,y2)) |
| box | 32 字节 | 矩形 | ((x1,y1),(x2,y2)) |
| path | 16+16n 字节 | 闭合路径(与多边形类似) | ((x1,y1),...) |
| path | 16+16n 字节 | 开放路径 | \[(x1,y1),...\] |
| polygon | 40+16n 字节 | 多边形(与闭合路径相似) | ((x1,y1),...) |
| circle | 24 字节 | 圆 | <(x,y),r> (圆心和半径) |



### 网络地址类型

PostgreSQL 提供用于存储 IPv4 、IPv6 、MAC 地址的数据类型。

用这些数据类型存储网络地址比用纯文本类型好， 因为这些类型提供输入错误检查和特殊的操作和功能。

| 名字 | 存储空间 | 描述 |
| --- | --- | --- |
| cidr | 7 或 19 字节 | IPv4 或 IPv6 网络 |
| inet | 7 或 19 字节 | IPv4 或 IPv6 主机和网络 |
| macaddr | 6 字节 | MAC 地址 |

在对 inet 或 cidr 数据类型进行排序的时候， IPv4 地址总是排在 IPv6 地址前面，包括那些封装或者是映射在 IPv6 地址里的 IPv4 地址， 比如 ::10.2.3.4 或 ::ffff:10.4.3.2。



### 位串类型

位串就是一串 1 和 0 的字符串。它们可以用于存储和直观化位掩码。 我们有两种 SQL 位类型：bit(n) 和bit varying(n)， 这里的n是一个正整数。

bit 类型的数据必须准确匹配长度 n， 试图存储短些或者长一些的数据都是错误的。bit varying 类型数据是最长 n 的变长类型；更长的串会被拒绝。 写一个没有长度的bit 等效于 bit(1)， 没有长度的 bit varying 意思是没有长度限制。



### 文本搜索类型

全文检索即通过自然语言文档的集合来找到那些匹配一个查询的检索。

PostgreSQL 提供了两种数据类型用于支持全文检索：

| 名字 | 描述 |
| --- | --- |
| tsvector | 是一个无重复值的 lexemes 排序列表， 即一些同一个词的不同变种的标准化 |
| tsquery | 存储用于检索的词汇，并且使用布尔操作符 &(AND)，\|(OR)和!(NOT) 来组合它们，括号用来强调操作符的分组 |

### UUID 类型

uuid 数据类型用来存储 RFC 4122，ISO/IEF 9834-8:2005 以及相关标准定义的通用唯一标识符（UUID）。 （一些系统认为这个数据类型为全球唯一标识符，或GUID。） 这个标识符是一个由算法产生的 128 位标识符，使它不可能在已知使用相同算法的模块中和其他方式产生的标识符相同。 因此，对分布式系统而言，这种标识符比序列能更好的提供唯一性保证，因为序列只能在单一数据库中保证唯一。

UUID 被写成一个小写十六进制数字的序列，由分字符分成几组， 特别是一组8位数字+3组4位数字+一组12位数字，总共 32 个数字代表 128 位， 一个这种标准的 UUID 例子如下：

```
a0eebc99\-9c0b\-4ef8\-bb6d\-6bb9bd380a11
```

### XML 类型

xml 数据类型可以用于存储XML数据。 将 XML 数据存到 text 类型中的优势在于它能够为结构良好性来检查输入值， 并且还支持函数对其进行类型安全性检查。 要使用这个数据类型，编译时必须使用 configure --with-libxml。

xml 可以存储由XML标准定义的格式良好的"文档"， 以及由 XML 标准中的 **XMLDecl? content** 定义的"内容"片段， 大致上，这意味着内容片段可以有多个顶级元素或字符节点。 xmlvalue IS DOCUMENT 表达式可以用来判断一个特定的 xml 值是一个完整的文件还是内容片段。

``创建XML值``

使用函数 xmlparse: 来从字符数据产生 xml 类型的值：

```
XMLPARSE (DOCUMENT '<?xml version="1.0"?><book><title>Manual</title><chapter>...</chapter></book>') XMLPARSE (CONTENT 'abc<foo>bar</foo><bar>foo</bar>')
```



### JSON 类型

json 数据类型可以用来存储 JSON（JavaScript Object Notation）数据， 这样的数据也可以存储为 text，但是 json 数据类型更有利于检查每个存储的数值是可用的 JSON 值。

此外还有相关的函数来处理 json 数据：

| 实例 | 实例结果 |
| --- | --- |
| array\_to\_json('{``{{1,5},{99,100}}``')::int\[\] | \[\[1,5\],\[99,100\]\] |
| row\_to\_json(row(1,'foo')) | {"f1":1,"f2":"foo"} |



### 数组类型

PostgreSQL 允许将字段定义成变长的多维数组。

数组类型可以是任何基本类型或用户定义类型，枚举类型或复合类型。

``声明数组``

创建表的时候，我们可以声明数组，方式如下：

```
CREATE TABLE sal\_emp ( name            text, pay\_by\_quarter  integer\[\], schedule        text\[\]\[\]  );
```

pay\_by\_quarter 为一维整型数组、schedule 为二维文本类型数组。

我们也可以使用 "ARRAY" 关键字，如下所示：

```
CREATE TABLE sal\_emp ( name text, pay\_by\_quarter integer ARRAY\[4\], schedule text\[\]\[\]  );
```

``插入值``

插入值使用花括号 {}，元素在 {} 使用逗号隔开：

```
INSERT INTO sal\_emp
    VALUES ('Bill',  '{10000, 10000, 10000, 10000}',  '{{"meeting", "lunch"}, {"training", "presentation"}}'); INSERT INTO sal\_emp
    VALUES ('Carol',  '{20000, 25000, 25000, 25000}',  '{{"breakfast", "consulting"}, {"meeting", "lunch"}}');
```

``访问数组``

现在我们可以在这个表上运行一些查询。

首先，我们演示如何访问数组的一个元素。 这个查询检索在第二季度薪水变化的雇员名：

```
SELECT name FROM sal\_emp WHERE pay\_by\_quarter\[1\]  <> pay\_by\_quarter\[2\]; name \-------  Carol  (1 row)
```

数组的下标数字是写在方括弧内的。

``修改数组``

我们可以对数组的值进行修改：

```
UPDATE sal\_emp SET pay\_by\_quarter \=  '{25000,25000,27000,27000}' WHERE name \=  'Carol';
```

或者使用 ARRAY 构造器语法：

```
UPDATE sal\_emp SET pay\_by\_quarter \= ARRAY\[25000,25000,27000,27000\] WHERE name \=  'Carol';
```

``数组中检索``

要搜索一个数组中的数值，你必须检查该数组的每一个值。

比如：

```
SELECT \* FROM sal\_emp WHERE pay\_by\_quarter\[1\]  \=  10000 OR
                            pay\_by\_quarter\[2\]  \=  10000 OR
                            pay\_by\_quarter\[3\]  \=  10000 OR
                            pay\_by\_quarter\[4\]  \=  10000;
```

另外，你可以用下面的语句找出数组中所有元素值都等于 10000 的行：

```
SELECT \* FROM sal\_emp WHERE 10000  \= ALL (pay\_by\_quarter);
```

或者，可以使用 generate\_subscripts 函数。例如：

```
SELECT \* FROM (SELECT pay\_by\_quarter, generate\_subscripts(pay\_by\_quarter,  1) AS s
      FROM sal\_emp) AS foo
 WHERE pay\_by\_quarter\[s\]  \=  10000;
```

### 复合类型

复合类型表示一行或者一条记录的结构； 它实际上只是一个字段名和它们的数据类型的列表。PostgreSQL 允许像简单数据类型那样使用复合类型。比如，一个表的某个字段可以声明为一个复合类型。

``声明复合类型``

下面是两个定义复合类型的简单例子：

```
CREATE TYPE complex AS ( r double precision, i double precision ); CREATE TYPE inventory\_item AS ( name            text, supplier\_id     integer, price           numeric );
```

语法类似于 CREATE TABLE，只是这里只可以声明字段名字和类型。

定义了类型，我们就可以用它创建表：

```
CREATE TABLE on\_hand ( item      inventory\_item, count     integer ); INSERT INTO on\_hand VALUES (ROW('fuzzy dice',  42,  1.99),  1000);
```

``复合类型值输入``

要以文本常量书写复合类型值，在圆括弧里包围字段值并且用逗号分隔他们。 你可以在任何字段值周围放上双引号，如果值本身包含逗号或者圆括弧， 你必须用双引号括起。

复合类型常量的一般格式如下：

```
'( val1 , val2 , ... )'
```

一个例子是:

```
'("fuzzy dice",42,1.99)'
```

``访问复合类型``

要访问复合类型字段的一个域，我们写出一个点以及域的名字， 非常类似从一个表名字里选出一个字段。实际上，因为实在太像从表名字中选取字段， 所以我们经常需要用圆括弧来避免分析器混淆。比如，你可能需要从on\_hand 例子表中选取一些子域，像下面这样：

```
SELECT item.name FROM on\_hand WHERE item.price \>  9.99;
```

这样将不能工作，因为根据 SQL 语法，item是从一个表名字选取的， 而不是一个字段名字。你必须像下面这样写：

```
SELECT (item).name FROM on\_hand WHERE (item).price \>  9.99;
```

或者如果你也需要使用表名字(比如，在一个多表查询里)，那么这么写：

```
SELECT (on\_hand.item).name FROM on\_hand WHERE (on\_hand.item).price \>  9.99;
```

现在圆括弧对象正确地解析为一个指向item字段的引用，然后就可以从中选取子域。

### 范围类型

范围数据类型代表着某一元素类型在一定范围内的值。

例如，timestamp 范围可能被用于代表一间会议室被预定的时间范围。

PostgreSQL 内置的范围类型有：

*   int4range — integer的范围
    
*   int8range —bigint的范围
    
*   numrange —numeric的范围
    
*   tsrange —timestamp without time zone的范围
    
*   tstzrange —timestamp with time zone的范围
    
*   daterange —date的范围
    

此外，你可以定义你自己的范围类型。

```
CREATE TABLE reservation (room int, during tsrange); INSERT INTO reservation VALUES (1108,  '\[2010-01-01 14:30, 2010-01-01 15:30)');  \--  包含 SELECT int4range(10,  20)  @>  3;  \--  重叠 SELECT numrange(11.1,  22.2)  && numrange(20.0,  30.0);  \--  提取上边界 SELECT upper(int8range(15,  25));  \--  计算交叉 SELECT int4range(10,  20)  \* int4range(15,  25);  \--  范围是否为空 SELECT isempty(numrange(1,  5));
```

范围值的输入必须遵循下面的格式：

```
(下边界,上边界)  (下边界,上边界\]  \[下边界,上边界)  \[下边界,上边界\]  空
```

圆括号或者方括号显示下边界和上边界是不包含的还是包含的。注意最后的格式是 空，代表着一个空的范围（一个不含有值的范围）。

```
\--  包括3，不包括7，并且包括二者之间的所有点 SELECT '\[3,7)'::int4range;  \--  不包括3和7，但是包括二者之间所有点 SELECT '(3,7)'::int4range;  \--  只包括单一值4 SELECT '\[4,4\]'::int4range;  \--  不包括点（被标准化为‘空’） SELECT '\[4,4)'::int4range;
```



### 对象标识符类型

PostgreSQL 在内部使用对象标识符(OID)作为各种系统表的主键。

同时，系统不会给用户创建的表增加一个 OID 系统字段(除非在建表时声明了WITH OIDS 或者配置参数default\_with\_oids设置为开启)。oid 类型代表一个对象标识符。除此以外 oid 还有几个别名：regproc, regprocedure, regoper, regoperator, regclass, regtype, regconfig, 和regdictionary。

| 名字 | 引用 | 描述 | 数值例子 |
| --- | --- | --- | --- |
| oid | 任意 | 数字化的对象标识符 | 564182 |
| regproc | pg\_proc | 函数名字 | sum |
| regprocedure | pg\_proc | 带参数类型的函数 | sum(int4) |
| regoper | pg\_operator | 操作符名 | + |
| regoperator | pg\_operator | 带参数类型的操作符 | \*(integer,integer) 或 -(NONE,integer) |
| regclass | pg\_class | 关系名 | pg\_type |
| regtype | pg\_type | 数据类型名 | integer |
| regconfig | pg\_ts\_config | 文本搜索配置 | english |
| regdictionary | pg\_ts\_dict | 文本搜索字典 | simple |



### 伪类型

PostgreSQL类型系统包含一系列特殊用途的条目， 它们按照类别来说叫做伪类型。伪类型不能作为字段的数据类型， 但是它可以用于声明一个函数的参数或者结果类型。 伪类型在一个函数不只是简单地接受并返回某种SQL 数据类型的情况下很有用。

下表列出了所有的伪类型：

| 名字 | 描述 |
| --- | --- |
| any | 表示一个函数接受任何输入数据类型。 |
| anyelement | 表示一个函数接受任何数据类型。 |
| anyarray | 表示一个函数接受任意数组数据类型。 |
| anynonarray | 表示一个函数接受任意非数组数据类型。 |
| anyenum | 表示一个函数接受任意枚举数据类型。 |
| anyrange | 表示一个函数接受任意范围数据类型。 |
| cstring | 表示一个函数接受或者返回一个空结尾的 C 字符串。 |
| internal | 表示一个函数接受或者返回一种服务器内部的数据类型。 |
| language\_handler | 一个过程语言调用处理器声明为返回language\_handler。 |
| fdw\_handler | 一个外部数据封装器声明为返回fdw\_handler。 |
| record | 标识一个函数返回一个未声明的行类型。 |
| trigger | 一个触发器函数声明为返回trigger。 |
| void | 表示一个函数不返回数值。 |
| opaque | 一个已经过时的类型，以前用于所有上面这些用途。 |

> 更多内容参考：[PostgreSQL 数据类型](https://www.runoob.com/manual/PostgreSQL/datatype.html)

[](https://www.runoob.com/postgresql/postgresql-syntax.html)[PostgreSQL 语法](https://www.runoob.com/postgresql/postgresql-syntax.html "PostgreSQL 语法")

[PostgreSQL 创建数据库](https://www.runoob.com/postgresql/postgresql-create-database.html "PostgreSQL 创建数据库")[](https://www.runoob.com/postgresql/postgresql-create-database.html)