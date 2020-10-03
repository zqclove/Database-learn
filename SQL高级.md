# 	SQL高级

# SQL SELECT TOP, LIMIT, ROWNUM 子句

------

## SQL SELECT TOP 子句

SELECT TOP 子句用于规定要返回的记录的数目。

SELECT TOP 子句对于拥有数千条记录的大型表来说，是非常有用的。

> **注意:**并非所有的数据库系统都支持 SELECT TOP 语句。 MySQL 支持 LIMIT 语句来选取指定的条数数据， Oracle 可以使用 ROWNUM 来选取。

### SQL Server / MS Access 语法

```sql
SELECT TOP number|percent column_name(s)
FROM table_name;
```

### MySQL 语法

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

#### 实例

```sql
SELECT *
FROM Persons
LIMIT 5;
```

### Oracle 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```

#### 实例

```sql
SELECT *
FROM Persons
WHERE ROWNUM <=5;
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
mysql> SELECT * FROM Websites;
+----+---------------+---------------------------+-------+---------+
| id | name          | url                       | alexa | country |
+----+---------------+---------------------------+-------+---------+
|  1 | Google        | https://www.google.cm/    |     1 | USA     |
|  2 | 淘宝          | https://www.taobao.com/   |    13 | CN      |
|  3 | 菜鸟教程       | http://www.runoob.com/    |  5000 | USA     |
|  4 | 微博           | http://weibo.com/         |    20 | CN      |
|  5 | Facebook      | https://www.facebook.com/ |     3 | USA     |
|  7 | stackoverflow | http://stackoverflow.com/ |     0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

------

## MySQL SELECT LIMIT 实例

下面的 SQL 语句从 "Websites" 表中选取头两条记录：

## 实例

```sql
SELECT * FROM Websites LIMIT 2;
```

执行以上 SQL，数据如下所示：

![img](https://www.runoob.com/wp-content/uploads/2013/09/A90E535B-A499-4E3D-83DD-6A7AD1144B05.jpg)

------

## SQL SELECT TOP PERCENT 实例

在 Microsoft SQL Server 中还可以使用百分比作为参数。

下面的 SQL 语句从 websites 表中选取前面百分之 50 的记录：

### 实例

以下操作在 Microsoft SQL Server 数据库中可执行。

```sql
SELECT TOP 50 PERCENT * FROM Websites;
```

# SQL LIKE 操作符

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

## SQL LIKE 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```sql
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

------

## SQL LIKE 操作符实例

### 实例1

下面的 SQL 语句选取 name 以字母 "G" 开头的所有行：

```sql
SELECT * FROM Websites
WHERE name LIKE 'G%'; 
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/like1.jpg)

**提示：**"%" 符号用于在模式的前后定义通配符（默认字母）。您将在下一章中学习更多有关通配符的知识。

### 实例2

下面的 SQL 语句选取 name 以字母 "k" 结尾的所有行：

```sql
SELECT * FROM Websites
WHERE name LIKE '%k'; 
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/like2.jpg)

### 实例3

下面的 SQL 语句选取 name 包含模式 "oo" 的所有行：

```sql
SELECT * FROM Websites
WHERE name LIKE '%oo%';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/like3.jpg)

通过使用 NOT 关键字，您可以选取不匹配模式的记录。

### 实例4

下面的 SQL 语句选取 name 不包含模式 "oo" 的所有行：

```sql
SELECT * FROM Websites
WHERE name NOT LIKE '%oo%';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/like4.jpg)

# SQL 通配符

通配符可用于替代字符串中的任何其他字符。

在 SQL 中，通配符与 SQL LIKE 操作符一起使用。

SQL 通配符用于搜索表中的数据。

在 SQL 中，可使用以下通配符：

| 通配符                         | 描述                       |
| ------------------------------ | -------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

------

## 使用 SQL % 通配符

### 实例1

下面的 SQL 语句选取 url 以字母 "https" 开头有网站：

```sql
SELECT * FROM Websites
WHERE url LIKE 'https%'; 
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards1.jpg)

### 实例2

下面的 SQL 语句选取 url 包含模式 "oo" 的所有网站：

```sql
SELECT * FROM Websites
WHERE url LIKE '%oo%';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards2.jpg)

------

## 使用 SQL _ 通配符

### 实例1

下面的 SQL 语句选取 name 以一个任意字符开始，然后是 "oogle" 的所有行

```sql
SELECT * FROM Websites
WHERE name LIKE '_oogle';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards3.jpg)

### 实例2

下面的 SQL 语句选取 name 以 "G" 开始，然后是一个任意字符，然后是 "o"，然后是一个任意字符，然后是 "le" 的所有网站：

```sql
SELECT * FROM Websites
WHERE name LIKE 'G_o_le';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards4.jpg)

------

## 使用 SQL [charlist] 通配符

MySQL 中使用 **REGEXP** 或 **NOT REGEXP** 运算符 (或 RLIKE 和 NOT RLIKE) 来操作正则表达式。

### 实例1

下面的 SQL 语句选取 name 以 "G"、"F" 或 "s" 开始的所有网站：

```sql
SELECT * FROM Websites
WHERE name REGEXP '^[GFs]';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards5.jpg)

### 实例2

下面的 SQL 语句选取 name 以 A 到 H 字母开头的网站：

```sql
SELECT * FROM Websites
WHERE name REGEXP '^[A-H]';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards6.jpg)

### 实例3

下面的 SQL 语句选取 name 不以 A 到 H 字母开头的网站：

```sql
SELECT * FROM Websites
WHERE name REGEXP '^[^A-H]';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/wildcards7.jpg)

## 笔记

MySQL 、SQLite 只支持 % 和 _ 通配符，不支持 \[^charlist] 或 [!charlist] 通配符（ MS Access 支持，微软 office 对通配符一直支持良好，但微软有时候的通配符不支持 %，而是 *，具体看对应软件说明）。

**通配符和正则不是一回事。**

MySQL 和 SQLite 会把 like '[xxx]yyy' 的**中括号**当成普通字符，而不是通配符。

比如：

```sql
select * from persons WHERE City LIKE '[b]eijing'
```

将查出  **city** 为  **[b]eijing** 的行，而不是 **city** 为  **beijing**  的行。

MySQL 中要完成 \[^charlist] 或 [!charlist] 通配符的查询效果，需要通过**正则表达式REGEXP**来完成。

```sql
select * from persons WHERE City REGEXP '[b]eijing'
```

SQLite不支持Regexp正则方法。

# SQL IN 操作符

IN 操作符允许您在 WHERE 子句中规定多个值。

## SQL IN 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...);
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

------

## IN 操作符实例

## 实例

下面的 SQL 语句选取 name 为 "Google" 或 "菜鸟教程" 的所有网站：

```sql
SELECT * FROM Websites
WHERE name IN ('Google','菜鸟教程');
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/in1.jpg)

# SQL BETWEEN 操作符

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。

## SQL BETWEEN 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

------

## BETWEEN 操作符实例

### 实例

下面的 SQL 语句选取 alexa 介于 1 和 20 之间的所有网站：

```sql
SELECT * FROM Websites
WHERE alexa BETWEEN 1 AND 20;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/btw1.jpg)

------

## NOT BETWEEN 操作符实例

### 实例

如需显示不在上面实例范围内的网站，请使用 NOT BETWEEN：

```sql
SELECT * FROM Websites
WHERE alexa NOT BETWEEN 1 AND 20;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/btw2.jpg)

------

## 带有 IN 的 BETWEEN 操作符实例

### 实例

下面的 SQL 语句选取 alexa 介于 1 和 20 之间但 country 不为 USA 和 IND 的所有网站：

```sql
SELECT * FROM Websites
WHERE (alexa BETWEEN 1 AND 20)
AND country NOT  IN ('USA', 'IND');
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/7E2FE939-4679-4C2C-BAFC-FC5BFF6697DC.jpg)

------

## 带有文本值的 BETWEEN 操作符实例

### 实例

下面的 SQL 语句选取 name 以介于 'A' 和 'H' 之间字母开始的所有网站：

```sql
SELECT * FROM Websites
WHERE name BETWEEN 'A' AND 'H';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/btw4.jpg)

------

## 带有文本值的 NOT BETWEEN 操作符实例

### 实例

下面的 SQL 语句选取 name 不介于 'A' 和 'H' 之间字母开始的所有网站：

```sql
SELECT * FROM Websites
WHERE name NOT BETWEEN 'A' AND 'H';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/btw5.jpg)

------

## 示例表

下面是 "access_log" 网站访问记录表的数据，其中：

**aid：**为自增 id。

**site_id**：为对应 websites表的网站 id。

**count**：访问次数。

**date：**为访问日期。(date类型)

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

------

## 带有日期值的 BETWEEN 操作符实例

### 实例

下面的 SQL 语句选取 date 介于 '2016-05-10' 和 '2016-05-14' 之间的所有访问记录：

```sql
SELECT * FROM access_log
WHERE date BETWEEN '2016-05-10' AND '2016-05-14';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/btw6.jpg)



**请注意，在不同的数据库中，BETWEEN 操作符会产生不同的结果！ **在某些数据库中，BETWEEN 选取介于两个值之间但不包括两个测试值的字段。 在某些数据库中，BETWEEN 选取介于两个值之间且包括两个测试值的字段。 在某些数据库中，BETWEEN 选取介于两个值之间且包括第一个测试值但不包括最后一个测试值的字段。 **因此，请检查您的数据库是如何处理 BETWEEN 操作符！**			

# SQL 别名

通过使用 SQL，可以为表名称或列名称指定别名。

基本上，创建别名是为了让列名称的可读性更强。

## 列的 SQL 别名语法

```sql
SELECT column_name AS alias_name
FROM table_name;
```

## 表的 SQL 别名语法

```sql
SELECT column_name(s)
FROM table_name AS alias_name;
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

------

## 列的别名实例

### 实例1

下面的 SQL 语句指定了两个别名，一个是 name 列的别名，一个是 country 列的别名。**提示：**如果列名称包含空格，要求使用双引号或方括号：

```sql
SELECT name AS n, country AS c
FROM Websites;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/alias1.jpg)

### 实例2

在下面的 SQL 语句中，我们把三个列（url、alexa 和 country）结合在一起，并创建一个名为 "site_info" 的别名：

```sql
SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info
FROM Websites;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/alias2.jpg)

------

## 表的别名实例

### 实例1

下面的 SQL 语句选取 "菜鸟教程" 的所访问记录。我们使用 "Websites" 和 "access_log" 表，并分别为它们指定表别名 "w" 和 "a"（通过使用别名让 SQL 更简短）：

```sql
SELECT w.name, w.url, a.count, a.date 
FROM Websites AS w, access_log AS a  
WHERE a.site_id=w.id and w.name="菜鸟教程";
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/alias3.jpg)

### 实例2

不带别名的相同的 SQL 语句：

```sql
SELECT Websites.name, Websites.url, access_log.count, access_log.date 
FROM Websites, access_log  
WHERE Websites.id=access_log.site_id and Websites.name="菜鸟教程";
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/alias4.jpg)

在下面的情况下，使用别名很有用：

- 在查询中涉及超过一个表
- 在查询中使用了函数
- 列名称很长或者可读性差
- 需要把两个列或者多个列结合在一起

# SQL 连接(JOIN)

------

SQL join 用于把来自两个或多个表的行结合起来。

下图展示了 LEFT JOIN、RIGHT JOIN、INNER JOIN、OUTER JOIN 相关的 7 种用法。

[![img](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)](https://www.runoob.com/wp-content/uploads/2019/01/sql-join.png)

------

## SQL JOIN

SQL JOIN 子句用于把来自两个或多个表的行结合起来，基于这些表之间的共同字段。

最常见的 JOIN 类型：**SQL INNER JOIN（简单的 JOIN）**。  SQL INNER JOIN 从多个表中返回满足 JOIN 条件的所有行。

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

请注意，"Websites" 表中的 "**id**" 列指向 "access_log" 表中的字段  "**site_id**"。上面这两个表是通过 "site_id" 列联系起来的。

然后，如果我们运行下面的 SQL 语句（包含 INNER JOIN）：

## 实例

```sql
SELECT Websites.id, Websites.name, access_log.count, access_log.date
FROM  Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/join1.jpg)



------

## 不同的 SQL JOIN

在我们继续讲解实例之前，我们先列出您可以使用的不同的 SQL JOIN 类型：

- **INNER JOIN**：如果表中有至少一个匹配，则返回行
- **LEFT JOIN**：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN**：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN**：只要其中一个表中存在匹配，则返回行

# SQL INNER JOIN 关键字

INNER JOIN 关键字在表中存在至少一个匹配时返回行。

## SQL INNER JOIN 语法

```sql
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;
```

或：

```sql
SELECT column_name(s)
FROM table1
JOIN table2
ON table1.column_name*table2.column_name;
```

**注释：**INNER JOIN 与 JOIN 是相同的。

![SQL INNER JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_innerjoin.gif)

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

------

## SQL INNER JOIN 实例

下面的 SQL 语句将返回所有网站的访问记录：

```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
INNER  JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY  access_log.count;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/inner-join1.jpg)

**注释：**INNER JOIN 关键字在表中存在至少一个匹配时返回行。如果 "Websites" 表中的行在 "access_log" 中没有匹配，则不会列出这些行。

# SQL LEFT JOIN 关键字

LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。

## SQL LEFT JOIN 语法

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
```

或：

```sql
SELECT column_name(s)
FROM table1
LEFT OUTER JOIN table2
ON table1.column_name=table2.column_name;
```

**注释：**在某些数据库中，LEFT JOIN 称为 LEFT OUTER JOIN。

![SQL LEFT JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_leftjoin.gif)

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

------

## SQL LEFT JOIN 实例

下面的 SQL 语句将返回所有网站及他们的访问量（如果有的话）。

以下实例中我们把 Websites 作为左表，access_log 作为右表：

```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
LEFT JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count DESC;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/left-join1.jpg)

**注释：**LEFT JOIN 关键字从左表（Websites）返回所有的行，即使右表（access_log）中没有匹配。

# SQL RIGHT JOIN 关键字

RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。

## SQL RIGHT JOIN 语法

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
```

或：

```sql
SELECT column_name(s)
FROM table1
RIGHT OUTER JOIN table2
ON table1.column_name=table2.column_name;
```

**注释：**在某些数据库中，RIGHT JOIN 称为 RIGHT OUTER JOIN。

![SQL RIGHT JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_rightjoin.gif)

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

操作前先先在 access_log 表添加一条数据，该数据在 Websites 表没有对应的数据：

```sql
INSERT INTO `access_log` (`aid`, `site_id`, `count`, `date`) 
VALUES ('10', '6', '111', '2016-03-09');
```

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
mysql> SELECT * FROM access_log;
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
|  10 |       6 |   111 | 2016-03-19 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```



------

## SQL RIGHT JOIN 实例

下面的 SQL 语句将返回网站的访问记录。

以下实例中我们把 Websites 作为左表，access_log  作为右表：

```sql
SELECT websites.name, access_log.count, access_log.date 
FROM websites 
RIGHT JOIN access_log 
ON access_log.site_id=websites.id 
ORDER BY access_log.count DESC;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/402A662D-3553-449C-B980-942D864412BD.jpg)

**注释：**RIGHT JOIN 关键字从右表（Websites）返回所有的行，即使左表（access_log）中没有匹配。

# SQL FULL OUTER JOIN 关键字

FULL OUTER JOIN 关键字只要左表（table1）和右表（table2）其中一个表中存在匹配，则返回行.

FULL OUTER JOIN 关键字结合了 LEFT JOIN 和 RIGHT JOIN 的结果。

## SQL FULL OUTER JOIN 语法

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name=table2.column_name;
```

![SQL FULL OUTER JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_fulljoin.gif)

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "access_log" 网站访问记录表的数据：

```
+-----+---------+-------+------------+
| aid | site_id | count | date       |
+-----+---------+-------+------------+
|   1 |       1 |    45 | 2016-05-10 |
|   2 |       3 |   100 | 2016-05-13 |
|   3 |       1 |   230 | 2016-05-14 |
|   4 |       2 |    10 | 2016-05-14 |
|   5 |       5 |   205 | 2016-05-14 |
|   6 |       4 |    13 | 2016-05-15 |
|   7 |       3 |   220 | 2016-05-15 |
|   8 |       5 |   545 | 2016-05-16 |
|   9 |       3 |   201 | 2016-05-17 |
+-----+---------+-------+------------+
9 rows in set (0.00 sec)
```

------

## SQL FULL OUTER JOIN 实例

下面的 SQL 语句选取所有网站访问记录。

**MySQL中不支持 FULL OUTER JOIN**，你可以在 SQL Server 测试以下实例。

```sql
SELECT Websites.name, access_log.count, access_log.date
FROM Websites
FULL OUTER JOIN access_log
ON Websites.id=access_log.site_id
ORDER BY access_log.count DESC;
```

**注释：**FULL OUTER JOIN  关键字返回左表（Websites）和右表（access_log）中所有的行。如果 "Websites" 表中的行在 "access_log"  中没有匹配或者 "access_log" 表中的行在 "Websites" 表中没有匹配，也会列出这些行。

# SQL UNION 操作符

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

**请注意，UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。**

## SQL UNION 语法

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

**注释：**默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

## SQL UNION ALL 语法

```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

**注释：**UNION 结果集中的列名总是等于 UNION 中第一个 SELECT 语句中的列名。

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
mysql> SELECT * FROM Websites;
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "apps" APP 的数据：

```
mysql> SELECT * FROM apps;
+----+------------+-------------------------+---------+
| id | app_name   | url                     | country |
+----+------------+-------------------------+---------+
|  1 | QQ APP     | http://im.qq.com/       | CN      |
|  2 | 微博 APP | http://weibo.com/       | CN      |
|  3 | 淘宝 APP | https://www.taobao.com/ | CN      |
+----+------------+-------------------------+---------+
3 rows in set (0.00 sec)
```

------

## SQL UNION 实例

下面的 SQL 语句从 "Websites" 和 "apps" 表中选取所有**不同的**country（只有不同的值）：

```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps
ORDER BY country;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/union1.jpg)

**注释：**UNION 不能用于列出两个表中所有的country。如果一些网站和APP来自同一个国家，每个国家只会列出一次。UNION 只会选取不同的值。请使用 UNION ALL 来选取重复的值！

------

## SQL UNION ALL 实例

下面的 SQL 语句使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**country（也有重复的值）：

```sql、
SELECT country FROM Websites
UNION ALL
SELECT country FROM apps
ORDER BY country;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/union2.jpg)

------

## 带有 WHERE 的 SQL UNION ALL

下面的 SQL 语句使用 UNION ALL 从 "Websites" 和 "apps" 表中选取**所有的**中国(CN)的数据（也有重复的值）：

```sql
SELECT country, name FROM Websites
WHERE country='CN'
UNION ALL
SELECT country, app_name FROM apps
WHERE country='CN'
ORDER BY  country;
```

执行以上 SQL 输出结果如下：

![img](https://www.runoob.com/wp-content/uploads/2013/09/AAA99C7B-36A5-43FB-B489-F8CE63B62C71.jpg)

# SQL SELECT INTO 语句

SELECT INTO 语句从一个表复制数据，然后把数据插入到另一个新表中。

**注意：**

**MySQL 数据库不支持 SELECT ... INTO 语句**，但支持  **INSERT INTO ... SELECT**

当然你可以使用以下语句来拷贝表结构及数据：

```sql
CREATE TABLE 新表
AS
SELECT * FROM 旧表 
```

### SQL SELECT INTO 语法

我们可以复制所有的列插入到新表中：

```sql
SELECT *
INTO newtable [IN externaldb]
FROM table1;
```

或者只复制希望的列插入到新表中：

```sql
SELECT column_name(s)
INTO newtable [IN externaldb]
FROM table1;
```

| ![lamp](https://www.runoob.com/images/lamp.jpg) | **提示：**新表将会使用 SELECT 语句中定义的列名称和类型进行创建。您可以使用 AS 子句来应用新名称。 |
| ----------------------------------------------- | ------------------------------------------------------------ |
|                                                 |                                                              |

------

## SQL SELECT INTO 实例

创建 Websites 的备份复件：

```sql
SELECT *
INTO WebsitesBackup2016
FROM Websites;
```

只复制一些列插入到新表中：

```sql
SELECT name,  url
INTO WebsitesBackup2016
FROM Websites;
```

只复制中国的网站插入到新表中：

```sql
SELECT *
INTO WebsitesBackup2016
FROM Websites
WHERE country='CN';
```

复制多个表中的数据插入到新表中：

```sql
SELECT Websites.name, access_log.count, access_log.date
INTO WebsitesBackup2016
FROM Websites
LEFT JOIN access_log
ON Websites.id=access_log.site_id;
```

**提示：**SELECT INTO 语句可用于通过另一种模式创建一个新的空表。只需要添加促使查询没有数据返回的 WHERE 子句即可：

```sql
SELECT *
INTO newtable
FROM table1
WHERE 1=0;
```

只复制表结构

# SQL INSERT INTO SELECT 语句

INSERT INTO SELECT 语句从一个表复制数据，然后把数据插入到一个已存在的表中。目标表中任何已存在的行都不会受影响。

## SQL INSERT INTO SELECT 语法

我们可以从一个表中复制所有的列插入到另一个已存在的表中：

```sql
INSERT INTO table2
SELECT * FROM table1;
```

或者我们可以只复制希望的列插入到另一个已存在的表中：

```sql
INSERT INTO table2
(column_name(s))
SELECT column_name(s)
FROM table1;
```

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
| 7  | stackoverflow | http://stackoverflow.com/ |   0 | IND     |
+----+---------------+---------------------------+-------+---------+
```

下面是 "apps" APP 的数据：

```
mysql> SELECT * FROM apps;
+----+------------+-------------------------+---------+
| id | app_name   | url                     | country |
+----+------------+-------------------------+---------+
|  1 | QQ APP     | http://im.qq.com/       | CN      |
|  2 | 微博 APP | http://weibo.com/       | CN      |
|  3 | 淘宝 APP | https://www.taobao.com/ | CN      |
+----+------------+-------------------------+---------+
3 rows in set (0.00 sec)
```

------

## SQL INSERT INTO SELECT 实例

复制 "apps" 中的数据插入到 "Websites" 中：

```sql
INSERT INTO Websites (name, country)
SELECT app_name, country FROM apps;
```

只复 QQ 的 APP 到 "Websites" 中：

```sql
INSERT INTO Websites (name, country)
SELECT app_name, country FROM apps
WHERE id=1;
```

# SQL 约束（Constraints）

SQL 约束用于规定表中的数据规则。

如果存在违反约束的数据行为，行为会被约束终止。

约束可以在创建表时规定（通过 CREATE TABLE 语句），或者在表创建之后规定（通过 ALTER TABLE 语句）。

## SQL CREATE TABLE + CONSTRAINT 语法

```sql
CREATE TABLE table_name
 (
column_name1 data_type(size) constraint_name,
column_name2 data_type(size) constraint_name,
column_name3 data_type(size) constraint_name,
 ....
);
```

在 SQL 中，我们有如下约束：

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- **CHECK** - 保证列中的值符合指定的条件。
- **DEFAULT** - 规定没有给列赋值时的默认值。

在下面的章节，我们会详细讲解每一种约束。

# SQL NOT NULL 约束

NOT NULL 约束强制列不接受 NULL 值。

NOT NULL 约束强制字段始终包含值。这意味着，如果不向字段添加值，就无法插入新记录或者更新记录。

下面的 SQL 强制 "ID" 列、 "LastName" 列以及 "FirstName" 列不接受 NULL 值：

## 实例

```sql
CREATE TABLE Persons (    
    ID int NOT NULL,    
    LastName varchar(255) NOT NULL,    
    FirstName varchar(255) NOT NULL,    
    Age int 
);
```

## 添加 NOT NULL 约束

在一个已创建的表的 "Age" 字段中添加 NOT NULL 约束如下所示：

### 实例

```sql
ALTER TABLE Persons MODIFY Age int NOT NULL;
```

## 删除 NOT NULL 约束

在一个已创建的表的 "Age" 字段中删除 NOT NULL 约束如下所示：

### 实例

```sql
ALTER TABLE Persons MODIFY Age int NULL;
```

# SQL UNIQUE 约束

UNIQUE 约束唯一标识数据库表中的每条记录。

UNIQUE 和 PRIMARY KEY 约束均为列或列集合提供了唯一性的保证。

PRIMARY KEY 约束拥有自动定义的 UNIQUE 约束。

请注意，每个表可以有多个 UNIQUE 约束，但是每个表只能有一个 PRIMARY KEY 约束。

------

## CREATE TABLE 时的 SQL UNIQUE 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 UNIQUE 约束：

**MySQL：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL,
 LastName varchar(255) NOT NULL, 
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 Age int UNIQUE, 
 -- 也可以在最后添加 UNIQUE (Age)
 )
```

如需命名 UNIQUE 约束，并定义多个列的 UNIQUE 约束，请使用下面的 SQL 语法：

**MySQL / SQL Server / Oracle / MS Access：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 CONSTRAINT uc_PersonID UNIQUE (P_Id,LastName)
 )
```

------

## ALTER TABLE 时的 SQL UNIQUE 约束

当表已被创建时，如需在 "P_Id" 列创建 UNIQUE 约束，请使用下面的 SQL：

**MySQL / SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
ADD UNIQUE (P_Id) -- 如果需要多列参考创建的时候的语法 
```

------

## 撤销 UNIQUE 约束

如需撤销 UNIQUE 约束，请使用下面的 SQL：

```sql
ALTER TABLE Persons
DROP INDEX uc_PersonID -- DROP CONSTRAINT uc_PersonID
```

# SQL PRIMARY KEY 约束

PRIMARY KEY 约束唯一标识数据库表中的每条记录。

主键必须包含唯一的值。

主键列不能包含 NULL 值。

每个表都应该有一个主键，并且每个表只能有一个主键。

------

## CREATE TABLE 时的 SQL PRIMARY KEY 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 PRIMARY KEY 约束：

**MySQL：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL PRIMARY KEY,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 -- PRIMARY KEY (P_Id)
 )
```

如需命名 PRIMARY KEY 约束，并定义多个列的 PRIMARY KEY 约束（联合主键），请使用下面的 SQL 语法：

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
 )
```

**注释：**在上面的实例中，只有一个主键 PRIMARY KEY（pk_PersonID）。然而，pk_PersonID 的值是由两个列（P_Id 和  LastName）组成的。

------

## ALTER TABLE 时的 SQL PRIMARY KEY 约束

当表已被创建时，如需在 "P_Id" 列创建 PRIMARY KEY 约束，请使用下面的 SQL：

**MySQL / SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
ADD PRIMARY KEY (P_Id) 
-- 多列的情况 ADD CONSTRAINT pk_PersonID PRIMARY KEY (P_Id,LastName)
```

**注释：**如果您使用 ALTER TABLE 语句添加主键，必须把主键列声明为不包含 NULL 值（在表首次创建时）。 

------

## 撤销 PRIMARY KEY 约束

如需撤销 PRIMARY KEY 约束，请使用下面的 SQL：

**MySQL：**

```sql
ALTER TABLE Persons
DROP PRIMARY KEY
```

**SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
DROP CONSTRAINT pk_PersonID
```

# SQL FOREIGN KEY 约束

FOREIGN KEY 约束用于预防破坏表之间连接的行为。

FOREIGN KEY 约束也能防止非法数据插入外键列，因为它必须是它指向的那个表中的值之一。

------

## CREATE TABLE 时的 SQL FOREIGN KEY 约束

在创建表的时候指定外键约束

```sql
CREATE TABLE 表名
    (
        column1 datatype null/not null,
        column2 datatype null/not null,
        ...
        CONSTRAINT 外键约束名 FOREIGN KEY  (column1,column2,... column_n) 
        REFERENCES 外键依赖的表 (column1,column2,...column_n)
        ON DELETE CASCADE--级联删除
    );
```

------

## ALTER TABLE 时的 SQL FOREIGN KEY 约束

在创建表后增加外键约束

```sql
ALTER TABLE 表名
    ADD CONSTRAINT 外键约束名
    FOREIGN KEY (column1, column2,...column_n) 
    REFERENCES 外键所依赖的表 (column1,column2,...column_n)
    ON DELETE CASCADE;--级联删除
```

------

## 撤销 FOREIGN KEY 约束

如需撤销 FOREIGN KEY 约束，请使用下面的 SQL：

**MySQL：**

```sql
ALTER TABLE 表名
DROP FOREIGN KEY 外键约束名
```

**SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE 表名
DROP CONSTRAINT 外键约束名
```

注意，在创建外键约束时，必须先创建外键约束所依赖的表，并且该列为该表的主键

# SQL CHECK 约束

**CHECK 约束用于限制列中的值的范围。**

如果对单个列定义 CHECK 约束，那么该列只允许特定的值。

如果对一个表定义 CHECK 约束，那么此约束会基于行中其他列的值在特定的列中对值进行限制。

------

## CREATE TABLE 时的 SQL CHECK 约束

下面的 SQL 在 "Persons" 表创建时在 "P_Id" 列上创建 CHECK 约束。CHECK 约束规定 "P_Id" 列必须只包含大于 0 的整数。

**MySQL：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 CHECK (P_Id>0)
 )
```

**SQL Server / Oracle / MS Access：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL CHECK (P_Id>0),
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255)
 )
```

如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：

**MySQL / SQL Server / Oracle / MS Access：**

```sql
CREATE TABLE Persons
 (
 P_Id int NOT NULL,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
 )
```

------

## ALTER TABLE 时的 SQL CHECK 约束

当表已被创建时，如需在 "P_Id" 列创建 CHECK 约束，请使用下面的 SQL：

**MySQL / SQL Server / Oracle / MS Access:**

```sql
ALTER TABLE Persons
 ADD CHECK (P_Id>0)
```

如需命名 CHECK 约束，并定义多个列的 CHECK 约束，请使用下面的 SQL 语法：

**MySQL / SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
 ADD CONSTRAINT chk_Person CHECK (P_Id>0 AND City='Sandnes')
```

------

## 撤销 CHECK 约束

如需撤销 CHECK 约束，请使用下面的 SQL：

**SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
 DROP CONSTRAINT chk_Person
```

**MySQL：**

```sql
ALTER TABLE Persons
 DROP CHECK chk_Person
```

# SQL DEFAULT 约束

DEFAULT 约束用于向列中插入默认值。

如果没有规定其他的值，那么会将默认值添加到所有的新记录。

------

## CREATE TABLE 时的 SQL DEFAULT 约束

下面的 SQL 在 "Persons" 表创建时在 "City" 列上创建 DEFAULT 约束：

**My SQL / SQL Server / Oracle / MS Access：**

```sql
CREATE TABLE Persons
(
    P_Id int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255) DEFAULT 'Sandnes'
)
```

通过使用类似 GETDATE() 这样的函数，DEFAULT 约束也可以用于插入系统值：

```sql
CREATE TABLE Orders
(
    O_Id int NOT NULL,
    OrderNo int NOT NULL,
    P_Id int,
    OrderDate date DEFAULT GETDATE()
)
```

------

## ALTER TABLE 时的 SQL DEFAULT 约束

当表已被创建时，如需在 "City" 列创建 DEFAULT 约束，请使用下面的 SQL：

**MySQL：**

```sql
ALTER TABLE Persons
ALTER City SET DEFAULT 'SANDNES'
```

**SQL Server / MS Access：**

```sql
ALTER TABLE Persons
ADD CONSTRAINT ab_c DEFAULT 'SANDNES' for City
```

**Oracle：**

```sql
ALTER TABLE Persons
MODIFY City DEFAULT 'SANDNES'
```

------

## 撤销 DEFAULT 约束

如需撤销 DEFAULT 约束，请使用下面的 SQL：

**MySQL：**

```sql
ALTER TABLE Persons
ALTER City DROP DEFAULT
```

**SQL Server / Oracle / MS Access：**

```sql
ALTER TABLE Persons
ALTER COLUMN City DROP DEFAULT
```

# 参考资料

[菜鸟教程]: https://www.runoob.com/sql/sql-intro.html

