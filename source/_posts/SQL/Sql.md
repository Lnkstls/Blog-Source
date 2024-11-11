---
title: SQL命令
date: 2022-10-15 14:21:07
categories: 
    - SQL
tags:
    - SQL
---

# SQL命令

各数据库部分指令不一样。

SQL命令不区分大小写。

名词：
- 表（整张表）
- 字段、列（表中的一列）
- 元组、记录（表中记录的一行数据）
- 主键（记录唯一标识）
- 外键（关联额外的相同数据）

## SELECT/查询

```sql
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
+----+--------------+---------------------------+-------+---------+

SELECT name,country FROM Websites; -- 选取name与country列
```

### SELECT DISTINCT/返回唯一不同

```sql
SELECT DISTINCT country FROM Websites; -- 返回country唯一不同的值（USA、CN）
```

### ORDER BY/结果排序

按照升序对记录进行排序，可以使用``DESC``按照降序。

```sql
SELECT * FROM Websites ORDER BY alexa; -- 按照alexa列排序
```

### NULL/未知值

NULL无法被比较。

```sql
SELECT LastName,FirstName,Address FROM Persons WHERE Address IS NULL -- 过滤Address字段为NULL的元组

SELECT LastName,FirstName,Address FROM Persons WHERE Address IS NOT NULL -- 过滤不为NULL的元组
```

## INSERT INTO/增加

```sql
INSERT INTO Websites (name, url, alexa, country) VALUES ('百度','https://www.baidu.com/','4','CN'); -- 向Websites表插入新的元组
```

## UPDATE/更新

使用时请注意使用``WHERE``过滤条件。

```sql
UPDATE Websites SET alexa='5000' country='USA' WHERE name='菜鸟教程'; -- 将name等于菜鸟教程元组的alexa、country数据修改
```

## DELETE/删除

数据无价，谨慎操作。

```sql
DELETE FROM Websites WHERE name='Facebook' AND country='USA'; -- 删除name等于Faceboot并且country等于USA的元组
```

## WHERE/过滤条件

```sql
SELECT * FROM Websites WHERE country='CN'; -- 选取country为CN的元组
```

运算符：

| 运算符 | 说明 |
|----------|----------|
| = | 等于 |
| <> | 不等于。注释：在 SQL 的一些版本中，该操作符可被写成 != |
| > | 大于 |
| < | 小于 |
| >= | 大于等于 |
| <= | 小于等于 |
| BETWEEN | 在某个范围内 |
| LIKE | 搜索某种模式 |
| IN | 指定针对某个列的多个可能值 |

### AND & OR/运算符

```sql
SELECT * FROM Websites WHERE country='USA' OR country='CN'; -- 选取country等于USA或CN的元组

SELECT * FROM Websites WHERE alexa > 15 AND (country='CN' OR country='USA'); -- 套接更多的逻辑
```

### LINK/搜索模式

指定WHERE搜索模式。

- 通配符百分比(%)：任何零个或多个字符的字符串。- 下划线(_)通配符：任何单个字符。
- [list of characters]通配符：指定集合中的任何单个字符。
- [character-character]：指定范围内的任何单个字符。
- [^]：不在列表或范围内的任何单个字符。

```sql
SELECT * FROM Websites WHERE name LIKE 'G%'; -- 完整示例

LIKE 'z%' -- 以z开头的元组
LIKE '%er' -- 以er结尾的元组
LIKE 't%s' -- 以t开头s结尾的元组
LIKE '_u%' -- 第二个字符为u的元组
LIKE '[YZ]%' -- 第一个字符为Y或Z的元组
LIKE '[A-C]%' -- 第一个字符为A-C范围的元组
LIKE '[^A-X]%' -- 第一个字符不为A-X范围的元组
NOT LIKE 'A%' -- 第一个字符不是A的元组
```

### IN/多个过滤

设置WHERE多个过滤值。

```sql
SELECT * FROM Websites WHERE name IN ('Google','菜鸟教程'); -- 匹配字段name为Google、菜鸟教程的元组
```

### BETWEEN/范围取值

选取两个值之间的范围内的值，这些值可以是数值、文本、日期。

```sql
SELECT * FROM Websites WHERE alexa BETWEEN 1 AND 20; -- 匹配alexa字段为1-20范围的元组

NOT BETWEEN 1 AND 20 -- 匹配取反

WHERE (alexa BETWEEN 1 AND 20) AND country NOT IN ('USA', 'IND') -- 匹配alexa字段为1-20范围之间但country字段不为USA、IND的元组

WHERE name BETWEEN 'A' AND 'H' -- 匹配A-H范围之间的字段

WHERE date BETWEEN '2016-05-10' AND '2016-05-14' -- 匹配时间范围之间的字段
```

### AS/别名

为匹配的结果按别名显示。

在下面的情况下，使用别名很有用：

- 在查询中涉及超过一个表
- 在查询中使用了函数
- 列名称很长或者可读性差
- 需要把两个列或者多个列结合在一起

```sql
SELECT name AS n, country AS c FROM Websites -- 将name字段改为n，country字段改为c

SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info FROM Websites -- 将字段url、alexa、country字段合并为site_info输出
```

### JOIN/联表查询

{% asset_img sql-join.png %}

### UNION/合并结果

必须拥有相同的字段，字段类型必须相同。

```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country; -- 匹配Websites与apps表country字段的数据，不会显示重复的值

SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country; -- 使用UNION ALL显示重复的值

SELECT country, name FROM Websites
WHERE country='CN'
UNION ALL
SELECT country, app_name FROM apps
WHERE country='CN'
ORDER BY country; -- 带有WHERE的示例
```

## SELECT INTO/复制到新表

```sql
SELECT * INTO WebsitesBackup2016 FROM Websites; --复制全部数据

SELECT name, url INTO WebsitesBackup2016 FROM Websites; -- 复制name、url数据

SELECT * INTO WebsitesBackup2016 FROM Websites WHERE country='CN'; -- 复制条件匹配的数据

SELECT Websites.name, access_log.count, access_log.date
INTO WebsitesBackup2016
FROM Websites
LEFT JOIN access_log
ON Websites.id=access_log.site_id; -- 复制多个表的数据到新表
```

## INSERT INTO SELECT/复制到已存在的表

```sql
INSERT INTO Websites (name, country) SELECT app_name, country FROM apps; -- 复制apps表中app_name、country字段到Websites表name、country中

INSERT INTO Websites (name, country) SELECT app_name, country FROM apps WHERE id=1; -- 添加过滤
```

## CREATE DATABASE/创建数据库

```sql
CREATE DATABASE dbname;
```

## CREATE TABLE/创建表

```sql
CREATE TABLE Persons
(
PersonID int,
LastName varchar(255),
FirstName varchar(255),
Address varchar(255),
City varchar(255)
);
```

### 约束

约束字段的数据规则。

- NOT NULL - 指示某列不能存储 NULL 值。
- UNIQUE - 保证某列的每行必须有唯一的值。
- PRIMARY KEY - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- FOREIGN KEY - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- CHECK - 保证列中的值符合指定的条件。
- DEFAULT - 规定没有给列赋值时的默认值。

```sql
CREATE TABLE table_name
(
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
....
);
```

### AUTO INCREMENT/创建自动主键字段

```sql
CREATE TABLE Persons
(
ID int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (ID)
) -- 将ID定义为主键，开始值为1
```

## CREATE INDEX/创建索引

用户无法看到索引。

```sql
CREATE INDEX index_name ON table_name (column_name) -- 创建一个简单索引，允许重复值

CREATE UNIQUE INDEX index_name
ON table_name (column_name) -- 创建一个唯一索引，不允许重复
```

## DROP/删除索引、表、库

```sql
ALTER TABLE table_name DROP INDEX index_name -- MySQL的删除索引

DROP TABLE table_name -- 删除表

DROP DATABASE database_name -- 删除库

TRUNCATE TABLE table_name -- 删除表内数据
```

## ALTER TABLE/操作字段

```sql
ALTER TABLE Persons ADD DateOfBirth date -- 添加DateOfBirth字段类型为date

ALTER TABLE Persons ALTER COLUMN DateOfBirth year -- 更改字段类型为year

ALTER TABLE Persons DROP COLUMN DateOfBirth -- 删除字段
```

## 函数

SQL Aggregate 函数计算从列中取得的值，返回一个单一的值：

- AVG() - 返回平均值
- COUNT() - 返回行数
- FIRST() - 返回第一个记录的值
- LAST() - 返回最后一个记录的值
- MAX() - 返回最大值
- MIN() - 返回最小值
- SUM() - 返回总和

SQL Scalar 函数基于输入值，返回一个单一的值：

- UCASE() - 将某个字段转换为大写
- LCASE() - 将某个字段转换为小写
- MID() - 从某个文本字段提取字符，MySql 中使用
- SubString(字段，1，end) - 从某个文本字段提取字符
- LEN() - 返回某个文本字段的长度
- ROUND() - 对某个数值字段进行指定小数位数的四舍五入
- NOW() - 返回当前的系统日期和时间
- FORMAT() - 格式化某个字段的显示方式
