# Redis持久化

​	Redis提供了两种不同范围的持久化选项：

- RDB（Redis DataBase）；
- AOF（Append Only File）；

# RDB持久化

​	Redis是一个键值对数据库服务器，服务器中通常包含着任意个非空数据库，而每个非空数据库中又可以包含任意个键值对，为方便期间，我们将服务器中的全部非空数据库及其它们的键值对统称为**数据库状态**。

​	Redis提供的RDB持久化功能，会将Redis在内存中的数据库状态保存到磁盘中，避免数据意外丢失。RDB持久化既可以**手动执行**，也可以根据服务器配置文件中的配置选项**定期执行**。RDB基本原理是通过生成RDB文件保存数据库状态，并将其保存到磁盘中。RDB文件是一个**经过压缩的二进制文件**，当Redis服务器因某些原因需要还原数据库状态时，就可通过这个文件进行还原。

​	可以发现，RDB持久化就是以一种快照的方式保存数据，并不能完美地实时持久化Redis在内存中的全部数据，即可能会丢失掉两次RDB持久化之间的数据。

## RDB文件的创建与载入

​	有两个Redis命令可以生成RDB文件：`SAVE` 和 `BGSAVE`。

- `SAVE`：使用`SAVE`创建RDB文件时，Redis服务器进程会**阻塞**直到RDB文件创建完毕，在这期间服务器进程不能处理任何命令请求；
- `BGSAVE`：使用`BGSAVE`创建RDB文件时，Redis服务器进程会派生出一个子进程，然后由该子进程负责创建RDB文件，服务器进程（父进程）继续处理命令请求（该子进程会阻塞）；



​	生成RDB文件由源码中 **rdb.c/rdbSave**函数完成，在该函数中，会调用同文件下的 **startSaving**函数，这个函数会进行持久化方式的判断，根据判断结果执行持久化。

​	与RDB文件的创建不同，**RDB的载入工作是服务器启动时自动执行的**，所以Redis并没有专门用于载入RDB文件的命令，只要Redis服务器在启动时检测到RDB文件存在，它就会自动载入RDB文件。而在服务器启动时，不是只会载入RBD文件，而是**优先载入AOF文件**。如果服务器未开启AOF持久化功能，才会使用RDB文件还原数据库状态。而且在载入RDB文件期间，服务器会一直出于阻塞状态。

​	`SAVE`与`BGSAVE`主要区别在于使用哪个进程执行持久化工作。当使用`BGSAVE`命令时，在其执行期间服务器不会接受 `SAVE`、另一个`BGSAVE`和 `BGREWRITEAOF`[^注2.1.1]的命令请求。

[^注2.1.1]:`BGREWEITEAOF`命令与`BGSAVE`命令出于性能考虑是不能同时执行的，只有当另一个执行完成后，才会开始执行。这两个命令都是由子进程执行，都会执行大量的磁盘写入操作。

## 自动间隔性保存

​	自动间隔性保存是指Redis服务器会规定在一定时间内操作数超过某个值时，就会执行一次 `BGSAVE`[^注2.2.1]。Redis允许用户在配置文件**redis.conf** 中的 **SNAPSHOTTING** 下自定义 `save` 选项，形式如下：

```shell
save <seconds> <changes>
```

​	表示如果在 \<seconds> 秒内发生过 \<changes> 次修改（增删改等），则会执行一次 `BGSAVE` 命令。

​	该配置信息会在 `server.h/redisServer` 结构中记录，如下所示：

```c
struct redisServer {
    ...
	struct saveparam *saveparams;   /* Save points array for RDB */
    ...
};
```

​	可以看到该属性是一个数组，数组的每一个元素都是一个 `server.h/saveparams`结构，其结构如下所示：

```c
struct saveparam {
    time_t seconds; // <seconds>
    int changes;    // <changes>
};	
```

​	Redis为了保证准确执行自动间隔性保存，还维持了一个 **dirty**计数器和一个 **lastsave**时间属性。**dirty**属性主要是记录上次成功`BGSAVE`或`SAVE`之后的数据库状态修改次数。**lastsave**属性是一个unix时间戳，记录上次成功`BGSAVE`或`SAVE`。它们的定义如下：

```c
struct redisServer {
	...
	long long dirty;  /* Changes to DB from the last save */
	...
	time_t lastsave;  /* Unix time of last successful save */
	...
};
```

​	当服务器成功执行一个数据库修改命令之后，程序就会对 dirty计数器进行更新，命令修改了多少次数据库，dirty计数器的值就增加多少。例如，向一个集合键添加三个元素，则dirty计数器的值加3。

------

​	对于自动间隔性保存，以上的属性定义保证了两次保存的间隔性，那就剩下“自动”了。Redis服务器是通过周期性操作函数 `server.c/serverCron()`保证“自动”。该方法默认每隔100ms[^注2.3.1]就会执行一次，该函数主要是用于对正在运行的服务器进行维护，检测自动间隔性保存的条件只是其中的一项工作。





[^注2.2.1]:为什么不是`SAVE`呢？因为它不配！其实还是主进程阻塞问题，`SAVE`对主进程影响太大了。

[^注2.3.1]:该时间在`redisServer`结构中的 hz属性记录，该记录通过配置文件中的 `hz`选项配置，默认值是10赫兹，意思是1秒10次。

## RDB文件结构



# AOF持久化



# 参考资料

《Redis设计与实现》——黄健宏著

