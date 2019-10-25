# 一、字符串

## SET key value [EX seconds] [PX milliseconds] [NX|XX]

将字符串值 value 关联到 key 。
如果 key 已经持有其他值， SET 就覆写旧值， 无视类型。
当 SET 命令对一个带有生存时间（TTL）的键进行设置之后， 该键原有的 TTL 将被清除。

可选参数:

- EX seconds ： 将键的过期时间设置为 seconds 秒。 执行 SET key value EX seconds 的效果等同于执行 SETEX key seconds value 。
- PX milliseconds ： 将键的过期时间设置为 milliseconds 毫秒。 执行 SET key value PX milliseconds 的效果等同于执行 PSETEX key milliseconds value 。
- NX ： 只在键不存在时， 才对键进行设置操作。 执行 SET key value NX 的效果等同于执行 SETNX key value 。
- XX ： 只在键已经存在时， 才对键进行设置操作。

```
redis> SET key "value"
OK

redis> GET key
"value"
```

使用 EX 选项：

```
redis> SET key-with-expire-time "hello" EX 10086
OK

redis> GET key-with-expire-time
"hello"

redis> TTL key-with-expire-time
(integer) 10069
```

使用 NX 选项：

```
redis> SET not-exists-key "value" NX
OK      # 键不存在，设置成功

redis> GET not-exists-key
"value"

redis> SET not-exists-key "new-value" NX
(nil)   # 键已经存在，设置失败

redis> GEt not-exists-key
"value" # 维持原值不变
```


## GETSET key value

将键 key 的值设为 value ， 并返回键 key 在被设置之前的旧值。

```
redis> GETSET db mongodb    # 没有旧值，返回 nil
(nil)

redis> GET db
"mongodb"

redis> GETSET db redis      # 返回旧值 mongodb
"mongodb"

redis> GET db
"redis"
```

## STRLEN key

STRLEN 命令返回字符串值的长度。

当键 key 不存在时， 命令返回 0 。

当 key 储存的不是字符串值时， 返回一个错误。

```
redis> SET mykey "Hello world"
OK

redis> STRLEN mykey
(integer) 11

redis> STRLEN nonexisting
(integer) 0
```

## APPEND key value

如果键 `key` 已经存在并且它的值是一个字符串， `APPEND` 命令将把 `value` 追加到键 `key` 现有值的末尾。

如果 `key` 不存在， `APPEND` 就简单地将键 `key` 的值设为 `value` ， 就像执行 `SET key value` 一样。

返回值：:

追加 `value` 之后， 键 `key` 的值的长度。

```
redis> EXISTS myphone               # myphone 不存在
(integer) 0

redis> APPEND myphone "nokia"       # 对不存在的 key 进行 APPEND ，等同于 SET myphone "nokia"
(integer) 5                         # 字符长度

redis> APPEND myphone " - 1110"     # 长度从 5 个字符增加到 12 个字符
(integer) 12

redis> GET myphone
"nokia - 1110"
```

## INCR key

为键 `key` 储存的数字值加上一。

如果键 `key` 不存在， 那么它的值会先被初始化为 `0` ， 然后再执行 `INCR` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `INCR` 命令将返回一个错误。

> ! NOTE <br>
`INCR` 命令是一个针对字符串的操作。 因为 `Redis` 并没有专用的整数类型， 所以键 `key` 储存的值在执行 `INCR` 命令时会被解释为十进制 `64` 位有符号整数。

返回值：

`INCR` 命令会返回键 `key` 在执行加一操作之后的值。

```
redis> SET page_view 20
OK

redis> INCR page_view
(integer) 21

redis> GET page_view    # 数字值在 Redis 中以字符串的形式保存
"21"
```

## INCRBY key increment

为键 `key` 储存的数字值加上增量 `increment` 。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `INCRBY` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `INCRBY` 命令将返回一个错误。

返回值：

在加上增量 `increment` 之后， 键 `key` 当前的值。

键存在，并且值为数字：

```
redis> SET rank 50
OK

redis> INCRBY rank 20
(integer) 70

redis> GET rank
"70"
```
键不存在：

```
redis> EXISTS counter
(integer) 0

redis> INCRBY counter 30
(integer) 30

redis> GET counter
"30"
```
键存在，但值无法被解释为数字：
```
redis> SET book "long long ago..."
OK

redis> INCRBY book 200
(error) ERR value is not an integer or out of range
```

## INCRBYFLOAT key increment

为键 `key` 储存的值加上浮点数增量 `increment` 。

无论是键 `key` 的值还是增量 `increment` ， 都可以使用像 `2.0e7` 、 `3e5` 、 `90e-2` 那样的指数符号(exponential notation)来表示， 但是， 执行 `INCRBYFLOAT` 命令之后的值总是以同样的形式储存， 也即是， 它们总是由一个数字， 一个（可选的）小数点和一个任意长度的小数部分组成（比如 `3.14` 、 `69.768` ，诸如此类)， 小数部分尾随的 `0` 会被移除， 如果可能的话， 命令还会将浮点数转换为整数（比如 `3.0` 会被保存成 `3` ）。

此外， 无论加法计算所得的浮点数的实际精度有多长， `INCRBYFLOAT` 命令的计算结果最多只保留小数点的后十七位。

```
redis> GET decimal
"3.0"

redis> INCRBYFLOAT decimal 2.56
"5.56"

redis> GET decimal
"5.56"
```

## DECR key

为键 `key` 储存的数字值减去一。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECR` 操作。

如果键 `key` 储存的值不能被解释为数字， 那么 `DECR` 命令将返回一个错误。

本操作的值限制在 `64` 位(bit)有符号数字表示之内。

关于递增(increment) / 递减(decrement)操作的更多信息， 请参见 INCR 命令的文档。

返回值：

`DECR` 命令会返回键 `key` 在执行减一操作之后的值。

```
redis> SET failure_times 10
OK

redis> DECR failure_times
(integer) 9

redis> EXISTS count
(integer) 0

redis> DECR count
(integer) -1
```

## DECRBY key decrement

将键 `key` 储存的整数值减去减量 `decrement` 。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECRBY` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `DECRBY` 命令将返回一个错误。

本操作的值限制在 `64` 位(bit)有符号数字表示之内。

返回值：

`DECRBY` 命令会返回键在执行减法操作之后的值。

```
redis> SET count 100
OK

redis> DECRBY count 20
(integer) 80

redis> EXISTS pages
(integer) 0

redis> DECRBY pages 10
(integer) -10
```

## MSET key value [key value …]

同时为多个键设置值。

如果某个给定键已经存在， 那么 `MSET` 将使用新值去覆盖旧值， 如果这不是你所希望的效果， 请考虑使用 `MSETNX` 命令， 这个命令只会在所有给定键都不存在的情况下进行设置。

`MSET` 是一个原子性(atomic)操作， 所有给定键都会在同一时间内被设置， 不会出现某些键被设置了但是另一些键没有被设置的情况。

返回值：

`MSET` 命令总是返回 `OK` 。

```
redis> MSET date "2012.3.30" time "11:00 a.m." weather "sunny"
OK

redis> MGET date time weather
1) "2012.3.30"
2) "11:00 a.m."
3) "sunny"
```

# 二、哈希表

## HSET hash field value

将哈希表 `hash` 中域 `field` 的值设置为 `value` 。

如果给定的哈希表并不存在， 那么一个新的哈希表将被创建并执行 `HSET` 操作。

如果域 `field` 已经存在于哈希表中， 那么它的旧值将被新值 `value` 覆盖。

返回值：

当 `HSET` 命令在哈希表中新创建 `field` 域并成功为它设置值时， 命令返回 `1` ； 如果域 `field` 已经存在于哈希表， 并且 `HSET` 命令成功使用新值覆盖了它的旧值， 那么命令返回 `0` 。

```
redis> HSET website google "www.g.cn"
(integer) 1

redis> HGET website google
"www.g.cn"

redis> HSET website google "www.google.com"
(integer) 0

redis> HGET website google
"www.google.com"
```

## HSETNX hash field value

当且仅当域 `field` 尚未存在于哈希表的情况下， 将它的值设置为 `value` 。

如果给定域已经存在于哈希表当中， 那么命令将放弃执行设置操作。

如果哈希表 `hash` 不存在， 那么一个新的哈希表将被创建并执行 `HSETNX` 命令。


返回值：

`HSETNX` 命令在设置成功时返回 `1` ， 在给定域已经存在而放弃执行设置操作时返回 `0` 。

```
redis> HSETNX database key-value-store Redis
(integer) 1

redis> HGET database key-value-store
"Redis"

redis> HSETNX database key-value-store Riak
(integer) 0

redis> HGET database key-value-store
"Redis"
```

## HDEL key field [field …]

删除哈希表 `key` 中的一个或多个指定域，不存在的域将被忽略。

返回值:

被成功移除的域的数量，不包括被忽略的域。

```
# 测试数据

redis> HGETALL abbr
1) "a"
2) "apple"
3) "b"
4) "banana"
5) "c"
6) "cat"
7) "d"
8) "dog"


# 删除单个域

redis> HDEL abbr a
(integer) 1


# 删除不存在的域

redis> HDEL abbr not-exists-field
(integer) 0


# 删除多个域

redis> HDEL abbr b c
(integer) 2

redis> HGETALL abbr
1) "d"
2) "dog"
```

## HLEN key

返回哈希表 `key` 中域的数量。

返回值：

哈希表中域的数量。
当 `key` 不存在时，返回 `0` 

```
redis> HSET db redis redis.com
(integer) 1

redis> HSET db mysql mysql.com
(integer) 1

redis> HLEN db
(integer) 2

redis> HSET db mongodb mongodb.org
(integer) 1

redis> HLEN db
(integer) 3
```

## HSTRLEN key field

返回哈希表 `key` 中， 与给定域 `field` 相关联的值的字符串长度（string length）。

如果给定的键或者域不存在， 那么命令返回 `0` 

```
redis> HMSET myhash f1 "HelloWorld" f2 "99" f3 "-256"
OK

redis> HSTRLEN myhash f1
(integer) 10

redis> HSTRLEN myhash f2
(integer) 2

redis> HSTRLEN myhash f3
(integer) 4
```

## HINCRBY key field increment

为哈希表 key 中的域 field 的值加上增量 increment 。

增量也可以为负数，相当于对给定域进行减法操作。

如果 key 不存在，一个新的哈希表被创建并执行 HINCRBY 命令。

如果域 field 不存在，那么在执行命令前，域的值被初始化为 0 。

对一个储存字符串值的域 field 执行 HINCRBY 命令将造成一个错误。

本操作的值被限制在 64 位(bit)有符号数字表示之内。

返回值：

执行 HINCRBY 命令之后，哈希表 key 中域 field 的值。

```
# increment 为正数

redis> HEXISTS counter page_view    # 对空域进行设置
(integer) 0

redis> HINCRBY counter page_view 200
(integer) 200

redis> HGET counter page_view
"200"


# increment 为负数

redis> HGET counter page_view
"200"

redis> HINCRBY counter page_view -50
(integer) 150

redis> HGET counter page_view
"150"


# 尝试对字符串值的域执行HINCRBY命令

redis> HSET myhash string hello,world       # 设定一个字符串值
(integer) 1

redis> HGET myhash string
"hello,world"

redis> HINCRBY myhash string 1              # 命令执行失败，错误。
(error) ERR hash value is not an integer

redis> HGET myhash string                   # 原值不变
"hello,world"
```

## HINCRBYFLOAT key field increment

为哈希表 `key` 中的域 `field` 加上浮点数增量 `increment` 。

如果哈希表中没有域 `field` ，那么 `HINCRBYFLOAT` 会先将域 `field` 的值设为 `0` ，然后再执行加法操作。

如果键 `key` 不存在，那么 `HINCRBYFLOAT` 会先创建一个哈希表，再创建域 `field` ，最后再执行加法操作。

当以下任意一个条件发生时，返回一个错误：

- 域 `field` 的值不是字符串类型(因为 `redis` 中的数字和浮点数都以字符串的形式保存，所以它们都属于字符串类型）
- 域 `field` 当前的值或给定的增量 `increment` 不能解释(parse)为双精度浮点数(double precision floating point number)

返回值：

执行加法操作之后 field 域的值。

```
# 值和增量都是普通小数

redis> HSET mykey field 10.50
(integer) 1
redis> HINCRBYFLOAT mykey field 0.1
"10.6"


# 值和增量都是指数符号

redis> HSET mykey field 5.0e3
(integer) 0
redis> HINCRBYFLOAT mykey field 2.0e2
"5200"


# 对不存在的键执行 HINCRBYFLOAT

redis> EXISTS price
(integer) 0
redis> HINCRBYFLOAT price milk 3.5
"3.5"
redis> HGETALL price
1) "milk"
2) "3.5"


# 对不存在的域进行 HINCRBYFLOAT

redis> HGETALL price
1) "milk"
2) "3.5"
redis> HINCRBYFLOAT price coffee 4.5   # 新增 coffee 域
"4.5"
redis> HGETALL price
1) "milk"
2) "3.5"
3) "coffee"
4) "4.5"
```

## HKEYS key

返回哈希表 `key` 中的所有域。

返回值：

一个包含哈希表中所有域的表。
当 `key` 不存在时，返回一个空表。

```
# 哈希表非空

redis> HMSET website google www.google.com yahoo www.yahoo.com
OK

redis> HKEYS website
1) "google"
2) "yahoo"


# 空哈希表/key不存在

redis> EXISTS fake_key
(integer) 0

redis> HKEYS fake_key
(empty list or set)
```

## HVALS key

返回哈希表 `key` 中所有域的值。

返回值：

一个包含哈希表中所有值的表。
当 key 不存在时，返回一个空表。

```
# 非空哈希表

redis> HMSET website google www.google.com yahoo www.yahoo.com
OK

redis> HVALS website
1) "www.google.com"
2) "www.yahoo.com"


# 空哈希表/不存在的key

redis> EXISTS not_exists
(integer) 0

redis> HVALS not_exists
(empty list or set)
```

# 三、列表

## LPUSH key value [value …]

将一个或多个值 `value` 插入到列表 `key` 的表头

如果有多个 `value` 值，那么各个 `value` 值按从左到右的顺序依次插入到表头： 比如说，对空列表 `mylist` 执行命令 `LPUSH mylist a b c` ，列表的值将是 `c b a` ，这等同于原子性地执行 `LPUSH mylist a` 、 `LPUSH mylist b` 和 `LPUSH mylist c` 三个命令。

如果 `key` 不存在，一个空列表会被创建并执行 `LPUSH` 操作。

当 `key` 存在但不是列表类型时，返回一个错误。

返回值

执行 `LPUSH` 命令后，列表的长度。

```
# 加入单个元素

redis> LPUSH languages python
(integer) 1


# 加入重复元素

redis> LPUSH languages python
(integer) 2

redis> LRANGE languages 0 -1     # 列表允许重复元素
1) "python"
2) "python"


# 加入多个元素

redis> LPUSH mylist a b c
(integer) 3

redis> LRANGE mylist 0 -1
1) "c"
2) "b"
3) "a"
```

## LPUSHX key value

将值 `value` 插入到列表 `key` 的表头，当且仅当 `key` 存在并且是一个列表。

和 `LPUSH key value [value …]` 命令相反，当 `key` 不存在时， `LPUSHX` 命令什么也不做。

返回值:

`LPUSHX` 命令执行之后，表的长度。

```
# 对空列表执行 LPUSHX

redis> LLEN greet                       # greet 是一个空列表
(integer) 0

redis> LPUSHX greet "hello"             # 尝试 LPUSHX，失败，因为列表为空
(integer) 0


# 对非空列表执行 LPUSHX

redis> LPUSH greet "hello"              # 先用 LPUSH 创建一个有一个元素的列表
(integer) 1

redis> LPUSHX greet "good morning"      # 这次 LPUSHX 执行成功
(integer) 2

redis> LRANGE greet 0 -1
1) "good morning"
2) "hello"
```

## RPUSH key value [value …]

将一个或多个值 `value` 插入到列表 `key` 的表尾(最右边)。

如果有多个 `value` 值，那么各个 `value` 值按从左到右的顺序依次插入到表尾：比如对一个空列表 `mylist` 执行 `RPUSH mylist a b c` ，得出的结果列表为 `a b c` ，等同于执行命令 `RPUSH mylist a` 、 `RPUSH mylist b` 、 `RPUSH mylist c` 。

如果 `key` 不存在，一个空列表会被创建并执行 `RPUSH` 操作。

当 `key` 存在但不是列表类型时，返回一个错误。

返回值:

执行 `RPUSH` 操作后，表的长度。

```
# 添加单个元素

redis> RPUSH languages c
(integer) 1


# 添加重复元素

redis> RPUSH languages c
(integer) 2

redis> LRANGE languages 0 -1 # 列表允许重复元素
1) "c"
2) "c"


# 添加多个元素

redis> RPUSH mylist a b c
(integer) 3

redis> LRANGE mylist 0 -1
1) "a"
2) "b"
3) "c"
```

## LPOP key

返回值:

列表的头元素。 当 `key` 不存在时，返回 `nil` 。

```
redis> LLEN course
(integer) 0

redis> RPUSH course algorithm001
(integer) 1

redis> RPUSH course c++101
(integer) 2

redis> LPOP course  # 移除头元素
"algorithm001"
```

## RPOP key

移除并返回列表 `key` 的尾元素。

返回值:

列表的尾元素。 当 `key` 不存在时，返回 `nil` 。

```
redis> RPUSH mylist "one"
(integer) 1

redis> RPUSH mylist "two"
(integer) 2

redis> RPUSH mylist "three"
(integer) 3

redis> RPOP mylist           # 返回被弹出的元素
"three"

redis> LRANGE mylist 0 -1    # 列表剩下的元素
1) "one"
2) "two"
```

## LREM key count value

根据参数 `count` 的值，移除列表中与参数 `value` 相等的元素。

`count` 的值可以是以下几种：

- `count > 0` : 从表头开始向表尾搜索，移除与 `value` 相等的元素，数量为 `count` 。
- `count < 0` : 从表尾开始向表头搜索，移除与 `value` 相等的元素，数量为 `count` 的绝对值。
- `count = 0` : 移除表中所有与 `value` 相等的值。

返回值

被移除元素的数量。 因为不存在的 `key` 被视作空表(empty list)，所以当 `key` 不存在时， `LREM` 命令总是返回 `0` 。

```
# 先创建一个表，内容排列是
# morning hello morning helllo morning

redis> LPUSH greet "morning"
(integer) 1
redis> LPUSH greet "hello"
(integer) 2
redis> LPUSH greet "morning"
(integer) 3
redis> LPUSH greet "hello"
(integer) 4
redis> LPUSH greet "morning"
(integer) 5

redis> LRANGE greet 0 4         # 查看所有元素
1) "morning"
2) "hello"
3) "morning"
4) "hello"
5) "morning"

redis> LREM greet 2 morning     # 移除从表头到表尾，最先发现的两个 morning
(integer) 2                     # 两个元素被移除

redis> LLEN greet               # 还剩 3 个元素
(integer) 3

redis> LRANGE greet 0 2
1) "hello"
2) "hello"
3) "morning"

redis> LREM greet -1 morning    # 移除从表尾到表头，第一个 morning
(integer) 1

redis> LLEN greet               # 剩下两个元素
(integer) 2

redis> LRANGE greet 0 1
1) "hello"
2) "hello"

redis> LREM greet 0 hello      # 移除表中所有 hello
(integer) 2                    # 两个 hello 被移除

redis> LLEN greet
(integer) 0
```


## LLEN key

返回列表 `key` 的长度。

如果 `key` 不存在，则 `key` 被解释为一个空列表，返回 `0` .

如果 `key` 不是列表类型，返回一个错误。

```
# 空列表

redis> LLEN job
(integer) 0


# 非空列表

redis> LPUSH job "cook food"
(integer) 1

redis> LPUSH job "have lunch"
(integer) 2

redis> LLEN job
(integer) 2
```

## LINDEX key index

返回列表 `key` 中，下标为 `index` 的元素。

下标(index)参数 `start` 和 `stop` 都以 `0` 为底，也就是说，以 `0` 表示列表的第一个元素，以 `1` 表示列表的第二个元素，以此类推。

你也可以使用负数下标，以 -`1` 表示列表的最后一个元素， `-2` 表示列表的倒数第二个元素，以此类推。

如果 `key` 不是列表类型，返回一个错误。

返回值:

列表中下标为 `index` 的元素。 如果 `index` 参数的值不在列表的区间范围内(out of range)，返回 `nil` 。

```
redis> LPUSH mylist "World"
(integer) 1

redis> LPUSH mylist "Hello"
(integer) 2

redis> LINDEX mylist 0
"Hello"

redis> LINDEX mylist -1
"World"

redis> LINDEX mylist 3        # index不在 mylist 的区间范围内
(nil)
```


## LINSERT key BEFORE|AFTER pivot value

将值 `value` 插入到列表 `key` 当中，位于值 `pivot` 之前或之后。

当 `pivot` 不存在于列表 `key` 时，不执行任何操作。

当 `key` 不存在时， `key` 被视为空列表，不执行任何操作。

如果 `key` 不是列表类型，返回一个错误

返回值

如果命令执行成功，返回插入操作完成之后，列表的长度。 如果没有找到 `pivot` ，返回 `-1` 。 如果 `key` 不存在或为空列表，返回 `0` 。

```
redis> RPUSH mylist "Hello"
(integer) 1

redis> RPUSH mylist "World"
(integer) 2

redis> LINSERT mylist BEFORE "World" "There"
(integer) 3

redis> LRANGE mylist 0 -1
1) "Hello"
2) "There"
3) "World"


# 对一个非空列表插入，查找一个不存在的 pivot

redis> LINSERT mylist BEFORE "go" "let's"
(integer) -1                                    # 失败


# 对一个空列表执行 LINSERT 命令

redis> EXISTS fake_list
(integer) 0

redis> LINSERT fake_list BEFORE "nono" "gogogog"
(integer) 0                             
```

## LSET key index value

将列表 `key` 下标为 `index` 的元素的值设置为 `value` 。

当 `index` 参数超出范围，或对一个空列表( `key` 不存在)进行 `LSET` 时，返回一个错误。

```
# 对空列表(key 不存在)进行 LSET

redis> EXISTS list
(integer) 0

redis> LSET list 0 item
(error) ERR no such key


# 对非空列表进行 LSET

redis> LPUSH job "cook food"
(integer) 1

redis> LRANGE job 0 0
1) "cook food"

redis> LSET job 0 "play game"
OK

redis> LRANGE job  0 0
1) "play game"


# index 超出范围

redis> LLEN list                    # 列表长度为 1
(integer) 1

redis> LSET list 3 'out of range'
(error) ERR index out of range
```

## LRANGE key start stop

返回列表 `key` 中指定区间内的元素，区间以偏移量 `start` 和 `stop` 指定。

下标(index)参数 `start` 和 `stop` 都以 `0` 为底，也就是说，以 `0` 表示列表的第一个元素，以 `1` 表示列表的第二个元素，以此类推。

你也可以使用负数下标，以 `-1` 表示列表的最后一个元素， `-2` 表示列表的倒数第二个元素，以此类推。

**注意LRANGE命令和编程语言区间函数的区别**

假如你有一个包含一百个元素的列表，对该列表执行 `LRANGE list 0 10` ，结果是一个包含11个元素的列表，这表明 `stop` 下标也在 `LRANGE` 命令的取值范围之内(闭区间)，这和某些语言的区间函数可能不一致，比如Ruby的 `Range.new` 、 `Array#slice` 和Python的 `range()` 函数。

**超出范围的下标**

超出范围的下标值不会引起错误。

如果 `start` 下标比列表的最大下标 `end` ( `LLEN list` 减去 `1` )还要大，那么 `LRANGE` 返回一个空列表。

如果 `stop` 下标比 `end` 下标还要大，Redis将 `stop` 的值设置为 `end` 。

```
redis> RPUSH fp-language lisp
(integer) 1

redis> LRANGE fp-language 0 0
1) "lisp"

redis> RPUSH fp-language scheme
(integer) 2

redis> LRANGE fp-language 0 1
1) "lisp"
2) "scheme"
```

## LTRIM key start stop

对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。

举个例子，执行命令 `LTRIM list 0 2` ，表示只保留列表 `list` 的前三个元素，其余元素全部删除。

下标(index)参数 `start` 和 `stop` 都以 `0` 为底，也就是说，以 `0` 表示列表的第一个元素，以 `1` 表示列表的第二个元素，以此类推。

你也可以使用负数下标，以 `-1` 表示列表的最后一个元素， `-2` 表示列表的倒数第二个元素，以此类推。

当 `key` 不是列表类型时，返回一个错误。

`LTRIM` `命令通常和 LPUSH key value [value …]` 命令或 `RPUSH key value [value …] `命令配合使用，举个例子：

```
LPUSH log newest_log
LTRIM log 0 99
```

**超出范围的下标**

超出范围的下标值不会引起错误。

如果 `start` 下标比列表的最大下标 `end` ( `LLEN list` 减去 `1` )还要大，或者 `start > stop` ， `LTRIM` 返回一个空列表(因为 `LTRIM` 已经将整个列表清空)。

如果 `stop` 下标比 `end` 下标还要大，Redis将 `stop` 的值设置为 `end` 。

```

redis> LRANGE alpha 0 -1       # alpha 是一个包含 5 个字符串的列表
1) "h"
2) "e"
3) "l"
4) "l"
5) "o"

redis> LTRIM alpha 1 -1        # 删除 alpha 列表索引为 0 的元素
OK

redis> LRANGE alpha 0 -1       # "h" 被删除了
1) "e"
2) "l"
3) "l"
4) "o"


# 情况 2： stop 比列表的最大下标还要大


redis> LTRIM alpha 1 10086     # 保留 alpha 列表索引 1 至索引 10086 上的元素
OK

redis> LRANGE alpha 0 -1       # 只有索引 0 上的元素 "e" 被删除了，其他元素还在
1) "l"
2) "l"
3) "o"


# 情况 3： start 和 stop 都比列表的最大下标要大，并且 start < stop

redis> LTRIM alpha 10086 123321
OK

redis> LRANGE alpha 0 -1        # 列表被清空
(empty list or set)


# 情况 4： start 和 stop 都比列表的最大下标要大，并且 start > stop

redis> RPUSH new-alpha "h" "e" "l" "l" "o"     # 重新建立一个新列表
(integer) 5

redis> LRANGE new-alpha 0 -1
1) "h"
2) "e"
3) "l"
4) "l"
5) "o"

redis> LTRIM new-alpha 123321 10086    # 执行 LTRIM
OK

redis> LRANGE new-alpha 0 -1           # 同样被清空
(empty list or set)
```