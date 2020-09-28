### Redis数据库操作

#### 一、String类型

- （1）**保存**，如果设置的键不存在则为添加，存在则修改

1. 设置键值：<font color="#0099ff">set key value</font>
2. 设置键值级过期时间，以**秒**为单位：<font color="#0099ff">setex key secodes value</font>
3. 设置多个键值：<font color="#0099ff">mset key1 value1 key2 value2</font>
4. 追加值：<font color="#0099ff">append key value</font>

- （2）**获取**，根据键值获取值，==如果不存在此键则返回nil==

1. 获取键值：<font color="#0099ff">get key</font>
2. 获取多个键及多个值：<font color="#0099ff">mget key1 key2 </font>

- （3）**删除**

1. 删除键及对应的值：<font color="#0099ff">del key1 key2</font>
2. 设置键的过期时间，以秒为单位：<font color="#0099ff">expire key seconds</font>

------

#### 二、键命令

- （1）**查找键**，参数支持正则

1. 查找所有键：<font color="#0099ff">key *</font>
2. 查看名称中包含‘a’的键：<font color="#0099ff">keys a*</font>
3. 判断建是否存在，存在返回1，不存在返回0：<font color="#0099ff">exists key</font>
4. 查看键对应的类型：<font color="#0099ff">type key</font>
5. 删除键及对应的值：<font color="#0099ff">del key1 key2</font>
6. 设置键的过期时间，以秒为单位：<font color="#0099ff">expire key seconds</font>
7. 查看有效时间，以秒为单位：<font color="#0099ff">ttl key</font>

---

#### 三、hash类型

<font color="#FF6347">hash</font>用于存储对象、对象的结构为属性、值，值的类型为string

- （1）**增加、修改**

1. 设置单个属性：<font color="#0099ff">hset key field value</font>   （field属性，设置键的属性和值）
2. 设置多个属性：<font color="#0099ff">hmset key1 field1 value1  field2 value2...</font>

例如：<font color="#0099ff">hmset user name Allen age 20</font>

- （2）**获取**

1. 获取指定键所有的属性：<font color="#0099ff">hkeys key</font>
2. 获取指定键一个属性的值：<font color="#0099ff">hget key field</font>
3. 获取指定键的多个属性的值：<font color="#0099ff">hmget key1 field1  field2..</font>

例如：<font color="#0099ff"f>hmget user name age</font>

- （3）**删除**

删除整个hash键及值，使用del命令

1. 删除属性，属性对应的值会被一起删除：<font color="#0099ff">hdel key field1 field2..</font>

例如：<font color="#0099ff">hdel user age</font>

------

#### 四、list类型

列表的元素类型为string，按照插入顺序排序

- （1）**增加**

1. 在左侧插入数据：<font color="#0099ff">lpush key value1 value2..</font>

例如：<font color="#0099ff">lpush a1 0 1 2</font>

1. 在右侧插入数据：<font color="#0099ff">rpush key value1 value2..</font>
2. 在指定元素的前后插入新元素：linsert key before/after 现有元素 新元素

例如：<font color="#0099ff">linsert a1 after 2 3</font>

- （2）**获取**

获取列表里指定范围内的元素，索引从0开始，第一个元素为0，索引为负数，表示从尾部开始，如-1表示最后一个

1. 获取键的所有元素：lrange a1 0 -1

- （3）设置指定索引位置的元素值：lset key index value

索引从左侧开始，第一个元素为0，索引为负数，表示从尾部开始，如-1表示最后一个

1. 修改键为a1的列表中下标为1的元素值为‘z’：lset a1 1 z

- （3）**删除**

1. 删除指定元素

   将列表中前count次出现的值为value的元素移除：<font color="#0099ff">lrem key count value</font>
   count>0：从头往为移除
   count=：移除所有
   count<0：从尾往前移除
   例如：<font color="#0099ff">lrem a1 -2 b  </font>（从左侧开始删除两个b）

---

#### 五、set类型

<font color="#FF6347">无序集合，元素为string类型，元素唯一性，不重复</font>说明：对集合没有修改操作

- （1）**增加**

1. 添加元素：<font color="#0099ff">sadd key member1 member2..</font>

- （2）**获取**

1. 返回所有元素：<font color="#0099ff">smembers key</font>

- （3）**删除**

1. 删除指定元素：<font color="#0099ff">srem key</font>

---

#### 六、zset类型

<font color="#FF6347">sorted set，有序集合，元素为string类型，元素具有唯一性，不重复</font>
<font color="#FF6347">每个元素都会有double类型的score，表示权重，通过权重将元素从小到大排序，没有修改操作</font>

- （1）**增加**

1. 添加：<font color="#0099ff">zadd key score1 member1 score2 member2..</font>

例如：<font color="#0099ff">zadd a2 3 zhangsan 4 lisi</font>

- （2）**获取**

1. 返回指定范围内的元素，start，stop下标索引：zrange key start stop
2. 返回score值在max和min之间的成员：zrangebyscore key max min
3. 返回成员member的score值：zscore key member

例如：<font color="#0099ff">zscore a2 zhangsan</font>

- （3）**删除**

1. 删除指定元素：zrem key member1 member2..

例如：<font color="#0099ff">zrem a2 zhangsan</font>

1. 删除权重范围内的元素：zremrangebyscore key max min

例如：<font color="#0099ff">zremrangebyscore a2 5 6</font>

