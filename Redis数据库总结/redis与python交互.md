### Python标准库系列之Redis模块--Redis常用命令

参考网址：[link]https://www.cnblogs.com/qtxdy/p/7723411.html

官方文档：[link]<https://redis-py.readthedocs.io/en/latest/#indices-and-tables>

####1）  连接操作命令

- quit：关闭连接（connection）

- auth：简单密码认证

- help cmd： 查看cmd帮助，例如：help quit


 

####2）持久化

- save：将数据同步保存到磁盘
- bgsave：将数据异步保存到磁盘

- lastsave：返回上次成功将数据保存到磁盘的Unix时戳

- shundown：将数据同步保存到磁盘，然后关闭服务


####3）远程服务控制

- info：提供服务器的信息和统计
- monitor：实时转储收到的请求

- slaveof：改变复制策略设置

- config：在运行时配置Redis服务器


####4）对value操作的命令

1. exists(key)：确认一个key是否存在
2. del(key)：删除一个key

3. type(key)：返回值的类型

4. keys(pattern)：返回满足给定pattern的所有key

5. randomkey：随机返回key空间的一个

6. keyrename(oldname, newname)：重命名key

7. dbsize：返回当前数据库中key的数目

8. expire：设定一个key的活动时间（s）

9. ttl：获得一个key的活动时间

10. select(index)：按索引查询

11. move(key, dbindex)：移动当前数据库中的key到dbindex数据库

12. flushdb：删除当前选择数据库中的所有key

13. flushall：删除所有数据库中的所有key


####5）String

1. set(key, value)：给数据库中名称为key的string赋予值value
2. get(key)：返回数据库中名称为key的string的value

3. getset(key, value)：给名称为key的string赋予上一次的value

4. mget(key1, key2,…, key N)：返回库中多个string的value

5. setnx(key, value)：添加string，名称为key，值为value

6. setex(key, time, value)：向库中添加string，设定过期时间time

7. mset(key N, value N)：批量设置多个string的值

8. msetnx(key N, value N)：如果所有名称为key i的string都不存在

9. incr(key)：名称为key的string增1操作

10. incrby(key, integer)：名称为key的string增加integer

11. decr(key)：名称为key的string减1操作

12. decrby(key, integer)：名称为key的string减少integer

13. append(key, value)：名称为key的string的值附加value

14. substr(key, start, end)：返回名称为key的string的value的子串


####6）List 

<font color="00FF7F">Redis的list类型其实就是一个每个子元素都是string类型的双向链表，链表的最大长度是2^32。list既可以用做栈，也可以用做队列。</font>

1. rpush(key, value)：在名称为key的list尾添加一个值为value的元素
2. lpush(key, value)：在名称为key的list头添加一个值为value的 元素
3. llen(key)：返回名称为key的list的长度
4. lrange(key, start, end)：返回名称为key的list中start至end之间的元素
5. ltrim(key, start, end)：截取名称为key的list
6. lindex(key, index)：返回名称为key的list中index位置的元素
7. lset(key, index, value)：给名称为key的list中index位置的元素赋值
8. lrem(key, count, value)：删除count个key的list中值为value的元素
9. lpop(key)：返回并删除名称为key的list中的首元素
10. rpop(key)：返回并删除名称为key的list中的尾元素
11. blpop(key1, key2,… key N, timeout)：lpop命令的block版本。
12. brpop(key1, key2,… key N, timeout)：rpop的block版本。
13. rpoplpush(srckey, dstkey)：返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部

####7）Set

<font color="00FF7F">特点：无序性、确定性、唯一性

1. sadd(key, member)：向名称为key的set中添加元素member
2. srem(key, member) ：删除名称为key的set中的元素member
3. spop(key) ：随机返回并删除名称为key的set中一个元素
4. smove(srckey, dstkey, member) ：移到集合元素
5. scard(key) ：返回名称为key的set的基数
6. sismember(key, member) ：member是否是名称为key的set的元素
7. sinter(key1, key2,…key N) ：求交集
8. sinterstore(dstkey, (keys)) ：求交集并将交集保存到dstkey的集合
9. sunion(key1, (keys)) ：求并集
10. sunionstore(dstkey, (keys)) ：求并集并将并集保存到dstkey的集合
11. sdiff(key1, (keys)) ：求差集
12. sdiffstore(dstkey, (keys)) ：求差集并将差集保存到dstkey的集合
13. smembers(key) ：返回名称为key的set的所有元素
14. srandmember(key) ：随机返回名称为key的set的一个元素

####8）Hash

<font color="00FF7F">Redis hash 是一个string类型的field和value的映射表，它的添加、删除操作都是O(1)（平均）。hash特别适用于存储对象，将一个对象存储在hash类型中会占用更少的内存，并且可以方便的存取整个对象。</font>

1. hset(key, field, value)：向名称为key的hash中添加元素field
2. hget(key, field)：返回名称为key的hash中field对应的value
3. hmget(key, (fields))：返回名称为key的hash中field i对应的value
4. hmset(key, (fields))：向名称为key的hash中添加元素field 
5. hincrby(key, field, integer)：将名称为key的hash中field的value增加integer
6. hexists(key, field)：名称为key的hash中是否存在键为field的域
7. hdel(key, field)：删除名称为key的hash中键为field的域
8. hlen(key)：返回名称为key的hash中元素个数
9. hkeys(key)：返回名称为key的hash中所有键
10. hvals(key)：返回名称为key的hash中所有键对应的value
11. hgetall(key)：返回名称为key的hash中所有的键（field）及其对应的value

####9）Zest

　<font color="00FF7F">概念：它是在set的基础上增加了一个顺序属性，这一属性在添加修改元素的时候可以指定，每次指定后，zset会自动按新的值调整顺序。可以理解为有两列的mysql表，一列存储value，一列存储顺序，操作中key理解为zset的名字。</font>

1. zadd key score1 value1：添加元素
2. zrange key start stop [withscore]：把集合排序后,返回名次[start,stop]的元素  默认是升续排列  withscores 是把score也打印出来
3. zrank key member：查询member的排名（升序0名开始）
4. zrevrank key member：查询member排名（降序 0名开始）
5. zrem key value1 value2：删除集合中的元素
6. zremrangebyrank key start end：按排名删除元素，删除名次在[start, end]之间的
7. zcard key：返回集合元素的个数
8. zcount key min max：返回[min, max]区间内元素数量

参考链接：[link]<https://www.jb51.net/article/61793.htm>