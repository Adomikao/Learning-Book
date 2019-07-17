# 一. 基础知识
## 1.变量问题
- 变量存储在内存中的值，在创建变量时会在内存中开辟一个空间。
基于变量的数据类型，解释器会分配指定内存，并决定什么数据可以被存储在内存中。因此，变量可以指定不同的数据类型，这些变量可以存储整数，小数或字符。
- Python 中的变量赋值不需要类型声明。每个变量在内存中创建，都包括变量的标识，名称和数据这些信息。
- 每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
- 变量名只能是字母，数字，下划线的任意组合。变量名的第一个字符不能是数字。关键字不能声明为变量名。大小写敏感
- 使用下划线划分单词以及驼峰命名法。
- Python中没有常量，约定用全体大写代表常量。

## 2.运算符
- 算术运算符 ： + -  * /  //(取整除)  %（取余）  **(次方)
- 赋值运算符： =     +=    -=    *=    /=    %=    //=    **=
- 比较运算符：>、 <、 >=、 <=、 ==、!=
- 逻辑运算符： not 、and、 or
首先，‘and’、‘or’和‘not’的优先级是not>and>or。其次，逻辑操作符and 和or 也称作短路操作符（short-circuitlogic）或者惰性求值（lazy evaluation）：它们的参数从左向右解析，一旦结果可以确定就停止。例如，如果A 和C 为真而B 为假， A and B and C 不会解析C 。
- 成员运算符： not in 、in （判断某个单词里是不是有某个字母）
- 身份运算符： is、is not（讲数据类型时讲解，一般用来判断变量的数据类型）

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



