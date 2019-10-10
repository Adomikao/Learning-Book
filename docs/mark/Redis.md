# 一、字符串

**SET key value [EX seconds] [PX milliseconds] [NX|XX]**

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


**GETSET key value**

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

**STRLEN key**

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

**APPEND key value**

如果键 `key` 已经存在并且它的值是一个字符串， `APPEND` 命令将把 `value` 追加到键 `key` 现有值的末尾。

如果 `key` 不存在， `APPEND` 就简单地将键 `key` 的值设为 `value` ， 就像执行 `SET key value` 一样。

返回值:

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

**INCR key**

为键 `key` 储存的数字值加上一。

如果键 `key` 不存在， 那么它的值会先被初始化为 `0` ， 然后再执行 `INCR` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `INCR` 命令将返回一个错误。

> ! NOTE <br>
`INCR` 命令是一个针对字符串的操作。 因为 `Redis` 并没有专用的整数类型， 所以键 `key` 储存的值在执行 `INCR` 命令时会被解释为十进制 `64` 位有符号整数。

返回值

`INCR` 命令会返回键 `key` 在执行加一操作之后的值。

```
redis> SET page_view 20
OK

redis> INCR page_view
(integer) 21

redis> GET page_view    # 数字值在 Redis 中以字符串的形式保存
"21"
```

**INCRBY key increment**

为键 `key` 储存的数字值加上增量 `increment` 。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `INCRBY` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `INCRBY` 命令将返回一个错误。

返回值

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

**INCRBYFLOAT key increment**

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

**DECR key**

为键 `key` 储存的数字值减去一。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECR` 操作。

如果键 `key` 储存的值不能被解释为数字， 那么 `DECR` 命令将返回一个错误。

本操作的值限制在 `64` 位(bit)有符号数字表示之内。

关于递增(increment) / 递减(decrement)操作的更多信息， 请参见 INCR 命令的文档。

返回值

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

**DECRBY key decrement**

将键 `key` 储存的整数值减去减量 `decrement` 。

如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECRBY` 命令。

如果键 `key` 储存的值不能被解释为数字， 那么 `DECRBY` 命令将返回一个错误。

本操作的值限制在 `64` 位(bit)有符号数字表示之内。

返回值

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