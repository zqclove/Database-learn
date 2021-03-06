# 全局锁和表级锁

​	根据加锁的范围，MySQL里的锁大致可以分为**全局锁**、**表锁**和**行锁**。本章只介绍全局锁和表锁，不涉及具体实现细节。

## 全局锁

​	**全局锁是对整个数据库实例加锁**。MySQL提供了一个全局读锁的方法，命令是`Flush tables with read lock`（FTWRL），使用该命令后，整个数据库实例将处于只读状态，对数据库状态有更新的语句（例如修改行数据、创建新表或修改表、更新类事务的提交语句等）将被阻塞。

（**自我问题**：整个数据库实例加锁是指整个MySQL所有数据，还是指MySQL中其中一个数据库的全部数据？

​	答：经过使用两个客户端对同一数据库和不同数据库在**FTWRL**状态下进行修改时，会阻塞修改的事务。意味着整个数据库实例加锁是指整个MySQL所有数据。）

​	全局锁的典型使用场景是，做全库逻辑备份。也就是把整库每个表都 select 出来存成文本。

​	让整个数据库处于只读状态也存在问题：

- 如果在主库上备份，那么在备份期间都不能执行更新，意味着业务基本停摆。
- 如果在从库上备份，那么备份期间从库就无法执行主库同步过来的`binlog`，会导致主从延迟。

​	虽然全局锁会存在许多问题，但如果不使用全局锁进行备份，就有可能导致备份数据不是处于同一逻辑时间点的数据，存在不一致性的问题。当然，数据备份可以使用**事务隔离级别中的RR级别**生成一致性视图，这样也就不会造成不一致性的问题。但对于一些不支持事务的引擎来说，备份时还是需要全局锁的。

​	官方自带的备份工具是**mysqldump**。当**mysqldump**使用参数`-single-transaction`的时候，导数据之前会开启一个事务，来确保拿到一致性视图。而由于**MVCC**的支持，这个过程数据是可以正常更新的。**该参数只适用于所有表都使用支持事务的引擎的库**。

​	同样可以使全库处于只读状态的语句有**set global readonly=true**。但还是推荐使用**FTWRL**的原因有：

1. **影响面的问题**：在有些系统中，readonly 的值会被用来做其他逻辑，比如用来判断一个库是主库还是备库。因此，修改 `globa`l 变量的方式**影响面更大**，不建议使用。
2. **异常情况的问题**：在异常处理机制上有差异。如果执行 **FTWRL**  命令之后由于客户端发生异常断开，那么 MySQL 会自动释放这个全局锁，整个库回到可以正常更新的状态。而将整个库设置为 **readonly**  之后，如果客户端发生异常，则数据库就会一直保持 **readonly** 状态，这样会导致整个库长时间处于不可写状态，风险较高。

## 表级别锁

​	MySQL中的表级别的锁分为两种：**表锁** 和 **元数据锁（meta data lock，MDL）**。两种锁的目的不同，但锁级别都是表级。

### 表锁

​	表锁的语法是**lock tables ... read / write**。与**FTWRL**一样，释放锁可以通过**unlock tables**主动释放锁，也可以在客户端断开的时候自动释放锁。由表锁的语法可以看出，**表锁的主要目的是为了同步表数据的读写操作**。

​	**lock tables** 语法除了会限制别的线程的读写外，也限定了本线程接下来的操作对象。线程A持有**表锁中的读锁**，其他线程对于该表的写操作会被阻塞，但线程A也只能执行基于该表的**读操作**；持有**表锁中的写锁**，其他线程对于该表的读写操作都会被阻塞，但线程A只能执行基于该表**读写操作**。

​	**个人问题**：写锁和读锁是互斥关系，为什么持有表级别的写锁可以执行读写操作？

​	**答**：基于MySQL官方文档的回答，持有表级别的写锁就是可以读写操作。。。

### 元数据锁

​	**MDL**不需要显示使用，在访问一个表的时候会被自动加上。**MDL的目的是保证读写的正确性**，防止在访问表的过程中，其他线程对该表的结果进行修改，而导致最后结果的不正确。

​	在MySQL5.5版本中引入**MDL**，当对一个表**进行增删改查操作**的时候，加**MDL读锁**；当对一个表做**结构修改操作**的时候，加**MDL写锁**。

- **读锁和读锁之间不互斥**，因此你可以有多个线程同时对一张表增删改查。
- **读锁和写锁之间、写锁和写锁之间是互斥的**，用来保证变更表结构操作的安全性。因此，如果有两个线程要同时给一个表加字段，其中一个要等另一个执行完才能开始执行。

​	关于**MDL**应当注意的“坑”（这里的实验环境是 MySQL 5.6）：

![MDL的坑](C:\Users\Administrator\Desktop\学习\数据库\Database-learn\img\MDL的坑.jpg)

​	我们可以看到 session A  先启动，这时候会对表 t 加一个 MDL 读锁。由于 session B 需要的也是 MDL 读锁，因此可以正常执行。

​	之后 session C  会被 blocked，是因为 session A 的 MDL 读锁还没有释放，而 session C 需要 MDL  写锁，因此只能被阻塞。

​	如果只有 session C 自己被阻塞还没什么关系，但是之后所有要在表 t 上新申请 MDL 读锁的请求也会被  session C 阻塞。前面我们说了，所有对表的增删改查操作都需要先申请 MDL  读锁，就都被锁住，等于这个表现在完全不可读写了。

​	如果某个表上的查询语句频繁，而且客户端有**重试机制**，也就是说超时后会再起一个新 session  再请求的话，这个库的线程很快就会爆满。

​	**事务中的 MDL 锁，在语句执行开始时申请，但是语句结束后并不会马上释放，而会等到整个事务提交后再释放**。

​	**个人问题**：为什么需要读锁的sessionD会被MDL读锁阻塞？

​	**答**：参考博客https://blog.csdn.net/q2878948/article/details/96430129，**申请MDL锁的操作会形成一个队列**（不是阻塞的session会形成队列），**并且队列中写锁获取优先级高于读锁**。一旦出现等待写锁的情况，不但当前操作会被阻塞，同时还会阻塞后续该表的所有操作。

> ​	**TIP：**
>
> ​	如果事务中包含DDL操作，mysql会在DDL操作语句执行前，隐式提交commit，以保证该DDL语句操作作为一个单独的事务存在，同时也保证MDL排他锁的释放。



# 参考文献

MySQL实战45讲 — 丁奇（著）。