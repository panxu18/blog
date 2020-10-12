##### Redis高性能的原因

1. 纯内存，Redis将所有数据放在内存中。
2. 单线程避免了线程切换和竞争产生的消耗。
3. 非阻塞IO。

##### Redis数据类型

1. string
   1. string使用SDS实现。
   2. 通过len变量降低获取字符串长度的开销。
   3. 自动扩容。
   4. 通过空间预分配和惰性空间释放减少内存重分配的次数。
   5. 可以保存文本和二进制数据。
2. list
   1. 元素较少时使用zipList,使用连续的内存，将所有元素以此存储。
   2. 元素较多时使用quickList，将多个zipList通过链表连接。
3. hash
   1. 小数据量使用压缩zipList。
   2. 数据量大了就是用dict。
   3. 使用两个hash表ht0，ht1，ht1在扩容时使用。
   4. 使用渐进式rehash，在对ht0进行访问时同时将其rehash到ht1，将rehash时间分摊到每次访问上。
4. set
5. zset
   1. 使用hash表用来进行根据成员查找分数的操作。
   2. 使用跳表实现分数的排序，范围查找等操作。

##### hash为什么使用zipList

1. 条件：哈希对象保存的所有键值的字符串长度小于64字节j,键值对数量小于512个
2. 作用：ziplist结构少了指针，连续分配，内存碎片少。
3. 存储方式：将同一键值对两个节点点紧挨着，新加入的键值对放在ziplist尾部。

##### zipList结构

1. zlbytes：压缩列表的长度。
2. zltail：最后一个元素的偏移量。
3. zlen：压缩列表中节点的数量。
4. entry：列表中的节点。
   1. prevlen：上一个节点的长度。
   2. encoding：节点的数据类型。
   3. content：节点的数据。
5. zlend：一个特殊值0xff表示压缩列表结尾。

##### Redis事务

1. Redis事务的定义是一次按顺序执行多条命令，具有排他性。
2. 不具有原子性，事务开始执行后，已经执行的命令无法回滚。
3. watch监控在事务提交准备执行事务中的命令前检查。

##### 过期删除策略

1. 定期删除：每隔100ms随机抽取设置了过期时间的key，如果过期就删除。如果过期key的比重超过25%，再次执一次清除策略。
2. 惰性删除：访问key时检查是否过期，如果过期就立即删除。

##### 淘汰策略

当内存不足时需要删除已有数据，有以下6种策略：

1. no-enviction: 不删除数据，不接受新的写请求，继续接受请求。
2. volatile-lru：在设有过期时间的key中使用LRU淘汰。
3. volatile-ttl：在设有过期时间的key中淘汰即将要过期的key。
4. volatile-random：在设有过期之间的key中随机淘汰。
5. allkeys-lru：在所有key中使用LRU淘汰。
6. allkeys-random：在所有key中随机淘汰。

##### 分布式锁简单实现

1. 加锁

   ```java
   public static boolean tryLock(Jedis jedis, String lockKey, String requestId, int expireTime) {
       return "OK".equals(jedis.set(lockKey, requestId, "NX", "PX", expireTime));
   }
   ```

2. 解锁

   ```java
   public static boolean releaseDistributedLock(Jedis jedis, String lockKey, String requestId) {
       Long RELEASE_SUCCESS = 1L;
       String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
       return RELEASE_SUCCESS.equals(jedis.eval(script, Arrays.asList(lockKey), Arrays.asList(requestId)));
   }
   ```

##### 缓冲穿透、击穿、雪崩

1. 缓冲穿透：由于查询数据在数据库不存在，所以缓冲肯定没有，使查询直接到数据库。
2. 缓冲击穿：由于热点key过期，查询同一条数据的请求会同时到数据库查询数据。
3. 缓冲雪崩：大量key过期，导致所有请求同时查询数据库。

##### AOF和RDB的区别和适用场景

1. RDB是全量快照，数据不是最新数据，恢复快。
2. AOF类似日志方式，数据保持最新（可以控制在1s内），文件较大恢复慢。

##### 主从一致性

1. 初次全量同步：
   1. master生成RDB快照，slave根据RDB同步数据。
   2. master会写命令保存在缓冲区，slave可以根据缓冲区进行命令同步。
2. 命令传播
3. 重新复制
   1. 判断slave的偏移量于master的偏移量是否一致，如果一致进行部分同步。
   2. 如果不一致进行重新同步。

##### 主从切换

1. 主从切换是指某个master不可用时，他的其中一个从节点升级为master的操作。
2. 

