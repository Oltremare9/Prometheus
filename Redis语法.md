# Redis

## 命令

### Ltrim

#### 功能

> 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。
>
> 下标 0 表示列表的第一个元素，以 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推。

#### 语法

```
redis 127.0.0.1:6379> LTRIM KEY_NAME START STOP
```

#### 返回值

命令执行成功时，返回 ok 。



### Lrem命令

#### 功能

> 根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素。
>
> - count > 0 : 从表头开始向表尾搜索，移除与 VALUE 相等的元素，数量为 COUNT 。
> - count < 0 : 从表尾开始向表头搜索，移除与 VALUE 相等的元素，数量为 COUNT 的绝对值。
> - count = 0 : 移除表中所有与 VALUE 相等的值。

#### 语法

```
redis 127.0.0.1:6379> LREM KEY_NAME COUNT VALUE
```

#### 返回值

被移除元素的数量。 列表不存在时返回 0 。



### Lrange命令

#### 功能

> 返回列表中指定区间内的元素，区间以偏移量 START 和 END 指定。 其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。 你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此类推。

#### 语法

```
redis 127.0.0.1:6379> LRANGE KEY_NAME START END
```

#### 返回值

> 一个列表，包含指定区间内的元素。

#### 示例

```
redis> RPUSH mylist "one"
(integer) 1
redis> RPUSH mylist "two"
(integer) 2
redis> RPUSH mylist "three"
(integer) 3
redis> LRANGE mylist 0 0
1) "one"
redis> LRANGE mylist -3 2
1) "one"
2) "two"
3) "three"
redis> LRANGE mylist -100 100
1) "one"
2) "two"
3) "three"
redis> LRANGE mylist 5 10
(empty list or set)
redis> 
```



### expire命令

#### 功能

> Redis Expire 命令用于设置 key 的过期时间，key 过期后将不再可用。单位以秒计。

#### 语法

```
redis 127.0.0.1:6379> Expire KEY_NAME TIME_IN_SECONDS
```

#### 返回值

> 设置成功返回 1 。 当 key 不存在或者不能为 key 设置过期时间时返回 0 。

#### 示例

```
redis 127.0.0.1:6379> EXPIRE runooobkey 60
```



### TTL命令

#### 功能

> 以秒为单位返回 key 的剩余过期时间。

#### 语法

```
redis 127.0.0.1:6379> TTL KEY_NAME
```

#### 返回值

> 当 key 不存在时，返回 -2 。 当 key 存在但没有设置剩余生存时间时，返回 -1 。 否则，以秒为单位，返回 key 的剩余生存时间。

#### 示例

```
# 不存在的 key

redis> FLUSHDB
OK

redis> TTL key
(integer) -2


# key 存在，但没有设置剩余生存时间

redis> SET key value
OK

redis> TTL key
(integer) -1


# 有剩余生存时间的 key

redis> EXPIRE key 10086
(integer) 1

redis> TTL key
(integer) 10084
```



### Hget命令

#### 功能

> 用于返回**哈希表**中指定字段的值。

#### 语法

```
redis 127.0.0.1:6379> HGET KEY_NAME FIELD_NAME 
```

#### 返回值

> 返回给定字段的值。如果给定的字段或 key 不存在时，返回 nil 。

#### 示例

```
# 字段存在

redis> HSET site redis redis.com
(integer) 1

redis> HGET site redis
"redis.com"


# 字段不存在

redis> HGET site mysql
(nil)
```



### Get命令

#### 功能

> 用于获取指定 key 的值。如果 key 不存在，返回 nil 。如果key 储存的值不是字符串类型，返回一个错误。

#### 语法

```
redis 127.0.0.1:6379> GET KEY_NAME
```

#### 返回值

> 返回 key 的值，如果 key **不存在**时，返回 nil。 如果 key **不是字符串**类型，那么返回一个错误。

#### 示例

```
# 对不存在的 key 或字符串类型 key 进行 GET

redis> GET db
(nil)

redis> SET db redis
OK

redis> GET db
"redis"


# 对不是字符串类型的 key 进行 GET

redis> DEL db
(integer) 1

redis> LPUSH db redis mongodb mysql
(integer) 3

redis> GET db
(error) ERR Operation against a key holding the wrong kind of value
```



### setnx命令

#### 功能

> Redis Setnx（**SET** if **N**ot e**X**ists） 命令在指定的 key 不存在时，为 key 设置指定的值。

#### 语法

```
redis 127.0.0.1:6379> SETNX KEY_NAME VALUE
```

#### 返回值

> 设置成功，返回 1 。 设置失败，返回 0 。

#### 示例

```
redis> EXISTS job                # job 不存在
(integer) 0

redis> SETNX job "programmer"    # job 设置成功
(integer) 1

redis> SETNX job "code-farmer"   # 尝试覆盖 job ，失败
(integer) 0

redis> GET job                   # 没有被覆盖
"programmer"
```



### expire命令

#### 功能

> 用于设置 key 的过期时间，key 过期后将不再可用。单位以秒计。

#### 语法

```
redis 127.0.0.1:6379> Expire KEY_NAME TIME_IN_SECONDS
```

#### 返回值

> 设置成功返回 1 。 当 key 不存在或者不能为 key 设置过期时间时(比如在低于 2.1.3 版本的 Redis 中你尝试更新 key 的过期时间)返回 0 。



### incr命令

#### 功能

> 将 key 中储存的数字值增一。
>
> 如果 key 不存在，那么 key 的值会先被初始化为 0 ，然后再执行 INCR 操作。
>
> 如果值包含错误的类型，或字符串类型的值不能表示为数字，那么返回一个错误。
>
> 本操作的值限制在 64 位(bit)有符号数字表示之内。

#### 语法

```
redis 127.0.0.1:6379> INCR KEY_NAME 
```

#### 返回值

> 执行 INCR 命令之后 key 的值。



### Rpush命令

#### 功能

> 将一个或多个值插入到列表的尾部(最右边)。
>
> 如果列表不存在，一个空列表会被创建并执行 RPUSH 操作。 当列表存在但不是列表类型时，返回一个错误。

#### 语法

```
redis 127.0.0.1:6379> RPUSH KEY_NAME VALUE1..VALUEN
```

#### 返回值

> 执行 RPUSH 操作后，列表的长度。

#### 示例

```
redis 127.0.0.1:6379> RPUSH mylist "hello"
(integer) 1
redis 127.0.0.1:6379> RPUSH mylist "foo"
(integer) 2
redis 127.0.0.1:6379> RPUSH mylist "bar"
(integer) 3
redis 127.0.0.1:6379> LRANGE mylist 0 -1
1) "hello"
2) "foo"
3) "bar"
```



### Lpop命令

#### 功能

> 用于移除并返回**列表**的第一个元素。

#### 语法

```
redis 127.0.0.1:6379> Lpop KEY_NAME 
```

#### 返回值

> 列表的第一个元素。 当列表 key 不存在时，返回 nil 。

#### 示例

```
redis 127.0.0.1:6379> RPUSH list1 "foo"
(integer) 1
redis 127.0.0.1:6379> RPUSH list1 "bar"
(integer) 2
redis 127.0.0.1:6379> LPOP list1
"foo"
```



### hDel命令

#### 功能

> 用于删除哈希表 key 中的一个或多个指定字段，不存在的字段将被忽略。

#### 语法

```
redis 127.0.0.1:6379> HDEL KEY_NAME FIELD1.. FIELDN 
```

#### 返回值

> 被成功删除字段的数量，不包括被忽略的字段。

#### 实例

```
redis 127.0.0.1:6379> HSET myhash field1 "foo"
(integer) 1
redis 127.0.0.1:6379> HDEL myhash field1
(integer) 1
redis 127.0.0.1:6379> HDEL myhash field2
(integer) 0
```

