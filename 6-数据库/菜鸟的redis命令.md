### key相关

#### keys *
查看所有键

#### exists <key>
检测key是否存在

#### del <key1> <key2> ...
删除key

#### expire <key> <seconds>
设置过期时间



### string 类型

#### set <key> <value>
新增

#### get <key>
获取key对应的value

#### mset <key1> <value1> <key2> <value2> ...
批量新增

#### mget <key1> <key2> ...
批量获取

#### append <key> <append_value>
追加字符串

#### strlen <key>
key对应字符串长度

#### incr <key>
将key中存储的数据值增1

#### incrby <key> <increment>
将key所存储的值加上给定的增量值（increment）

#### getset <key> <value>
对key的值设为value，返回key的旧值



### hash类型

#### hset <key> <field> <value>
新增

#### hget <key> <field>
获取value

#### hmget <key> <field1> <field2> ...
批量获取

#### hlen <key>
统计field个数

#### hkeys <key>
列出所有的field

#### hvals <key>
列出所有的value

#### hgetall <key>
列出所有的key和value



### List类型

#### lpush <key> <value>
左新增

#### rpush <key> <value>
右新增

#### lpop <key>
左删除并返回

#### rpop <key>
有删除并返回

#### llen <key>
返回list长度

#### lset <key> <index> <value>
按index设置value

#### lindex <key> <index>
按index返回value

#### lrange <key> <start> <stop>
按index范围返回list

#### lrange <key> 0 -1
返回全部的list

#### lrem <key> <count> <value>
删除count个等于value的值，count > 0正向搜索，count < 0逆向搜索，count = 0全部删除



### Set类型

#### sadd <key> <member1> <member2> ...
新增

#### scard <key>
统计成员数

#### srem <key> <member1> <member2> ...
删除成员

#### smember <key>
显示成员



### Sorted Set类型

#### zadd <key> <score1> <member1> <score2> <member2> ...
新增

#### zcard <key>
统计成员个数

#### zrem <key> <member1> <member2> ...
删除成员

#### zrange <key> <start> <stop>
按index显示成员

#### zrange <key> 0 -1
显示全部成员

#### zrangebyscore <key> <min> <max>
按分数范围显示成员

