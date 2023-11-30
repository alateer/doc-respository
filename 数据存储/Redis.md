## Redis

### NoSQL

NoSQL，not only sql ，泛指非关系型数据库

场景：数据高并发、数据海量读写、数据高可扩展

常见：Redis、MongoDB、HBase


### 数据类型

#### Redis 键

`key *`   获取当前库的所有key值

set key value  

exists key

del key  删除制定key

unlink key  根据value选择非阻塞删除

先将keys从keyspace中删除，真正的删除会在后续的异步操作中

expire key 10   过期时间

ttl key 还有多少秒过期  -1不过期  -2 过期

select 切换数据库、

dbsize 查看当前数据库key数量

flushdb 清空当前库

flushall 清空所有库



### Redis 字符串

key - value 

二进制安全

set key value

get key

append 末尾追加

strlen 长度

setnx 只有在key不存在时，设置key的值

incr 将库中的值加1

decr 减1

incrby、decrby key value  自增自减指定步长

mset、mget 设置多个键值对/获取多个key

msetnx  原子的设置多键值对

getrange  key start end  截取字符串

setrange key start value  覆盖字符串

setex  key  过期时间  value   设置键值的同时，设置过期时间

getset key  value  新 -》 旧



数据结构：简单动态字符串SDS，可修改字符串，类似于ArrayList，可以预分配冗余空间

<1M时，2倍，》1M时只增加1M，最大512M



### Redis List

简单字符串列表，按照插入顺序排序，双向链表，两端操作很高

lpush、rpush

lpop、rpop

rpoplpush  右取左插

lrange key start stop  按索引获取元素

lindex 按索引下标找值

llen  长度

linsert key before value newvalue

lrem key n value 从左删n个

lset key index value  替换

list 数据结构为quickList，压缩列表

redis将链表和zipList结合组成了quickList



### Redis  Set

string类型的无需集合，是一个hash表

sadd key v1 v2

smembers key 取出所有值

sismember key value  key中是否存在value

scard 集合元素个数

srem key v1 v2 删除某个元素

spop key 随机吐出一个值

srandmember 随机取出n个值，不吐出

smove source to value   将一个值从一个集合到另一个集合

sinter  交集

sunion  并集

sdiff  差集 左边的

数据结构是hash表



Redis Hash

键值对，key - value

string 类型的field 和 value 的映射表，特别适合存储对象

类似于Map<String, Object>

key field value

hset key field value

hget key field

hmset 多个

hexists key field 存在

hkeys key 所有field

hvals  所有value

hincrby key field increment 加1

hsetnx key field value 当不存在时设置

数据结构：ziplist和hashtable

### Redis Zset

zset是一个没有重复元素的集合

带有评分（优先级）

zadd key score value score value

zrange key start stop 【withscores】位置

zrangebyscore key minmax 排序

zrevrangebyscore key minmax 倒

zincrby key incrment value  增加指定值 

zrem key value  删

zcount key min max 统计

zrank key value 排名

数据结构

hash 关联元素value和权重score

跳跃表，给元素value排序，多层

### Redis Bitmap

Redis Bitmap
即位图，是一串连续的二进制数组（0和1），可以通过偏移量（offset）来确定元素位置
BitMap通过最小的单位bit来进行 0｜1的设置，表示某个元素的值或状态
时间复杂度是O（1）
适用于数据量非常大且使用二值统计的场景
不属于Redis的基本数据类型，是基于String类型的位操作
Redis字符串的最大长度是512M，即：2^3(8 -> 1bytes) * 2^10(1K) * 2^10(1M) * 2^9 = 2^32，这也是offset的上限
操作：
SETBIT key offset value
GETBIT key offset
BITCOUNT key [start end]

### 配置文件

bind 127.0.0.1 # 只支持本地访问 需要注释

protected-mode



### Redis 发布订阅

pub sub

ridis可以订阅多个频道

打开一个客户端，订阅一个频道：

subscribe channel1

打开另一个客户端，订阅一个频道

publish channel1 hello


### Redis 事务

Redis 事务是一个单独的隔离操作：事务中的所有命令都会被序列化，按顺序执行，事务在执行的过程中，不会被其他客户端发送过来的命令请求打断

Redis事务的主要作用时串联多个命令防止别的命令插队

命令：Multi、Exec、Discard

Multi：命令进入命令队列，但不会执行

Exec：命令开始依次执行

Discard：放弃组队

组队过程中（multi）出现了错误，则执行整个的所有队列都会被取消

执行的过程中（exec后）出现了错误，则只会不处理不成功的那个命令，不会回滚

#### 事务冲突问题

解决方案：悲观锁和乐观锁

悲观锁：

很悲观，每次别人去拿数据的时候都会认为别人会修改，所以每次拿数据的时候都会上锁，这样其他人再想拿数据就会被block，知道拿到锁，这种锁机制在传统关系型数据库有很多应用，如表锁，行锁，读锁，写锁等，在做操作之前要先上锁

乐观锁：

比较乐观，别人拿数据的时候都认为别人不会进行修改，所以不会上锁，但是在更新的时候回去判断别人有没有在此期间操作过这个数据（使用版本号等机制），乐观锁适用于多读的数据类型，可以提高吞吐量，Redis就是利用了这种check-and-set机制

#### watch key 

在执行multi之前，先执行watch key1 key2，可以监视一个或多个key版本是否一致，如果事务执行之前这个（这些）key被命令所改动，则事务将被打断

unwatch可以取消监视

##### 三特性

+ 单独的隔离操作

可以通过开启事务实现一个单独的隔离操作

+ 没有隔离级别的概念

事务执行前失败就都失败了

+ 不保证原子性

不回滚



### 秒杀系统

秒杀过程（单机环境）

1. 判断用户和商品id是否存在
2. 连接redis
3. 拼接key：库存key和秒杀成功用户key
4. 判断库存是否存在，不存在则提示
5. 判断用户是否重复秒杀操作
6. 如果商品数量小于1，则秒杀结束
7. 秒杀过程：库存减1，将成功用户加入到成功用户清单中
8. 关闭

并发情况下库存会低于0（没有事务支持）

+ 秒杀结束还会提示秒杀成功
+ 连接超时（当并发人数很多时）

#### 超卖问题

并发情况下发生冲突

解决方法：乐观锁，即加一个watch监控

watch监控的是库存（事务之前）

在库存操作时使用事务（multi、exec）

如果exec的结果为空，则失败，表示秒杀失败



#### 超时问题

通过单例懒加载双重检查（两个if）禁止指令重排（volatile）的方式构建一个Redis连接池

通过连接池来连接redis，这样连接好的实例就可以反复利用，减少每次连接redis服务带来的消耗，确保并发情况下的超时连接问题



#### 库存遗留问题

乐观锁会造成这个问题

比较版本号时如果不一致，则就会直接不进行后续获取库存数量的比较，导致库存遗留

解决方案：

使用LUA脚本：Lua是一个小巧的脚本语言，可以很容易的被C/C++代码调用，也可以反过来调用C/C++的函数，Lua没有提供强大的库，一个完整的Lua解释器不过200k，不适合作为独立开发的语言，适合做嵌入式的脚本语言，实现可配置性， 可扩展性

Lua脚本的优势：

+ 将复杂或者多步的redis操作，写为一个脚本，一次性提交给redis执行，减少反复连接redis的次数
+ Lua脚本类似于redis事务，有一定的原子性，不被其他命令插队
+ 只有redis2.6以上版本才支持
+ 利用lua脚本淘汰用户，解决超卖问题
+ 解决争抢问题，实际上是利用redis单线程任务队列的特性解决多并发问题



### Redis 持久化 RDB

Redis DataBase

在指定的**时间间隔**内将内存中的**数据集快照**写入磁盘

备份执行：

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写进一个临时文件中，当持久化过程结束，再将这个临时文件代替上次的持久化好的文件，主线程不进行任何IO操作，保证了极高的性能

如果需要进行大规模的数据恢复，且对数据恢复的完整性不是非常敏感，则RDB较之AOF要跟高效

RDB的缺点是：**最后一次持久化后的数据容易丢失**

生成持久化文件：dump.rdb

配置默认会生成rdb文件



#### Fork

fork的作用就是复制一个与当前进程一样的进程，新进程的所有数据与原进程一致，但是是一个全新的进程，作为子进程

+ 在Linux程序中，fork（）会产生一个与父进程一摸一样的子进程，但子进程此后会exec系统调用，出于效率，Linux引入“写时复制技术”



#### 配置

save 多少秒之内执行的写操作次数

save 20 3  20秒内有超过3个写入操作则进行持久化



#### 备份



### Redis 持久化 AOF

Append Only File

以日志的形式来记录每一个写操作（增量保存），将Reids中的写记录保存下来（读操作不记录），只能追加文件但不能该文件

默认不开启，配置中开启：appendonly yes

aof配置文件名：appendonly.aof

生成文件位置：./  当前目录

aof和rdb同时开启时，默认取aof中的数据（加入原本库里有数据，rdb中保存了，但是配置了aof，同时开启，这时候，查看库中数据为空，即以aof中的数据为主）

可以以两者为主，冷备热储



同步频率设置

appendonly always  始终同步，没写入一次就记入日志，性能低安全高

appendonly everysec   每秒同步，只丢一秒数据

appendonly no  不主动同步，同步时机交给操作系统



压缩操作

将aof中的指令进行压缩，单次变多次

文件大于64M、大一倍后就重写压缩



### Redis 主从复制

主机数据更新后，根据配置和策略，自动同步到备份机的 master/slave机制，master写为主，slave读为主，一主多存（且只能有一个主）

+ 读写分离，性能提升（多读，因为读的频率高，所以slave多，少写，写的频率低，所以master少）
+ 容灾快速恢复



#### 配置一主两从

1. 创建/myredis目录
2. 复制redis.conf配置文件到目录中
3. 配置一主两从
   1. 创建三个配置文件：redis6379.conf、redis6380.conf、redis6381.conf
   2. 三个配置文件内容：include、pidfile、port、dbfilename
   3. 里面的端口号保持一致
4. 启动三个redis服务
5. info replication  查看主从服务器的信息
6. 配从不配主：slaveof ip port（在6380和6381上执行）



#### 一主二仆

从服务器挂了，则再次启动后，变为默认的主服务器，并不会变为原来的从服务器，需要手动加入，加入后，会将主服务器的数据同步过来

当主服务器挂了以后，从服务器还是从服务器，其主服务器依然是挂掉的，当主服务器启动后，主服务器依然还是主服务器

#### 主从复制原理

1. 从服务器连接上主服务器后，从服务器会向主服务器发送一个消息同步的消息
2. 主服务器接受到从服务器发送过来的同步消息，把主服务器的数据进行持久化到rdb文件，把rdb文件发送给从服务器，从服务器拿到rdb文件后进行读取（全量复制）
3. 后续每次主服务器进行写操作后，会和从服务器进行数据同步（增量复制）、

#### 薪火相传

当从服务器过多时，主服务器的同步过程会非常消耗性能，这时候可以让从服务器去向其他从服务器进行数据的同步

命令：slaveof ip port  当前为从服务器，ip port为其他从服务器

这时候主服务器的从服务器为直接同步数据的服务器

缺点：当中间从服务器挂了，则后续的从服务器数据会丢失，因为从服务器不向他们同步信息

#### 反客为主

当主服务器挂了以后，可以通过命令让从服务器变为主服务器

命令：slaveof no one  将从机变为主机（手动）

#### 哨兵模式

反客为主的自动版，后台监控主机是否故障，故障则根据投票数自动将从服务器转化为主服务器

模式为一主二仆模式

在/myredis目录下新建sentinel.conf的文件（名字不能错）

写入：sentinel monitor mymaster ip port number 

mymaster为主服务器别名，number为至少从服务器同意的个数

命令：

主机挂了后，哨兵监控到了，然后选举从机中的某一个作为主机，其他从机依然为从机，原来的从机也变为了从机，即使重新启动后，也依然是从机



复制延时

选举规则

+ 根据replace-propertice的值（越小）
+ 偏移量组最大的
+ runid最小的



### Redis 集群

代理主机：nginx 反向代理（加一层）

缺点：服务器过多，不容易管理

#### 无中心化集群

任何一台服务器都可以作为进入的口，进入的请求如果不是当前服务承担的，则转发给其他可以处理请求的服务器（把代理主机的那一层去掉了）

#### redis 集群

实现了对Reids 的水平扩容，即启动N个Redis节点，会将数据库分布式的存储到N个服务器上，每个节点承担N分之一

Redis集群通过分区（partition）来提供一定程度的可用性

#### 集群搭建

三主三从

配置基本信息

cluster-enabled yes  开启集群模式

cluster-config-file  配置文件名称

cluster-node-time  超时时间

启动

一个集群中至少要有3个主节点

分配原则：尽量保证每个主服务器不在同一个IP地址，保证从库不在主库所在的服务器上



##### slots 插槽

集群中包含一共16384个插槽，数据库中的每一个键都是这16384个插槽中的一个

集群使用公式：CRC16（key）%16384来计算key 属于哪个插槽

先计算插槽值，再交给指定范围插槽值的主服务器执行操作

集群中不能执行批量操作

当前主服务器只能看自己插槽的值（一个插槽中可以有多个 值？）



某个主服务器挂掉后，15秒超时后，其从机变为主机，主服务器恢复后，会变为从机



如果某一段插槽中的主从都挂掉了以后，如果配置了一个属性为yes时，那么，整个集群都挂掉

如果配置该属性为no时，则集群不挂掉，只不过挂掉的那段插槽不能使用了



### 缓存穿透

流程

1. 引用服务器的压力突然变大了
2. redis命中率低了
3. 一直查数据库

原因

1. redis中查询不到数据库了
2. 出现很多非正常url访问（黑客恶意访问）

解决方案

1. 对空值进行缓存，但设置过期时间很短，不超过5分钟
2. 访问可访问的名单（白名单），即用bitmaps定义一个可以访问的名单
3. 布隆过滤器（底层与上面相似，不过优化了），也是检索一个元素是否在集合中
4. 实时监控，命中率降低时，排查访问对象和访问数据，配合运维人员，设置黑名单



### 缓存击穿

现象

1. 数据库访问压力瞬间增大
2. redis中key没有出现过期
3. redis正常运行

原因

1. redis中某个key过期了（热门访问的key），大量的访问请求了这个key

解决方案

1. 预先设置一些热门数据，加大这些数据key的时长
2. 实时调控，监控数据热门，实时调整key的过期时长
3. 使用锁



### 缓存雪崩

1. 数据库压力变大
2. 服务器崩溃

原因：

在极少时间段，查询大量key的过期情况

解决方案：

1. 构建多级缓存架构：nginx缓存、redis缓存、其他缓存等
2. 使用锁或队列
3. 设置过期标志更新缓存
4. 将缓存失效的时间分散开



### 分布式锁

单体系统进入集群系统后，多线程、进程分布在不同的机器上，原先单体系统中的并发控制锁机制失效了，为解决这个问题，需要一种跨JVM的互斥机制来控制共享资源的访问（锁共享）

分布式锁的实现方案：

+ 基于数据库实现
+ 基于缓存（Redis）
+ 基于Zookeeper

优缺点都有，：

性能：redis最高

可靠性：zookeeper最高



#### redis实现分布式锁

命令：setnx key value  加入分布式锁

想要去掉分布式锁，需要del删除key才可以（即setnx上锁，del释放锁）



+ 当 锁一直不可用时，需要锁的会一直等待

解决方案：设置过期时间

+ 上锁之后产生了异常，导致无法设置过期时间

上锁的同时设置过期时间：set key 10 nx ex 12



存在的问题：释放锁的时候释放的是别人的锁

解决方案：UUID防误删：锁每次都不一样

当释放锁的时候，比较设置的UUID与当前的UUID是否一致判断是否删除



删除操作缺乏原子性

解决方案：使用Lua脚本

## Redis

#### 缓存的基本思想

+ 缓存可以提高系统性能以及减少请求响应事件
+ 基本思想：**空间换时间**
+ 缓存的实际应用：如CPU Cache 缓存的内存数据用于解决 CPU 处理速度与内存不匹配的问题；内存缓存是硬盘数据用于解决硬盘访问速度过慢的问题；还有操作系统中在页表方案的基础上引入了快表来加速虚拟地址到物理地址的转换（可以把快表理解为一种特殊的高速缓冲存储器）
+ 我们为了避免用户在请求数据的时候获取速度过于缓慢，所以我们在数据库之上增加了缓存这一层来弥补

#### 使用缓存为系统带来的问题

引入本地缓存来做简单业务场景的化，实际代价几乎可以忽略，主要针对分布式缓存来说

+ **系统复杂性增加**：引入缓存后，要维持缓存和数据库的数据一致性，维护热点缓存等
+ **系统开发成本增加**：引入缓存意味着要一个单独的缓存服务，需要花费相应的成本

#### 本地缓存解决方案

> 本地缓存在实际的很多项目中用到， 特别是在单体架构时，数据量不大，并且无分布式要求
>
> 常见的单体架构使用 Nginx 来做负载均衡，部署到两个相同的服务到服务器，两个服务器使用同一个数据库，并且使用本地缓存

+ JDK 自带的 HashMap 和 ConcurrentHashMap

两者都存放 key/value 形式的键值对，但大部分场景下都不会使用这两种作为缓存，因为一个稍微完善一点的缓存框架至少要提供：过期时间、淘汰机制、命中率这三点

+ **Ehcache、Guava Cache、Spring Cache** 

Ehcache 相较于其他两者更加重量，但 Ehcache 支持可以嵌入到 hibernate 和 mybatis 中作为多级缓存，并且可以将缓存数据持久化到本地磁盘中，同时提供了集群方案（鸡肋)

Guava 相比于 Spring Cache 的话使用的更多一点，他提供了 API 非常方便我们使用，同时也提供了设置缓存有效时间的功能，并且内部实现比较干净

使用 Spring Cache 的注解实现缓存的话，代码干净和优雅，但容易出现问题，比如缓存穿透和内存溢出

+ 后起之秀 **Caffeine**

相比于Guava，Caffeine 在各个方面（比如性能）要更加优秀，一般建议用来代替Guava

#### 为什么要有分布式缓存？/为什么不直接用本地缓存

我们可以把分布式缓存（Distributed Cache）看作是一种内存数据库的服务，它的最终作用就是提供缓存数据服务

使用分布式缓存后，缓存部署在一台单独的服务器上，即使同一个相同的服务部署在再多机器上，也是使用的同一份缓存，并且，单独的分布式缓存服务到性能、容量和提供的功能都要更加强大

缺点是要引入单独的服务

#### 缓存读写模式/更新策略

介绍三种模式，各有优劣，不存在最佳模式，根据具体的业务场景选择合适自己的缓存读取模式

+ Cache Aside Pattern（旁路缓存模型）

写：更新DB，然后直接删除 cache

读：从 cache 中读取数据，读取到就直接返回，读取不到，就从 DB 中取数据返回，并放到cache中

结果以 DB 的结果为准

+ Read/Write Through Pattern（读写穿透）

写：先查cache，cache中不存在，直接更新DB，cache中存在，则先更新cache，然后cache服务自己更新DB（同步更新 cache 和 DB）

读：从cache中读取数据，读取到就直接返回，读取不到的话，先从DB加载，写入到cache后返回相应

+ Write Behind Pattern（异步缓存写入）

Read/Write Through 是同步更新 cache 和 DB，而 Write Behind 则只是更新缓存，不直接更新DB，而是改为异步批量的方式来更新 DB

Write Behind Pattern 下的写性能非常高，尤其适合一些数据经常变化的业务场景比如说一篇文章的点赞数量，阅读数量等

#### Redis介绍

Redis 是一个使用C语言开发的数据库，其数据存储在内存中，读写速度非常快

应用方向：主要用于缓存，也用来做分布式锁，甚至是消息队列

Redis提供多种数据类型来支持不同的业务场景，支持事务，持久化，Lua 脚本，多种集群方案

#### 分布式缓存常见的技术选型方案

使用的比较多的主要是 Memcached 和 Redis，不过，现在基本都是直接用 Redis了

#### Redis 和 Memcached 的区别

+ **Redis 支持更丰富的数据类型（或者说更复杂的应用场景）**。Redis不仅支持简单的 k/v类型数据，还提供list，set，zset，hash等数据结构，而Memcached 只支持简单的 k/v 
+ **Redis支持数据的持久化**，可以将内存中的数据保持到磁盘中，重启后还可以再次加载进行使用，而 Memcached 将所有数据都存在内存中
+ **Redis 有灾难恢复机制**。因为支持数据的持久化
+ Redis在服务器内存使用完之后，可以将不用的数据放到磁盘上，但 Memcached 在服务器内存使用完之后，会直接报异常
+ Memcached 没有原生的集群模式，需要依靠客户端来实现往集群中分片写入数据；但 Redis 目前是支持原生 cluster 模式的
+ Memcached 是多线程，非阻塞 IO 复用的网络模型；Redis 使用单线程的多路IO复用模型
+ Redis支持发布订阅模型，Lua 脚本，事务，多编程语言等功能，Memcached不支持
+ Memcached 过期数据的删除策略只用了惰性删除，而Redis同时使用了惰性删除和定期删除

#### 为什么要用Redis

+ 高性能

保证用户下一次再访问这些数据的时候可以直接从缓存中获取，操作缓存就是直接操作内存

+ 高并发

一般MySQL的QPS（每秒查询次数）大概在1w左右（4核8g），而使用Redis之后很容易达到10w+，最高甚至能达到30w+

#### Redis常见数据结构及使用场景分析

Redis官方提供在线环境：https://try.redis.io/

+ string

string数据结构是简单的key-value类型，没有使用C的字符串表示，而是自己构建了一种 简单动态字符串。相比于C的字符串，Redis 的string 可以保持文本数据和二进制数据，并且获取字符串长度的复杂度是O（1），另外该API 是安全的，不会造成缓冲区溢出

常用命令：set,get,strlen,exists,dect,incr,setex等等

应用场景：用于需要计数的场景，比如用户的访问次数、热点文章的点赞转发数量等

```sql
set key value #设置 key-value 类型的值
get key # 根据 key 获得对应的 value
exists key  # 判断某个 key 是否存在
strlen key # 返回 key 所储存的字符串值的长度。
del key # 删除某个 key 对应的值
get key
# 批量设置-----------------------------------
mset key1 value1 key2 value2 # 批量设置 key-value 类型的值
mget key1 key2 # 批量获取多个 key 对应的 value
# 计数器（字符串的内容为整数的时候使用）-----------------------
set number 1
incr number # 将 key 中储存的数字值增一
get number
decr number # 将 key 中储存的数字值减一
get number
# 过期---------------------------------------
expire key  60 # 数据在 60s 后过期
setex key 60 value # 数据在 60s 后过期 (setex:[set] + [ex]pire)
ttl key # 查看数据还有多久过期
```

+ list

list 是 链表，特点是易于插入和删除，不易随机访问。Reids 的 list 实现是一个 双向链表

常用命令：rpush,lpop,lpush,rpop,lrange,llen等（r 和 l 分别代表右，左）

应用场景：发布与订阅或者或消息队列、慢查询

```mysql
---------通过rpush/lpop 实现队列-----------
rpush myList value1 # 向 list 的头部（右边）添加元素
rpush myList value2 value3 # 向list的头部（最右边）添加多个元素
lpop myList # 将 list的尾部(最左边)元素取出
lrange myList 0 1 # 查看对应下标的list列表， 0 为 start,1为 end
lrange myList 0 -1 # 查看列表中的所有元素，-1表示倒数第一
# lrange命令可以实现分页查询，性能非常高
---------通过rpush/rpop 实现栈-----------
rpush myList2 value1 value2 value3
rpop myList2 # 将 list的头部(最右边)元素取出
# llen 查看链表长度
llen myList
```

+ hash

hash 类似于 JDK1.8以前的HashMap，内部实现差不多（数组+链表）。另外，hash是一个string 类型的field 和 value 的映射表，特别适合存储对象，操作时，直接仅仅修改对象的某个字段的值

常用命令：hset,hmset,hexists,hget,hgetall,hkeys,,hvals等

应用场景：系统中对象数据的存储，用户信息，商品信息等

```sql
127.0.0.1:6379> hset userInfoKey name "guide" description "dev" age "24"
OK
127.0.0.1:6379> hexists userInfoKey name # 查看 key 对应的 value中指定的字段是否存在。
(integer) 1
127.0.0.1:6379> hget userInfoKey name # 获取存储在哈希表中指定字段的值。
"guide"
127.0.0.1:6379> hget userInfoKey age
"24"
127.0.0.1:6379> hgetall userInfoKey # 获取在哈希表中指定 key 的所有字段和值
1) "name"
2) "guide"
3) "description"
4) "dev"
5) "age"
6) "24"
127.0.0.1:6379> hkeys userInfoKey # 获取 key 列表
1) "name"
2) "description"
3) "age"
127.0.0.1:6379> hvals userInfoKey # 获取 value 列表
1) "guide"
2) "dev"
3) "24"
127.0.0.1:6379> hset userInfoKey name "GuideGeGe" # 修改某个字段对应的值
127.0.0.1:6379> hget userInfoKey name
"GuideGeGe"
```

+ set

类似于 HashSet ，无序，无重复数据，提供了判断某个成员是否在一个 set 集合内的重要接口，可以基于set轻易实现交集，并集，差集的操作

常用命令：sadd,spop,smembers,sismember,scard,sinterstore,sunion等

应用场景：存放的数据不能重复且需要获取多个数据源交集和并集的场景

```sql
127.0.0.1:6379> sadd mySet value1 value2 # 添加元素进去
(integer) 2
127.0.0.1:6379> sadd mySet value1 # 不允许有重复元素
(integer) 0
127.0.0.1:6379> smembers mySet # 查看 set 中所有的元素
1) "value1"
2) "value2"
127.0.0.1:6379> scard mySet # 查看 set 的长度
(integer) 2
127.0.0.1:6379> sismember mySet value1 # 检查某个元素是否存在set 中，只能接收单个元素
(integer) 1
127.0.0.1:6379> sadd mySet2 value2 value3
(integer) 2
127.0.0.1:6379> sinterstore mySet3 mySet mySet2 #获取mySet和mySet2的交集存放mySet3中
(integer) 1
127.0.0.1:6379> smembers mySet3
1) "value2"
```

+ sorted set

和set 相比，sorted set 增加了一个权重参数score，使得集合中的元素可以按照 score 进行有序排序，还可以通过 score 的范围来获取元素的列表

常用命令：zadd,zcard,zscore,zrange,zrevrange,zrem等

应用场景：需要对数据根据权重进行排序的场景，如礼物排行榜等z

```sql
127.0.0.1:6379> zadd myZset 3.0 value1 # 添加元素到 sorted set 中 3.0 为权重
(integer) 1
127.0.0.1:6379> zadd myZset 2.0 value2 1.0 value3 # 一次添加多个元素
(integer) 2
127.0.0.1:6379> zcard myZset # 查看 sorted set 中的元素数量
(integer) 3
127.0.0.1:6379> zscore myZset value1 # 查看某个 value 的权重
"3"
127.0.0.1:6379> zrange  myZset 0 -1 # 顺序输出某个范围区间的元素，0 -1表示输出所有元素
1) "value3"
2) "value2"
3) "value1"
127.0.0.1:6379> zrange  myZset 0 1 # 顺序输出某个范围区间的元素，0为 start 1为stop
1) "value3"
2) "value2"
127.0.0.1:6379> zrevrange  myZset 0 1 # 逆序输出某个范围区间的元素，0为start 1为stop
1) "value1"
2) "value2"
```

#### Redis 单线程模型详解

Redis 是基于 Reactor 模式来设计开发了自己的一套高效的事件处理模型（Netty 的线程模型也是基于 Reactor 模式）

Redis 通过IO 多路复用程序来监听来自客户端的大量连接

I/O多路复用技术的使用让 Redis 不需要额外的多余线程来监听客户端的大量连接，降低了资源消耗

#### Redis 给缓存数据设置过期时间的作用

内存是有限的，如果缓存中所有的数据都一直保存的话，分分钟直接OOM

某些业务场景需要某个数据只在某一段时间内存在，如短信验证码，用户登录的token

#### 过期数据的删除策略

+ 惰性删除

只会在取出key的时候才会对数据进行过期检查，这样对CPU好，但可能造成太多过期key没被删

+ 定期删除

每隔一段时间抽取一批 key 执行删除过期key操作，对内存更加友好，故Redis采用两种方式结合

#### Redis 内存淘汰机制

如何保证 Redis 中的数据都是热点数据，提供了多种数据淘汰策略

+ volatile-lru：从已设置过期时间的数据集中挑选最近最少使用的数据淘汰
+ volatile-ttl：从已设置过期时间的数据集中挑选将要过期的数据淘汰
+ volatile-random：从已设置过期时间的数据集中任意挑选数据淘汰
+ allkeys-lru：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的key（常用）
+ allkeys-random：从数据集中任意选择数据淘汰
+ no-eviction：禁止驱逐数据，内存不够容纳新写入数据时，新写入数据操作会报错（不用）

4.0版本后新增

+ volatile-lfu:从已设置过期时间的数据集中挑选最不经常使用的数据淘汰
+ allkeys-lfu：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的key

#### Redis 持久化机制

两种方式：快照 和 只追加文件

+ 快照（RDB）

通过创建快照来获取存储在内存里面的数据在某个时间点上的副本

+ 只追加文件（AOF）

每执行一条会修改 Redis 中数据的命令，Redis 就会将命令写入硬盘中的 AOF 文件

4.0后开始支持 RDB 和 AOF 的混合持久化

#### Redis 事务

可以通过 MULTL，EXEC，DICARD 和 WATCH 等命令实现事务功能

Redis 是不支持 roll back的，所以不满足原子性（也不满足持久性）

Redis事务提供了一种将多个命令请求打包的功能。然后，再按顺序执行打包的所有命令，并且不会被中途打断

#### 缓存穿透

大量请求的 key 根本不存在于缓存上，导致请求直接到了数据库上，根本没有经过缓存这一层

解决办法：

+ 做好参数校验，不合法的参数请求直接抛出异常信息，如id不能小于0，邮箱格式不对等
+ 缓存无效key：写一个无效的 key 到缓存中，设置过期时间
+ 布隆过滤器：某个元素存在，小概率会误判，某个元素不存在，那么一定不在

#### 缓存雪崩

缓存在同一时间大面积的失效，后面的请求都直接落到了数据库上，造成数据库短时间内承受大量请求

一些被大量访问数据（热点缓存）在某一时刻大面积失效，导致对于请求直接落在数据库上

解决方案：

+ 针对Redis 服务不可用情况
  + 采用Redis集群，避免单机出现问题整个缓存服务都没办法使用
  + 限流，避免同时处理大量请求
+ 针对热点缓存失效的情况
  + 设置不同失效的时间比如随即设置缓存失效时间
  + 缓存永不失效
