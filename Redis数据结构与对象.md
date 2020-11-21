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

注意：在Redis6.0中，**sdshdr结构**又分为**sdshdr5**、**sdshdr8**、**sdshdr16**、**sdshdr32**、**sdshdr64**，其中**5**是从不使用，只做直接访问标记字节。

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

​	Redis字典的实现设计的四个角色：**哈希表节点（dictEntry）**、**哈希表（dictht）**、**字典（dict）**和**操函数（dictType）**。

​	Redis字典使用哈希表作为底层实现，一个哈希表（数组）里有多个哈希表节点，每个哈希表节点就保存了字典中的一个键值对。

​	*源码在dict.h*

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

​	跳跃表是一种有序数据结构，它通过在每个节点维持多个指向其他节点的指针，从而达到快速访问节点的目的。

​	跳跃表支持平均O(logN)、最坏O(N)复杂度的节点查找，还可以通过顺序性操作来批量处理节点。

​	Redis使用**跳跃表作为有序集合键的底层实现之一**，如果一个有序集合包含的元素数量比较多，又或者有序集合中元素的成员是比较长的字符串时，Redis就会使用跳跃表来作为有序集合键的底层实现。

### 实现

​	Redis跳跃表的实现设计有两个角色：**用于表示跳跃表节点的`zskiplistNode`结构** 和 **用于保存跳跃表节点相关信息的`zskiplist`结构**。

​	*源码在server.h*

#### 跳跃表节点

```C
typedef struct zskiplistNode {
    sds ele;
    double score;
    struct zskiplistNode *backward;
    struct zskiplistLevel {
        struct zskiplistNode *forward;
        unsigned long span;
    } level[];
} zskiplistNode;
```

- **level[]**：该数组为`zskiplistLevel`结构的数组，表示跳跃表的“层”。该属性将节点分成多层，不同节点可能有不同的层高。该属性可以加快查找某些节点（类似二分跳过某些节点的原理）。该结构有两个属性：
    - ***forward** ：该属性称为**前进指针**，用指向**同层**的下一个跳跃表节点，实现节点遍历功能；
    - **span**：该属性用于记录前进指针指向的节点与该节点的**跨度**，也就是两个节点之间的距离；

- **score**：该属性表示节点的分值，跳跃表中的所有节点都按分值从小到大来排序，可以同分。
- **ele**：sds结构的属性，类似于成员名称，用于表明节点在跳跃表的名称，该值是**唯一的**；
- ***backward**：该属性称为**后退指针**，就是双端链表中前驱指针的意思，用于从表尾向表头方向访问节点，该指针的路径不能跳跃；

​	**个人总结**：跳跃表节点中的“层”是跳跃表的核心思想，在链表上增加“层”的概念，使得查找某个节点时，可以跳过一些节点，增加查找的速度。有种二分查找的思想，过滤掉目标不在的区域。

#### 跳跃表

​	该结构主要用于记录跳跃表的基本信息，使程序可以更加方便地对整个跳跃表进行某些操作，比如快速获取表头节点、表尾节点、跳跃表节点数量等信息；

```C
typedef struct zskiplist {
    struct zskiplistNode *header, *tail;
    unsigned long length;
    int level;
} zskiplist;
```

- **header**：指向跳跃表的表头指针；
- **tail**：指向跳跃表的表尾指针；
- **length**：记录节点的数量；
- **level**：用于记录除表头节点之外最高的层数；

​	**注意**：跳跃表的表头节点一般为空节点（类似链表的dummy节点），有很高的层数，只是用作层遍历起点。



## 整数集合

​	整数集合是集合键的底层实现之一，当一个解只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现。

​	整数集合可以保存类型为**int16_t、int32_t或者int64_t**的整数值，并且保证集合中不会出现重复元素。

### 实现

```c
typedef struct intset {
    uint32_t encoding;
    uint32_t length;
    int8_t contents[];
} intset;
```

- **contents[]**：该属性用于存放整数集合的值，各个元素在数组中按值的大小从小到大排列，并且数组不包含任何重复的元素；

- **length**：该属性记录了整数集合包含的元素数量，也是**contents**数组的长度；

- **encoding**：该属性决定**contents**数组的真正类型，也就是说**contents**数组的类型并不是**int8_t**；

    - **encoding**属性值为**INTSET_ENC_INT16**，那么**contents**就是一个**int16_t**类型的数组，数组每个元素都为**int16_t**类型的整数值（最小值为 -32 768，最大值为 32 767）；
    - **encoding**属性值为**INTSET_ENC_INT32**，那么**contents**就是一个**int32_t**类型的数组，数组每个元素都为**int32_t**类型的整数值（最小值为 -2 147 483 648，最大值为 2 147 483 647）；
    - **encoding**属性值为**INTSET_ENC_INT64**，那么**contents**就是一个**int64_t**类型的数组，数组每个元素都是**int64_t**类型的整数值（最小值为 -9 223 372 036 854 775 808，最大值为 9 223 372 036 854 775 808）；

    每次向**intset**添加元素时，都会重新分配空间（调用**zrealloc**函数），并更新**length**。

### 升级

​	升级（upgrade）是**指当我们要将一个新元素添加到整数集合里面，并且新元素的值超出整数集合目前类型的范围时，就需要先对整数集合升级，然后才能将新元素添加到整数集合里面**。也就是说整数集合为了能够装下新元素，会对整数集合中所有元素的类型进行升级，将范围扩大。

​	升级整数集合并添加新元素共分为三步进行：

1. 根据新元素的类型，扩展整数集合底层数组的空间大小，并为新元素分配空间；
2. 将底层数组现有的所有元素都转换成与新元素相同的类型，并将类型转换后的元素放置到正确的位置上，而且在放置元素的过程中，需要继续维持底层数组的有序性质不变；
3. 将新元素添加到底层数组里面；

源码如下：

```c
/* Upgrades the intset to a larger encoding and inserts the given integer. */
static intset *intsetUpgradeAndAdd(intset *is, int64_t value) {
    uint8_t curenc = intrev32ifbe(is->encoding);
    uint8_t newenc = _intsetValueEncoding(value);
    int length = intrev32ifbe(is->length);
    int prepend = value < 0 ? 1 : 0;

    /* First set new encoding and resize */
    is->encoding = intrev32ifbe(newenc); // 修改encoding，后面数组的操作都与该属性有关
    is = intsetResize(is,intrev32ifbe(is->length)+1); // 分配空间

    /* Upgrade back-to-front so we don't overwrite values.
     * Note that the "prepend" variable is used to make sure we have an empty
     * space at either the beginning or the end of the intset. */
    // 将现有的所有元素放到正确的位置，_intsetSet函数会根据encoding属性修改数组类型
    while(length--)
        _intsetSet(is,length+prepend,_intsetGetEncoded(is,length,curenc));

    /* Set the value at the beginning or the end. */
    // 根据正负将新元素放置正确位置
    if (prepend)
        _intsetSet(is,0,value);
    else
        _intsetSet(is,intrev32ifbe(is->length),value);
    is->length = intrev32ifbe(intrev32ifbe(is->length)+1);
    return is;
}
```

------

​	**个人问题**：查看源码，发现在最后一部插入新元素时，要么在数组的头部插入，要么在数组尾部插入。如果新元素的值应该在数组中间，那不就是破坏了集合的有序性吗？

​	**个人猜测**：在普通的添加函数（***intsetAdd()**）中，会对新元素进行判断，如果值大小超过现整数集合的类型的范围才会升级，如果值大小未超过，就算新元素的值的类型是较高级的，也会转换从现整数集合的类型。更何况在redis-cli中向集合新增值时，也无法指定类型。（猜测正确，有资料表明引起升级的元素必然超过现有整数集合类型的范围）

------

​	因为每次向整数集合添加新元素都可能会引起升级，而升级会将底层数组所有元素进行类型转换，所有整数集合添加新元素的时间复杂度为O(n)。

### 升级的好处

​	整数集合的升级策略有两个好处：**提高整数集合的灵活性**和**尽可能地节约内存**。

- **提高灵活性**：因为C语言是静态类型语言，为了避免类型错误，通常不会将两种不同类型的值放在同一个数据结构里面。例如，一般只使用**int16_t**类型的数组来保存**int16_t**类型的值。但是，因为整数集合可以通过自动升级底层数组来适应新元素，所以我们可以随意地将**int16_t、int32_t或int64_t**类型的整数添加到集合中，而不必担心出现类型错误，这种做法非常灵活。
- **节约内存**：如果没有升级策略，为了容纳**int64_t**类型的值，那么一开始数组就会固定为**int64_t**类型，而如果该数组只存更小类型的值时，那就会出现浪费内存的情况。但是有了升级策略之后，数组是自适应地调整数组类型，减少浪费内存的可能性。

**Tip**：**整数集合不支持降级操作。**



## 压缩列表



## 对象

​	在Redis中，数据库的键值对都是使用对象实现的，而对象系统是基于上述的数据结构创建的。对象系统包含字符串对象、列表对象、哈希对象、集合对象和有序集合对象五中类型的对象，每种对象都用到了至少一种上述的数据结构。

​	**在数据库的Key-Value键值对中，Key值总是字符串对象（后面以数据库键表示），而Value值可以是五种对象类型的一种（后面以数据库值表示）**。使用对象的好处是，可以针对不同的使用场景，为对象设置多种不同的数据结构实现，从而优化对象在不同场景下的使用效率（也就是说对象并不是由唯一数据结构实现的）。

### 对象的类型与编码

​	对象在源码（server.h/redisObject）中的定义如下：

```c
typedef struct redisObject {
    unsigned type:4;
    unsigned encoding:4;
    unsigned lru:LRU_BITS; /* LRU time (relative to global lru_clock) or
                            * LFU data (least significant 8 bits frequency
                            * and most significant 16 bits access time). */
    /* 这个属性与页面置换算法有关 */
    
    int refcount;
    void *ptr;
} robj;
```

#### 类型

​	对象的`type`属性记录了对象的类型，这些类型在源码中使用如下的宏定义：

```C
#define OBJ_STRING 0    /* String object. */
#define OBJ_LIST 1      /* List object. */
#define OBJ_SET 2       /* Set object. */
#define OBJ_ZSET 3      /* Sorted set object. */
#define OBJ_HASH 4      /* Hash object. */
```

​	可以看到，这些类型常量就如前面所说有五种类型。当我们对一个数据库键执行`TYPE`命令时，命令返回结果为数据库键对应的值对象的类型。

​	**个人总结**：Redis数据库是一种Key-Value数据，也就是说Redis使用键值对存储任何对象类型，而存储这些键值对的数据结构被称为**字典**，也就是Redis名字的由来——Remote Dictionary Server。我们可以把Redis数据库想象成一本字典，每个键对应一个值，键是唯一且只能是字符串对象，而值可以是其他类型的对象。

#### 编码和底层实现

​	**redisObject**结构中的**ptr**指针指向的是对象的底层实现数据结构，而这些数据结构由对象的**encoding**属性决定。

​	**encoding**属性记录了对象所使用的编码[^注7.1.1]，也就是说这个对象使用了什么数据结构作为对象的底层实现，这些编码常量在源码中使用如下的宏定义：

```C
#define OBJ_ENCODING_RAW 0     /* Raw representation */
#define OBJ_ENCODING_INT 1     /* Encoded as integer */
#define OBJ_ENCODING_HT 2      /* Encoded as hash table */
#define OBJ_ENCODING_ZIPMAP 3  /* Encoded as zipmap */
#define OBJ_ENCODING_LINKEDLIST 4 /* No longer used: old list encoding. */
#define OBJ_ENCODING_ZIPLIST 5 /* Encoded as ziplist */
#define OBJ_ENCODING_INTSET 6  /* Encoded as intset */
#define OBJ_ENCODING_SKIPLIST 7  /* Encoded as skiplist */
#define OBJ_ENCODING_EMBSTR 8  /* Embedded sds string encoding */
#define OBJ_ENCODING_QUICKLIST 9 /* Encoded as linked list of ziplists */
#define  OBJ_ENCODING_STREAM 10 /* Encoded as a radix tree of listpacks */
```

​	每种类型的对象都至少使用了两种不同的编码，后面介绍每种对象都可以使用哪种数据结构。

​	使用 `OBJECT ENCODING`命令可以查看数据库值对象的编码。

[^注7.1.1]:这里的编码不是将字符“编码”成字节的编码，而是将数据结构“编码”成对象的编码，类似于实现类与抽象类的关系

### 字符串对象

​	字符串对象的编码可以是 `INT`、`RAW`或者`EMBSTR` （省略了`OBJ_ENCODING`）。

​	下面列出创建不同编码的字符串对象时的情况：

- 如果一个字符串对象保存的是**整数值**，并且这个整数值可以用**long**类型来表示，那么字符串对象会将整数值保存在字符串对象结构的**ptr**属性里面（将**void***转换成**long**），并将字符串对象的**encoding**属性设置为`INT`。

- 如果一个字符串对象保存的是一个**字符串值**，并且这个字符串值的长度大于44字节，那么字符串对象使用一个SDS来保存这个字符串值，并将对象的**encoding**属性设置为`RAW`。

- 如果一个字符串对象保存的是一个**字符串值**，并且这个字符串值的长度小于等于44字节，那么字符串对象将使用`EMBSTR`编码的方式来保存这个字符串值。

    **Tips**：3.2版本之前是“39”作为两种编码的分界线，之后就是“44”。

​	下面是源码中一个存储字符串值的对象创建函数：

```c
#define OBJ_ENCODING_EMBSTR_SIZE_LIMIT 44
robj *createStringObject(const char *ptr, size_t len) {
    if (len <= OBJ_ENCODING_EMBSTR_SIZE_LIMIT)
        return createEmbeddedStringObject(ptr,len);
    else
        return createRawStringObject(ptr,len);
}
```

​	**`EMBSTR`编码是专门用于保存短字符串的一种优化编码方式**，这种编码和`RAW`编码一样，都是用**redisObject**结构和**sdshdr**结构来表示字符串对象，但`RAW`编码会**调用两次内存分配函数**来分别创建**redisObject**结构和**sdshdr**结构，而`EMBSTR`编码则通过**调用一次内存分配函数**来分配一块连续的空间，空间依次包含**redisObject**结构和**sdshdr**结构。

​	创建`EMBSTR`编码的字符串对象函数如下：

```c
robj *createEmbeddedStringObject(const char *ptr, size_t len) {
    robj *o = zmalloc(sizeof(robj)+sizeof(struct sdshdr8)+len+1);
    struct sdshdr8 *sh = (void*)(o+1);
    // 其余不贴出...
}
```

​	创建`RAW`编码的字符串对象函数如下：

```c
robj *createRawStringObject(const char *ptr, size_t len) {
    return createObject(OBJ_STRING, sdsnewlen(ptr,len));
}
// sdsnewlen()函数是会为sds结构分配内存的函数；
// createObject()函数会为redisObject结构分配内存的函数；
```

​	

​	使用`EMBSTR`编码的字符串对象来保存短字符串指有以下好处：

- 内存分配次数从两次降为一次；同理，调用释放内存函数次数也降低；
- `EMBSTR`编码的字符串对象的所有数据都保存在一块连续的内存里面，所以这种编码的字符串对象比`RAW`编码的字符串对象能够更好地利用缓存带来的优势；

> **Tips**：
>
> ​	long double类型表示的浮点数在 Redis中也是作为字符串值来保存的。如果我们要保存一个浮点数到字符串对象里面，那么程序会先将这个浮点数转换成字符串值，然后再保存转所得的字符串值。
>
> ​	在需要浮点数值的时候，则会将字符串值转换成浮点数值。

------

**个人问题**：在验证字符串对象编码中，发现如果字符串长度大于等于40时，字符串对象的编码是`RAW`；如果字符串长度小于40时，字符串对象的编码是`EMBSTR`，测试截图如下：

​	编码为`RAW`：

![字符串编码测试raw](C:\Users\Administrator\Desktop\学习\数据库\Database-learn\img\字符串编码测试raw.png)

​	编码为`EMBSTR`：

![字符串编码测试embstr](C:\Users\Administrator\Desktop\学习\数据库\Database-learn\img\字符串编码测试embstr.png)

​	**个人猜测**：可能在传递到实现函数的过程中，保留了或附加了5个长度。

​	**答：**我的Redis是3.0版本的。。。（淦）

​	**个人问题**：为什么会以39或44作为编码的分解点？

​	**参考**：https://blog.csdn.net/qq_39809613/article/details/100146081

------

#### 编码的转换

​	`INT`编码和`EMBSTR`编码的字符串对象，在某条件满足时，会被装换为`RAW`编码的字符串对象。

​	对于`INT`编码的字符串对象来说，如果我们向对象执行一些命令，使得这个对象存的不再是整数值，而是一个字符串值，那么字符串对象的编码将从`INT`变为`RAW`。例如通过`APPEND`命令，在`INT`编码的字符串对象中追加一个字符串值，因为追加操作只能对字符串值执行，所以程序会先将之前保存的整数值转换成字符串值，然后再执行追加操作。

​	对于`EMBSTR`编码的字符串对象来说，只要对其执行任何修改命令时，程序就会先将对的编码从`EMBSTR`编程`RAW`，再执行修改命令。这是因为在Redis中，没有为`EMBSTR`编码的字符串对象编写相应的修改程序，所以`EMBSTR`编码的字符串对象实际上是只读的。

#### 字符串命令的实现

待

### 列表对象

​	列表对象的编码可以是`ZIPLIST`、`LINKEDLIST`或`QUICKLIST`。

​	`ZIPLIST`编码的列表对象使用压缩列表作为底层实现，每个压缩列表节点保存了一个列表元素。

​	`LINKEDLIST`编码不再使用。

# 参考资料

《Redis设计与实现》——黄健宏著（该书基于redis3.0之前编写）