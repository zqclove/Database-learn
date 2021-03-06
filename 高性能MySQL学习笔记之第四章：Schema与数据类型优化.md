

# 第四章：Schema与数据类型优化

​	本章节学习内容：MySQL数据库的设计，主要介绍的是MySQL数据库设计与其他关系型数据库管理系统的区别。

​	本章内容是为第五章和第六章做铺垫。这三章讨论逻辑设计、物理设计和查询执行，以及它们之间的相互作用。

## 选择优化的数据类型

​	选择正确的数据类型对于获得高性能至关重要。不管存储哪种类型的数据，下面几个简单的原则都有助于做出更好的选择：

- **更小的通常更好**：

    ​	一般情况下，**应该尽量使用可以正确存储数据的最小数据类型**。更小的数据类型占用**更少的磁盘、内存和CPU缓存**，并且处理时需要的**CPU周期也更少**，因此它们会更快。

- **简单就好**：

    ​	简单数据类型的操作通常需要**更少的CPU周期**。例如，整型比字符操作代价更低，因为字符集和校对规则（排序规则）使字符的比较 比 整型比较更复杂。因为字符操作的代价高昂，所以应该使用MySQL内建类型存储日期和时间，使用整型存储IP地址。

- **尽量避免NULL**：

    ​	如果查询中包含可为 NULL 的列，对 MySQL 来说更难优化，因为**可为 NULL 的列使得索引、索引统计和值比较都更复杂**，并且会**使用更多的存储空间**，在MySQL里也需要特殊处理。

    ​	当可为 NULL 的列被索引时，每个索引记录需要一个额外的字节，在 MyISAM 里甚至还可能导致固定大小的索引（例如只有一个整数列的索引）变成可变大小的索引。



​	在为列选择合适的数据类型时，第一步需要**确定合适的大类型**：数组、字符串、时间等。第二步是**选择具体类型**，MySQL的数据类型可以存储相同类型的数据，只是存储的长度和范围不一样、允许的精度不同，或者需要的物理空间不同，也会有一些不同的行为和属性。

​	**个人总结**：在设计字段时，应该以“更轻、更快、更简单”为目标确定数据类型。从资源使用角度上看，占用越少资源的数据类型越好；从操作代价角度上看，代价越少的数据类型越好。总的来说，一切都是为了更简单、更快捷。

### 整数类型

​	整数类型根据存储范围和大小，又分为五种：

| Type        | Storage (Bytes) | Minimum Value Signed | Minimum Value Unsigned | Maximum Value Signed | Maximum Value Unsigned |
| ----------- | --------------- | -------------------- | ---------------------- | -------------------- | ---------------------- |
| `TINYINT`   | 1               | `-128`               | `0`                    | `127`                | `255`                  |
| `SMALLINT`  | 2               | `-32768`             | `0`                    | `32767`              | `65535`                |
| `MEDIUMINT` | 3               | `-8388608`           | `0`                    | `8388607`            | `16777215`             |
| `INT`       | 4               | `-2147483648`        | `0`                    | `2147483647`         | `4294967295`           |
| `BIGINT`    | 8               | `-2^63`              | `0`                    | `2^63-1`             | `2^64-1`               |

​	整型类型有可选的 `unsigned`属性，表示不允许负值，使用该属性的范围也会发生变化，大概可以使整数的上上限提高一倍。

​	有符号和无符号类型使用相同的存储空间，具有相同的性能，因此可以根据实际情况选择合适的类型。

​	**整数计算**一般使用 64位 的 `BIGINT`整数，即使在 32位 环境也是如此。（一些聚合函数是例外，它们使用 `DECIMAL` 或 `DOUBLE` 进行计算）。

​	MySQL可以为整数类型指定宽度，但它对存储空间无关，只是规定了MySQL的一些交互工具用来显示字符的个数。

### 实数类型

​	实数是**带有小数部分**的数字。但它们不只是为了存储小数部分，也可以使用`DECIMAL`存储比`BIGINT`还大的整数。在MySQL即支持精确类型，也支持不精确类型。

​	`DECIMAL`和`NUMERIC`类型存储实数值，在MySQL中，`NUMERIC`被实现为`DECIMAL`，所以后面的`DECIMAL`介绍同样适用于`NUMERIC`。（`NUMERIC`相当于`DECIMAL`的别名，同 `INT`与`INTEGER`）

​	`DECIMAL`类型用于**存储精确的小数**。在MySQL中，`DECIMAL`的值**存储在二进制字符串**（4个字节存9个数字，小数点占1个字节）中。

​	`DECIMAL`声明例子如下：

```sql
DECIMAL(5,2)
```

​	其中5表示精度，2表示小数点后的位数，所以该声明的值的存储范围是 -999.99 到 999.99 。

​	如果小数位为0，则十进制值不包含小数点或小数部分。

​	`DECAMAL`类型最多存储65位数字，但是给定的`DECIMAL`列的实际范围可以受到给定列的精度或小数位的限制。

​	因为CPU不支持对`DECIMAL`的直接计算，所以**MySQL服务器自身实现了`DECIMAL`的高精度计算**。相对而言，CPU直接支持原生浮点计算，所以浮点运算明显更快（`FLOAT`和`DOUBLE`类型支持使用标准的浮点运算进行近似计算）。

​	因为需要额外的空间和计算开销，所以**应该尽量只在对小数进行精确计算时才使用`DECIMAL`**，例如财务数据相关。但在数据量比较大的时候，可以考虑使用`BIGINT`代替`DECIMAL`，将需要存储的货币单位根据小数点的位数乘以相应的倍数即可。例如要存储财务数据精确到0.00001，则可以把所有金额乘以10^5，然后将结果存储在`BIGINT`里，这样可以同时避免浮点存储计算不精确和`DECIMAL`精确计算代价高的问题。

### 字符串类型

​	MySQL支持多种字符串类型，每种类型还有很多变种。从MySQL 4.1开始，每个字符串可以定义自己的字符集和排序规则（校对规则）。

#### CHAR和VARCHAR

​	**CHAR**和**VARCHAR**是两种最主要的字符串类型。它们的值是如何存储在磁盘和内存中，是与存储引擎的具体实现有关。下面的描述都是建立在 InnoDB 或 MyISAM 两种存储引擎上的。

​	**注意**：存储引擎存储 **CHAR** 或者 **VARCHAR** 值的方式在内存中和在磁盘上可能不一样，所以 MySQL 服务器从存储引擎读出的值可能需要转换为另一种存储方式。

##### VARCHAR

​	**VARCHAR类型用于存储可变长字符串**。它比定长类型**更节约空间**，因为它仅适用必要的空间。但是如果MySQL表使用`ROW_FORMAT=FIXED`（填充的意思）创建的话，每一行都会使用定长存储，这就会浪费空间。

​	**VARCHAR需要使用1个或2个额外字节记录字符串的长度**。如果列的最大长度小于或等于255字节，使用1个额外字节记录长度；如果列的最大长度大于255字节，使用2个额外字节记录长度。

​	因为**VARCHAR**定义的行是变长的原因，在`UPDATE`时可能使得行变得比原来更长，这就导致需要做额外的工作。如果一个行占用的空间增长，并且在页内没有更多的空间可以存储，在这种情况下，不同的存储引擎的处理方式是不一样的。例如，MyISAM 会将行拆成不同的片段存储，InnoDB 则需要分裂页来使行可以放进页内。

​	MySQL在存储和检索**VARCHAR类型**时会保留末尾空格。

​	适用**VARCHAR**类型的情况如下：

- 字符串列的最大长度比平均长度大很多（绝大部分的字符串长度都是很短的，少数是很长的，如果用定长类型可能每行都要存储最大长度所需的字节）；
- 列的更新很少，所以碎片不是问题（避免遇到使用**VARCHAR**时的不良特性，只使用变长节约空间的良好特性）；
- 使用了像**UTF-8**这样复杂的字符集，每个字符都使用不同的字节数进行存储；

> **Tips**：
>
> 使用`VARCHAR(5)`和`VARCHAR(200)`存储'hello'的空间开销是一样的。那么使用更短的列有什么优势呢？
>
> 事实证明有很大的优势。更长的列会消耗更多的内存，因为MySQL通常会分配固定大小的内存块来保存内部直。尤其是使用内存临时表排序或操作时会特别糟糕。在利用磁盘临时表进行排序时也同样糟糕。
>
> 所以最好的策略是只分配真正需要的空间。

##### CHAR

​	**CHAR类型是定长类型**，MySQL总是根据定义的字符串长度分配足够的空间，并且允许的长度为0~255。存储引擎存储**CHAR**类型时，会使用空格进行填空；在MySQL检索时，会删除所有的末尾空格，除非`PAD_CHAR_TO_FULL_LENGTH`SQL模式开启。

​	值得注意的是，填充和截取空格的行为在不同存储引擎都是一样的，因为这是在MySQL服务层进行处理的。也就是说**VARCHAR**即不填充也不截取空格的行为在任何存储引擎都一样，**CHAR**即填充也截取空格同理。

​	适用**CHAR**类型的情况如下：

- 存储很短的字符串，或者所有值都接近同一个长度。例如存储密码的MD5值；
- 存储经常变更的数据，相较于边长类型，定长的**CHAR**类型不容易产生碎片；

##### 两者存储比较

| Value        | `CHAR(4)` | Storage Required | `VARCHAR(4)` | Storage Required |
| ------------ | --------- | ---------------- | ------------ | ---------------- |
| `''`         | `'  '`    | 4 bytes          | `''`         | 1 byte           |
| `'ab'`       | `'ab '`   | 4 bytes          | `'ab'`       | 3 bytes          |
| `'abcd'`     | `'abcd'`  | 4 bytes          | `'abcd'`     | 5 bytes          |
| `'abcdefgh'` | `'abcd'`  | 4 bytes          | `'abcd'`     | 5 bytes          |

##### 总结

​	这两种类型的具体存储方式还是取决于存储引擎，只不过填充和截取空格是在服务器层处理的。如果要研究这两种类型具体如何存储，需要翻看相应的存储引擎文档。

#### BINARY和VARBINARY————待



#### BLOB和TEXT

​	BLOB和TEXT都是为存储很大的数据而设计的字符串数据类型，分别采用**二进制**和**字符**方式存储。

​	实际上，它们分别属于两组不同的数据类型家族：字符类型是**TINYTEXT、TEXT、MEDIUMTEXT和LONGTEXT**。二进制类型是**TINYBLOB、BLOB、MEDIUMBLOB和LONGBLOB**。其中**BLOB**是**SMALLBLOB**的同义词，**TEXT**是**SMALLTEXT**的同义词。表示存储不同的最大长度。

​	**BLOB类型**存储的值是**二进制字符串**，**TEXT类型**存储的值是**字符串**，它们都有着各自的排序规则和字符集，**BLOB**类型有二进制字符集和排序规则，**TEXT**类型有字符集和排序规则。

​	**BLOB**类型和**TEXT**类型其实就是**VARBINARY**类型和**VARCHAR**类型的加长版，但也有些不同：	

- 在使用**BLOB**类型和**TEXT**类型的列作为索引列时，必须指定索引列的前缀长度。因为这两种类型长度很长，需要指定前缀长度，使用前缀长度作为索引值；
- **BLOB**类型和**TEXT**类型不能有默认值；
- **BLOB**类型和**TEXT**类型的排序与`max_sort_length`参数有关（默认值是1024），在对这两种类型进行排序时，会使用该参数指定长度的前缀值进行排序；

> **Tip**：
>
> 因为Memory引擎不支持BLOB和TEXT类型，所以如果查询使用了BLOB或TEXT列并且需要使用隐式临时表，将不得不使用MyISAM磁盘临时表，即使只有几行数据也是如此。
>
> 索引应当尽量避免使用BLOB和TEXT类型。如果无法避免，有个技巧是在所有用到BLOB字段的地方都是用SUBSTRING(column，length)将列值转换为字符串（ORDER BY 子句 中也适用），这样就可以使用内存临时表了。但是也要确保截取的子字符串足够短，不会使临时表的大小超过`max_heap_table_size`或`tmp_table_size`，超过以后MySQL会将内存临时表转换成MyISAM磁盘临时表。

#### ENUM枚举类型

​	*统一表明字符串列表在 enum和 set章节表示定义时声明的字符串。*

​	可以使用枚举类型列代替常用的字符串类型（例如存储性别等数据）。枚举类型在存储时非常紧凑，会根据列表值的数量压缩到一个或者两个字节中。枚举列可以把一些**不重复的字符串**存储成一个预定义的集合，且在定义枚举列时，**会将字符串自动编码为数字**，数字从1到n代表定义时字符串的顺序（即数字与字符串有映射关系）。这些数字在查询和输出时被转换回查询结果中相应的字符串。

​	因为枚举列会自动编码成数字，所以在定义时尽量避免定义数字，避免出现二义性问题。

​	值得注意的是，**枚举字段是按照内部存储的整数（即定义时字符串的顺序）进行排序**的。但也可以在查询时使用 **FIELD()** 函数显示地指定排序顺序，但这会导致MySQL无法利用索引消除排序。

​	枚举列的字符串列表是固定的，添加或删除列表中的字符串必须使用 **ALTER TABLE** 。因此，对于可能修改的字符串，使用枚举类型代替并不是好主意。（除非能接受只在列表末尾添加元素，这样在MySQL 5.1中就可以不重建整个表来完成修改）

​	在定义枚举列时，字符串不能使变量和表达式。

​	在定义枚举列时，如果不指定非空限制，字符串列表会有null值。

#### SET类型

​	SET类型与枚举类型相似，在定义时会将字符串列表编码成数字，但SET类型是转换成二进制的方式，第一个字符串用0001存储，第二个字符串用0010存储，第三个字符串用0100存储，以此类推。

​	在SET字段中，值可以是字符串列表中多个字符串的组合（不过需要用逗号分隔），也可以是空字符串，如果定义时允许空值也可以是null值。

​	创建一个表用于测试set类型：

```sql
create table set_test (
	col1 set('a','b','c')
);
```

​	如下图所示为set字段值的部分可能（省略插入语句）：

![set类型测试](C:\Users\Administrator\Desktop\学习\数据库\Database-learn\img\set类型测试.PNG)

​	由上图可以看出，set类型的存储类型是以二进制位存储的，当多个字符串组合时，其值是相加的结果。

​	set类型列也是按照内部存储的值（二进制）进行排序。

​	set类型在改变定义时也是需要 **ALTER TABLE**。

​	因为set类型的存储特性，一般可以用作存储权限或IO多路复用中的事件集等设计二进制位的数据。

### 日期和时间类型



# 参考资料

《高性能MySQL》（第三版）——Baron Scbwartz等著