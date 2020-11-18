[TOC]



# Redis数据结构与对象

​	Redis数据库使用键值对的形式存储数据，每个键值对都是由对象组成的，即键和值都是对象。

​	键是由一个字符串对象表示，值则可以分为五种对象：**字符串对象**、**列表对象**、**哈希对象**、**集合对象**、**有序集合对象**，分别对于redis数据的五种基本类型。

## 简单动态字符串

​	Redis没有直接会用C语言传统的字符串表示，而是自己构建了一种名为**简单动态字符串（simple dynamic string，SDS）**的抽象类型。

### SDS的定义

​	在redis源码中，sds.h/sdshdr定义了SDS：

```c
struct sdshdr {
	// 记录buf数组中已使用字节的数量
    // 等于SDS所保存字符串的长度
	int len;
    
    // 记录buf数组中未使用的数量 
	int free;
    
    // 字节数组，用于保存字符串；最后一个字节是空字符'\0'，不计入len
	char buf[];
};
```

### SDS与C语言字符串的区别

1. **获取字符串长度的复杂度**：SDS结构保存了字符串长度，C字符串没有；SDS获取字符串长度直接访问变量`len`（复杂度为常量级），C字符串需要遍历整个字符数组进行计算（复杂度取决于长度，即为n）；
2. **杜绝缓冲区溢出**：SDS结构的`free`变量可以帮助SDS在修改（增）时空间是否足够并自动扩充，而C字符串如果不事先进行空间分配，则可能会在修改（增）时出现溢出；
3. **内存重分配次数**：C字符串的字符数组长度总是等于字符串中字符数量+1，这就意味着在增加或减少字符时，就需要先重新分配空间；而SDS通过**空间预分配**和**惰性空间释放**进行优化；
4. **二进制安全**：C字符串会识别字符数组中的空字符来判断字符串的结束，意味着字符串中间不允许有空字符，也就导致C字符串不能保存二进制数据（图片视频等）；而SDS结构中的`buf`字节数组，不是用来保存字符，而是用来保存一系列二进制数据；
5. **C字符串函数库**：C字符串可以使用所有<string.h>库的函数；SDS虽然也遵循以空字符结尾，但存储的是二进制数据，所以只能使用部分库函数；

> **空间预分配：根据SDS增长后的大小预分配空间。**
>
> 当SDS修改后的`len`小于1MB时，预分配空间为`len`的值，即修改后SDS的`len`=`free`；
>
> 当SDS修改后的`len`大于等于1MB时，预分配空间固定位1MB，即`free`=1MB；
>
> **惰性空间释放：SDS缩短操作后，会将缩短的长度加到`free`变量中，不立即进行内存重分配回收多出来的字节。**

## 链表

​	链表就是数据结构的链表，具有顺序性，增删节点速度快，可以灵活调整链表长度。**链表是列表键的底层实现之一。**因链表是常用且基础的数据结构，在这不作过多阐述。

​	因为C语言没有内置这种数据结构，所以Redis构建了自己的链表实现，其中链表结构定义如下所示：

```c
// adlist.h/listNode
typedef struct listNode {
    struct listNode *prev;
    struct listNode *next;
    void *value;
}listNode;
```

​	由上面定义可知，该链表是双端链表。Redis为了操作方便，也定义了一个`list`表示整个链表：

```c
// adlist.h/list
typedef struct list {
    listNode *head;
    listNode *tail;
    // 链表所包含的节点数量
    unsigned long len;
    // 节点值复制函数
    void *(*dup)(void *ptr);
    // 节点值释放函数
    void (*free)(void *ptr);
    // 节点值对比函数
    int (*match)(void *ptr,void *key);
}list;
```

​	能看到`listNode`使用**void***指针来保存节点值，并且可以通过`list`结构的**dup、free、match**三个属性为节点值设置类型特定函数，所以链表可以用于保存各种不同类型的值。

## 字典

​	字典又称为符号表（symbol table）、关联数组（associative array）或映射（map），是一种用于保存键值对的抽象数据结构。

​	Redis数据库中键值对的存储方式就是基于字典实现的，并且哈希键的底层实现之一也是字典。

### 实现

​	Redis字典的实现设计三个角色：**哈希表节点（dictEntry）**、**哈希表（dictht）**、**字典（dict）**和**操函数（dictType）**。

​	Redis字典使用哈希表作为底层实现，一个哈希表（数组）里有多个哈希表节点，每个哈希表节点就保存了字典中的一个键值对。

#### 哈希表节点

```c
typedef struct dictEntry {
    void *key;   
    union {
        void *val;
        unit64_t u64;
        int64_t s64;
    }v;
    struct dictEntry *next;
} dictEntry;
```

- **key** 属性是键值对中的键；
- **v** 属性是键值对中的值，值可以是一个**指针**，或是一个**uint64_t**整数，或是一个**int64_t**整数；
- **next** 属性是指向下一个节点的指针，意味着使用**链地址法**解决哈希冲突，并且插入是使用**头插法**；

#### 哈希表

```c
typedef struct dictht {    
    dictEntry **table;   
    unsigned long size;   
    unsigned long sizemask;
    unsigned long used;
} dictht;
```

- **table** 属性是一个数组，数组的每个元素都是一个指向`dictEntry`结构的指针；
- **size **属性用于记录**table**数组大小；
- **sizemask** 属性是哈希表大小掩码，用于计算索引值，总是等于**size-1**；
- **used **属性记录当前哈希表已有节点的数量；

#### 字典

```c
typedef struct dict {
    dictType *type;
    void *privdata;
    dictht ht[2];
    long rehashidx; /* rehashing not in progress if rehashidx == -1 */
    unsigned long iterators; /* number of iterators currently running */
} dict;
```

- **type** 属性是一个指向`dictType`结构的指针，每个`dictType`结构保存了一簇用于操作特定类型键值对的函数，Redis会作为用途不同的字典设置不同的类型特定函数；
- **privdata** 属性保存了需要给那些类型特性函数的可选参数；
- **ht** 属性是一个包含两个哈希表的数组，**ht[1]**只会在对**ht[0]**进行**rehash**时使用；
- **rehashidx** 属性是 **rehash** 时的索引，值为 **-1**时，表示目前没有在进行**rehash**；
- **iterators** 属性（新增属性）表示当前正在运行的迭代器数量；

#### 操作函数

```c
typedef struct dictType {
    uint64_t (*hashFunction)(const void *key);
    void *(*keyDup)(void *privdata, const void *key);
    void *(*valDup)(void *privdata, const void *obj);
    int (*keyCompare)(void *privdata, const void *key1, const void *key2);
    void (*keyDestructor)(void *privdata, void *key);
    void (*valDestructor)(void *privdata, void *obj);
} dictType;
```

- **(*hashFunction)** 函数用于计算哈希值；
- **\*(*keyDup)** 用于复制键的函数；
- **\*(*valDup)** 用于复制值的函数；
- **(*keyCompare)** 用于比较键的函数；
- **(*keyDestructor)** 用于销毁键的函数；
- **(*valDestructor)** 用于销毁值的函数；

### 哈希算法

​	当要将一个新的键值对添加到字典里面时，程序需要先根据键值对的键计算出哈希值和索引值，然后再根据索引值，将包含新键值对的哈希表节点放到哈希表数组的指定索引上面。

​	阅读源码后，发现Redis将键值对中的键和值是分两步复制给哈希表节点。

​	**添加元素**源码如下：

```c
// 以下代码在dict.c文件下
/* Add an element to the target hash table */
int dictAdd(dict *d, void *key, void *val)
{
    dictEntry *entry = dictAddRaw(d,key,NULL);

    if (!entry) return DICT_ERR;
    dictSetVal(d, entry, val);
    return DICT_OK;
}

/* Low level add or find:
 * This function adds the entry but instead of setting a value returns the
 * dictEntry structure to the user, that will make sure to fill the value
 * field as he wishes.
 *
 * This function is also directly exposed to the user API to be called
 * mainly in order to store non-pointers inside the hash value, example:
 *
 * entry = dictAddRaw(dict,mykey,NULL);
 * if (entry != NULL) dictSetSignedIntegerVal(entry,1000);
 *
 * Return values:
 *
 * If key already exists NULL is returned, and "*existing" is populated
 * with the existing entry if existing is not NULL.
 *
 * If key was added, the hash entry is returned to be manipulated by the caller.
 */
dictEntry *dictAddRaw(dict *d, void *key, dictEntry **existing)
{
    long index;
    dictEntry *entry;
    dictht *ht;

    if (dictIsRehashing(d)) _dictRehashStep(d);

    /* Get the index of the new element, or -1 if
     * the element already exists. */
    if ((index = _dictKeyIndex(d, key, dictHashKey(d,key), existing)) == -1)
        return NULL;

    /* Allocate the memory and store the new entry.
     * Insert the element in top, with the assumption that in a database
     * system it is more likely that recently added entries are accessed
     * more frequently. */
    ht = dictIsRehashing(d) ? &d->ht[1] : &d->ht[0];
    entry = zmalloc(sizeof(*entry));
    entry->next = ht->table[index];
    ht->table[index] = entry;
    ht->used++;

    /* Set the hash entry fields. */
    dictSetKey(d, entry, key);
    return entry;
}

// 以下代码在dict.h文件下
#define dictSetVal(d, entry, _val_) do { \
    if ((d)->type->valDup) \
        (entry)->v.val = (d)->type->valDup((d)->privdata, _val_); \
    else \
        (entry)->v.val = (_val_); \
} while(0)

```

​	其它算法可自行查看源码https://github.com/redis/redis

### rehash

​	随着操作的不断执行，哈希表保存的键值对会逐渐地增多或减少，为了让哈希表的负载因子维持在一个合理的范围之内，当哈希表保存的键值对数量过多或过少时，程序需要对哈希表的大小进行相应的扩展或收缩。

​	扩展或收缩哈希表的工作可以通过**rehash**操作完成，执行步骤如下：

1. 为字典的**ht[1]**哈希表分配空间，空间大小取决于扩展还是收缩，以及**ht[0].used**的值：
    - 如果是**扩展**操作：那么**ht[1]**的大小为第一个大于等于**ht[0].used*2** 的 **2的n次方幂（2^n）**；
    - 如果是**收缩**操作：那么**ht[1]**的大小为第一个大于等于**ht[0].used** 的 2^n；
2. 将保存在**ht[0]**中的所有键值对根据**ht[1].sizemask**重新计算索引值，然后放置到**ht[1]**的指定索引位置上；
3. 所有键值对迁移完成后，释放**ht[0]**，将**ht[1]**设置为**ht[0]**，并在重新在**ht[1]**上创建一个空的哈希表，为下一次**rehash**做准备；

​	以上步骤的源码参考**dict.c/dictResize**函数。

> **关于扩展与收缩的提示：**
>
> ​	当以下条件中的任意一个被满足时，程序会自动开始对哈希表执行**扩展**操作：
>
> 1. 服务器目前没有在执行`BGSAVE`命令或者`BGREWRITEAOF`命令，并且哈希表的负载因子大于等于1。
> 2. 服务器目前正在执行`BGSAVE`命令或者`BGREWRITEAOF`命令，并且哈希表的负载因子大于等于5。
>
> ​	其中哈希表的负载因子公式如下：
>
> ​		**load_factor = ht[0].used/ht[0].size**
>
> ​	根据`BGSAVE`命令或者`BGREWRITEAOF`命令是否正在执行，服务器执行扩展操作所需的负载因子并不相同，这是因为在执行`BGSAVE`命令或者`BGREWRITEAOF`命令的过程中，Redis需要创建当前服务器进程的子进程，而大多数操作系统都采用**写时复制（copy-on-write）技术**来优化子进程的使用效率，所以在子进程存在期间，服务器会提高执行扩展操作所需的负载因子，从而尽可能地避免在子进程存在期间进行哈希表扩展操作，这可以避免不必要的内存写入操作，最大限度地节约内存。
>
> ​	当哈希表的负载因子小于0.1时，程序自动开始执行**收缩**操作。

### 渐进式rehash

​	对于扩展或收缩操作需要将哈希表**ht[0]**所有键值对**rehash**到**ht[1]**的过程，并不是一次性、集中式完成的，而是分多次、渐进式地完成的。

​	Redis采用**渐进式rehash**的原因是：如果服务器中哈希表的键值对数量百万量级、千万量级或更高时，一次性**rehash**全部键值对将可能导致服务器在一段时间内停止服务。为了避免该情况发生，就采用**渐进式rehash**。

​	在源码**dict.c**中，有如下函数：

```c
static void _dictRehashStep(dict *d) {
    if (d->iterators == 0) dictRehash(d,1); 
}
// 该函数会在对字典进行增删查改时调用
// 并且在rehash期间，对字典的增加操作是会添加到ht[1]中，查找则会先查找ht[0]，再找ht[1]
// 在rehash时的查找时间会变长。
```

​	**渐进式rehash**的详细步骤如下：

1. 为 **ht[1]** 分配空间，让字典同时持有 **ht[0]** 和 **ht[1]** 两个哈希表；
2. 在字典中维持一个索引计数器变量 **rehashidx**，并将它的值设置为0，表示 **rehash** 工作正式开始；
3. 在**rehash**进行期间，每次对字典执行添加、删除、查找或者更新操作时，程序出了执行指定的操作之外，还会顺带将**ht[0]**哈希表在**rehashidx**索引上的所有键值对**rehash**到**ht[1]**，当**rehash**工作完成之后，程序将**rehashidx**的值加一；
4. 随着字典操作的不断执行，最后**ht[0]**的所有键值对都会被**rehash**到**ht[1]**，这时程序会执行最后的善后工作；



​	**个人总结**：**渐进式rehash**采用分而治之的思想，将一项有着庞大的计算量操作分成多步进行。从另一个角度上看，虽然避免了服务器可能会在**rehash**长时间停止服务的可能，但也增加了**rehash**所需要的时间，是不是甚至可能一直都不会**rehash**（没有对字典增删改查的操作）？

## 跳跃表



# 参考资料

《Redis设计与实现》——黄健宏著（该书基于redis3.0之前编写）