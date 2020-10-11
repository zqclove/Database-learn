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

# SQL CREATE INDEX 语句

------

CREATE INDEX 语句用于在表中创建索引。

在不读取整个表的情况下，索引使数据库应用程序可以更快地查找数据。

------

## 索引

您可以在表中创建索引，以便更加快速高效地查询数据。

用户无法看到索引，它们只能被用来加速搜索/查询。

**注释：**更新一个包含索引的表需要比更新一个没有索引的表花费更多的时间，这是由于索引本身也需要更新。因此，理想的做法是仅仅在常常被搜索的列（以及表）上面创建索引。

### SQL CREATE INDEX 语法

在表上创建一个简单的索引。允许使用重复的值：

```sql
CREATE INDEX index_name
ON table_name (column_name)
```

### SQL CREATE UNIQUE INDEX 语法

在表上创建一个唯一的索引。不允许使用重复的值：唯一的索引意味着两个行不能拥有相同的索引值。Creates a unique index on a table. Duplicate values are not allowed:

```sql
CREATE UNIQUE INDEX index_name
ON table_name (column_name)
```

**注释：**用于创建索引的语法在不同的数据库中不一样。因此，检查您的数据库中创建索引的语法。 

------

## CREATE INDEX 实例

下面的 SQL 语句在 "Persons" 表的 "LastName" 列上创建一个名为 "PIndex" 的索引：

```sql
CREATE INDEX PIndex
ON Persons (LastName)
```

如果您希望索引不止一个列，您可以在括号中列出这些列的名称，用逗号隔开：

```sql
CREATE INDEX PIndex
ON Persons (LastName, FirstName)
```

# SQL 撤销索引、撤销表以及撤销数据库

------

通过使用 DROP 语句，可以轻松地删除索引、表和数据库。

------

## DROP INDEX 语句

DROP INDEX 语句用于删除表中的索引。

**用于 MS Access 的 DROP INDEX 语法：**

```sql
DROP INDEX index_name ON table_name
```

**用于 MS SQL Server 的 DROP INDEX 语法：**

```sql
DROP INDEX table_name.index_name
```

**用于 DB2/Oracle 的 DROP INDEX 语法：**

```sql
DROP INDEX index_name
```

**用于 MySQL 的 DROP INDEX 语法：**

```sql
ALTER TABLE table_name DROP INDEX index_name
```

------

## DROP TABLE 语句

DROP TABLE 语句用于删除表。

```sql
DROP TABLE table_name
```

------

## DROP DATABASE 语句

DROP DATABASE 语句用于删除数据库。

```sql
DROP DATABASE database_name
```

------

## TRUNCATE TABLE 语句

如果我们仅仅需要删除表内的数据，但并不删除表本身，那么我们该如何做呢？

请使用 TRUNCATE TABLE 语句：

```sql
TRUNCATE TABLE table_name
```

# SQL ALTER TABLE 语句

------

## ALTER TABLE 语句

ALTER TABLE 语句用于在已有的表中添加、删除或修改列。

### SQL ALTER TABLE 语法

如需在表中添加列，请使用下面的语法:

```sql
ALTER TABLE table_name
ADD column_name datatype
```

如需删除表中的列，请使用下面的语法（请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：

```sql
ALTER TABLE table_name
DROP COLUMN column_name
```

要改变表中列的数据类型，请使用下面的语法：

**SQL Server / MS Access：**

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

**My SQL / Oracle：**

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name datatype
```

Oracle 10G 之后版本:

```sql
ALTER TABLE table_name
MODIFY column_name datatype;
```

------

## SQL ALTER TABLE 实例

请看 "Persons" 表：

| P_Id | LastName  | FirstName | Address      | City      |
| ---- | --------- | --------- | ------------ | --------- |
| 1    | Hansen    | Ola       | Timoteivn 10 | Sandnes   |
| 2    | Svendson  | Tove      | Borgvn 23    | Sandnes   |
| 3    | Pettersen | Kari      | Storgt 20    | Stavanger |

现在，我们想在 "Persons" 表中添加一个名为 "DateOfBirth" 的列。

我们使用下面的 SQL 语句：

```sql
ALTER TABLE Persons
 ADD DateOfBirth date
```

请注意，新列 "DateOfBirth" 的类型是 date，可以存放日期。数据类型规定列中可以存放的数据的类型。

现在，"Persons" 表将如下所示：

| P_Id | LastName  | FirstName | Address      | City      | DateOfBirth |
| ---- | --------- | --------- | ------------ | --------- | ----------- |
| 1    | Hansen    | Ola       | Timoteivn 10 | Sandnes   |             |
| 2    | Svendson  | Tove      | Borgvn 23    | Sandnes   |             |
| 3    | Pettersen | Kari      | Storgt 20    | Stavanger |             |

------

## 改变数据类型实例

现在，我们想要改变 "Persons" 表中 "DateOfBirth" 列的数据类型。

我们使用下面的 SQL 语句：

```sql
ALTER TABLE Persons
ALTER COLUMN DateOfBirth year
```

请注意，现在 "DateOfBirth" 列的类型是 year，可以存放 2 位或 4 位格式的年份。

------

## DROP COLUMN 实例

接下来，我们想要删除 "Person" 表中的 "DateOfBirth" 列。

我们使用下面的 SQL 语句：

```sql
ALTER TABLE Persons
DROP COLUMN DateOfBirth
```

现在，"Persons" 表将如下所示：

| P_Id | LastName  | FirstName | Address      | City      |
| ---- | --------- | --------- | ------------ | --------- |
| 1    | Hansen    | Ola       | Timoteivn 10 | Sandnes   |
| 2    | Svendson  | Tove      | Borgvn 23    | Sandnes   |
| 3    | Pettersen | Kari      | Storgt 20    | Stavanger |

# SQL AUTO INCREMENT 字段

------

Auto-increment 会在新记录插入表中时生成一个唯一的数字。

------

## AUTO INCREMENT 字段

我们通常希望在每次插入新记录时，自动地创建主键字段的值。

我们可以在表中创建一个 auto-increment 字段。

------

## 用于 MySQL 的语法

下面的 SQL 语句把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段：

```sql
CREATE TABLE Persons
 (
 ID int NOT NULL AUTO_INCREMENT,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255),
 PRIMARY KEY (ID)
 )
```

MySQL 使用 AUTO_INCREMENT 关键字来执行 auto-increment 任务。

默认地，AUTO_INCREMENT 的开始值是 1，每条新记录递增 1。

要让 AUTO_INCREMENT 序列以其他的值起始，请使用下面的 SQL 语法：

```sql
ALTER TABLE Persons AUTO_INCREMENT=100
```

要在 "Persons" 表中插入新记录，我们不必为 "ID" 列规定值（会自动添加一个唯一的值）：

```sql
INSERT INTO Persons (FirstName,LastName)
VALUES ('Lars','Monsen')
```

上面的 SQL 语句会在 "Persons" 表中插入一条新记录。"ID" 列会被赋予一个唯一的值。"FirstName" 列会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。

------

## 用于 SQL Server 的语法

下面的 SQL 语句把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段：

```sql
CREATE TABLE Persons
 (
 ID int IDENTITY(1,1) PRIMARY KEY,
 LastName varchar(255) NOT NULL,
 FirstName varchar(255),
 Address varchar(255),
 City varchar(255)
 )
```

**MS SQL Server 使用 IDENTITY 关键字来执行 auto-increment 任务。**

在上面的实例中，IDENTITY 的开始值是 1，每条新记录递增 1。

**提示：**要规定 "ID" 列以 10 起始且递增 5，请把 identity 改为 IDENTITY(10,5)。

要在 "Persons" 表中插入新记录，我们不必为 "ID" 列规定值（会自动添加一个唯一的值）：

```sql
INSERT INTO Persons (FirstName,LastName)
VALUES ('Lars','Monsen')
```

上面的 SQL 语句会在 "Persons" 表中插入一条新记录。"ID" 列会被赋予一个唯一的值。"FirstName" 列会被设置为 "Lars"，"LastName" 列会被设置为 "Monsen"。

**给已经存在的colume添加自增语法：**

```sql
ALTER TABLE table_name CHANGE column_name 
column_name data_type(size) constraint_name AUTO_INCREMENT;
```

比如：

```sql
ALTER TABLE student CHANGE id 
id INT( 11 ) NOT NULL AUTO_INCREMENT;
```

# SQL 视图（Views）

------

视图是可视化的表。

**视图的作用：**

1、视图隐藏了底层的表结构，简化了数据访问操作，客户端不再需要知道底层表的结构及其之间的关系。

2、视图提供了一个统一访问数据的接口。（即可以允许用户通过视图访问数据的安全机制，而不授予用户直接访问底层表的权限）

3、从而加强了安全性，使用户只能看到视图所显示的数据。

4、视图还可以被嵌套，一个视图中可以嵌套另一个视图。

------

## SQL CREATE VIEW 语句

在 SQL 中，视图是基于 SQL 语句的结果集的可视化的表。

视图包含行和列，就像一个真实的表。**视图中的字段就是来自一个或多个数据库中的真实的表中的字段。**

您可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，也可以呈现数据，就像这些数据来自于某个单一的表一样。

### SQL CREATE VIEW 语法

```sql
CREATE VIEW view_name AS
 SELECT column_name(s)
 FROM table_name
 WHERE condition
```

**注释：**视图总是显示最新的数据！每当用户查询视图时，数据库引擎通过使用视图的 SQL 语句重建数据。

------

## SQL CREATE VIEW 实例

样本数据库 Northwind 拥有一些被默认安装的视图。

视图 "Current Product List" 会从 "Products" 表列出所有正在使用的产品（未停产的产品）。这个视图使用下面的 SQL 创建：

```sql
CREATE VIEW [Current Product List] AS
SELECT ProductID,ProductName
FROM Products
WHERE Discontinued=No
```

我们可以像这样查询上面这个视图：

```sql
SELECT * FROM [Current Product List]
```

Northwind 样本数据库的另一个视图会选取 "Products" 表中所有单位价格高于平均单位价格的产品：

```sql
CREATE VIEW [Products Above Average Price] AS
SELECT ProductName,UnitPrice
FROM Products
WHERE UnitPrice>(SELECT AVG(UnitPrice) FROM Products)
```

我们可以像这样查询上面这个视图：

```sql
SELECT * FROM [Products Above Average Price]
```

Northwind 样本数据库的另一个视图会计算在 1997 年每个种类的销售总数。请注意，**这个视图会从另一个名为 "Product Sales for 1997" 的视图那里选取数据**：

```sql
CREATE VIEW [Category Sales For 1997] AS
SELECT DISTINCT CategoryName,Sum(ProductSales) AS CategorySales
FROM [Product Sales for 1997]
GROUP BY CategoryName
```

我们可以像这样查询上面这个视图：

```sql
SELECT * FROM [Category Sales For 1997]
```

我们也可以向查询添加条件。现在，我们仅仅需要查看 "Beverages" 类的销售总数：

```sql
SELECT * FROM [Category Sales For 1997]
WHERE CategoryName='Beverages'
```

------

## SQL 更新视图

您可以使用下面的语法来更新视图：

### SQL CREATE OR REPLACE VIEW 语法

```sql
CREATE OR REPLACE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition
```

现在，我们希望向 "Current Product List" 视图添加 "Category" 列。我们将通过下列 SQL 更新视图：

```sql
CREATE VIEW [Current Product List] AS
SELECT ProductID,ProductName,Category
FROM Products
WHERE Discontinued=No
```

### SQL Server

```sql
ALTER VIEW [ schema_name . ] view_name [ ( column [ ,...n ] ) ] 
[ WITH <view_attribute> [ ,...n ] ] 
AS select_statement 
[ WITH CHECK OPTION ] [ ; ]

<view_attribute> ::= 
{ 
    [ ENCRYPTION ]
    [ SCHEMABINDING ]
    [ VIEW_METADATA ]     
} 
```

- **schema_name:** 视图所属架构的名称。
- **view_name:** 要更改的视图。
- **column:** 将成为指定视图的一部分的一个或多个列的名称（以逗号分隔）。

------

## SQL 撤销视图

您可以通过 DROP VIEW 命令来删除视图。

### SQL DROP VIEW 语法

```sql
DROP VIEW view_name
```

# SQL Date 函数

------

## SQL 日期（Dates）

![Note](https://www.runoob.com/images/lamp.gif)当我们处理日期时，最难的任务恐怕是确保所插入的日期的格式，与数据库中日期列的格式相匹配。

只要您的数据包含的只是日期部分，运行查询就不会出问题。但是，如果涉及时间部分，情况就有点复杂了。

在讨论日期查询的复杂性之前，我们先来看看最重要的内建日期处理函数。

------

## MySQL Date 函数

下面的表格列出了 MySQL 中最重要的内建日期函数：

| 函数                                                         | 描述                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [NOW()](https://www.runoob.com/sql/func-now.html)            | 返回当前的日期和时间                |
| [CURDATE()](https://www.runoob.com/sql/func-curdate.html)    | 返回当前的日期                      |
| [CURTIME()](https://www.runoob.com/sql/func-curtime.html)    | 返回当前的时间                      |
| [DATE()](https://www.runoob.com/sql/func-date.html)          | 提取日期或日期/时间表达式的日期部分 |
| [EXTRACT()](https://www.runoob.com/sql/func-extract.html)    | 返回日期/时间的单独部分             |
| [DATE_ADD()](https://www.runoob.com/sql/func-date-add.html)  | 向日期添加指定的时间间隔            |
| [DATE_SUB()](https://www.runoob.com/sql/func-date-sub.html)  | 从日期减去指定的时间间隔            |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff-mysql.html) | 返回两个日期之间的天数              |
| [DATE_FORMAT()](https://www.runoob.com/sql/func-date-format.html) | 用不同的格式显示日期/时间           |



------

## SQL Server Date 函数

下面的表格列出了 SQL Server 中最重要的内建日期函数：

| 函数                                                        | 描述                             |
| ----------------------------------------------------------- | -------------------------------- |
| [GETDATE()](https://www.runoob.com/sql/func-getdate.html)   | 返回当前的日期和时间             |
| [DATEPART()](https://www.runoob.com/sql/func-datepart.html) | 返回日期/时间的单独部分          |
| [DATEADD()](https://www.runoob.com/sql/func-dateadd.html)   | 在日期中添加或减去指定的时间间隔 |
| [DATEDIFF()](https://www.runoob.com/sql/func-datediff.html) | 返回两个日期之间的时间           |
| [CONVERT()](https://www.runoob.com/sql/func-convert.html)   | 用不同的格式显示日期/时间        |

------

## SQL Date 数据类型

**MySQL** 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：YYYY-MM-DD HH:MM:SS
- YEAR - 格式：YYYY 或 YY

**SQL Server** 使用下列数据类型在数据库中存储日期或日期/时间值：

- DATE - 格式：YYYY-MM-DD
- DATETIME - 格式：YYYY-MM-DD HH:MM:SS
- SMALLDATETIME - 格式：YYYY-MM-DD HH:MM:SS
- TIMESTAMP - 格式：唯一的数字

**注释：**当您在数据库中创建一个新表时，需要为列选择数据类型！

------

## SQL 日期处理

![Note](https://www.runoob.com/images/lamp.gif)如果不涉及时间部分，那么我们可以轻松地比较两个日期！

假设我们有如下的 "Orders" 表：

| OrderId | ProductName            | OrderDate  |
| ------- | ---------------------- | ---------- |
| 1       | Geitost                | 2008-11-11 |
| 2       | Camembert Pierrot      | 2008-11-09 |
| 3       | Mozzarella di Giovanni | 2008-11-11 |
| 4       | Mascarpone Fabioli     | 2008-10-29 |

现在，我们希望从上表中选取 OrderDate 为 "2008-11-11" 的记录。

我们使用下面的 SELECT 语句：

```sql
SELECT * FROM Orders WHERE OrderDate='2008-11-11'
```

结果集如下所示：

| OrderId | ProductName            | OrderDate  |
| ------- | ---------------------- | ---------- |
| 1       | Geitost                | 2008-11-11 |
| 3       | Mozzarella di Giovanni | 2008-11-11 |

现在，假设 "Orders" 表如下所示（请注意 "OrderDate" 列中的时间部分）：

| OrderId | ProductName            | OrderDate           |
| ------- | ---------------------- | ------------------- |
| 1       | Geitost                | 2008-11-11 13:23:44 |
| 2       | Camembert Pierrot      | 2008-11-09 15:45:21 |
| 3       | Mozzarella di Giovanni | 2008-11-11 11:12:01 |
| 4       | Mascarpone Fabioli     | 2008-10-29 14:56:59 |

如果我们使用和上面一样的 SELECT 语句：

```SQL
SELECT * FROM Orders WHERE OrderDate='2008-11-11'

或

SELECT * FROM Orders WHERE OrderDate='2008-11-11 00：00：00'
```

那么我们将得不到结果！因为表中没有"2008-11-11 00:00:00"日期。如果没有时间部分，默认时间为 00:00:00。

**提示：**如果您希望使查询简单且更易维护，那么请不要在日期中使用时间部分！

# SQL NULL 值

------

NULL 值代表遗漏的未知数据。

默认地，表的列可以存放 NULL 值。

本章讲解 IS NULL 和 IS NOT NULL 操作符。

------

## SQL NULL 值

如果表中的某个列是可选的，那么我们可以在不向该列添加值的情况下插入新记录或更新已有的记录。这意味着该字段将以 NULL 值保存。

NULL 值的处理方式与其他值不同。

NULL 用作未知的或不适用的值的占位符。

![Note](https://www.runoob.com/images/lamp.gif)**注释：**无法比较 NULL 和 0；它们是不等价的。

------

## SQL 的 NULL 值处理

请看下面的 "Persons" 表：

| P_Id | LastName  | FirstName | Address   | City      |
| ---- | --------- | --------- | --------- | --------- |
| 1    | Hansen    | Ola       |           | Sandnes   |
| 2    | Svendson  | Tove      | Borgvn 23 | Sandnes   |
| 3    | Pettersen | Kari      |           | Stavanger |

假如 "Persons" 表中的 "Address" 列是可选的。这意味着如果在 "Address" 列插入一条不带值的记录，"Address" 列会使用 NULL 值保存。

那么我们如何测试 NULL 值呢？

**无法使用比较运算符来测试 NULL 值，比如 =、< 或 <>。**

我们必须使用 IS NULL 和 IS NOT NULL 操作符。

------

## SQL IS NULL

我们如何仅仅选取在 "Address" 列中带有 NULL 值的记录呢？

我们必须使用 IS NULL 操作符：

```SQL
SELECT LastName,FirstName,Address FROM Persons
 WHERE Address IS NULL
```

结果集如下所示：

| LastName  | FirstName | Address |
| --------- | --------- | ------- |
| Hansen    | Ola       |         |
| Pettersen | Kari      |         |

![Note](https://www.runoob.com/images/lamp.gif)**提示：****请始终使用 IS NULL 来查找 NULL 值。**

------

## SQL IS NOT NULL

我们如何仅仅选取在 "Address" 列中不带有 NULL 值的记录呢？

我们必须使用 IS NOT NULL 操作符：

```sql
SELECT LastName,FirstName,Address FROM Persons
 WHERE Address IS NOT NULL
```

结果集如下所示：

| LastName | FirstName | Address   |
| -------- | --------- | --------- |
| Svendson | Tove      | Borgvn 23 |

# SQL NULL 函数

------

## SQL ISNULL()、NVL()、IFNULL() 和 COALESCE() 函数

请看下面的 "Products" 表：

| P_Id | ProductName | UnitPrice | UnitsInStock | UnitsOnOrder |
| ---- | ----------- | --------- | ------------ | ------------ |
| 1    | Jarlsberg   | 10.45     | 16           | 15           |
| 2    | Mascarpone  | 32.56     | 23           |              |
| 3    | Gorgonzola  | 15.67     | 9            | 20           |

假如 "UnitsOnOrder" 是可选的，而且可以包含 NULL 值。

我们使用下面的 SELECT 语句：

```sql
SELECT ProductName,UnitPrice*(UnitsInStock+UnitsOnOrder)
 FROM Products
```

在上面的实例中，如果有 "UnitsOnOrder" 值是 NULL，那么结果是 NULL。

微软的 ISNULL() 函数用于规定如何处理 NULL 值。

NVL()、IFNULL() 和 COALESCE() 函数也可以达到相同的结果。

在这里，我们希望 NULL 值为 0。

下面，如果 "UnitsOnOrder" 是 NULL，则不会影响计算，因为如果值是 NULL 则 ISNULL() 返回 0：

**SQL Server / MS Access**

```sql
SELECT ProductName,UnitPrice*(UnitsInStock+ISNULL(UnitsOnOrder,0))
 FROM Products
```

**Oracle**

Oracle 没有 ISNULL() 函数。不过，我们可以使用 NVL() 函数达到相同的结果：

```sql
SELECT ProductName,UnitPrice*(UnitsInStock+NVL(UnitsOnOrder,0))
 FROM Products
```

**MySQL**

MySQL 也拥有类似 ISNULL() 的函数。不过它的工作方式与微软的 ISNULL() 函数有点不同。

在 MySQL 中，我们可以使用 IFNULL() 函数，如下所示：

```sql
SELECT ProductName,UnitPrice*(UnitsInStock+IFNULL(UnitsOnOrder,0))
 FROM Products
```

或者我们可以使用 COALESCE() 函数，如下所示：

```sql
SELECT ProductName,UnitPrice*(UnitsInStock+COALESCE(UnitsOnOrder,0))
 FROM Products
```

# 参考资料

[菜鸟教程]: https://www.runoob.com/sql/sql-intro.html

