# SQL基础

# SQL 是什么？

- SQL，指结构化查询语言，全称是 Structured Query Language。
- SQL 让您可以访问和处理数据库。
- SQL 是一种 ANSI（American National Standards Institute 美国国家标准化组织）标准的计算机语言。

------

# SQL 能做什么？

- SQL 面向数据库执行查询
- SQL 可从数据库取回数据
- SQL 可在数据库中插入新的记录
- SQL 可更新数据库中的数据
- SQL 可从数据库删除记录
- SQL 可创建新数据库
- SQL 可在数据库中创建新表
- SQL 可在数据库中创建存储过程
- SQL 可在数据库中创建视图
- SQL 可以设置表、存储过程和视图的权限

# SQL 语法

------

## 数据库表

一个数据库通常包含一个或多个表。每个表有一个名字标识（例如:"Websites"）,表包含带有数据的记录（行）。

在本教程中，我们在 MySQL 的 RUNOOB 数据库中创建了 Websites 表，用于存储网站记录。

我们可以通过以下命令查看 "Websites" 表的数据：

```
mysql> use RUNOOB;
Database changed

mysql> set names utf8;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT * FROM Websites;
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝          | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程      | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博          | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
+----+--------------+---------------------------+-------+---------+
5 rows in set (0.01 sec)
```

### 解析

- **use RUNOOB;** 命令用于选择数据库。
- **set names utf8;** 命令用于设置使用的字符集。
- **SELECT \* FROM Websites;** 读取数据表的信息。
- 上面的表包含五条记录（每一条对应一个网站信息）和5个列（id、name、url、alexa 和country）。

------

## SQL 语句

您需要在数据库上执行的大部分工作都由 SQL 语句完成。

下面的 SQL 语句从 "Websites" 表中选取所有记录：

## 实例

SELECT * FROM Websites;

在本教程中，我们将为您讲解各种不同的 SQL 语句。

------

## 请记住...

- SQL 对大小写不敏感：SELECT 与 select 是相同的。

------

## SQL 语句后面的分号？

某些数据库系统要求在每条 SQL 语句的末端使用分号。

分号是在数据库系统中分隔每条 SQL 语句的标准方法，这样就可以在对服务器的相同请求中执行一条以上的 SQL 语句。

在本教程中，我们将在每条 SQL 语句的末端使用分号。

------

## 一些最重要的 SQL 命令

- **SELECT** - 从数据库中提取数据
- **UPDATE** - 更新数据库中的数据
- **DELETE** - 从数据库中删除数据
- **INSERT INTO** - 向数据库中插入新数据
- **CREATE DATABASE** - 创建新数据库
- **ALTER DATABASE** - 修改数据库
- **CREATE TABLE** - 创建新表
- **ALTER TABLE** - 变更（改变）数据库表
- **DROP TABLE** - 删除表
- **CREATE INDEX** - 创建索引（搜索键）
- **DROP INDEX** - 删除索引  

# SQL SELECT 语句

SELECT 语句用于从数据库中选取数据。

结果被存储在一个结果表中，称为结果集。

## SQL SELECT 语法

```sql
SELECT *column_name*,*column_name*
 FROM *table_name*;

与

SELECT * FROM *table_name*;
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
+----+--------------+---------------------------+-------+---------+
```

------

## SELECT Column 实例

下面的 SQL 语句从 "Websites" 表中选取 "name" 和 "country" 列：

### 实例

```sql
SELECT name,country FROM Websites;
```

输出结果为:

![img](https://www.runoob.com/wp-content/uploads/2013/09/98E6B49C-06AF-469B-B907-81C52BBE6BDC.jpg)

------

## SELECT * 实例

下面的 SQL 语句从 "Websites" 表中选取所有列：

### 实例

```sql
SELECT * FROM Websites;
```

输出结果为:

![img](https://www.runoob.com/wp-content/uploads/2013/09/DE979628-6FAF-46BD-920F-18F9565ADD78.jpg)

------

## 结果集中的导航

大多数数据库软件系统都允许使用编程函数在结果集中进行导航，比如：Move-To-First-Record、Get-Record-Content、Move-To-Next-Record 等等。

# SQL SELECT DISTINCT 语句

在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。

DISTINCT 关键词用于返回唯一不同的值。

## SQL SELECT DISTINCT 语法

```sql
SELECT DISTINCT *column_name*,*column_name*
 FROM *table_name*;
```

------

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
+----+--------------+---------------------------+-------+---------+
```

------

## SELECT DISTINCT 实例

下面的 SQL 语句仅从 "Websites" 表的 "country" 列中选取唯一不同的值，也就是去掉 "country" 列重复值：

### 实例

```sql
SELECT DISTINCT country FROM Websites;
```

输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/E3012A35-35DF-4BBB-8657-8A312C5AEAB6.jpg)

​	

# SQL WHERE 子句

WHERE 子句用于提取那些满足指定条件的记录。

## SQL WHERE 语法

```sql
SELECT *column_name*,*column_name*
 FROM *table_name*
 WHERE *column_name operator value*;
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
+----+--------------+---------------------------+-------+---------+
```

------

## WHERE 子句实例

下面的 SQL 语句从 "Websites" 表中选取国家为 "CN" 的所有网站：

### 实例

```sql
SELECT * FROM Websites WHERE country='CN';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/4B7980AC-2566-43F7-843A-256E868B92A4.jpg)

------

## 文本字段 vs. 数值字段

**SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。**

**在上个实例中 'CN' 文本字段使用了单引号。**

**如果是数值字段，请不要使用引号。**

### 实例

```sql
SELECT * FROM Websites WHERE id=1;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/639D2956-99CE-44E9-B960-EA14D296820E.jpg)

------

## WHERE 子句中的运算符

下面的运算符可以在 WHERE 子句中使用：

| 运算符  | 描述                                                       |
| ------- | ---------------------------------------------------------- |
| =       | 等于                                                       |
| <>      | 不等于。**注释：**在 SQL 的一些版本中，该操作符可被写成 != |
| >       | 大于                                                       |
| <       | 小于                                                       |
| >=      | 大于等于                                                   |
| <=      | 小于等于                                                   |
| BETWEEN | 在某个范围内                                               |
| LIKE    | 搜索某种模式（模糊查询）                                   |
| IN      | 指定针对某个列的多个可能值                                 |

# SQL AND & OR 运算符

如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

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
+----+--------------+---------------------------+-------+---------+
```

------

## AND 运算符实例

下面的 SQL 语句从 "Websites" 表中选取国家为 "CN" 且alexa排名大于 "50" 的所有网站：

### 实例

```sql
SELECT * FROM Websites WHERE country='CN' AND alexa > 50;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/and-or1.jpg)

------

## OR 运算符实例

下面的 SQL 语句从 "Websites" 表中选取国家为 "USA" 或者 "CN" 的所有客户：

### 实例

```sql
SELECT * FROM Websites WHERE country='USA' OR country='CN';
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/and-or2.jpg)

------

## 结合 AND & OR

您也可以把 AND 和 OR 结合起来（使用圆括号来组成复杂的表达式）。

下面的 SQL 语句从 "Websites" 表中选取 alexa 排名大于 "15"  且国家为 "CN"  或 "USA" 的所有网站：

### 实例

```sql
SELECT * FROM Websites WHERE alexa > 15 AND (country='CN' OR country='USA');
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/and-or3.jpg)

# SQL ORDER BY 关键字

ORDER BY 关键字用于对结果集按照一个列或者多个列进行排序。

ORDER BY 关键字**默认按照升序对记录进行排序。**如果需要按照降序对记录进行排序，您可以使用 DESC 关键字。

## SQL ORDER BY 语法

```sql
SELECT *column_name*,*column_name*
 FROM *table_name*
 ORDER BY *column_name*,*column_name* ASC|DESC;
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
+----+--------------+---------------------------+-------+---------+
```

------

## ORDER BY 实例

下面的 SQL 语句从 "Websites" 表中选取所有网站，并按照 "alexa" 列排序：

### 实例

```sql
SELECT * FROM Websites ORDER BY alexa;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/orderby1.jpg)



------

## ORDER BY DESC 实例

下面的 SQL 语句从 "Websites" 表中选取所有网站，并按照 "alexa" 列**降序排序**：

### 实例

```sql
SELECT * FROM Websites ORDER BY alexa DESC;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/orderby2.jpg)

------

## ORDER BY 多列

下面的 SQL 语句从 "Websites" 表中选取所有网站，并按照 "country" 和 "alexa" 列排序：

### 实例

```sql
SELECT * FROM Websites ORDER BY country,alexa;
```

执行输出结果：

![img](https://www.runoob.com/wp-content/uploads/2013/09/orderby3.jpg)

ORDER BY 多列的时候，eg:

```sql
order by A,B        这个时候都是默认按升序排列
order by A desc,B   这个时候 A 降序，B 升序排列
order by A ,B desc  这个时候 A 升序，B 降序排列
```

即 **desc** 或者 **asc** 只对它紧跟着的第一个列名有效，其他不受影响，仍然是默认的升序。

# SQL INSERT INTO 语句

INSERT INTO 语句用于向表中插入新记录。

## SQL INSERT INTO 语法

INSERT INTO 语句可以有两种编写形式。

第一种形式无需指定要插入数据的列名，只需提供被插入的值即可。

没有指定要插入数据的列名的形式需要列出插入行的每一列数据:

```sql
INSERT INTO *table_name*
 VALUES (*value1*,*value2*,*value3*,...);
```

第二种形式需要指定列名及被插入的值：

```sql
INSERT INTO *table_name* (*column1*,*column2*,*column3*,...)
 VALUES (*value1*,*value2*,*value3*,...);
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
+----+--------------+---------------------------+-------+---------+
```

------

## INSERT INTO 实例

假设我们要向 "Websites" 表中插入一个新行。

我们可以使用下面的 SQL 语句：

### 实例

```sql
INSERT INTO Websites (name, url, alexa, country) 
VALUES ('百度','https://www.baidu.com/','4','CN');
```

执行以上 SQL，再读取 "Websites" 表，数据如下所示：

![img](https://www.runoob.com/wp-content/uploads/2013/09/insert1.jpg)

| ![lamp](https://www.runoob.com/images/lamp.jpg) | **您是否注意到，我们没有向 id 字段插入任何数字？**        |
| ----------------------------------------------- | --------------------------------------------------------- |
|                                                 | **id 列是自动更新的，表中的每条记录都有一个唯一的数字。** |

------

## 在指定的列插入数据

我们也可以在指定的列插入数据。

下面的 SQL 语句将插入一个新行，但是只在 "name"、"url" 和 "country" 列插入数据（id 字段会自动更新）：

### 实例

```sql
INSERT INTO Websites (name, url, country) 
VALUES ('stackoverflow', 'http://stackoverflow.com/', 'IND');
```

执行以上 SQL，再读取 "Websites" 表，数据如下所示：

![img](https://www.runoob.com/wp-content/uploads/2013/09/insert2.jpg)

## insert into  select 和select into  from 的区别

```sql
insert into TABLE1 select * from TABLE2 (where FIELD1='XXX')   
--插入一行（插入select后的结果）,要求表TABLE1 必须存在。也可指定字段（字段也必须存在）
select *  into TABLE1 from TABLE2  (where FIELD1='XXX')
--也是插入一行,要求表TABLE1 不存在，插入时自动创建TABLE1
```

# SQL UPDATE 语句

UPDATE 语句用于更新表中已存在的记录。

## SQL UPDATE 语法

```sql
UPDATE *table_name*
SET *column1*=*value1*,*column2*=*value2*,...
WHERE *some_column*=*some_value*;
```

| ![lamp](https://www.runoob.com/images/lamp.jpg) | **请注意 SQL UPDATE 语句中的 WHERE 子句！**                  |
| ----------------------------------------------- | ------------------------------------------------------------ |
|                                                 | **WHERE 子句规定哪条记录或者哪些记录需要更新。如果您省略了 WHERE 子句，所有的记录都将被更新！** |

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
+----+--------------+---------------------------+-------+---------+
```

------

## SQL UPDATE 实例

假设我们要把 "菜鸟教程" 的 alexa 排名更新为 5000，country 改为 USA。

我们使用下面的 SQL 语句：

### 实例

```sql
UPDATE Websites  SET alexa='5000', country='USA'  WHERE name='菜鸟教程';
```

执行以上 SQL，再读取 "Websites" 表，数据如下所示：

![img](https://www.runoob.com/wp-content/uploads/2013/09/update1.jpg)

------

## Update 警告！

在更新记录时要格外小心！在上面的实例中，如果我们省略了 WHERE 子句，如下所示：

```sql
UPDATE Websites
SET alexa='5000', country='USA'
```

执行以上代码会将 Websites 表中所有数据的 alexa 改为 5000，country 改为 USA。

**执行没有 WHERE 子句的 UPDATE 要慎重，再慎重。**

## 设置sql_safe_updates

在 MySQL 中可以通过设置 **sql_safe_updates** 这个自带的参数来解决，当该参数开启的情况下，你必须在update 语句后携带 where 条件，否则就会报错。

```sql
set sql_safe_updates=1; --表示开启该参数
```

# SQL DELETE 语句

DELETE 语句用于删除表中的行。

## SQL DELETE 语法

```sql
DELETE FROM *table_name*
 WHERE *some_column*=*some_value*;
```

| ![lamp](https://www.runoob.com/images/lamp.jpg) | **请注意 SQL DELETE 语句中的 WHERE 子句！**  WHERE 子句规定哪条记录或者哪些记录需要删除。如果您省略了 WHERE 子句，所有的记录都将被删除！ |
| ----------------------------------------------- | ------------------------------------------------------------ |
|                                                 |                                                              |

------

## 演示数据库

在本教程中，我们将使用 RUNOOB 样本数据库。

下面是选自 "Websites" 表的数据：

```
+----+--------------+---------------------------+-------+---------+
| id | name         | url                       | alexa | country |
+----+--------------+---------------------------+-------+---------+
| 1  | Google       | https://www.google.cm/    | 1     | USA     |
| 2  | 淘宝       | https://www.taobao.com/   | 13    | CN      |
| 3  | 菜鸟教程 | http://www.runoob.com/    | 4689  | CN      |
| 4  | 微博       | http://weibo.com/         | 20    | CN      |
| 5  | Facebook     | https://www.facebook.com/ | 3     | USA     |
+----+--------------+---------------------------+-------+---------+
```

------

## SQL DELETE 实例

假设我们要从 "Websites" 表中删除网站名为 "Facebook" 且国家为 USA 的网站。

我们使用下面的 SQL 语句：

### 实例

```sql
DELETE FROM Websites WHERE name='Facebook' AND country='USA';
```

执行以上 SQL，再读取 "Websites" 表，数据如下所示：

![img](https://www.runoob.com/wp-content/uploads/2013/09/BD5EFB9A-2A65-4AF8-81F3-022E051811DC.jpg)

------

## 删除所有数据

您可以在**不删除表**的情况下，删除表中所有的行。**这意味着表结构、属性、索引将保持不变（变成空表）**：

```sql
DELETE FROM *table_name*;

 或

 DELETE * FROM *table_name*;
```

**注意：**在删除记录时要格外小心！因为您不能重来！

# 参考资料

[菜鸟教程]: https://www.runoob.com/sql/sql-intro.html

