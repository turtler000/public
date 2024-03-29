面试总结：

redis高并发：主从架构，一主多从，单主用来写入数据，单机几万QPS，多从用来查询数据，多个从实例可以提供每秒10万的QPS。

redis高并发的同时，还需要容纳大量的数据：一主多从，每个实例都容纳了完整的数据，比如redis主就10G的内存量，其实你就最对只能容纳10g的数据量。如果你的缓存要容纳的数据量很大，达到了几十g，甚至几百g，或者是几t，那你就需要redis集群，而且用redis集群之后，可以提供可能每秒几十万的读写并发。

redis高可用：如果你做主从架构部署，其实就是加上哨兵就可以了，就可以实现，任何一个实例宕机，自动会进行主备切换。

### 一、基础知识

1、数据结构

string

hash

list: lrange,可以分页查询

set : sadd key ...value 

1.不允许有重复的元素，2.集合中的元素是无序的，不能通过索引下标获取元素，3.支持集合间的操作，可以取多个集合取交集、并集、差集。集群条件下全局去重

sort-set:  

```
zadd key score member
zrevrange key 0 3 //获取排名前3的用户
zrank key value //获取value排名

```

2、为什么redis快

1>.纯内存操作;

2>.核心是基于非阻塞的IO多路复用机制;

3>.底层使用C语言实现,一般来说,C 语言实现的程序"距离"操作系统更近,执行速度相对会更快;

4>.单线程同时也避免了多线程的上下文频繁切换问题,预防了多线程可能产生的竞争问题;

### 二、过期时间

1、定期删除，惰性删除

定期删除：100ms,随机检测一部分，若过期则删除

惰性删除：取数据的时候检测

2、内存淘汰

1）noeviction：当内存不足以容纳新写入数据时，新写入操作会报错

2）allkeys-lru：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的key（这个是最常用的）

3）allkeys-random：当内存不足以容纳新写入数据时，在键空间中，随机移除某个key

5）volatile-random：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，随机移除某个key

6）volatile-ttl：当内存不足以容纳新写入数据时，在设置了过期时间的键空间中，有更早过期时间的key优先移除

### 三、集群

#### 1、基础

读高并发：起集群，做读写分离，master负责写，slave负责读，每个slave存全部数据

可以水平扩容，如果QPS增加，只需要增加slave

redis replication的核心机制

（1）redis采用异步方式复制数据到slave节点，不过redis 2.8开始，slave node会周期性地确认自己每次复制的数据量
（2）一个master node是可以配置多个slave node的
（3）slave node也可以连接其他的slave node
（4）slave node做复制的时候，是不会block master node的正常工作的
（5）slave node在做复制的时候，也不会block对自己的查询操作，它会用旧的数据集来提供服务; 但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了
（6）slave node主要用来进行横向扩容，做读写分离，扩容的slave node可以提高读的吞吐量

开启RDB和AOF备份，RDB （Redis DataBase）和 AOF （Append Only File）

RDB 是 Redis 默认的持久化方案。在指定的时间间隔内，执行指定次数的写操作，则会将内存中的数据写入到磁盘中。即在指定目录下生成一个dump.rdb文件。Redis 重启会通过加载dump.rdb文件恢复数据。

AOF ：Redis 默认不开启。它的出现是为了弥补RDB的不足（数据的不一致性），所以它采用日志的形式来记录每个**写操作**，并**追加**到文件中。Redis 重启的会根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作。

#### 2、实现

1、初始化

salve新建，ping master,master发送full synchronized(全量)

master生成RDB，发给slave,slave保存磁盘，然后加载到内存上（全量）

在这个时间段内，master再发送修改的数据给slave(增量)

2、主从复制的断点续传

主从复制过程中，网络连接断掉了，那么可以接着上次复制的地方，继续复制下去，而不是从头开始复制一份

3、无磁盘化复制

master在内存中直接创建rdb，然后发送给slave，不会在自己本地落地磁盘了

repl-diskless-sync yes
repl-diskless-sync-delay，等待一定时长再开始复制，因为要等更多slave重新连接过来

4、过期key处理

slave不会过期key，只会等待master过期key。如果master过期了一个key，或者通过LRU淘汰了一个key，那么会模拟一条del命令发送给slave。

#### 3、核心机制

（1）master和slave都会维护一个offset

master会在自身不断累加offset，slave也会在自身不断累加offset
slave每秒都会上报自己的offset给master，同时master也会保存每个slave的offset

（2）backlog

master node有一个backlog，默认是1MB大小
master node给slave node复制数据时，也会将数据在backlog中同步写一份
backlog主要是用来做全量复制中断候的增量复制的

（3）master run id

info server，可以看到master run id
如果根据host+ip定位master node，是不靠谱的，如果master node重启或者数据出现了变化，那么slave node应该根据不同的run id区分，run id不同就做全量复制

（4）psync

```
PSYNC <runid> <offset>
runid:主服务器ID
offset:从服务器最后接收命令的偏移量
```

master node会根据自身的情况返回响应信息，可能是FULLRESYNC runid offset触发全量复制，可能是CONTINUE触发增量复制

#### 4、高可用高并发 replication主从复制

1、经典三哨兵集群

M1,S1,S2 共三个哨兵，majority=2，即有两个哨兵运行即可进行故障转移

2、问题：异步复制，脑裂（两个master)

解决：

min-slaves-to-write 1
min-slaves-max-lag 10

要求至少有1个slave，数据复制和同步的延迟不能超过10秒

如果说一旦所有的slave，数据复制和同步的延迟都超过了10秒钟，那么这个时候，master就不会再接收任何请求了，做本地缓存

3、哨兵

1、sdown和odown转换机制

sdown是主观宕机，就一个哨兵如果自己觉得一个master宕机了，那么就是主观宕机

odown是客观宕机，如果quorum数量的哨兵都觉得一个master宕机了，那么就是客观宕机

2、哨兵集群的自动发现机制

哨兵互相之间的发现，是通过redis的pub/sub系统实现的，每个哨兵都会往__sentinel__:hello这个channel里发送一个消息，这时候所有其他哨兵都可以消费到这个消息，并感知到其他的哨兵的存在

每隔两秒钟，每个哨兵都会往自己监控的某个master+slaves对应的__sentinel__:hello channel里发送一个消息，内容是自己的host、ip和runid还有对这个master的监控配置

每个哨兵也会去监听自己监控的每个master+slaves对应的__sentinel__:hello channel，然后去感知到同样在监听这个master+slaves的其他哨兵的存在

每个哨兵还会跟其他哨兵交换对master的监控配置，互相进行监控配置的同步

3、slave配置的自动纠正

哨兵会负责自动纠正slave的一些配置，比如slave如果要成为潜在的master候选人，哨兵会确保slave在复制现有master的数据; 如果slave连接到了一个错误的master上，比如故障转移之后，那么哨兵会确保它们连接到正确的master上

4、slave->master选举算法

如果一个master被认为odown了，而且majority哨兵都允许了主备切换，那么某个哨兵就会执行主备切换操作，此时首先要选举一个slave来

会考虑slave的一些信息

（1）跟master断开连接的时长
（2）slave优先级
（3）复制offset
（4）run id

5、quorum和majority，哨兵数量

每次一个哨兵要做主备切换，首先需要quorum数量的哨兵认为odown，然后选举出一个哨兵来做切换，这个哨兵还得得到majority哨兵的授权，才能正式执行切换

#### 5、高可用高并发 集群 cluster

真·集群

哈希一致性算法，每个master管理部分hash-slot(key)

### 四、持久化

备份为做故障恢复，备份到磁盘，磁盘备份到云服务

1、RDB和AOF两种持久化机制的介绍

RDB持久化机制，对redis中的数据执行周期性的持久化

AOF机制对每条写入命令作为日志，以append-only的模式写入一个日志文件中，在redis重启的时候，可以通过回放AOF日志中的写入指令来重新构建整个数据集

2、RDB持久化机制的优缺点

优点：

多个数据文件，每个时刻都代表某一时刻

读写影响小，恢复快

缺点：

数据丢失

如果数据文件特别大，可能会导致对客户端提供的服务暂停数毫秒，或者甚至数秒

3、AOF持久化机制的优缺点

优点

（1）AOF可以更好的保护数据不丢失

（2）AOF日志文件以append-only模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复

缺点

慢，文件大等等

总结：

综合使用AOF和RDB两种持久化机制，用AOF来保证数据不丢失，作为数据恢复的第一选择; 用RDB来做不同程度的冷备，在AOF文件都丢失或损坏不可用的时候，还可以使用RDB来进行快速的数据恢复

### 五、容灾

#### 雪崩

1、硬件故障；
2、程序Bug；
3、缓存击穿（用户大量访问缓存中没有的键值，导致大量请求查询数据库，使数据库压力过大）；
4、用户大量请求；

事前：redis高可用，主从+哨兵，redis cluster，避免全盘崩溃

事中：本地ehcache缓存 + hystrix限流&降级，避免MySQL被打死

事后：redis持久化，快速恢复缓存数据

encache  ,本地缓存，请求先查encache再查redis

hystrix ,熔断降级，控制只有两千个请求到数据库

#### 穿透

被攻击了，写个空值进去

### 六、与数据库一致性

1、延时双删

（1）读的时候，先读缓存，缓存没有的话，那么就读数据库，然后取出数据后放入缓存，同时返回响应

（2）更新的时候，先删除缓存，然后再更新数据库，等待一秒，再删除一次

2、Cache-Aside pattern

1. 失效：应用程序先从cache取数据，没有得到，则从数据库中取数据，成功后，放到缓存中。
2. 命中：应用程序从cache中取数据，取到后返回。
3. 更新：先把数据存到数据库中，成功后，再让缓存失效。

2、针对读高并发

起一个专门线程，读写放在队列里依次执行

nginx里根据路由hash分配服务，确保同一key的请求打在同一服务上。

一般来说，就是如果你的系统不是严格要求缓存+数据库必须一致性的话，缓存可以稍微的跟数据库偶尔有不一致的情况，最好不要做这个方案，读请求和写请求串行化，串到一个内存队列里去，这样就可以保证一定不会出现不一致的情况

串行化之后，就会导致系统的吞吐量会大幅度的降低，用比正常情况下多几倍的机器去支撑线上的一个请求。