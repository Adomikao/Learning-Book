# 一. 基础知识
## 1.常识
### 1.1 变量
- 变量存储在内存中的值，在创建变量时会在内存中开辟一个空间。
基于变量的数据类型，解释器会分配指定内存，并决定什么数据可以被存储在内存中。因此，变量可以指定不同的数据类型，这些变量可以存储整数，小数或字符。
- Python 中的变量赋值不需要类型声明。每个变量在内存中创建，都包括变量的标识，名称和数据这些信息。
- 每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
- 变量名只能是字母，数字，下划线的任意组合。变量名的第一个字符不能是数字。关键字不能声明为变量名。大小写敏感
- 使用下划线划分单词以及驼峰命名法。
- Python中没有常量，约定用全体大写代表常量。

### 1.2 注释
Python中单行注释以 # 开头，例如：
```py
# 这是一个注释
print("Hello, World!")
```
多行注释用三个单引号 ''' 或者三个双引号 """ 将注释括起来，例如：
```py
class Solution:
    def twoSum(self, nums, target):
        """ 多行注释
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        i = 0
        while i < len(nums):
            if i == len(nums) - 1:
                return "No solution here!"
            r = target - nums[i]
            # Can't use a num twice
            num_follow = nums[i + 1:]
            if r in num_follow:
                return [i, num_follow.index(r) + i + 1]
            i = i + 1
```

## 2.运算符
### 2.1 运算符的类型
- 算术运算符 ： + -  * /  //(取整除)  %（取余）  **(次方)
- 赋值运算符： =     +=    -=    *=    /=    %=    //=    **=
- 比较运算符：>、 <、 >=、 <=、 ==、!=
- 逻辑运算符： not 、and、 or
首先，‘and’、‘or’和‘not’的优先级是not>and>or。其次，逻辑操作符and 和or 也称作短路操作符（short-circuitlogic）或者惰性求值（lazy evaluation）：它们的参数从左向右解析，一旦结果可以确定就停止。例如，如果A 和C 为真而B 为假， A and B and C 不会解析C 。
- 成员运算符： not in 、in （判断某个单词里是不是有某个字母）
- 身份运算符： is、is not（讲数据类型时讲解，一般用来判断变量的数据类型）

### 2.2 优先级

|运算符说明|Python运算符|优先级|
|:-----  |:-----   |:-----   |
|索引运算符|x[index] 或 x[index:index2[:index3]]|18、19|
|属性访问|x.attrbute |17|
|乘方|** |16|
|按位取反 |	~ |	15|
|符号运算符	|+（正号）或 -（负号）|	14|
|乘、除	|*、/、//、% |	13|
|加、减	|+、-	|12|
|位移	|>>、<<	|11|
|按位与	|&	|10|
|按位异或	|^|	9|
|按位或|	\| |	8|
|比较运算符|	==、!=、>、>=、<、<= |	7|
|is 运算符|	is、is not|	6|
|in 运算符|	in、not in|	5|
|逻辑非	|not|	4|
|逻辑与	|and|	3|
|逻辑或	|or	|2|


## 3.数据结构
### 3.1 列表 list
列表是Python中最基本的数据结构，列表是最常用的Python数据类型，列表的数据项不需要具有相同的类型。列表中的每个元素都分配一个数字 - 它的位置，或索引，不仅可以从前往后，还可以从后往前。第一个索引是0，第二个索引是1，最后一个索引是-1，依此类推。

**列表操作的方法**
- list.append(obj)：          在列表末尾添加新的对象
- list.count(obj)：             统计某个元素在列表中出现的次数
- list.extend(seq)：          在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）
- list.index(obj)：             从列表中找出某个值第一个匹配项的索引位置
- list.insert(index, obj)：  将对象插入列表中的自己指定的位置
- list.pop(obj=list[-1])：   移除列表中的一个元素（默认最后一个元素），并且返回该元素的值，按索引删除
- list.remove(obj)：         移除列表中某个值的第一个匹配项 ，按值删除
- list.reverse()：              反向列表中元素
- list.sort([func])：           对原列表进行排序，默认升序

**函数**
- len(list): 列表元素个数
- max(list): 返回列表元素最大值, 最小值用min()
- list(tup): 将元组转换为列表

**示例**

```python
#!/usr/bin/python

list0 = ['physics', 'chemistry', 1997, 2000]
list0.append('Google')   ## 使用 append() 添加元素
print list0
del list0[2]    ##使用 del 语句来删除列表的元素
print "the list length : ", len(list0);

aTuple = (123, 'xyz', 'zara', 'abc');
aList = list(aTuple)
```

### 3.2 元组

Python 的元组与列表类似，不同之处在于元组的元素不能修改。元组使用小括号，列表使用方括号。

**元组的方法**
-  count(obj) : 统计某个元素出现的次数
-  index(obj) : 查询某个元素的下标  

**示例**

```py
tup1 = ('physics', 'chemistry', 1997, 2000)
tup2 = (50,) ## 元组中只包含一个元素时，需要在元素后面添加逗号

tup3 = (12, 34.56)
tup4 = ('abc', 'xyz')
 
# 以下修改元组元素操作是非法的。
# tup3[0] = 100
# 创建一个新的元组
tup5 = tup1 + tup2
```

### 3.3 字典
字典是 Python 中唯一的映射类型，采用键值对（key-value）的形式存储数据。Python 对 key 进行哈希函数运算，根据计算的结果决定 value 的存储地址，且 key 必须是可哈希的。可哈希表示 key 必须是不可变类型，如：数字、字符串、元组。

**字典的方法**
- dict.clear() ：删除字典内所有元素
- dict.copy() ： 返回一个字典的浅复制
- dict.fromkeys(seq[, val]) ：创建一个新字典，以序列 seq 中元素做字典的键，val 为字典所有键对应的初始值
- dict.get(key, default=None) ： 返回指定键的值，如果值不在字典中返回default值
- dict.has_key(key) ： 如果键在字典dict里返回true，否则返回false
- dict.items() ： 以列表返回可遍历的(键, 值) 元组数组
- dict.keys() ： 以列表返回一个字典所有的键
- dict.update(dict2) ： 把字典dict2的键/值对更新到dict里
- dict.values() ： 以列表返回字典中的所有值
- pop(key[,default]) ： 删除字典给定键 key 所对应的值，返回值为被删除的值。key值必须给出。 否则，返回default值。

**示例**

```py
dic1={'name':'panlei','age':23,'sex':'male'}
dic2=dict((('name','panlei'),))

dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
dict['Age'] = 8 # 更新
dict['School'] = "RUNOOB" # 添加

del dict['Name']  # 删除键是'Name'的条目
dict.clear()      # 清空字典所有条目
del dict          # 删除字典

person={'name':'lizhong','age':'26','city':'BeiJing'}
for key in person.keys():  ##遍历key
    print(key)

for value in person.values():  ##遍历value
    print(value)

for key, value in person.items():  ##遍历key-value
    print(key, value)

```

### 3.4 集合

集合是一个无序的，不重复的数据组合，它的主要作用如下：
- 去重，把一个列表变成集合，就自动去重了 
- 关系测试，测试两组数据之前的交集、差集、并集等关系

**示例**
```py
>>>basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # 这里演示的是去重功能
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # 快速判断元素是否在集合内
True
>>> 'crabgrass' in basket
False
 
>>> # 下面展示两个集合间的运算.
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # 集合a中包含而集合b中不包含的元素
{'r', 'd', 'b'}
>>> a | b                              # 集合a或b中包含的所有元素
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # 集合a和b中都包含了的元素
{'a', 'c'}
>>> a ^ b                              # 不同时包含于a和b的元素
{'r', 'd', 'b', 'm', 'z', 'l'}
```
**集合的方法**
- add() ： 为集合添加元素
- clear() ： 移除集合中的所有元素
- difference() ： 	返回多个集合的差集
- difference_update() ： 移除集合中的元素，该元素在指定的集合也存在。
- discard() ： 	删除集合中指定的元素


## 4.迭代器和生成器
### 4.1 迭代(Iteration)

 迭代：是通过重复执行的代码处理相似的数据集的过程，并且本次迭代的处理数据要依赖上一次的结果继续往下做，上一次产生的结果为下一次产生结果的初始状态，如果中途有任何停顿，都不能算是迭代。

**示例**
```py
# 1)    非迭代例子
loop = 0
while loop < 3:
    print("Hello world!")
    loop += 1
```
```py
# 2)     迭代例子
loop = 0
while loop < 3:
    print(loop)
loop += 1
```
例1仅是循环3次输出" Hello world!"，输出的数据不依赖上一次的数据，因此不是跌代。

### 4.2 容器(container)

容器：容器是一种把多个元素组织在一起的数据结构，容器中的元素可以逐个地迭代获取，可以用in, not in关键字判断元素是否包含在容器中。

1)    这个定义与在列表中定义的容器“可以包含其他类型对象（如列表、元组、字典等）作为元素的对象，在 Python中称为容器（container）”从字面上看是不同的，但本质上是一样的，因为基本上所有有元素的数据类型（字符串除外）都能包含其他类型的对象；

2)    容器仅仅只是用来存放数据的，我们平常看到的 l = [1,2,3,4]等等，好像我们可以直接从列表这个容器中取出元素，但事实上容器并不提供这种能力，而是可迭代对象赋予了容器这种能力。

### 4.3 可迭代对象(Iterable)

可迭代对象(Iterable)：可迭代对象并不是指某种具体的数据类型，它是指存储了元素的一个容器对象，且容器中的元素可以通过__iter__( )方法或__getitem__( )方法访问。

1)    __iter__方法的作用是让对象可以用for ... in循环遍历，__getitem__( )方法是让对象可以通过“实例名[index]”的方式访问实例中的元素。老猿认为这两个方法的目的是Python实现一个通用的外部可以访问可迭代对象内部数据的接口。

2)    一个可迭代对象是不能独立进行迭代的，Python中，迭代是通过for ... in来完成的。凡是可迭代对象都可以直接用for… in…循环访问，这个语句其实做了两件事：第一件事是调用__iter__()获得一个可迭代器，第二件事是循环调用__next__()。
3)    常见的可迭代对象包括：
- a)    集合数据类型，如list、tuple、dict、set、str等；
- b)    生成器(generator)，包括生成器和带yield的生成器函数(generator function)


### 4.4 迭代器（Iterator）

迭代器：迭代器可以看作是一个特殊的对象，每次调用该对象时会返回自身的下一个元素，从实现上来看，一个迭代器对象必须是定义了__iter__()方法和next()方法的对象。

- Python 的 Iterator 对象表示的是一个数据流，可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，所以 Iterator 的计算是惰性的，只有在需要返回下一个数据时它才会计算；
- Iterator 对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出 StopIteration 错误；
- 所有的 Iterable 可迭代对象均可以通过内置函数iter()来转变为迭代器 Iterator。__iter__( )方法是让对象可以用for ... in循环遍历时找到数据对象的位置，__next__( )方法是让对象可以通过next(实例名)访问下一个元素。除了通过内置函数next调用可以判断是否为迭代器外，还可以通过collection中的Iterator类型判断。如：   isinstance('', Iterator)可以判断字符串类型是否迭代器。注意： list、dict、str虽然是 Iterable，却不是 Iterator。
- 迭代器优点：节约内存（循环过程中，数据不用一次读入，在处理文件对象时特别有用，因为文件也是迭代器对象）、不依赖索引取值、实现惰性计算(需要时再取值计算)；
> 举例：用迭代器的方式访问文件
for line in open("test.txt"):print(line)
这样每次读取一行就输出一行，而不是一次性将整个文件读入，节约内存。
-  迭代器使用上存在限制：只能向前一个个地访问数据，已访问数据无法再次访问、遍历访问一次后再访问无数据
> 举例：
```py
l = [1,2,3,4]
i=iter(l)  #从list列表生成迭代器i
list(i)   #将迭代器内容转换成列表，输出[1,2,3,4]
list(i)   #将迭代器内容再次转换成列表，输出[]
用for循环访问：
i=iter(l)
for k in i:print(k)  #输出1、2、3、4
for k in i:print(k)  #再次循环没有输出
```
如果需要解决这个问题，可以分别定义一个可迭代对象，每次访问前从可迭代对象重新生成和迭代器对象，如本部分前面所介绍的，当用for..in方式访问可迭代对象时，系统就是这么干的；
- 迭代器当所有的元素全部取出后再次调用next就会抛出一个 StopIteration 异常，这并不是错误的发生，而是告诉外部调用者迭代完成了.

### 4.5 生成器 (Generator)

**生成器（generator）概念**

生成器是一个特殊的迭代器，它保存的是算法，每次调用next()或send()就计算出下一个元素的值，直到计算出最后一个元素，没有更多的元素时，抛出StopIteration。生成器有两种类型，一种是生成器表达式(又称为生成器推导)，一种是生成器函数。

**生成器表达式**

生成器表达式是通过一个 Python 表达式语句去计算一系列数据，但生成器定义的时候数据并没有生成，而是返回一个对象，这个对象只有在需要的时候才根据表达式计算当前需要返回的数据：

- 生成器表达式来源于迭代和列表解析（列表解析后面章节介绍）的组合，生成器和列表解析类似，但是它使用小括号而不是中括号。生成器返回按需产生结果的一个对象，而不是一次构建一个结果列表；
- 生成器表达式的语法如下：
```py
(expr for iter_var in iterable)
(expr for iter_var in iterable if cond_expr)
```
其中：<br>
expr为计算 生成器元素值的表达式<br>
for iter_var in iterable iter_var：表示针对在可迭代对象iter_var中的每个元素进行表达式运算<br>
if cond_exp：表示可迭代对象中的元素需要满足指定条件才会参与表达式运算
- 说明：
    - 直接在一对既有的小括号内（如在函数调用中）使用生成器表达式时，无需再添加一对小括号。例如：sum(i ** 2 for i in range(10))；
    - 生成器表达式与列表解析的语法非常相像。

**生成器函数**

生成器函数是一种语句中包含 yield 关键词的特殊的函数，它本身是一个迭代器，外部需要访问该迭代器数据的代码通过调用 next 函数（或迭代器的__next__方法）或 send 方法，触发函数执行计算并通过 yield 返回一个计算结果数据，返回数据后该函数立即停止执行，函数状态会保存在本地变量中，直到外部下次调用再激活，从上次停止执行部分开始执行。

- 生成器函数定义示意代码(非可执行代码)如下：
```
def fun():
初始化
循环：
计算得到k
nRet=yield k
其他循环代码
```
上面代码示意表示：生成器函数运行时计算得到结果 k 通过 yield 返回数据k给调用方，返回 k 给调用方之后，生成器函数停止执行，yield 的调用执行结果并没有返回给生成器函数， nRet    yield 本身的执行结果，并继续后续循环代码，直到再次执行 yield。

- 细节说明：
    - yield函数的执行是一条语句，但实际执行时该语句被分解成两部分，第一部分是将计算结果k返回给send或next调用处（下称触发方），保存当前环境，暂停执行，另一部分就是恢复当前环境，返回yield本身的执行结果给生成器函数的调用处，并继续往下执行后续循环。每次调用yield时，除了第一次是从第一部分执行，后续都是从第二部分开始执行。
    - 生成器函数在调用时只是生成一个生成器实例，并没有真正执行，真正执行只有第一次通过next触发时才会进入函数执行，注意第一次触发不能是send方式触发。

- 以下实例使用 yield 实现斐波那契数列：

```py
#!/usr/bin/python3
 
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```

### 4.6 与迭代相关的函数

enumerate、range、zip、reversed、sorted属于Python内置的函数或者类别，返回的对象都可通过迭代方法访问。

- enumerate经常用于遍历，可以对列表遍历之后得到一个带索引和值的元组，这样不需要外部再加参数去记录索引了

```py
alist = [3, 2, 4, 6, 8, 9, 1]
for tup in enumerate(alist):
    print(tup)
 
#打印

    (0, 3)  (1, 2)  (2, 4) (3, 6) (4, 8) (5, 9) (6, 1)
 
#另一个用法
alist = ['foo', 'bar', 'eye']
mapping = {}
for i,v in enumerate(alist):
    mapping[v] = i
 
print(mapping)
#打印
    {'bar': 1, 'eye': 2, 'foo': 0}
```

- sorted函数可以返回一个根据任意序列中的元素新建已排序列表

```py
alist = [7, 1, 2, 6, 0, 3, 2]
blist = sorted(alist)
print(blist)
#打印
    [0, 1, 2, 3, 6, 7]
 
blist = sorted('hello world')
print(blist)
#打印
    [' ', 'd', 'e', 'h', 'l', 'l', 'l', 'o', 'o', 'r', 'w']
```

- zip函数是将列表、元组或其他序列的元素配对，新建一个元组构成的列表

```py
alist = ['hello', 'world', 'code']
blist = ['year', 'month', 'day']
zipped = zip(alist, blist)
clist = list(zipped)
print(clist)
#打印
    [('hello', 'year'), ('world', 'month'), ('code', 'day')]

#zip可以处理任意长度的序列，但是长度由最短的序列决定：

#接上面代码
dlist = [False, True]
clist = list(zip(alist, blist, dlist))
print(clist)
#打印
    [('hello', 'year', False), ('world', 'month', True)]
 

#通过zip可以同时遍历多个序列，并且和enumerate同时使用：
for i,(a,b) in enumerate(zip(alist, blist)):
    print('{0}: {1}, {2}'.format(i, a, b))
#打印
    0: hello, year
    1: world, month
    2: code, day
 
#如果有已经配对好的序列，也可以通过zip去拆分：
alist = [('hello', 'world'), ('year', 'month'), ('one', 'two')]
first, last = zip(*alist)
print(first)
#打印
    ('hello', 'year', 'one')
 
print(last)
#打印
    ('world', 'month', 'two')
```

- reversed函数将序列的元素倒序排序

```py
alist = list(range(10))
blist = list(reversed(alist))
print(blist)
#打印
    [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```




