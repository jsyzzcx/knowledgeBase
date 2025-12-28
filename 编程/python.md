## 一、基础知识点

#### 1. 基本语法与数据类型

* **命名规范**
  
  - 区分大小写
  
  - 模块和包名小写，首字母小写，尽量不用下划线(除非多个单词，且数量不多)
  
  - 类名用驼峰(CamelCase)命名风格，首字母大写
  
  - 变量/函数名小写，如有多个单词，用下划线隔开
  
  - 常量大写，如有多个单词，使用下划线隔开。
  * 单下划线(_)开头的模块变量或函数，表示 protected (用import * from 时不会包含)
  
  * 双下划线(__)开头的实例变量或方法，表示private
  
  * Python 中的特殊方法（如 `__init__、__str__`）使用双下划线开头和结尾

* **Python对象**
  
  - 对象：身份（identify）+类型（type）+值（value）；可以赋值给一个变量、添加到对象集合中、作为函数的参数、作为函数的返回值
  
  - 对象布尔值：内置函数bool()可获取对象布尔值；False、数值0、None、空字符串‘’ “”、空列表[] list()、空元组() tuple()、空字典{} dict()、空集合 set() 的布尔值为False ；其他对象布尔值为True
  
  - object：所有类的基类，是由type创建；
  
  - type：继承自 object；是由type创建的；可以获取对象的类型；python中用来创建类的类；
- **Python变量**
  
  - 变量和数据是分开存储的:  数据保存在内存中的一个位置(地址)
    
    - id(变量名)       返回对象的唯一标识(内存地址)
    
    - type(变量名)   查看变量类型
  
  - 变量赋值( Python 无需声明变量类型，使用 `=` 进行赋值 ):   <mark>赋值并不拷贝对象给变量，而是让变量指向了对象；一个对象，可以被多个变量所指向</mark>
    
    - a = {1: [1,2,3]}  解释器流程：创建变量a；创建一个对象(分配一块内存)，来存储值 {1: [1,2,3]}；将变量与对象，通过指针连接，即：引用(变量引用对象)
    
    - 对于不可变对象，所有指向该对象的变量的值总是一样的。但是通过某些操作（+= 等等）更新不可变对象的值时，会返回一个新对象
    
    - 变量存储的是，数据在内存中的地址；即 引用
      
      - 在函数调用时，传递的是实参所存储的数据引用，而不是数据本身
      
      - 函数返回时，也是返回数据的引用，而不是数据本身
  
  - 变量删除:  变量可以被删除，但是对象无法被删除
  
  - 匿名变量: _ 表示匿名变量 如：_ = []
  
  - 局部变量：函数内定义，在函数内有效；函数结束后被回收；局部变量用global声明，就会变成全局变量  
  
  - 全局变量：函数外定义，在函数内外有效；   
    
    - 在函数内部对全局变量重新赋值，不会修改全局变量，而是重新定义了一个和全局变量同名的局部变量 
    - 要在函数内部改变全局变量，须加上global 声明

- **基本数据类型**
  
  - 数值类型：`int`, `float`, `complex`
    
    - 整数 int - 0b开头 - 二进制、0o开头 - 八进制、0x 开头 - 十六进制
    
    - 浮点数(小数) float - python浮点数有精度误差，可使用 decimal模块
      
      - 数学写法，如1.23，3.14，-9.01；
      - 科学计数法(很大/小的浮点数)，把10用e替代；如 0.000012 等同于 1.2e-5 、1.23e9 等同于 12.3e8
    
    - 复数 complex - 分为实部 + 虚部 (都是用浮点数表示）；在 Python 中，复数的虚部以j或者J作为后缀
      
      ```python
      实例1
      import  decimal  
      a = decimal.Decimal('0.1')
      b = decimal.Decimal('0.2')
      print( 0.3 == (0.1+0.2) )                # false
      print( decimal.Decimal('0.3') == (a+b) )  # true
      
      实例2
      x= 1+2j
      print(x.real)  #  1.0 
      print(x.imag)  #  2.0 
      ```
  
  - 字符串：`str`（支持单引号、双引号、三引号）
  
  - 布尔值：`True`, `False` - 0的布尔值是 False  ；非0的布尔值是 True
  
  - 空值：`None` 全局只有一个；空对象，没有方法和属性；可以赋值给任何一个变量；定义类的属性时，如果不知道设置什么初始值，可以设置为None
  
  - bytes 类型 - 用带b前缀的单引号或双引号表示，bytes的每个字符都只占用一个字节；`x = b'ABC'`

- **可变类型( <mark>值变地址不变</mark> ) & 不可变类型( <mark>值变地址也变</mark> )**
  
  - 可变数据类型(列表、字典、集合) ：当该数据类型的对应变量的值发生了改变，那么它对应的内存地址不发生改变；  
  - 不可变数据类型(None、字符串、元组、数字)： 当该数据类型的对应变量的值发生了改变，那么它对应的内存地址也会发生改变；
    - 多任务下不需加锁。应尽量设计成不变对象；
    - `a = a + 1`   如果 a 是不可变对象，a+1 并不是让 a 的值增加 1，而是重新创建了一个值为a+1 的对象，并让 a 指向它

```python
PS C:\Users\jsyzz> python
>>> a=1
>>> id(a)  140707767903016
>>> id(1)  140707767903016
>>> b=a
>>> id(b)  140707767903016
>>> c=1
>>> id(c)  140707767903016
>>> a=[1,2,3]
>>> id(a)  2856354514304
>>> a.append(4)
>>> id(a)  2856354514304
>>>
```

#### 2. 运算符

| 运算符说明                           | Python运算符                | 优先级    | 结合性 | 备注                                                              |
| ------------------------------- | ------------------------ | ------ | --- | --------------------------------------------------------------- |
| 小括号                             | ( )                      | 19(最高) | 无   |                                                                 |
| 索引运算符                           | x[i] 或 x[i1: i2 [:i3]]   | 19     | 左   |                                                                 |
| 属性访问                            | x.attribute              | 18     | 左   |                                                                 |
| 乘方( 幂运算)                        | **                       | 17     | 右   | 2**3 = 8                                                        |
| 按位取反                            | ~                        | 16     | 右   |                                                                 |
| 符号运算符                           | +（正号）、-（负号）              | 15     | 右   |                                                                 |
| 乘除                              | *、/、//、%                 | 14     | 左   | // 取整除   实例：9 // 2 = 4 返回除法的商                                   |
| 加减                              | +、-                      | 13     | 左   | % 取余数     实例：9%2 = 1                                            |
| 位移(<< 高位溢出舍弃，低位补0，等同乘2)         | >>、<<                    | 12     | 左   | "-" * 50 重复                                                     |
| 按位与（对应数位都是1，结果数位才是1）            | &                        | 11     | 右   | "aaa" + "bbb" 拼接                                                |
| 按位异或                            | ^                        | 10     | 左   |                                                                 |
| 按位或（对应数位都是0，结果数位才是0）            | \|                       | 9      | 左   |                                                                 |
| 比较运算符(<mark>比较对象的value</mark>)  | ==、!=、>、>=、<、<=          | 8      | 左   | 比较值是否相等                                                         |
| 赋值运算符                           | =、%=、/=、//=、-=、+=、*=、**= | 7      |     |                                                                 |
| is (<mark>比较对象的id</mark>)，身份运算符 | is、is not                | 6      | 左   | 判断是否同一个对象，是否指向同一个内存地址                                           |
| in 成员运算符                        | in、not in                | 5      | 左   |                                                                 |
| 逻辑非（取反）                         | not                      | 4      | 右   |                                                                 |
| 逻辑与（两边都为true，结果为true）           | and                      | 3      | 左   |                                                                 |
| 逻辑或（两边一个为true，结果为true）          | or                       | 2      | 左   |                                                                 |
| 逗号运算符                           | exp1, exp2               | 1(最低)  | 左   | a,b,c = 20,30,40         # 系列解包赋值 <br>a,b = b,a            # 交换 |
| 解包运算符                           | * ,  **                  |        |     | 用于解包和打包参数、扩展序列、字典、在函数参数中接受不定数量的参数                               |

```python
s1=[1,2,3]
s2=[1,2,3]
print(s1 == s2)        # True    ，s1 和 s2  值相同
print(s1 is   s2)      # False   , s1 和 s2   id不同（内存地址不同）

s1=1
s2=1
print(s1 == s2)       # True ，s1 和 s2 的 值相同
print(s1 is   s2)     # True , s1 和 s2 的  id相同（内存地址相同）

s1="abc"
s2="abc"
print(s1 == s2)       # True，s1 和 s2 的 值相同
print(s1 is   s2 )    # True，s1 和 s2 的  id相同（内存地址相同）
```

#### 3. 数据结构

- 容器比较

| 容器类型    | 是否可变 | 是否重复   | 是否有序 | 定义符号        | 切片  | 是否有生成式 | 特性                                    |
| ------- | ---- | ------ | ---- | ----------- | --- | ------ | ------------------------------------- |
| 列表 list | 可变   | 可      | 有序   | []          | 可   | 有      | 查找和插入时间随着元素的增加而增加；占用空间小，浪费内存很少        |
| 元组tuple | 不可变  | 可      | 有序   | ()          | 可   | 无      |                                       |
| 字典 dict | 可变   | key不重复 | 无序   | {key:value} | 不可  | 有      | 查找和插入速度极快，不会随着key的增加而变慢；需占用大量内存，内存浪费多 |
| 集合 set  | 可变   | 不重复    | 无序   | {key}       | 不可  | 有      |                                       |
| 字符串 str | 不可变  | 可      |      | ""、''       | 可   | 有      |                                       |

- 容器公共方法 - 所有的容器都是可迭代的（iterable）
  
  - len(iterable) 计算容器中元素个数  
  - del(iterable) 删除变量 实例：a = [1,2,3] del a[1] 或者 del(a[1])  
  - max(iterable) 返回容器中元素最大值(如果是字典，比较key)  
  - min(iterable) 返回容器中元素最小值(如果是字典，比较key)  
  - in not in 判断元素是否存在，判断元素是否不存在  
  - 遍历 for in  
  - 取值 []
  - `+ `链接 合并；仅对 字符串、列表、元组
  - `*` 重复；仅对 字符串、列表、元组
  - <,<=,>,>=,== 比较；仅对 字符串、列表、元组
  - 切片：[开始索引：结束索引：步长]

###### 3.1 列表(List)：有序、可变集合 `[1, 2, 3]`

- 有序：放入顺序是其存储的顺序；

- 可变：增，删，改操作，对象地址不会发生改变

- 根据需要动态分配和回收内存

- 索引映射唯一数据；从0开始；

```
列表数据  [  ‘hello’   ‘world’   123    99.8    ‘hello’ ‘world’  ]
正向索引         0          1         2       3        4          5
反向索引        -6         -5        -4      -3       -2          -1
```

- 创建列表

> [] lst = ['aaa','bbb',98,True]
> 
> 内置函数 list() lst = list( ['aaa','bbb',98,True] )
> 
> 列表生成式

- 访问元素

> index(元素) 获取指定元素的索引、元素不存在，则会抛出ValueError、多个相同元素，只返回第一个元素的索引
> 
> [n]   根据索引值返回元素、 如果指定索引不存在，则会抛出IndexError

- 增加元素

> list.append(数据)           在列表末尾追加元素
> 
> list.extend(可迭代对象)  在列表末尾合并元素；<mark>对列表变量使用 += ，本质上是执行了 extend </mark>
> 
> list.insert(索引, 数据)      指定位置插入元素

- 删除元素

> list.remove(数据)  删除第一个出现的指定元素，当元素不存在会抛出ValueError
> 
> list.pop()              删除末尾的元素
> 
> list.pop(索引)       删除指定索引位置上的元素，当索引不存在会抛出IndexError
> 
> list.clear()            清空列表
> 
> del                       删除列表； del 关键字 用于删除 内存中的变量；删除变量后，后续代码就不能再使用此变量了
> 
> del list[索引]        删除指定索引位置上的元素

```python
lst = [10,20,30,40,50,60,30]
lst.pop(1)
print(lst) # [10, 30, 40, 50, 60, 30]
lst.pop()
print(lst) # [10, 30, 40, 50, 60]
lst[1:3] = []
print(lst)  # [10, 50, 60]
lst.clear() 
print(lst)  #  []
del lst
```

- 修改元素

> list[ 索引 ] = 数据 #修改指定索引的数

- 切片

> 列表名 [ start: stop: setp ] <mark>切片是原列表片段的拷贝, 不改变原列表</mark>
> 
> 切片范围 [start , stop) ；step 默认1 ,-1 表示从后向前切片，简写 [ start: stop ]；start 是0 简写 [ : stop ]

```python
L = list(range(20)) # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
print(L[:10])       # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]  取前10个数，切片出的是新列表
print(L[-10:])      # [10, 11, 12, 13, 14, 15, 16, 17, 18, 19] 取后10个数，切片出的是新列表
print(L[10:15])     # [10, 11, 12, 13, 14] 前10-15个数，切片出的是新列表
print(L[:10:2])     # [0, 2, 4, 6, 8] 前10个数，每两个取一个，切片出的是新列表

print(L[::2])       # [0, 2, 4, 6, 8, 10, 12, 14, 16, 18] 取偶数，切片出的是新列表
print(L[1::2])      # [1, 3, 5, 7, 9, 11, 13, 15, 17, 19] 获奇数，切片出的是新列表


print(L[:])         # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]  原样复制一个list，切片出的是新列表
print(L[-5:-4])     # [15] ，反向索引

print(L[::-1])      # [19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0] 反向切片
print(L[10::-1])    # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0] 反向切片
```

- 排序

> list.sort()       升序，指定reverse = True ，降序；不产生新列表，原列表变
> 
> list.sorted()   升序，指定reverse =True ，降序，产生新列表，原列表不变
> 
> list.reverse()  逆序

```python
lst = [20, 40, 10, 98, 54]

lst.sort(reverse = True)
print(lst)       # [98, 54, 40, 20, 10]

new_lst = sorted(lst)
print(new_lst)  # [10,20,40,54,98]
print(lst)      # [20, 40, 10, 98, 54]
```

- 遍历

```python
year=[82,89,88,86,85,00,99]
for index,value in enumerate(year):  # 可以取出索引
    if str(value) !='0':
        year[index] = int('19'+str(value))
    else:
        year[index] = int('200'+str(value))

print(year) # [1982, 1989, 1988, 1986, 1985, 2000, 1999]
```

- 统计、拷贝、运算+、运算 *

```python
import copy
x = [[1,2,3],[4,5,6],[7,8,9]]
print(x.copy())             # 浅拷贝 [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(copy.copy(x))         # 浅拷贝（通过copy模块实现） [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(copy.deepcopy(x))     # 深拷贝（通过copy模块实现） [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

s1 = [1,2]
s2 = [1,2]
s = s1 + s2
print(s)           # [1, 2, 1, 2]
ss = s1*3
print(ss)          #  [1, 2, 1, 2, 1, 2]
print(ss.count(2)) # 3 统计数据在列表中出现的次数 
```

###### 3.2 元组(Tuple)：有序、不可变集合 `(1, 2, 3)`

> - 一般存储不同类型的数据，列表一般存储相同类型的数据
> 
> - <mark>初始化就不能修改；无增删改操作，仅可遍历</mark> ；数据不可以被修改，以保护数据安全；
>   
>   - 如果元组中的对象是可变对象，则可变对象的引用不允许改变，但数据可以改变( 下图中，整数100 是不可变对象 ，列表[20,30]是可变对象 )
>   
>   - 如果元组中的对象是不可变对象，则不能再引用其他对象

![](C:\Users\jsyzz\AppData\Roaming\marktext\images\2025-10-25-19-32-18-image.png)

- 元组应用场景

> 函数的参数和返回值
> 
> 格式化字符串后面的() 本质上就是一个元组
> 
> 不可变 - 在多任务下，同时操作对象不需加锁，代码更安全；能用tuple代替list就尽量用tuple

- 创建元组

> (元素1, 元素2, ... ) 或者 元素1, 元素2, ... ；` t1 = ('python','hello',90)   t2 = 'python','hello',90`
> 
> 内置函数tuple() `t3 = tuple( ('python','hello',90) )`
> 
> 只包含一个元素，要用逗号` t1 = ( 10, )   t2=5`    区别数学计算上的括号
> 
> 空元组` t = ()  t = tuple()`

- 访问元素

```python
t1  = 1,2,3,4,5, "abc"
print(t1 [0])           # 1
print(t1 [-1])          # "abc"
t1[0]  =9               # err  元组不支持修改

nums = (3,1,9,6,8,3,5,3)
print(nums.count(3) )   # 3，获取元素3出现的次数
print(nums.index(9) )   # 2，获取元素9的索引
```

- 遍历

```python
score=(('广州恒大',72),('北京国安',70),('上海上岗',66),('江苏苏宁',53))
for  index,item in enumerate(score):    # 可取出索引
       print( index+1, ',' , end=' ')   #  end=' '  不换行
       for score in item:
            print(score, end=' ')
       print()                          # 换行

输出
1 , 广州恒大 72 
2 , 北京国安 70 
3 , 上海上岗 66 
4 , 江苏苏宁 53 
```

- 切片 - 元组名[ start: stop: setp ]

- 解包/打包

```python
x, y, *z = 1, 2, 3,4
print(z)      # [3 ,4]
x, y = y, x
print(x, y)   # 2 1

t = (123,"FishC",3.14)   #打包
a,b,c = t                #解包
print(a,b,c)  # 123 FishC 3.14
```

###### 3.3 字典(Dict)：键值对集合 `{'name': 'Python', 'version': 3.9}`

> 用于 高速查找 的场景
> 
> 键值对；key不重复，value可重复；key须是不可变类型，即可hash
> 
> 可变序列：增，删，改，对象地址不会发生改变；
> 
> 无序序列：根据key来计算value的位置 - 哈希算法；放入顺序并不是其存储的顺序；会浪费较大的内存，用空间换时间；

- 创建字典

```python
scores = {'张三':100，'李四':98，'王五':45}  # 通过{}
d = dict( name='jack' , age = 20)            # 函数 dict
d = dict({"key1":"1" , "key2":"2"})          # 转换其他对象

通过 fromkeys(iterable[, values])
print(dict.fromkeys("Fish", 250))                 # { 'F':250, 'i':250,'s':250,'h':250 }
print(dict.fromkeys(['name', 'age']))             # {'name': None, 'age': None}
print(dict.fromkeys(['name', 'age'], 'unknown'))  # {'name': 'unknown', 'age': 'unknown'}
```

- 元素访问

> [key]，key不存在 ，抛出keyError异常
> 
> get(key, value) ，key不存在，返回value(默认值)
> 
> 字典.keys() 获取字典的所有key
> 
> 字典.values() 获取字典的所有value
> 
> 字典.items() 获取字典的所有( key，value) 元组

```python
scores = {'张三': 100,'李四': 98,'王五': 45}
keys = scores.keys()
print(list(keys))  # ['张三'，'李四'，'王五']
values = scores.values()
print(list(values))  # [100,98,45]
items = scores.items()
print(list(items))  # [('张三',100)，('李四',98)，('王五',45)]
```

- 元素修改

```python
scores['jack'] = 90                       # 更新单个值，key不存在时，是新增，已存在，是修改
scores.update( {'jack':105, 'key1':104} ) # 更新多个值，当key存在时，更新；不存在时，新增；{'张三': 100, '李四': 98, '王五': 45, 'jack': 105, 'key1': 104}
```

- 元素删除

```python
del scores['张三']      
scores.pop('张三')    # 当key不存在时，会报错      
scores.clear()        # 清空
```

- 遍历

```python
scores = {'张三': 100,'李四': 98,'王五': 45}
for key in scores :                # 对字典遍历， 遍历键
    print( key , scores[key] )     # 张三 100   李四 98  王五 45
for value in scores.values():
    print( value ) # 100 98 45
for key,value in  scores.items():  # 拆包的写法
    print( key , value )           # 张三 100   李四 98  王五 45
```

- 排序

```python
d = {'b': 1, 'a': 2, 'c': 10}
d_sorted_by_key = sorted(d.items(), key=lambda x: x[0])   # 根据字典键的升序排序
d_sorted_by_value = sorted(d.items(), key=lambda x: x[1]) # 根据字典值的升序排序
print(d_sorted_by_key)     # [('a', 2), ('b', 1), ('c', 10)]
print(d_sorted_by_value)   # [('b', 1), ('a', 2), ('c', 10)]
```

- 拷贝

```python
d = {  "张三": {"语文" : 60 , "数学": 70}   , "李四": {"语文" : 60 , "数学": 70}  }  # 嵌套字典
print(d["张三"]["数学"])  # 70
d1 = d.copy()             # 浅拷贝
d1["张三"]["数学"] =100
print(d["张三"]["数学"])  # 100
```

###### 3.4 集合(Set)：无序、无重复元素集合 `{1, 2, 3}`

> set和dict的唯一区别仅在于没有存储对应的value
> 
> 是一组key的集合{ key1,key2 } ，不存储value。key不能重复-元素唯一；通过hash计算存储位置-无序；
> 
> 可变序列：增，删，改操作，对象地址不会发生改变；
> 
> <mark>查找性能O(1)</mark>：list查找性能会随着数据增多变慢(O(n)；但是dict/set 的内存花销大；

- 创建集合

```python
s1 = { "fishc" , "python" } # 通过 {}   

set("FishC")        # {'h','i','F','C','s'} ，元素是无序的；通过内置函数set()
set( [1,1,2,3,5] )  # 通过转换 {1,2,3,5}  利用集合去重
set( (1,1,2,3,5) )  # 通过转换 {1,2,3,5} 
set( {1,1,2,3,5} )  # 通过转换 {1,2,3,5}  
```

- 新增元素

```python
s = {10,20,30,40,405,60}
s.add(80)                  # 一次添加一个元素 {80, 20, 405, 40, 10, 60, 30}
print(s)
s.update( {200,400,300} )  # 一次添加多个元素  {40, 200, 10, 300, 80, 400, 20, 405, 60, 30}
print(s)
```

- 删除元素

```python
s = {10,20,30,40,405,60}
s.remove(405)
print(s)       # {20, 40, 10, 60, 30}
# s.remove(500)   # KeyError
s.pop()
print(s)      # {40, 10, 60, 30}
s.pop()              
print(s)      # {10, 60, 30}
s.clear()
```

- 集合关系运算

> <= 检查子集、< 检查子集、>= 检查超集、>= 检查超集、> 检查超集
> 
> == 、 !=判断2个集合是否相等 (集合中的元素相同，即相等)
> 
> issubset() 判断子集、issuperset() 判断超集、isdisjoint() 判断交集

```python
s1={10,20,30,40,50,60}
s2={10,20,30,40}
s3={10,20,90}
s4={100,200,900}
print(  s2 <s1 )             # True
print(  s2.issubset(s1) )    # True
print(  s1.issuperset(s2) )  # True
print(  s2.isdisjoint(s4) )  # True，没有交集
```

- 集合数学运算

> | 并集、& 交集、- 差集、^ 对称差集

```python
s1 = {10,20,30,40}
s2 = {20,30,40,50,60}
print( s1.intersection(s2) )         # 返回一个新集合，交集 {40, 20, 30}
print( s1 & s2 )                     # 返回一个新集合，交集 {40, 20, 30}
print( s1.union(s2) )                # 返回一个新集合，并集 {40,10,50,20,60,30} ，会去重
print( s1 | s2 )                     # 返回一个新集合，并集 {40,10,50,20,60,30}，会去重
print( s1.difference(s2) )           # 返回一个新集合，差集 {10}
print( s1 - s2 )                     # 返回一个新集合，差集 {10}
print( s1.symmetric_difference(s2) ) # 返回一个新集合，对称差集 {50, 10, 60}
print( s1 ^ s2 )                     # 返回一个新集合，对称差集 {50, 10, 60}
```

###### 3.5 字符串(String)：不可变字符序列 `"Hello"`

> 不可变序列，无法进行增删改操作
> 
> 用 ‘’、 “” 来定义 ；单/双引号定义的字符串须在一行；
> 
> 用r''表示不转义、\" \' 做字符串的转义   print("d:\\\one\\\two\\\three\\\now")   print(r"d:\one\two\three\now")

- 字符串索引

```python
str = 'hello,hello'
数据      'h      e       l        l       o      ,     h     e     l     l     o’   
正向索引   0      1       2         3      4      5     6     7     8     9    10
反向索引  -11    -10     -9        -8     -7     -6    -5    -4    -3    -2     -1
```

- 过滤

```python
str = '  This is python.  '
print(str.strip())           # 'This is python.'
str = '***  This is python.!!!  ***'
print(str.strip('* !'))      # 'This is python.'
```

- 替换   replace( old, new, count=-1 ) 返回新串，不修改原来字符串内容； 将 old 替换为 new ，替换次数 count ，-1 表示全部替换

- 查询

```python
- 字符串[索引]                      # 取单个字符
- count( sub[ ,start[ , end] ] )    # 获取子串sub 出现的次数
- find( sub[ ,start[ , end] ] )     # 查找子串sub 的索引下标值 ，从左向右找 ， 找不到返回-1
- rfind( sub[ ,start[ , end] ] )    # 查找子串sub 的索引下标值 ，从右向左找， 找不到返回-1
- index( sub[ ,start[ , end] ] )    # 查找子串sub 的索引下标值 ，从左向右找 ， 找不到抛ValueError
- rindex( sub[ ,start[ , end] ] )   # 查找子串sub 的索引下标值 ，从右向左找， 找不到抛ValueError
- startswith(str)                   # 检查字符串是否以 str 开头，是则返回true
- endswith(str)                     # 检查字符串是否以str 结尾，是则返回true
```

- 大小写转换

```python
- upper()       # 返回新字符串：将字符串的字母大写
- lower()       # 返回新字符串：将字符串的字母小写
- swapcase()    # 返回新字符串：将字符串的大写变小写，小写变大写
- capitalize()  # 返回新字符串：将字符串的首字母大写，其他小写
- title()       # 返回新字符串：将字符串的每个单词的首字母大写，其他小写
- casefold()    # 返回新字符串：将字符串的字母小写
```

- 内容对齐 - 返回原字符串

```python
center(width , fillchar='')   # 指定的宽度大于源字符串，居中对齐，两边补空格
ljust(width , fillchar='')    # 指定的宽度大于源字符串，左对齐，右补空格
rjust(width , fillchar='')    # 指定的宽度大于源字符串，右对齐，左补空格
zfill(width)                  # 指定的宽度大于源字符串，右对齐，左补0

print("520".zfill(5))     # '00520'
print("-520".zfill(5))    # '-0520'
```

- 去空格

```python
strip()  #去左右两边的空格
lstrip()
rstrip()
```

- 拼接 - 产生新字符串； join(iterable) 先计算出所有字符串长度，然后再拷贝，只new 一次对象，效率比 “+” 高

```python
 '*'.join('python')                    # p*y*t*h*o*n
  ".".join(["www","ilovefishc","com"]) # www.ilovefishc.com
```

- 分割

```python
- partition(sep)               # sep分割字符串（从左到右），并返回三元组  
- rpartition(sep)              # sep分割字符串（从右到左），并返回三元组  
- split(sep , maxsplit =-1)    # sep分割字符串（从左到右），sep默认空格字符串，并返回列表；maxsplit分割次数
- rsplit(sep , maxsplit =-1)   # sep分割字符串（从右到左），sep默认空格字符串，并返回列表；maxsplit分割次数
- splitlines( keepends =False) # 将字符串按行进行分割，并返回列表 ，keepends 是否包含换行符

s = 'hello|world|python'
print(s.split(sep='|'))               # ['hello', 'world', 'python']
print(s.split(sep='|',maxsplit =1))   # ['hello', 'world|python']
```

- 判断

```python
startswith( prefix[ , start [ , end ]] )   # 判断 子串 prefix 是否出现在起始位置
endswith( suffix[ , start [ , end ]] )     # 判断 子串 suffix是否出现在结束位置
isupper()       # 判断是否所有的字母都是大写
islower()       # 判断是否所有的字母都是小写
istitle()       # 判断是否 字符串的每个单词的首字母大写，其他小写

isspace()       # 判断是否 字符串 都是空白字符组成（回车、换行、水平制表符）
isprintable()   # 判断是否 字符串 都是可打印的

isdecimal()     # 判断是否全部由数字组成
isdigit()       # 判断是否全部由数字组成，可以判断unicode 字符   \u00b2
isnumeric()     # 判断是否全部由数字组成，可以判断unicode 字符 、汉字数字
isalpha()       # 判断是否都是由字母构成
isalnum()       # 判断是否全部由字母和数字组成

isidentifier()  # 判断是否是一个合法的python标识符
```

- 格式化

> 格式化字符/占位符 -  <mark>"格式化字符串" % (变量1,变量2,变量3)</mark>
> 
> - % 格式化操作符
> 
> - %s 字符串
> 
> - %i、%d 有符号10进制数；%06d 输出的整数显示6位数，不足的地方用0补全
> 
> - %f 浮点数、%.2f 小数点后面显示2位，不足的地方用0补全
> 
> - %x 16进制数
> 
> - %% 输出 %   实例  'growth rate: %d %%'  %  7    输出 'growth rate: 7 %'
> 
> f/F字符串
> 格式化方法 format()

```python
实例1：格式化字符
name = '张三'
age = 20
print( '我叫%s，今年 %d岁 '  %  (name,age) )  # 我叫张三 ， 今年20岁 ； (name,age)  是个元组

no = 100
print("%06d"  % no )             # 000100

print( '%.3f'    % 3.1415926)    # 3.142  
print( '%.5f'    % 3.1)          #3.10000

实例2：f/F字符串
print(f'我叫{name}, 今年{age}岁' )   # 我叫张三 ， 今年20岁 
print(f "{-520:010}")                # -000000520
print(f"{123456789:,}”)             # 123,456,789
print(f"{3.1415:.2f}")               # 3.14   : 后面的.2f指定了格式化参数（即保留两位小数）

实例3：格式化方法 format()
print( '我叫{}，今年 {}岁 '.format(name,age) )                      # 我叫张三 ， 今年20岁 
print( '我叫{0}，今年 {1}岁 '.format(name,age) )                    # 我叫张三 ， 今年20岁 
print( '我叫{name}，今年 {age}岁 '.format(name = '张三',age = 20) ) # 我叫张三 ， 今年20岁 

print( '{0}'.format(3.1415926) )      # 3.1415926   ;  {} 占位符 ； 0 占位符的索引
print( '{0:.5}'.format(3.1415926) )   # 3.1416;    .5 一共5位数；   0 占位符的索引
print( '{0:.3f}'.format(3.1415926) )  # 3.142;    .3f 三位小数；    0 占位符的索引
print( '{:.3f}'.format(3.1415926) )   # 3.142;    .3f 三位小数； 
print( '{:7.3f}'.format(3.1415926) )  # 3.142;    .3f 三位小数；    7 宽度
```

#### 4. 语句与控制流

- **多行注释**
  
  ```
  """
  被注释内容
  """
  
  '''
  被注释内容
  '''
  ```

- **占位符** `pass`

- **条件语句** 
  
  ```
  单分支            if  condition :
                                statement
  双分支            if  condition :
                               statement
                       else :
                               statement
  多分支            if  condition1 :
                                   statement
                       elif   condition2 :
                                   statement
                       elif   condition3 :
                                  statement
                       else :
                                 statement
  ```

- **循环语句**
  
  - `for` 循环（遍历可迭代对象）
  
  - `while` 循环（条件为真时执行）
  
  - 控制语句：`break`, `continue`, `pass`
    
    ```python
    while condition :
        statement
        break         # 退出循环
        continue      # 跳过本次，执行下次循环
    else:             # 没有遇见break时执行 ；用于判断循环的是否正常退出； 即循环正常结束后，执行else块
        statement
    
    for   变量   in  可迭代对象 :   #一般用于遍历
        statement
        break          # 退出循环
        continue       # 跳过本次，执行下次循环
    else:              # 没有遇见break时执行 ；用于判断循环的是否正常退出；即循环正常结束后，执行else块
        statement
    for x, y in [(1, 1), (2, 4), (3, 9)]:
        print(x, y)
    ```

- **条件表达式** - 条件成立时执行的 statement   if   condition   else   条件不成立时执行的statement

- **match 语句**
  
  ```python
  age = 15
  match age:
    case x if x < 10:
        print(f'< 10 years old: {x}')
    case 10:
        print('10 years old.')
    case 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18:
        print('11~18 years old.')
    case 19:
        print('19 years old.')
    case _:                               # case _表示“任意值”
        print('not sure.')
  ```

- **with 语句** - 上下文管理器，<mark>能够自动分配并且释放资源</mark>；内部调用`__enter__() ， __exit__()` 
  
  ```python
  f = open("FishC.txt","w")
  f.write("I love Python")
  f.close()
  
  # 等同于
  
  with  open("FishC.txt","w", encoding='utf-8')  as  f :
      f.write("I love Python")
  
  # 实例：基于类的上下文管理器
  class FileManager:
      def __init__(self, name, mode):
          print('calling __init__ method')
          self.name = name
          self.mode = mode 
          self.file = None   
  
      def __enter__(self):    # 获取需要被管理的资源
          print('calling __enter__ method')
          self.file = open(self.name, self.mode)
          return self.file
  
      # “exc_type, exc_val, exc_tb”，当执行 with 语句时，有异常抛出，异常的信息就会包含在这三个变量中，传入方法“__exit__()”
      def __exit__(self, exc_type, exc_val, exc_tb):   # 用于释放资源
          print('calling __exit__ method')
          if self.file:
              self.file.close()            
  
  with FileManager('test.txt', 'w') as f:
      print('ready to write to file')
      f.write('hello world')
  
  # 输出：
  calling __init__ method
  calling __enter__ method
  ready to write to file
  calling __exit__ method
  ```

#### 5. 函数

###### 5.1 函数定义与调用：使用 `def` 关键字定义

```
def  函数名 ( [形参] ) :    #函数名保存的是函数内存地址
    """                     #函数注释说明
    这是一个xxxxx方法
    """

     [return xxx]            #没有return语句，默认返回None；返回多个值时，是一个tuple

函数名 ( [实参] )            #函数调用  
```

###### 5.2 参数类型：位置参数、关键字参数、默认参数、可变参数 `*args`、关键字可变参数 `**kwargs`

> Python 里所有的数据类型都是对象，<mark>传参 只是让新变量与原变量指向相同的对象</mark>
> 
> 函数内，对可变参数修改了数据，会影响到外部的数据，不可变参数无法修改，只能重新赋值
> 
> 函数内，对参数(可变|不可变)重新赋值，不会影响实参变量，只会在函数内部修改局部变量的引用

- 默认值参数 - 必选参数在前，默认参数在后(在参数列表末尾)；默认参数需用不可变对象，否则运行时会有逻辑错误；有多个默认参数，需指定参数名

```python
def fun(a, b=10):  # b 是默认值参数(调用时可以省略)
    print(a, b)

fun(100)  # 100 10
```

- 位置参数 & 关键字参数( 不必按照函数定义的顺序放置它们 )

```python
def fun(a,b,c,d) :
    print('a=',a,'b=',b,'c=',c,'d=',d)
fun(10,20,30,40)                # 传递位置传参
fun( a=100,c=300,b=200,d=400)   # 传递关键字实参
fun( 10,20,c=30,d=40)           # 混合使用
```

- 可变参数  `*args` - <mark>接收元组</mark>；`*args` 解包位置参数，将它们打包成一个元组；这使得函数可以接受不定数量的位置参数

```python
def fun1(x, *y, z):
    print(x,y,z)
fun1(1, 2, 3, 4, 5, 6, z=7) #1 (2, 3, 4, 5, 6) 7
```

- 关键字参数 `**kw` - <mark>接收字典</mark>；**kwargs 用于解包关键字参数，将它们打包成一个字典；这使得函数可以接受不定数量的关键字参数；关键字参数需写在可变参数后面

```python
实例1
def myFunc(a,*b,**c):
    print(a,b,c)
myFunc(1,2,3,4,x=5,y=6)     # 1 (2,3,4) {'x':5, 'y':6}

实例2
g_country ="China"
g_list =[6,6,6]
g_tuple = (1,2,3)
g_list1 = [1,2,3,4]
g_dict= {"name":"zcx","age":"25"}

if __name__ == '__main__':
    def fun(country , lst, *args, **kwargs):

        country  ="English"
        lst.append(6)

        args =(3,2,1)

        kwargs.update({'job':"Engineer"})
        kwargs['age'] =22

        print(country, lst, args, kwargs)

    fun(g_country,g_list,*g_tuple,**g_dict)   # English [6, 6, 6, 6] (3, 2, 1) {'name': 'zcx', 'age': 22, 'job': 'Engineer'}
    print(g_country,g_list,g_tuple,g_dict)    # China [6, 6, 6, 6] (1, 2, 3) {'name': 'zcx', 'age': '25'}

    dict1 = {'a': 1, 'b': 2, 'c': 3}
    def modify_dict1(my_dict1):
        my_dict1.update({"d":4})
        print("Inside function:", my_dict1)  # Inside function: {'a': 1, 'b': 2, 'c': 3, 'd': 4}
    modify_dict1(dict1)
    print("Outside function:", dict1)        # Outside function: {'a': 1, 'b': 2, 'c': 3, 'd': 4}

    dict2 = {'a': 1, 'b': 2, 'c': 3}
    def modify_dict2(**my_dict2):
        my_dict2.update({"d":4})
        print("Inside function:", my_dict2)  # Inside function: {'a': 1, 'b': 2, 'c': 3, 'd': 4}
    modify_dict2(**dict2)
    print("Outside function:", dict2)        # Outside function: {'a': 1, 'b': 2, 'c': 3}
```

- 命名关键字参数 - 分隔符* 后的参数被视为命名关键字参数，命名关键字参数必须传入参数名

```python
def fun(a,b,*, c,d) :  # ab是位置参数，cd 是关键字参数
    print('a=',a,'b=',b,'c=',c,'d=',d)
fun( 10,20,c=30,d=40)   # 调用正确 , a= 10 b= 20 c= 30 d= 40

def person(name, age, *args, city='nj', job):  # 如果已有一个可变参数，后面的命名关键字参数就不再需要分隔符*了；命名关键字参数可以有缺省值
    print(name, age, args, city, job)

person('Jack', 24, city="yz",job='Engineer') # Jack 24 () yz Engineer
```

###### 5.3 参数类型检查 isinstanc

```python
def my_abs(x):
    if not isinstance(x, (int, float)): #只允许整数和浮点数。 
        raise TypeError('bad operand type')
    if x >= 0:
        return x    #如果函数没有return语句，执行完毕后 会返回 None。return None可以简写为return
    else:
        return -x
print(my_abs(-99))  # 99
print(my_abs)       # <function my_abs at 0x000001C3D121A020>  函数名保持的是函数的内存地址
```

###### 5.4 返回值：返回多个值，用元组；无返回值，用 return；

```python
def  fun(num) :
    odd =[]
    even=[]
    for i  in num :
        if i %2 :      
            odd.append(i) 
        else:
            even.append(i)           
    return odd,even                 # 函数返回的类型是元组，小括号可以省略

print(fun([10,29,34,23,44,53,55]))  # 多个返回值 是元组  ( [29,23,53,55],[10,34,44] )
```

###### 5.5 global & nonlocal

> 要在函数内部改变全局变量，须加上global 声明
> 
> 嵌套函数的内部函数可以访问外部函数的变量，但是无法修改，若要修改，须加上nonlocal

###### 5.5 递归函数

> 每递归调用一次，都会在栈内存分配一个栈帧，每执行完，都会释放相应的空间；
> 
> 包含：递归调用 + 递归终止条件；步骤：递推+回归；
> 
> 递归函数的优点是逻辑简单清晰，缺点是过深的调用会导致栈溢出。解决递归调用栈溢出是通过尾递归优化。
> 
> 尾递归：指在函数返回时，调用自身本身，并且 return语句不能包含表达式。这样，编译器/解释器就可以做尾递归优化，使递归本身无论调用多少次，都只占用一个栈帧，不会出现栈溢出；Python标准的解释器没有针对尾递归做优化，任何递归函数都存在栈溢出的问题。

```python
实例：递归
def fact(n) ;
     if n==1:
           return 1
     else :
           return  n*fact(n-1)  # return n * fact(n - 1)引入了乘法表达式，所以不是尾递归

print(fact(5))  # 120
=> fact(5)
=> 5 * fact(4)
=> 5 * (4 * fact(3))
=> 5 * (4 * (3 * fact(2)))
=> 5 * (4 * (3 * (2 * fact(1))))
=> 5 * (4 * (3 * (2 * 1)))
=> 5 * (4 * (3 * 2))
=> 5 * (4 * 6)
=> 5 * 24
=> 120
fact(1000)  # 栈溢出

实例：尾递归优化
def fact(n):
    return fact_iter(n, 1)

def fact_iter(num, product):
    if num == 1:
        return product
    return fact_iter(num - 1, num * product)   # return fact_iter(num - 1, num * product)仅返回递归函数本身，num - 1和num * product在函数调用前就会被计算，不影响函数调用

print(fact(5))  # 120
=> fact(5) 
=> fact_iter(5, 1)
=> fact_iter(4, 5)
=> fact_iter(3, 20)
=> fact_iter(2, 60)
=> fact_iter(1, 120)
=> 120
```

###### 5.6 匿名函数  `lambda arg1, arg2,... : expression`  lambda 是一个表达式，并不是一个语句；只能写成一行

```python
实例 
squarey = lambda  y: y*y
>>> squarey(3)  # 9
```

###### 5.7 闭包函数

> 函数嵌套定义、内部函数可以引用外部函数的参数/局部变量、外部函数返回内部函数名，相关参数和变量都保存在返回的函数中；这种程序结构称为闭包
> 
> 返回闭包时：外部函数返回的函数通常赋于一个变量( 可在后面被执行调用 )。返回函数不要引用任何循环变量，或者后续会发生变化的变量
> 
> 使用闭包时：返回函数对外层变量赋值时，先用 nonlocal 声明该变量不是当前函数的局部变量，是外层函数的局部变量；对外层变量仅读取不需要nonlocal

```python
实例1
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    print(sum)              # <function lazy_sum.<locals>.sum at 0x000001EC64EF4900>
    return sum              # 返回内部函数地址
f = lazy_sum(1, 3, 5, 7, 9) # 调用lazy_sum()时，返回的并不是求和结果，而是求和函数f
print(f)                    # <function lazy_sum.<locals>.sum at 0x000001EC64EF4900> f 即 内部函数 sum
print(f())                  # 调用函数f时，才真正计算求和的结果  25

实例2
def nth_power(exponent):
    def exponent_of(base):
        return base ** exponent
    return exponent_of  # 返回值是 exponent_of 函数

square = nth_power(2)
cube = nth_power(2)
print(square(2))  # 4，base = 2;
print(cube(3))    # 9，base = 3;
```

###### 5.8 常用内置函数

- eval() 将字符串当成 表达式求值 ，并返回计算结果

```python
eval("1+1")                                      # 2
print(type(eval("[1,2,3]")))                     # <class 'list'>
print(type(eval("{'name':'zcx','age':'20'}")))   # <class 'dict'>
```

- input() 获取用户的键盘输入，输入内容的类型是str ; age = int(input('请输入年龄'))

- min()、max()、sum()、abs()、pow()、round()

```python
print(abs(-3))                   # 3,绝对值 
print(pow(2, 3))                 # 8,pow(x,y) : 求x的y次方，x^y
print(round(123.486, 2))         # 123.49 round(num,n) : 四舍五入
```

- sort()     应用在列表上的方法，直接修改原列表

- sorted() 返回的是一个新的列表，而不是在原来的基础上进行的操作

```python
a = [5, 7, 6, 3, 4, 1, 2]
b = sorted(a) # 保留原列表
print(a) # 输出: [5, 7, 6, 3, 4, 1, 2]
print(b) # 输出: [1, 2, 3, 4, 5, 6, 7]  

students = [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
print(sorted(students, key=lambda s: s[2]))               # 输出: [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
print(sorted(students, key=lambda s: s[2], reverse=True)) # 输出: [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]

print(sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower))               # ['about', 'bob', 'Credit', 'Zoo']
print(sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower, reverse=True)) # ['Zoo', 'Credit', 'bob', 'about']
```

- all()    判断可迭代对象中所有元素，是否都是真

- any()  判断可迭代对象中是否存在为真的元素

- enumerate() 返回一个枚举对象，[ ( 0开始的序号，可迭代对象的元素 ) ]

```python
names = ['Mike', 'John', 'Rose']
for index, name in enumerate(names):
    print(index, name)   # 0 Mike   1 John   2  Rose
```

- range(start,stop,step)  创建start～stop-1之间的整数序列；start 不指定，默认0；步长是step，不指定，默认1；

- zip()  将可迭代对象作为参数，将对象中对应的元素打包成一个元组，返回元组组成的列表；

```python
x=[1,2,3]
y=[4,5,6]
for i,j in zip(x,y):   # [(1, 4), (2, 5), (3, 6)]
    print(i,j)
```

- 类型转换 

```python
int(“123”) # 转换为整形
int("9.8")   # err
int(9.8)     # 转换为整数 9

float('9,8') # 转换为浮点数 9.8
float(9)     # 转换为浮点数 9.0

str(123)     # 转换为字符串 123
str(1.23)    # '1.23'

list()       # 将可迭代对象，转换列表
tuple()      # 将可迭代对象，转换元祖
dict()       # 将可迭代对象，转换字典
str()        # 将可迭代对象，转换字符串

ord()        # 获得指定字符的原始值-Unicode码
chr()        # 获得原始值对应的字符 chr(97)  'a'

bin()        # 将十进制数转换为二进制数；print('{0:b}'.format(10))
oct()        # 将十进制数转换为八进制数；print('{0:o}'.format(12))
hex()        # 将十进制数转换为十六进制表示的字符串；print('{0:x}'.format(12))

print("二进制转十进制:", int('1010', 2))  # 二进制转十进制
print('{0:d}'.format(0b11))
print(eval('0b11'))

print("八进制转十进制:", int('014', 8))  # 八进制转十进制
print('{0:d}'.format(0o14))
print(eval('0o14'))

print("十六进制转十进制:", int('0xc', 16))  # 十六进制转十进制
print('{0:d}'.format(0x1f))
print(eval('0x1f'))

bool(1)           # True
bool('')          # False   
```

- 对象类型相关

```python
id()                # 查看数据的内存地址(10进制的内存地址)
dir(对象名)         # 查看对象的所有的属性和方法
dir('ABC')          # ['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']

hasattr(对象名，方法名/属性名)   # 判断对象的方法/属性是否存在
setattr(对象名，属性名，属性值)  # 设置对象的属性值
getattr(对象名，方法名/属性名)   # 获取对象的方法名/属性名

type()               # 查看变量类型

isinstance(对象，类) # 判断的是一个对象是否是该类型本身，或者位于该类型的父继承链上
issubclass(子类，父类)
>>> isinstance('a', str)    # True
>>> isinstance(123, int)    # True
>>> isinstance(b'a', bytes) # True
>>> isinstance([1, 2, 3], (list, tuple)) # True
>>> isinstance((1, 2, 3), (list, tuple)) # True
```

#### 6. 模块与包

- **模块**：一个.py文件就是一个模块；目的：可重用+避免函数名/变量名冲突+供其他模块导入；

- **包**：有__init__.py 文件(指定对外提供的模块列表)+多个模块的目录；将功能相关模块组织在一个目录下；目的：避免模块名称冲突

- **模块导入** python解释器 在导入模块时，会先搜索当目录下指定的模块文件，有直接导入，没有再搜索系统目录
  
  ```python
  import  包名
  import  包名.模块名 [as 别名]
  import  模块名  [ as 别名 ]              #导入所有，需要  模块名.  |  别名.  访问
  
  from 包名  import  模块名
  from 包名.模块名 import  函数/变量/类
  from 模块名  import  函数/变量/类        #导入部分，不需要  模块名.   可直接访问
  
  实例：init.py
  from .  import module_name    #从当前目录下导入模块
  from .. import module_name    #从上级目录下导入模块
  from .  package_name import module_name   #从当前目录下导入包的模块
  from .. package_name import module_name   #从上级目录下导入包的模块
  ```

- **模块发布**
  
  ```python
  1,创建 setup.py
  from distutils.core import setup
  setup( name="hm_message",   # 包名
         version="1.0.1",     # 版本
         author="ZZL",        # 作者
         author_email="xxx",  # 作者邮箱
         url='xxx',           # 主页   
         description="xxx",                  # 描述信息
         long_description=long_description,  # 完整的描述信息
         long_description_content_type="text/markdown",
         py_modules=["hm_message.send_message","hm_message.receive_message"]) #包中模块
  
  2,构建模块       $ python3 setup.py build  #会生成包 bulid/lib/hm_message/模块文件
  3,生成发布压缩包 $ python3 setup.py sdist  #会生成dist/hm_message-1.0.tar.gz
  4,安装模块       $ tar -zxvf  hm_message-1.0.tar.gz
                   $ cd hm_message-1.0 
                   $ python3 setup.py install
  5,卸载模块       $ cd usr/loacl/lib/python3/5/dist-packages/
                   $ sudo rm -r  hm_message
  ```

#### 7. Python 内置模块 - 标准库

- os 模块 - 提供与操作系统交互的功能，如文件、目录、环境变量的操作
- sys 模块 - 提供与 Python 解释器交互的功能，如命令行参数、退出程序等 
- time 模块 - 提供时间相关的功能，如延时、时间戳等。    
- datetime 模块 - 日期和时间处理。
- calendar 日历模块 - 提供与日历相关的函数 
- uuid 模块 - 通用唯一识别码
- math 模块 - 提供数学运算函数，如三角函数、对数、幂运算等。
- random 模块 -提供生成随机数的功能
- decimal 用于进行精确控制运算精度、有效数位和四舍五入操作的十进制运算
- copy 模块 - 深拷贝、浅拷贝
- pickle - Python 对象序列化与反序列化
- json 模块 - 处理 JSON 数据的编码和解码
- yaml 模块 -  yaml格式的读取和转化
- hashlib 哈希模块 - 哈希算法（MD5、SHA256 等）
- re 模块 - 提供正则表达式操作
- collections 模块 - 提供额外的数据结构，如 `defaultdict`、`Counter` 等
- itertools 模块 - 提供迭代器工具，用于高效循环操作；返回值不是list，而是Iterator，只有用for循环迭代的时候才真正计算
- argparse 模块 - 命令行参数解析
- logging 模块 - 日志记录（调试、错误追踪等）
- subprocess 模块 - 用于创建和管理子进程
- threading 模块 - 提供多线程支持
- multiprocessing 模块 - 提供多进程支持
- http 模块 - 提供 HTTP 协议相关的功能。
- urllib 模块 - 处理 URL 请求（HTTP/HTTPS 客户端）
- socket 模块 - 网络通信（TCP/UDP 编程）
- csv 模块 - 读写 CSV 文件
- sqlite3 模块 - 操作 SQLite 数据库
- configparser 模块 - 读写配置文件
- base64 模块 - 一种用64个字符来表示任意二进制数据的方法；
- heapq 堆模块 - 提供了对最小堆( 默认 )的建立和使用
- queue队列模块 - 同步的、线程安全的队列，有很多的结构，比如栈 LifoQueue ，适用于在并发环境；栈为空时，去弹出栈顶元素，会出现阻塞，直到栈不为空为止

#### 8. 第三方模块

- numpy: 提供高效的数值计算功能，尤其是数组操作。
- pandas: 提供数据处理和分析功能，尤其是表格数据。
- matplotlib: 提供数据可视化功能。
- scipy: 提供科学计算功能，如积分、优化、信号处理等。
- requests: 提供 HTTP 请求功能，比标准库的 urllib 更易用。
- beautifulsoup4: 提供 HTML 和 XML 解析功能。
- flask: 提供轻量级 Web 开发框架。
- django: 提供全功能 Web 开发框架。
- sqlalchemy: 提供 ORM（对象关系映射）功能，用于数据库操作。
- pytest: 提供单元测试框架。
- scikit-learn: 提供机器学习算法和工具。
- tensorflow / pytorch: 提供深度学习框架。
- openpyxl: 提供 Excel 文件读写功能。
- pillow: 提供图像处理功能。
- pygame: 提供游戏开发功能。
- asyncio: 提供异步 I/O 支持。
- aiohttp: 提供异步 HTTP 客户端和服务器功能。
- click: 提供命令行工具开发功能。
- tqdm: 提供进度条功能。

## 二、进阶知识点

#### 1. 面向对象编程

###### 1.1类与对象

> 程序运行时，类也会加载到内存；类对象在内存中只有一份，通过 类名. 可以访问类属性和类方法
> 每个实例对象都有自己的独立空间，保存各自不同的属性；调用方法时，需要把对象的引用传递到方法内部

- 定义类

```python
class Student(object):              # 定义:类对象； 类对象()表示类的实例化(创建实例对象)； 函数名() 表示函数调用
    native_place ='吉林'            # 定义:类属性 native_place,被类的所有实例对象所共享
    def __init__(self , name, age): # 定义:构造函数/初始化方法；参数 self ，会自动传，表示调用此方法的当前实例对象 stu；
           self.name = name         # 定义:实例属性 name   
           self.age = age           # 定义:实例属性 age  
           self.address ="中国"     # 定义:默认属性，所有的实例对象都会有address

    def info(self):                 # 定义:实例方法(有参数self);用于需要访问实例属性的场景
           print('我的名字叫：'. self.name, '年龄是：',self.age，‘我住在：’ , Student.native_place)  

    @classmethod                                           
    def cm(cls):                    # 定义：类方法;用 @classmethod 装饰器修饰; 参数 cls 表示类对象 Student，可有其他参数；用于只访问类属性的场景
           print('类方法'  ,f('地址是：{cls.native_place }'))

    @staticmethod    
    def sm():                      # 定义：静态方法；用 @staticmethod 装饰器修饰; 用于不需要访问类属性、实例属性的场景；    
           print('静态方法')

stu = Student( 'Jack',20 )          # 位置传参；stu 是实例对象
stu = Student( name='Jack',age=20 ) # 关键字传参

print(stu.name)                     # 获取实例属性
stu.native_place = '吉林123'        # 定义：动态属性/实例属性，和原来的类属性没有关系
print(stu.native_place )            # 获取动态属性,  '吉林123'  
print(Student.native_place )        # 获取类属性,  '吉林'  

stu.info()         # 调用实例方法
Student.info(stu)  # 调用实例方法
Student.cm()       # 调用类方法
Student.sm()       # 调用静态方法

print(id(Student))     # 2926222013040
print(type(Student))   # <class 'type'> ；说明 Student 是由 type创建
print(id(stu))         # 1421362237888
print(type(stu))       # <class '__main__.Student'>；说明 stu 由 Student 创建
print(stu)             # <__main__.Student object at  0x000014AEFCA91C0>  ;  1421362237888 转成16进制  
```

- 类的封装 & 私有属性 & 私有方法 - 不希望在外部访问，用 ‘__’  开头属性/方法，仅类内部使用，无法被继承

```python
class ICBC(object):                   # 类对象 
     def __init__(self,name):
           self.name='工商银行'       # 公有属性
           self.__money =0            # 私有属性，余额
     def get_money(self):             # 公有方法
           print(f'当前卡的余额为：{self.__money}')
    def set_money(self,money):        # 公有方法
           self.__money +=money

icbc =ICBC("中国农行")
print(dir(icbc))            # 查看icbc 的所有的属性和方法
print(icbc.__money)         # ERR
print(icbc._ICBC__money)    # 可以通过  _类名__属性  来访问私有属性、 _类名__方法 来访问私有方法

icbc.__money =10000         # 新增了一个动态属性
print(icbc.__money )        # 10000   
icbc.get_money()            # 0
icbc.set_money(500)  
icbc.get_money()            # 500
```

- 计算属性/只读属性 - 在绑定属性时，如果直接把属性暴露出去，则无法检查参数；可以重新实现属性的setter和getter来解决；

```python
实例1
class Goods():
    def __init__(self,unit_price,weight):
        self.unit_price = unit_price
        self.weight = weight

        @property   # 作为方法的装饰器。可将类方法转变成类属性。用于值处理、转换处理
        def price(self):
            return self.unit_price * self.weight     

lemons = Goods(7,4)
lemons.price       # 28 

实例2
from datetime import date,datetime
class User:
    def __init__(self,name,birthday):
        self.name = name
        self.birthday = birthday
        self._age = 0

    @property       # 内置装饰器@property可把类方法变成属性
    def age(self):  # 重新实现一个属性的getter方法；有且只能有self一个参数
        return datetime.now().year - self.birthday.year

    @age.setter     # 重新实现一个属性的setter 方法；当不实现setter方法时，就是一个只读属性
    def age(self, value):
         if not isinstance(value, int):
             raise ValueError('score must be an integer!')
         if value < 0 or value > 100:
             raise ValueError('score must between 0 ~ 100!')
         self._age = value

if __name__ == "__main__":
    user = User("boddy",date(year=2020,month=1,day=1))
    user.age = 55
    print(user._age)  # 55
    print(user.age)   # 3
```

- 动态绑定

```python
实例1：创建对象后，可以给该对象动态地绑定属性和方法
class Student(object):
    def __init__(self , name, age ) :  
           self.name = name    # 实例属性 name , age  
           self.age = age  
    def show():
           print('我是一个函数')

stu1 = Student( '张三',20 )
stu2 = Student( '李四',30 )
stu1.gender = '男'              # stu1动态绑定性别属性; stu2 没有性别属性
stu1.show = show                # stu1动态绑定方法； stu2 没有show 方法

实例2：给class绑定方法
def set_score(self, score):         
       self.score = score
Student.set_score = set_score   # 给class绑定方法后，所有实例均可调用

>>> stu1.set_score(100)
>>> stu1.score                  # 100
>>> stu2.set_score(99)
>>> stu2.score                  # 99

实例3：通过self 拿到 动态属性
class People(object):        
    def play(self):
        print(f'{self.name} 在玩耍')  

p1= People()
p1.name ='张三' 
p1.play()
```

- 限制绑定 `__slots__` 用于限制class实例能添加的属性；仅对当前类实例起作用，对继承子类不起作用

```python
class Student(object):
    __slots__ = ('name', 'age')  #用tuple指定允许绑定的属性

>>> s = Student()        #  创建新的实例
>>> s.name = 'Michael'   # 绑定属性'name'
>>> s.age = 25           # 绑定属性'age'
>>> s.score = 99         # ERR 绑定属性'score'   
```

###### 1.2 继承与多态：单继承、多继承、方法重写

> 封装 - 根据职责将属性和方法封装到一个抽象的类中
> 
> 继承 - 实现代码的重用
> 
> - 如果一个类没有继承任何类，则默认继承object；python类支持多继承
> 
> - 如果子类继承了多个父类，而多个父类中有同名的属性/方法，子类在调用时，会使用第一个父类的属性/方法
> 
> - 如果子类继承了多个父类，还想去调用父类中同名的方法，可以使用 父类名.父类方法名(self)调用
> 
> - 子类重写父类方法时，可以用 <mark>super().父类方法</mark>，来执行父类的方法，用于对父类方法进行扩展
> 
> 多态 - 不同的对象调用相同的方法，产生不同的结果，实现代码的灵活度

```python
实例：继承
class Person(object):
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self.country = 'China'

    def info(self):
        print('姓名：{0}，年龄：{1} '.format(self.name, self.age))

    def info1(self):
        print("江苏")

class Student(Person):
    def __init__(self, name, age, score):
        super().__init__(name, age)
        self.score = score

    def info(self):                    # 方法重写
        # super(Student, self).info()  # 调用父类的方法（根据Student去找父类，找到后调用父类的info()方法）
        super().info()                 # 调用父类的方法(简写)，对父类方法扩展 ，python3.x 语法
        # Person.info(self)            # 调用父类的方法，python2.x 语法
        print('学号：{0}'.format(self.score))

        print(self.country)            # 调用父类的公有属性
        self.info1()                   # 调用父类的公有属性

stu = Student('jack', 20, '1001')
stu.info()

输出：
姓名：jack，年龄：20 
学号：1001
China
江苏
```

- 抽象基类 - 不能实例化；仅用于被继承；继承抽象基类，必重载其内部方法

```python
import abc                                  # 导入python全局的抽象基类 
class CacheBase( metaclass = abc.ABCMeta )  # 定义抽象类
    @abc.abstractmethod                     # 声明抽象方法
    def get(self,key):
        pass

    @abc.abstractmethod             
    def set(self,key,value):
        pass

class RedisCache(CacheBase):  # 继承抽象基类, 必须重载内部方法
    def get(self,key) :       # 必须要实现抽象类的抽象方法，才可以使用RedisCache 类
        pass
    def set(self,key,value):  # 必须要实现抽象类的抽象方法，才可以使用RedisCache 类
        pass

redis_cache = RedisCache()
```

- 多继承：不同继承关系，有不同的调用顺序（内部实现：c3算法）
  
  - 深度优先的调用顺序
  
  - 广度优先的调用顺序

```python
class E:
    pass
class D:
    pass
class C(E):
    pass
class B(D):
    pass
class A(B, C):  # 多继承
    pass

print(A.__mro__)  # 输出的顺序就是调用顺序

输出:深度优先
(<class '__main__.A'>, <class '__main__.B'>, <class '__main__.D'>, <class '__main__.C'>, <class '__main__.E'>, <class 'object'>)

class D:
    pass
class C(D):
    pass
class B(D):
    pass
class A(B, C):
    pass
print(A.__mro__)  # 输出的顺序就是调用顺序

输出:广度优先
(<class '__main__.A'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.D'>, <class 'object'>)
```

- 多态
  
  - 静态语言多态的3个条件：继承、方法重写、父类引用指向子类对象；
  
  - 对于Python这样的动态语言，<mark>多个类实现了同名方法，即实现多态</mark>  

```python
实例：多态
class Animal(object):
    def eat (self):
        print('动物会吃')
class Dog(Animal):
    def eat (self):
        print('狗吃肉')
class Cat(Animal):
    def eat (self):
        print('猫吃鱼')
class Person(Animal):
    def eat (self):
        print('人吃五谷杂粮')

def fun(obj):
    obj.eat()

if __name__ =='__main__':
    fun(Dog())          # 狗吃肉
    fun(Cat())          # 猫吃鱼
    fun(Animal())       # 动物会吃
    fun(Person())       # 人吃五谷杂粮
```

###### 1.3 魔术方法：用于类功能扩展和增强、定制类；双下划线开头和结尾的是特殊变量，可直接访问

- `__file__`         # 查看导入模块的路径
- `__name__`         # 当前模块下时值为 __main__；导入到其他文件时值为模块名；用于判断，只在当前模块下执行；
- `__dict__`         # 获得类对象/实例对象所绑定的所有属性和方法的字典
- `__dir__`           # 查看对象的所有的属性和方法
- `__class__`        # 获得对象所属的类型
- `__bases__`        # 获得所有的父类的类型
- `__base__`         # 获得最近的父类的类型; class C(A,B)   获取A的类型
- `__subclasses__` # 获得所有的子类的类型
- `__mro__`           # 查看类的层次结构（有继承关系下类的调用顺序）

```python
  class Animal(object):
    def eat(self):
        print('正在吃...')
  class Dog(Animal):
    def eat(self):
        print('正在啃骨头...')
  class XTQ(Dog):
    def eat(self):
        super().eat()           # 正在啃骨头...
        super(XTQ,self).eat()   # 正在啃骨头... ；找XTQ的父类
        super(Dog,self).eat()   # 正在吃...   ；找Dog的父类
        print('啸天犬吃蟠桃...')

xtq= XTQ()
xtq.eat()
print(XTQ.__mro__)  

输出
正在啃骨头...
正在啃骨头...
正在吃...
啸天犬吃蟠桃...
(<class '__main__.XTQ'>, <class '__main__.Dog'>, <class '__main__.Animal'>, <class 'object'>)
```

- `__call__`   在实例本身上调用，__call__还可定义参数。对实例直接调用就好比对一个函数调用一样 

```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __call__(self):
        print('My name is %s.' % self.name)

s = Student('Michael')
s()  # My name is Michael.
```

- `__new__` 创建对象时自动调用：给对象分配空间后，返回对象的引用给解释器

```python
class Person(object):
    def __new__(cls, *args, **kwargs):  # 会将Person 类传进来
        print(' __new__被调用执行了，cls的id 为{0}'.format(id(cls)))
        obj = super().__new__(cls)  # 1，为对象分配空间；重写__new__，一定要返回super().__new__(cls)；此处的super() 表示 object
        print(' 创建的对象的id 为{0}'.format(id(obj)))
        return obj  # 2，返回对象的引用；会将 obj 返回到 __init__() 的self 中

    def __init__(self, name, age):  # __init__() 执行结束后，会将self 赋值给 p1
        print(' __init__被调用执行了，self的id 为{0}'.format(id(self)))
        self.name = name
        self.age = age


print('object类对象的id 为{0}'.format(id(object)))
print('Person类对象的id 为{0}'.format(id(Person)))
p1 = Person('张三', 20)
print('p1实例对象的id 为{0}'.format(id(p1)))

输出
object类对象的id 为140707766439408
Person类对象的id 为2159689709808
__new__被调用执行了，cls的id 为2159689709808
创建的对象的id 为2159688603344
__init__被调用执行了，self的id 为2159688603344
p1实例对象的id 为2159688603344

实例：单例模式
class MusicPlayer(object):
    instance = None     #类属性
    init_flag = False   #类属性

    def __new__(cls, *args, **kwargs):
        if cls.instance is None:   #判断类属性，实现单例
            cls.instance = super().__new__(cls)
            return  cls.instance
        return cls.instance

    def __init__(self):
        if MusicPlayer.init_flag:  #判断类属性，只初始化一次
            return
        print("初始化")
        MusicPlayer.init_flag = True

p1 = MusicPlayer()
p2 = MusicPlayer()
print(p1)
print(p2)

输出
初始化
<__main__.MusicPlayer object at 0x000002E4CE38D590>
<__main__.MusicPlayer object at 0x000002E4CE38D590>
```

- `__init__` 创建对象时自动调用：解释器拿到对象的引用后，给对象属性设置初始值； 
- `__del__`  对象从内存中销毁前(对象引用计数为0)，会自动调用；当程序结束时也会自动调用；是python的内存管理机制。

```python
name ='张三'    # 引用计数1
name1 = name    # 引用计数2
del  name       # 手动删除对象      
del  name1      # __del__() 被调用
```

- `__getattr__`  动态返回一个属性/函数，只有在没有找到属性的情况下，才调用 `__getattr__`

```python
class Student(object):
    def __init__(self):
        self.name = 'Michael'
    def __getattr__(self, attr):
        if attr=='score':   # 返回属性
            return 99
        if attr=='age':      # 返回函数
            return lambda: 25
s = Student()
print(s.name)    # 'Michael'
print(s.score)   # 99
print(s.age())   # 25
```

- `__str__`       返回对象的描述(默认是对象的内存地址)；自定义print() 输出
- `__iter__`      返回一个迭代器（iterator）；使类的实例对象，变为可迭代对象
- `__next__`      返回容器的下一个元素
- `__getitem__` 按照下标取出元素

```python
实例1
class Student:
    def __init__(self,student_list):
        self.student = student_list
    def __getitem__(self,item):
        return self.student[item]

if __name__ =='__main__':
    stu = Student(['aaa','bbb','ccc'])
    for item in stu:
        print(item)    # aaa  bbb ccc

实例2
class Fib(object):  # 写一个Fib类，可以作用于for循环
    def __init__(self):
        self.a, self.b = 0, 1  # 初始化两个计数器a，b

    def __iter__(self):
        return self       # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b  # 计算下一个值
        if self.a > 20:  # 退出循环的条件
            raise StopIteration()
        return self.a    # 返回下一个值

    def __getitem__(self, n):   # 按照下标取出元素，需要实现__getitem__()方法
        if isinstance(n, int):  # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a

        if isinstance(n, slice):  # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L

if __name__ == '__main__':
    for n in Fib():
        print(n)  # 1 1 2 3 5 8 13
    f = Fib()
    print(f[0])  # 1
    print(f[1])  # 1
    print(f[2])  # 2
    print(f[3])  # 3
    print(f[4])  # 5
    print(f[:5])  # [1, 1, 2, 3, 5]
    print(f[2:5])  # [2, 3, 5]
```

- with上下文管理器 `__enter__()、__exit__()`
- 数值转换 `__bool__()、__int__()、 __float__()、__hash__()、__index__()`
- 协程相关：`__await__() 、__aiter__() 、__anext__()、__aenter__()、__aexit__()`
- 一元运算：`__neg__() -运算、__pos__() +运算、__abs__() 取绝对值`
- 二元运算：`__it__() <、__ie__() <=、 __eq__() ==、 __ne__() !=、__gt__() > 、__ge__() >=`
- 算术运算 `__add__() +、__sub__() -、 __mul__() *、__truediv__() / 、__floordiv__() // 、__mod__() % 、__divmod__() divmod()、__pow__() ** 或者 pow() 、 __round__() round()`
- 反向算术运算`__radd__()、 __rsub__()、__rmul__()、__rtruediv__()、 __rfloordiv__()、__rmod__()、__rdivmod__()、__rpow__()`  
- 增量赋值算术运算 `__iadd__()、__isub__()、__imul__()、__itruediv__()、__ifloordiv__()、 __imod__()、__ipow__()`  
- 位运算 `__invert__() ～、 __lshift__() <<、__rshift__() >>、__and__() &、__or__() |、__xor__() ^`

```python
实例1
class Nums:
    def __init__(self,num):
        self.num = num
    def __abs__(self):
        return abs(self.num)

my_num = Nums(-3)
print(abs(my_num )) # 3


实例2
class Student:
    def __init__(self, name):
        self.name = name

    def __add__(self, other):  # 重写__add__()，使自定义对象具有 “+” 功能
        return  self.name + other.name

stu1 = Student('张三')
stu2 = Student('李四')
s = stu1 + stu2  
print(s)  # 张三李四
```

#### 2. 异常处理

- **异常类型**：`Exception`, `TypeError`, `ValueError` 等
  
  - AssertionError      当 assert 关键字后的条件为假时，程序运行会停止并抛出异常
  - AttributeError        当试图访问的对象属性不存在时 
  - IndexError             索引超出序列范围
  - KeyError               字典中查找一个不存在的关键字 
  - NameError            尝试访问一个未声明的变量
  - TypeError              不同类型数据之间的无效操作
  - ZeroDivisionError  除法运算中除数为 0
  - SyntaxError           pytyon语法错误
  - ValueError             传入无效的参数；  a=int('hello')
  - Exception              所有异常的父类

- **异常捕获**：`try`, `except`, `else`, `finally` 一般在主函数中进行异常捕获，主函数中调用的其他函数出现异常，都会传递到主函数的异常捕获中

```python
    try :       
        1/0
        123+"abc'
        print("有异常时，不会被执行到这里")
    except  ZeroDivisionError as e :
        print("除数不能为0",e)
    except   ValueError as e :
         print("值不正确",e)
    except  TypeError as e:
         print("类型不正确",e)
    except (ZeroDivisionError, ValueError, TypeError) as e:  # 在一个元组中指定这些异常
         print('Error: {}'.format(err))
    except  Exception as  e:                                  #所有错误类型都继承自 Exception
         print("出错了",e)
    else:   
        print("没有异常时，会执行")
    finally:
        print("无论是否产生异常，都会执行；用于释放资源：关闭文件/数据库连接")
```

- 手动抛出异常 `raise`，raise语句如果不带参数，就会把当前错误原样抛出。  

```python
  try:
     score = int(input('请输入分数：'))
     if 0<=score<=100:
        print('分数为：',score)
     else:
        raise Exception('分数不正确')
  except Execption as  e:
     print(e)   # 捕获错误目的只是记录一下，便于后续追踪
     raise      # 由于当前函数不知道应该怎么处理该错误，所以继续往上抛，让顶层调用者去处理。
```

- **自定义异常**：继承 `Exception` 类

#### 3. 高级特性

###### 3.1 可迭代对象 iterable  - 是实现了`__iter__()`方法的对象。调用`iter()`方法会返回一个迭代器。

> 常见的可迭代对象包括列表（list）、元组（tuple）、字符串（str）、字典（dict）等；<mark>可以多次遍历</mark>、<mark>把所有元素加载到内存</mark>；
> 
> 可迭代对象可以通过 *for* 循环直接遍历，或者通过 *iter()* 函数转换为迭代器。

```python
from collections.abc import Iterable,Iterator
print(isinstance('abc', Iterable))                  # True  str是否可迭代
print(isinstance([1,2,3], Iterable))                # True  list是否可迭代
print(isinstance((x for x in range(10)), Iterable)) # True
print(isinstance(123, Iterable))    
```

###### 3.2 迭代器 Iterator - 是实现了`__iter__()` 和 `__next__()` 方法的对象。`__next__()`用于返回下一个元素，当没有更多元素时会抛出 *StopIteration* 异常

> <mark>只能遍历一次</mark>、<mark>是按需加载到内存</mark>，调用 `next()`，并不断返回下一个值的对象； 
> 
> 迭代器则可以使用 *next()* 函数逐个获取元素

```python
from collections.abc import Iterable,Iterator
print(isinstance((x for x in range(10)), Iterator)) # True
print(isinstance([], Iterator))                     # False
print(isinstance({}, Iterator))                     # False
print(isinstance('abc', Iterator))                  # False
```

###### 3.3 生成器(Generator)：使用 `yield` 关键字 ，即是生成器；包含 yield 语句的函数被称为生成器

> 是一个特殊的迭代器；不需要像普通迭代器一样实现`__iter__()`和`__next__()`方法了，只需要一个`yield`关键字；
> 
> 任何生成器是一种懒加载的模式生成值；
> 
> 支持惰性计算和协程，常用于大文件、无限序列或实时流。<mark>生成器不是一次生成所有数据，每次生成一个数据</mark>

```python
实例1
gen = (i for i in range(3))
print(gen)
for i in gen:
    print(i) 

输出
<generator object <genexpr> at 0x000001EC9F77FED0>
0  \n  1   \n  2

实例2
def counter():
    i =0
    while i<3 :
         yield i    # 每调用一次，就会提供一个数据，并且会记住当时的状态
         i+=1

for i in counter():
    print(i)

输出:0  \n  1   \n  2

实例3
def doubeNumber(i):
    return i * 2
def testYield(n):
    for i in range(n):
        print("当前值: ", i)
        yield doubeNumber(i)   # 遍历时，遇到yield语句中断返回，再次执行时从上次返回的yield语句处继续执行
        print("第 ", i, " 次运行")
    print("testYield 运行结束")

if __name__ == '__main__':
    for i in testYield(3):
        print(i, "===", i)
输出
当前值:  0
0 === 0
第  0  次运行

当前值:  1
2 === 2
第  1  次运行

当前值:  2
4 === 4
第  2  次运行

testYield 运行结束ield 运行结束
```

###### 3.3 装饰器(Decorator)：函数装饰器、类装饰器

> 用于在不修改原函数代码的情况下，为函数<mark>添加额外的功能</mark>。它<mark>本质上是一个闭包</mark>，接受另一个函数作为参数，并返回一个新的函数。
> 
> 修饰器的执行原理：当使用 *@修饰器名* 时，Python 会将被修饰的函数作为参数传递给修饰器函数，并用修饰器的返回值替换原函数
> 
> 用于一些切面场景：身份认证、权限校验、日志记录、输入合理性检查、缓存；
> 
> 类装饰器的实现是调用了类里面的__call__函数。当我们将类作为一个装饰器，工作流程： 
> 
> - 1，通过`__init__（）`方法初始化类
> - 2，通过`__call__（）`方法调用装饰方法

```python
实例1：my_decorator是一个装饰器，它接收一个函数func作为参数，并返回一个新的函数wrapper
def my_decorator(func):  # 定义闭包函数；fun ,即被修饰的函数 say_hello
    def wrapper():
        print("Something is happening before the function is called.")
        func()          
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator         # 使用闭包函数，执行装饰器后 say_hello= my_decorator(say_hello) ， say_hello相当于wrapper函数
def say_hello():      # 被修饰函数
    print("Hello!")

say_hello()           # 调用say_hello时，实际上是在调用wrapper函数

输出
Something is happening before the function is called.  \n    Hello!   \n    Something is happening after the function is called.

实例2：记录函数执行时间的装饰器
import time
from time import sleep

def timing_decorator(func): # 定义名为timing_decorator的装饰器，它可以用来测量任何函数的执行时间
    def wrapper(*args, **kwargs):
        start_time = time.time()
        sleep(2)
        result = func(*args, **kwargs)
        print(result)          # 3
        end_time = time.time()
        print(f"Function {func.__name__} took {end_time - start_time} seconds to execute.")
        return result
    return wrapper

@timing_decorator
def compute_sum(n):
    ret  = sum(range(3))
    return ret

compute_sum(3)

输出
3
Function compute_sum took 2.0010628700256348 seconds to execute.

实例3：装饰带参数函数
def repeat_decorator(num_times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat_decorator(3)
def greet():
    print("Hello!")

greet()

输出
Hello!  \n  Hello!   \n  Hello!

实例4：类来实现装饰器
import time
from time import sleep

class Timer:
    def __init__(self, func):
        self.func = func
    def __call__(self, *args, **kwargs):
        start = time.time()
        result = self.func(*args, **kwargs)
        end = time.time()
        print(f"执行时间: {end - start:.2f} 秒")
        return result

@Timer
def slow_function():
    time.sleep(2)
    print("函数执行完毕")

slow_function()

输出
函数执行完毕
执行时间: 2.00 秒

实例5：多个装饰器
def add(func):
    def inner():
        x = func()
        return x + 1
    return inner

def square(func):
    def inner():
        x = func()
        return x * x
    return inner

@add
@square  # 最先执行
def test():
    return 2

print(test())  # 65 = 2*2 +1
```

###### 3.4 上下文管理器：`with` 语句和 `contextlib` 模块

###### 3.5 描述符(Descriptor)：`__get__`, `__set__`, `__delete__` 方法

#### 4. 函数式编程

###### 4.1 高阶函数：`map()`, `filter()`, `reduce()、sorted()`、装饰器(是一种特殊的高阶函数，用于动态地增强函数的功能)

> 高阶函数是指能够接收其他函数作为参数，或者将函数作为返回值的函数。
> 
> 在 Python 中，函数是一等公民，可以像变量一样传递和使用。高阶函数的核心思想是通过函数的组合和传递实现更高的抽象和灵活性。

```python
实例：简单高阶函数
def add(x, y, f):
   return f(x) + f(y)

result = add(-5, 6, abs)
print(result) # 输出: 11

常用高阶函数
实例1
nums = [1, 2, 3, 4]
squared = map(lambda x: x**2, nums)           # map() 用于对可迭代对象的每个元素应用指定函数，返回一个新的迭代器
print(list(squared)) # 输出: [1, 4, 9, 16]  
实例2
nums = [1, 2, 3, 4, 5]
evens = filter(lambda x: x % 2 == 0, nums)   # filter() 用于筛选出满足条件的元素，返回一个新的迭代器
print(list(evens)) # 输出: [2, 4]
实例3
from functools import reduce
nums = [1, 2, 3, 4]
product = reduce(lambda x, y: x * y, nums)  # reduce() 用于对序列中的元素进行累积计算，最终返回一个单一结果
print(product) # 输出: 24
实例4
words = ["apple", "banana", "cherry"]
sorted_words = sorted(words, key=len)      # sorted() 用于对可迭代对象进行排序，支持自定义排序规则
print(sorted_words) # 输出: ['apple', 'banana', 'cherry']   
实例5
def log(func):
   def wrapper(*args, **kwargs):
       print(f"调用函数: {func.__name__}")
       return func(*args, **kwargs)
   return wrapper
@log
def add(a, b):
   return a + b
print(add(3, 5)) # 输出: 调用函数: add \n 8
```

###### 4.2 列表推导式：`[x for x in range(10) if x % 2 == 0]` 一次生产全部数据

- 列表生成式/推导式 [ expression for i in iterable if condition ]

> 先执行 for 语句，再执行 if 语句 ，最后执行 expression
> 
> 用于快速生成list ，性能高于列表操作；列表生成式中，for前面的if ... else是表达式，而for后面的if是过滤条件，不能带else

```python
print([i * 2 for i in  [1, 2, 3, 4, 5]])  # [2,4,6,8,10]
print([c * 2 for c in "FishC"])           # [ 'FF' , 'ii' , 'ss' ,'hh'  ,'CC' ]
print([[0] * 3 for i in range(3)])        # [ [0,0,0],[0,0,0],[0,0,0] ]
print([x if x % 2 == 0 else -x for x in range(1,11)]) # [-1, 2, -3, 4, -5, 6, -7, 8, -9, 10] 

实例：使用多个for循环
original_list1 = ['a', 'b']
original_list2 = [1, 2]

# 使用 列表推导式 推导出新列表
# for x in original_list1 是外层循环
# for y in original_list2 是内层循环
new_list = [(x, y) for x in original_list1 for y in original_list2]

print(new_list)  # 输出: [('a', 1), ('a', 2), ('b', 1), ('b', 2)]

实例：当需要处理二维列表时，嵌套列表推导式可以派上用场
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

rows = [row for row in matrix ]
print(rows)      # [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

transposed = [[row[i] for row in matrix] for i in range(3)]
print(transposed) # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

实例:计算阶乘;列表推导式还可以与自定义函数结合使用，以实现更复杂的操作
def factorial(n):  # 定义一个函数，计算一个数的阶乘
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)

factorials = [factorial(i) for i in range(5)]
print(factorials)  # 输出: [1, 1, 2, 6, 24]
```

###### 4.3 字典推导式：`{k: v for k, v in zip(keys, values)}`

- 字典生成式/推导式 { expression for ( key,value ) in iterable if condition } - 先执行 for 语句，再执行 if 语句 ，最后执行 expression

```python
实例：字母统计
text = "hello world"
counter = {char: text.count(char) for char in text if char != ' '}
print(counter)
# 输出：{'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}

实例：翻转字典（键值互换）
data = {'a': 1, 'b': 2}
inverted = {v: k for k, v in data.items()}
print(inverted)
# 输出：{1: 'a', 2: 'b'}

实例：多条件处理
scores = {'Tom': 80, 'Jerry': 59, 'Spike': 90}
status = {k: 'Pass' if v >= 60 else 'Fail' for k, v in scores.items()}
print(status)
# 输出：{'Tom': 'Pass', 'Jerry': 'Fail', 'Spike': 'Pass'}

实例：嵌套的字典推导式
d1={letter : i for i in range(4) for letter in "abcd"}
print(d1) # {'a': 3, 'b': 3, 'c': 3, 'd': 3}
```

###### 4.4 集合推导式：`{x for x in range(10) if x % 2 == 0}`

- 集合生成式/推导式 { expression for target in iterable if condition } - 先执行 for 语句，再执行 if 语句 ，最后执行 expression

```python
temp_list = [['a1','b1'],['a2','b2'],['a3','b3'],['a1','b1']]
nums1 = [ i for j in temp_list for i in j ]
print(nums1)  # ['a1', 'b1', 'a2', 'b2', 'a3', 'b3', 'a1', 'b1']

nums2 = set( [i for j in temp_list for i in j ] ) # 去重
print(nums2)  # ['b1', 'b3', 'a3', 'a1', 'b2', 'a2'] 
```

###### 4.5 生成器表达式：(expression for item in iterable)

> 是一种使用类似于列表推导式的语法来创建生成器的方法。非常适合于需要按需生成值的情况( 当处理的数据量很大、数据流是无限)
> 
> 区别：生成器表达式按需生成数据，内存使用效率高；列表推导式则一次性生成整个列表，可能消耗大量内存。
> 
> 应用场景：
> 
> - 大数据处理：当处理的数据集太大，无法一次性装入内存时，生成器表达式提供了一种有效的解决方案。
> - 文件读取：在处理大型文件时，可以使用生成器表达式逐行读取文件，而不是一次性读入整个文件内容。 
> - 网络数据：从网络API获取数据时，生成器表达式可以逐项处理数据，而无需等待所有数据都下载完成。
> - 数据转换：在需要对数据进行转换或过滤时，生成器表达式提供了一种简洁而高效的方式。

```python
实例：假设文件名为 numbers.txt，每行一个数字
with open('numbers.txt', 'r') as file:    
    numbers = (int(line.strip()) for line in file)   # 生成器表达式用于处理文件中的数字   
    total = sum(numbers)                             # 计算所有数字的总和
    print(total)

实例：需要多次使用生成器中的元素，建议将生成器的结果存储在一个列表中
gen_exp = (i * 2 for i in range(5))
# 第一次迭代
for num in gen_exp:
    print(num)

# 此时生成器已经耗尽，再次迭代不会有输出
for num in gen_exp:
    print(num)

# 如果需要多次使用，可将生成器转换为列表
gen_exp = (i * 2 for i in range(5))
result_list = list(gen_exp)
for num in result_list:
    print(num)
for num in result_list:
    print(num)
```

---

## 三、Python数据科学和机器学习相关知识点

### 1. 科学计算库

- **NumPy**：高效的数值计算库，提供多维数组对象和数学函数
- **Pandas**：数据分析和操作库，提供 DataFrame 和 Series 数据结构
- **SciPy**：科学计算库，提供统计、优化、积分等功能

### 2. 数据可视化

- **Matplotlib**：基础绘图库
- **Seaborn**：基于 Matplotlib 的高级可视化库
- **Plotly**：交互式可视化库

### 3. 机器学习

- **Scikit-learn**：机器学习算法库
- **TensorFlow**：深度学习框架
- **PyTorch**：深度学习框架
- **Keras**：高级神经网络 API

### 4. 数据处理

- **数据清洗与预处理**
- **特征工程**
- **数据转换与规范化**e

## 四、Python Web开发相关知识点

### 1. Web框架

- **Flask**：轻量级 Web 框架
- **Django**：全功能 Web 框架
- **FastAPI**：现代、高性能的 API 框架

### 2. 网络编程

- **HTTP 请求**：`requests` 库
- **Web 爬虫**：`BeautifulSoup`, `Scrapy`
- **WebSocket**：实时通信

### 3. API开发

- **RESTful API**：设计原则和实现
- **GraphQL**：查询语言和运行时
- **认证与授权**：JWT, OAuth

### 4. 数据库交互

- **SQL 数据库**：SQLAlchemy, Django ORM
- **NoSQL 数据库**：PyMongo, Redis-py

## 五、Python最佳实践和开发工具

### 1. 代码质量

- **PEP 8**：Python 代码风格指南
- **类型提示**：使用 `typing` 模块
- **文档字符串**：docstring 规范
- **单元测试**：`unittest`, `pytest`

### 2. 开发工具

- **包管理**：pip, conda, poetry
- **虚拟环境**：venv, virtualenv, conda
- **调试工具**：pdb, IDE 调试器
- **性能分析**：cProfile, memory_profiler

### 3. 并发编程

- **多线程**：`threading` 模块
- **多进程**：`multiprocessing` 模块
- **异步编程**：`asyncio`, `async/await` 语法

### 4. 项目组织

- **项目结构**：模块化设计
- **配置管理**：环境变量、配置文件
- **日志记录**：`logging` 模块
- **依赖管理**：`requirements.txt`, `setup.py`

## 六、AI编程工具

### 1. 主流AI编程工具对比

| 出品方       | 最新版本                    | 支持的模型           | 使用方式              | 定价方式              | 定价金额               | 优点                        | 缺点                    | 特色                 | 建议使用的项目类型                  | 使用人数      |
| --------- | ----------------------- | --------------- | ----------------- | ----------------- | ------------------ | ------------------------- | --------------------- | ------------------ | -------------------------- | --------- |
| OpenAI    | GPT-4 Turbo (2025)      | GPT-4/5系列       | API/Web界面/IDE插件   | 订阅制+API计费         | $25/月起或按Token计费    | 强大的代码理解能力，多语言支持，持续更新      | 高级功能成本较高，需联网使用        | 多模态理解，可处理图表和图像中的代码 | 复杂算法开发，跨语言项目，研究性项目         | 超1000万开发者 |
| GitHub    | Copilot Enterprise      | OpenAI+自研模型     | IDE插件/Web编辑器      | 订阅制               | 个人$15/月，企业$39/用户/月 | 深度IDE集成，实时代码建议，团队知识库      | 需要良好网络连接，企业版价格较高      | 企业级安全合规，私有代码库训练    | 企业级应用开发，团队协作项目             | 超500万付费用户 |
| Amazon    | CodeWhisperer Pro       | 自研大型代码模型        | IDE插件/AWS集成/CLI   | 免费基础版+专业版订阅       | 专业版$19/月，企业版定制     | 安全扫描功能强大，AWS服务深度集成        | 非AWS环境支持有限，自定义能力弱于竞品  | 实时安全漏洞检测，合规性检查     | AWS云服务开发，安全敏感型项目           | 超300万用户   |
| Google    | Gemini Code             | Gemini Ultra系列  | IDE插件/Cloud集成/API | 分层订阅+免费版          | 免费版有限额，Pro版$20/月   | 与Google搜索和知识库集成，文档生成优秀    | 某些小众语言支持有限，自定义选项较少    | 代码解释与教学功能，多语言翻译    | 数据科学项目，教育应用，Google Cloud项目 | 超400万用户   |
| Anthropic | Claude Coding Assistant | Claude 3.5 Opus | API/IDE插件/Web界面   | API计费+订阅制         | 基础版$15/月，专业版$30/月  | 长上下文窗口，复杂项目理解能力强          | 专用IDE插件较少，集成度不如竞品     | 超长上下文理解，整个代码库分析    | 大型代码库重构，复杂系统设计             | 超150万开发者  |
| JetBrains | AI Assistant Pro        | 多模型支持           | IDE深度集成           | IDE订阅附加           | IDE订阅+$10/月        | 与JetBrains IDE无缝集成，代码重构强大 | 仅限JetBrains生态系统，通用性较弱 | 智能代码导航，上下文感知重构     | JetBrains IDE用户，中大型项目开发    | 超200万用户   |
| Tabnine   | Enterprise              | 自研模型+开源模型       | IDE插件/本地部署        | 免费+订阅制+企业版        | 专业版$15/月，企业版定制     | 本地运行选项，保护代码隐私，低延迟         | 高级功能需付费，模型能力弱于顶级产品    | 私有化部署，团队代码风格学习     | 隐私敏感项目，需要离线工作的环境           | 超800万用户   |
| Replit    | Ghostwriter X           | 自研模型+合作伙伴模型     | 在线IDE内置           | 订阅制               | $20/月起             | 即时部署与测试，教育功能丰富            | 主要限于Replit平台，离线使用受限   | 实时协作编码，一键部署功能      | 教育项目，快速原型开发，Web应用          | 超400万用户   |
| Microsoft | Visual Studio AI        | 自研+OpenAI模型     | IDE集成/云服务         | Visual Studio订阅附加 | VS订阅+$20/月         | 与Azure和Microsoft生态系统深度集成  | 主要针对.NET和Microsoft技术栈 | 智能调试助手，代码审查自动化     | 企业级.NET开发，Microsoft技术栈项目   | 超250万用户   |
| Meta      | Code Llama Pro          | Llama 3.1/4系列   | API/开源部署/IDE插件    | 开源+企业服务           | 开源免费，企业服务定制        | 可本地部署，开源社区支持，定制灵活         | 企业级支持有限，部署要求较高        | 开源可定制，支持多种部署方式     | 开源项目，自定义AI工具开发，边缘计算        | 超600万开发者  |

注：数据截至2025年10月9日，部分工具的定价和使用人数为估算值

### 2. AI编程工具在Python开发中的应用

- **代码补全与生成**：根据上下文自动补全代码或生成完整函数
- **代码解释与重构**：解释复杂代码逻辑，提供重构建议
- **错误诊断与修复**：识别代码错误并提供修复方案
- **文档生成**：自动为函数和类生成文档字符串
- **测试用例生成**：根据函数功能自动生成单元测试

### 3. 使用AI编程工具的最佳实践

- **明确需求**：向AI工具提供清晰、具体的指令

- **代码审查**：始终审查AI生成的代码，确保质量和安全性

- **持续学习**：将AI视为辅助工具，不依赖于它进行所有编码工作

- **提示工程**：学习如何编写有效的提示以获得更好的结果

- **结合传统开发**：将AI工具与传统开发流程和工具结合使用
  
  ```
  
  ```
