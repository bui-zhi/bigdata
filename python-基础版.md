 路线：

​	基础 ->  PySpark ->  Pandas  ->  NumPy  ->  脚本 ->  机器学习 -> 日常办公



## 常用单词

​		columns	index	fillna	replace	inplace



# Python-基础版





### 语法基础



#### 注释

##### 单行注释

​		以 # 开头，# 的右边的所有文字都会被当成说明 

​		【# 号与注释的内容一般建议以一个空格隔开】





##### 多行注释

​		以 一对三个双引号 包围起来 （"""  注释内容 """）用于说明一段代码的作用使用方法

​		一般用于解释： 整个python代码文件，类或者方法





##### 类型注解

​		为变量设置类型注解，方便确认参数的类型

​		只是备注，即使写错了也不会影响程序正常运行



​		基础语法：	

​					变量:类型



​		也可以在注释中进行类型注解：

​					#  type: 类型				

​					# type: int

​					# type: dict[str,int]





​	**举例：**

​					**基础数据类型注解**：	var_1: int = 10



​					**类对象类型注解**：class Student：pass

​													stu : student = Student( )



​					**基础容器类型注解**：my_list: list = [1,2,3]

​													  my_tuple: tuple  =  (1,2,3)

​													  my_set: set = {1,2,3}



​					**容器类型详细注解**：my_list: list[int] = [1,2,3]

​													   my_tuple: tuple[str,int,bool] = ("buzhi",666,True )

​													   my_set: set[int] = {1,2,3}

​													   my_dict: dict[str,int] = {"buzhi":23}

​			

​		**注意：**

​							元组类型设置类型详细注解时，需要将每一个元素的类型都标记出来

​							字典类型设置类型详细注解时，需要两个类型，分别对应Key，value



##### 	返回值类型注解：

​			def func (data: list) -> list:

​						return data



​	**Union类型**

​		在容器类型中含有多种数据类型时，进行综合的数据类型注解

​	

​		例如：

​			my_list : list[ Union [str,int ] ] = [1,2,3,"nu","buzhi"]

​			my_dict : dict[str , Union[str,int] ] = {"困困" : "壮壮", "叶林生" : 23}







​		

#### **None**：

​	 Python 中的一个特殊的字面量，类型为 <class 'NoneType'>

​			用于表示 空，无意义，具有非常多的应用场景



1:	用在函数无返回值上



2:	用在 if 判断上，在 if 判断中None 等同于 False

​		一般函数中主动返回 None 值，配合 if 判断作出相关处理



3:	用于声明无内容的变量上

​		定义变量，但暂时不需要变量有具体值，可以使用 None 代替 即：暂不赋予变量具体值









### 常见数据类型

​		可通过 type（）函数查看数据（字面量 / 变量）的类型

​		type( 变量 ) 查看的是变量所存储数据的类型，因为变量没有类型，但它存储的数据具有类型





#### 	数字（ Number ）

##### **整数 / int**	

​	•	**任意精度**：Python 3 中的整数（int）支持任意大小，只有内存限制。

​	•	**无区别大小**：不区分长整型和短整型，所有整数都是 int 类型。

​	•	**数学运算**：支持标准的算术运算符，如 +, -, *, //（整除）, %（取余）, **（幂运算）。

​	•	**进制转换**：内置函数 bin(), oct(), hex() 可以将整数转换为不同的进制表示。

​	•	**位运算**：支持位运算符，如 &, |, ^, ~, <<, >>。





##### **浮点数 / **float

​	•	**双精度**：遵循 IEEE 754 标准，使用 64 位表示。

​	•	**有限精度**：存在舍入误差，不适合需要高精度的计算。

​	•	**精度控制**：使用 round() 函数或 decimal 模块进行精度控制。

​	•	**格式化输出**：使用 f-string、format() 或 % 进行格式化输出。





##### 复数 / complex

##### 布尔 / bool		

​	•	**两个值**：True 和 False。

​	•	**子类化**：bool 是 int 的子类，True 等于 1，False 等于 0。

​	•	**逻辑运算**：使用逻辑运算符 and, or, not 进行逻辑判断。

​	•	**条件判断**：常用于 if, while 等控制流语句中。



##### NoneType

​	•	**单一值**：None 是 NoneType 的唯一值，用于表示缺失或未定义的值。

​	•	**常用场景**：函数无返回值时默认返回 None，用于默认参数等。

​	•	**判断是否为空**：使用 is None 来判断变量是否为 None，避免使用 ==。











#### 字符串（ String ）

​		**字符串由任意数量的字符组成**

​	•	**不可变**：字符串一旦创建无法修改。

​	•	**Unicode**：支持 Unicode 编码，能够表示多种语言字符。

​	•	**丰富的方法**：提供大量的内置方法，如 split(), join(), replace(), find() 等。

​	•	**多行字符串**：使用三引号 ''' 或 """ 表示多行字符串。

•	**格式化字符串**：

​	•	**f-string**（推荐，Python 3.6+）：f"Hello, {name}!"

​	•	format() **方法**："Hello, {}".format(name)

​	•	**百分号** %："Hello, %s!" % name

​	•	**字符串操作**：使用切片、步长进行子字符串提取。

```python
name = "Alice"
greeting = f"Hello, {name}!"
print(greeting)  # Hello, Alice!
y
multiline = """This is a
multi-line string."""
print(multiline)

# 切片
s = "Hello, World!"
print(s[7:12])  # World

# 常用方法
print(s.lower())  # hello, world!
print(s.replace("World", "Python"))  # Hello, Python!
```





### 容器类型

​	**容器：** 一种可以容纳多份数据的数据类型，容纳的每一份数据称之为一个元素

​				每一个元素都可以是任意数据类型的数据，例如：字符串，数字，布尔等



​	**分类**：

​			列表 [ ] ：元素可重复，有序，可修改

​			元组 ( ) ：元素可重复，有序，不可修改

​			集合 { } :   元素不可重复，无序

​			字符串 " " :  元素可重复，不可修改

​	

**特性**	**列表（List）**	**元组（Tuple）**	**集合（Set）**	**字典（Dictionary）**	**字符串（String）**

**有序性**	有序	有序	无序	无序（Python 3.7+有序）	有序

**可变性**	可变	不可变	可变	可变	不可变

**重复元素**	允许重复	允许重复	不允许重复	键不重复，值可重复	不允许重复

**数据类型**	支持多种类型	支持多种类型	支持多种类型	键必须是不可变类型	只支持字符

**访问方式**	索引	索引	无	通过键访问	索引

**内存占用**	相对较大	相对较小	中等	中等	小

**应用场景**	需要顺序存储和修改	不可变、需要作为键使用	不重复元素、集合运算	键值对映射、高效查找	存储和操作文本数据





#### 对比

##### 	是否支持下标索引：

​			**支持：**列表，元组，字符串	- 序列类型

​			**不支持：**集合，字典	 - 非序列类型



##### 	是否支持重复元素：

​			**支持：**列表，元组，字符串			- 序列类型

​			**不支持：**集合，字典				   	 - 非序列类型



##### 	是否支持修改

​			**支持：**列表，集合，字典

​			**不支持：**元组，字符串



##### 应用场景：

​	**列表：**一批数据，可修改，可重复

​	**元组：**一批数据，不可修改，可重复

​	**集合：**一批数据，去重可修改，不重复

​	**字典：**一批数据，可用 key 检索 value  

​	**字符串：**一串不可修改可重复的字符存储场景

​	



#### 通用操作

##### 	**遍历**

​		五类容器都支持 for 循环进行遍历，但 集合，字典 不支持 while 循环	（因为无法使用下标索引）



##### 	**常见函数**

​		**len**（容器）：求元素个数

​		**min**（容器）：求最小元素

​		**max**（容器）：求最大元素

​		**list**（容器）：将给定容器转换为列表

​		**tuple**（容器）：将给定容器转换为元组

​		**set**（容器）：将给定容器转换为集合

​		**str**（容器）：将给定容器转合字符串

​		**sorted**（容器，[ reverse = True ]）：将给定容器进行排序，reverse 是否反向排序，排序返回的结果都是列表对象





#### 序列的切片操作

​	序列支持器表，即：列表，元组，字符串均支持切片操作

​	**切片**：从一个序列中取出一个子序列

​	**语法**：序列 [ 起始下标 : 结束下标 : 步长 ]

​				标识从序列中的指定位置开始，依次取出元素，到指定位置结束，返回一个新序列

​				起始下标表示从何处开始，可以留空，留空视为从头开始

​				结束下标（不含）表示何处结束，可以留空，留空视为截取到结尾

​				步长表示依次取元素的间隔

​					步长1表示一个个取元素（默认为一）

​					步长二表示每次跳过一个元素取值

​					步长N表示每次跳过N-1个元素取值

​				    步长为负数表示反向取值（此时的起始下标与结束下标也需要反向标记）





#### 列表（ List ）

​	**有序的可变序列**

##### **基本特征：**

​	•	**有序**：元素按插入顺序排列，支持索引和切片。

​	•	**可变**：可以动态添加、删除、修改元素。

​	•	**允许重复**：同一个元素可以多次出现。



##### 	基本语法：

​			**字面量**

​					[元素1，元素2，元素3，元素4]

​			**定义变量**

​					变量名称 = [元素1，元素2，元素3，元素4]

​			**定义空列表**

​					变量名称 = []

​					变量名称 =  list()

​		

​			列表中的每一个数据被称之为元素，[] 作为标识，列表中的元素用 ，分隔开

​			列表中可以同时存储不同数据类型的元素



​			列表的下标索引值从零开始：list[ num ]

​			嵌套列表的索引：list[ num1] [ num2 ]





##### **内置方法**：

​	•	添加元素：append(), extend(), insert()

​	•	删除元素：pop(), remove(), clear(),del

​	•	排序与反转：sort(), reverse()

​	•	**切片赋值**：可以通过切片一次性修改多个元素。







##### 常用操作：

​	**查询元素**

​			功能：查询指定元素在列表中的下标值，如果找不到则返回报错 Value Error

​			语法：列表对象.index( 元素) 	index就是列表对象（变量）类中内置的方法（函数）



​	**插入元素**

​			语法：列表对象.insert ( 索引值 , “对应元素值” )



​	**修改元素**

​			语法：列表对象 [ 索引值 ] = ' 新元素值 '



​	**追加元素**

​			功能：将指定元素，追加到列表的尾部

​			语法：列表对象.append ( “ 对应元素值” )

​						列表对象.extend ( 其他数据容器 )    用于将其他数据容器的内容取出，依次追加到列表尾部



​	**删除元素**

​			语法：del 列表对象 [ 索引值]				**删除指定索引值对应的元素**

​						列表对象.pop ( 索引值 )			**该方法可以将被删除的元素值返回**

​						列表对象.remove ( 元素值 )	 **该方法用于删除第一个匹配到的指定元素值**



​	**清空列表**

​			语法：列表对象.clear( )



​	**统计指定元素数量**

​			功能：统计指定元素在该列表中出现的次数

​			语法：列表对象.count( 元素值 )



​	**统计整个列表对象中的元素个数/列表长度**

​			语法：len（列表对象）





##### 具体实现

```python
# 定义列表
lst = [1, 2, 3]

# 追加列表 就地修改列表 不会返回新的列表
lst.append(4)          # [1, 2, 3, 4]

# 合并列表
lst.extend([5, 6])     # [1, 2, 3, 4, 5, 6]

# 插入列表
lst.insert(0, 0)       # [0, 1, 2, 3, 4, 5, 6]

# 删除索引对应元素
popped = lst.pop(2)    # popped = 2, lst = [0, 1, 3, 4, 5, 6]

# 删除第一个匹配到的元素
lst.remove(4)          # [0, 1, 3, 5, 6]

# 列表推导式
squares = [x**2 for x in range(4)]   # [x**2 for x in range(4)]

# 切片赋值,包含起点，不包终点
lst[1:3] = ['a', 'b']								 # [0, 'a', 'b', 5, 6]

# 负索引 最后一个元素的索引值为 -1
print(lst[-1])  										# 6



mylist = ["不知", "困困", "壮壮"]

# 查找元素在列表中的下标索引值
mylist.index('不知')		# 0

# 修改指定索引值位置对应的元素值
mylist[0] = '刀总'

# 在指定索引位置插入元素
mylist.insert(1, "条总")

# 追加元素
mylist.append("咪咪")

# 获取列表最后一位元素值
mylist[len(mylist) - 1]
mylist[-1]}


# 删除指定索引对应的元素
del mylist[len(mylist) - 1]

# 删除指定索引对应的元素，并返回该元素
drop = mylist.pop(1)
print(f"被删除的元素值为: {drop}")

# 删除第一个匹配上的元素
mylist.remove('困困')

# 拼接另一个数据容器
list01 = ["仙仙", "蠢萌"]
mylist.extend(list01)

# 切片赋值
a,b,c = mylist[1:4]


num = 0
while num <= len(mylist) - 1:
    print(f"下标索引 {num} 对应的列表元素值为： {mylist[num]}")
    num += 1

    
print("该列表现阶段对应的输出结果： ")
for x in mylist:
    print(f"{x}", end=" ")
print()


# 常用操作
strings = ['a', 'as', 'bat', 'car', 'dove', 'python'
[x.upper() for x in strings if len(x) > 2]
# ['BAT', 'CAR', 'DOVE', 'PYTHON']
           
           
# 列表推导式
# condition / kənˈdɪʃ(ə)n /	条件
result = []
for val in collection:
    if condition:
        result.append(expr)
        
        
# 可简化为以下代码
[expr for val in collection if condition]           
```







#### 元组（ Tuple ）

​	**有序的不可变序列**

**特点：**

​	•	**有序**：元素按插入顺序排列，支持索引和切片。

​	•	**不可变**：一旦创建无法修改，保证数据的完整性。

​	•	**允许重复**：同一个元素可以多次出现。

​	•	**可哈希**：可以作为字典的键或集合的元素（如果内部元素也是可哈希的）。

​	•	**解包**：可以将元组的元素解包到多个变量中。

​	•	**命名元组**：使用 collections.namedtuple 提高代码可读性。

​	•	**嵌套结构**：常用于存储不同类型的数据组合，如函数返回多个值。



```python
tup = (1, 2, 3)
x, y, z = tup  # 解包
print(x, y, z)  # 1 2 3

# 命名元组
from collections import namedtuple
Point = named tuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20

# 元组作为字典键
d = { (1, 2): "a", (3, 4): "b" }
print(d[(1, 2)])  # a
```





#### 集合（ Set ）

​	**无序不重复集合**

**特点：**

​	•	**无序**：元素没有固定顺序，无法通过索引访问。

​	•	**唯一性**：不允许重复元素。

​	•	**可变**：可以添加或删除元素。

​	•	**高效**：基于哈希实现，查找、添加和删除操作时间复杂度为 O(1)。

​	•	**集合推导式**：类似于列表推导式，创建集合。

​	•	**集合运算**：支持并集（|）、交集（&）、差集（-）、对称差（^）。

​	•	**去重**：快速去除列表中的重复元素。



​	元素无序-不重复-允许修改

​	**无序：不支持下标索引访问**

​	集合不支持下标索引，所以不能用while循环，只能使用for循环



##### 	**基本语法**：

​			**定义集合字面量**

​				{ 元素，元素，元素，元素 }

​			**定义集合变量**

​				变量名称 = { 元素，元素，元素 }

​			**定义空集合**

​				变量名称 = set( )





##### 常用操作

​	**添加元素**

​		语法：集合.add ( 元素 )，将指定元素添加至集合内

​		结果：集合本身被修改，添加了新元素



​	**移除元素**

​		语法：集合.remove ( 元素 )，将指定元素从集合内移除					

​		结果：集合本身被修改，移除了元素



​	**取出元素**

​		语法：集合.pop ( 元素 )，随机从集合内取出一个元素					

​		结果：得到一个元素的结果，但集合本身被修改，移除了该元素



​	**清空集合**

​		语法：集合.clear( )

​		结果：集合本身被清空



​	**合并集合**

​		语法：集合1.difference( 集合2 )，取出集合一与集合二的差集（集合一有，集合二没有）

​		 结果：得到一个新集合，而集合一二不变





​	**集合合并	 a | b**

​		语法：集合1.union ( 集合2 )，将集合一和集合二组合成新的集合

​		结果：返回一个新集合，集合一和集合二不变



​		

​	**消除两个集合的差集**

​		语法：集合1.difference_update( 集合2)

​		功能：对比集合1与集合2，在集合1中删除与集合2相同的元素

​		结果：集合1被修改，集合2不变



​	**提取两个集合的并集	 a & b**

​		语法：集合1.intersection( 集合2) 

​		结果：返回一个新的集合，其中包含两个集合都含有的元素

​	

​	**统计集合元素数量：len()**

```python
s = {1, 2, 3, 3}
print(s)  # {1, 2, 3}

# 添加与删除
s.add(4)        # {1, 2, 3, 4}
s.remove(2)     # {1, 3, 4}

# 集合运算
s1 = {1, 2, 3}
s2 = {3, 4, 5}
print(s1 | s2)  # {1, 2, 3, 4, 5}
print(s1 & s2)  # {3}
print(s1 - s2)  # {1, 2}
print(s1 ^ s2)  # {1, 2, 4, 5}

# 集合推导式
squares = {x**2 for x in range(10)}
set_comp = {expr for value in collection if condition}
```









#### 字典（ Dictionary ）

​	**无序 Key - Value 集合**

**基本特征：**

​	•	**无序（Python 3.7+ 保持插入顺序）**：键值对按照插入顺序存储。

​	•	**键唯一**：每个键必须唯一，值可以重复。

​	•	**可变**：可以动态添加、删除、修改键值对。

​	•	**高效**：基于哈希实现，查找、添加和删除操作时间复杂度为 O(1)。

​	•	**字典推导式**：快速创建和操作字典。

​	•	get() **方法**：安全获取键对应的值，避免 KeyError。

​	•	setdefault() **方法**：获取键对应的值，如果不存在则设置默认值。

​	•	dict **方法**：如  **keys() ,  values() ,   items()** 提供高效的视图。

​	•	**嵌套字典**：用于表示复杂的数据结构。





##### 	基础定义

​		Key - Value 类型

​		定义字典字面量：{ key : value ,  key : value , key : value }

​		定义字典变量：变量名 = { key : value ,  key : value , key : value }

​		定义空字典：变量名 = {}	/	变量名 =  dict ( )

​		获取字典中全部 key：字典.**keys**（ ），可用于 for 循环进行遍历

​		len（字典）：计算字典内的元素数量

​		使用	{}	定义，但元素需要满足键值对类型

​		key值不允许重复（ 重复会覆盖 value 值 ），不支持下标索引，允许修改，但可以通过 key 获取对应的value值



​		**字典中的  value 可以是任意类型(可以嵌套)，但 key 不能是字典类型，dict的key必须是不可变对象**

​		支持 for 循环，不支持 while 循环







##### 常用操作

​	**新增元素 / 更新元素：**

​		字典[ key ] = value 	结果：字典被修改 或 增加新元素

​		[ 字典 key 不可以重复，所以对已存在的 key 执行上述操作就是更新 value 的值]



​	**删除元素：**

​			字典. pop ( key )

​			结果：获取指定 key 的value ，同时字典被修改，指定 key 的数据也被删除



​	**清空字典：**

​			字典.clear( )

​			结果：字典被修改，元素被清空



##### 具体操作

```python
d = {'a': 1, 'b': 2, 'c': 3}

# 添加与修改
d['d'] = 4
d['a'] = 10

# 删除
del d['b']  				# 删除该键值对，但不返回值
value = d.pop('c')	# 删除该键值对，返回value

# 字典推导式
squares = {x: x**2 for x in range(5)}

# 使用 get
print(d.get('e', 'default'))  # 如果不存在该key，则返回默认值 default

# setdefault
d.setdefault('e', 5)
print(d)  # {'a': 10, 'd': 4, 'e': 5}

# 遍历
for key, value in d.items():
    print(key, value)
    
# 可以用检查列表和元组是否包含某个值的方法，检查字典中是否包含某个键：
'b' in d

# 获取字典所有的 key 并转换为list
list(d1.keys())
# ['a', 'b', 7]

# 获取字典所有的 value 并转换为list
list(d1.values())
# ['some value', [1, 2, 3, 4], 'an integer']


# update 方法可以将一个字典与另一个融合，原地改变字典，因此任何传递给 update 的键的旧的值都会被舍弃
d1.update({'b' : 'foo', 'c' : 12})

d1
# {'a': 'some value', 'b': 'foo', 7: 'an integer', 'c': 12}


# 用序列/列表/元组创建字典
mapping = {}
for key, value in zip(key_list, value_list):
    mapping[key] = value
    
# 字典本质上是2元元组的集合，dict可以接受2元元组的列表
mapping = dict(zip(range(5), reversed(range(5)))) # reversed 逆向取值

mapping
#  {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
```







```python
# 给出key进行取值，不存在key则为默认值
if key in some_dict:
    value = some_dict[key]
else:
    value = default_value
    
    
# 等效替代以上代码
value = some_dict.get(key, default_value)    


# 通过首字母，将一个列表中的单词分类
words = ['apple', 'bat', 'bar', 'atom', 'book']

by_letter = {}

for word in words:
    letter = word[0]
    if letter not in by_letter:
        by_letter[letter] = [word]
    else:
				by_letter[letter].append(word)

by_letter
# {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}


# 以上代码可使用 setdefault 方法简化优化
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
    
    
# defaultdict 可以进一步简化上面。传递类型或函数以生成每个位置的默认值：
from collections import defaultdict
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)    
    
# 字典推导式
dict_comp = {key-expr : value-expr for value in collection if condition}
```













#### **命名元组（namedtuple）**

**特点：**

​	•	**具名字段**：每个字段都有名字，提高代码可读性。

​	•	**不可变**：像元组一样不可变。

​	•	**内存效率**：比字典更节省内存。

​	•	**替代类**：用于轻量级的数据结构，避免定义完整的类。

​	•	**字段访问**：可以通过属性访问字段，而不仅仅是索引。

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20

# 替代字典
student = namedtuple('Student', ['name', 'age', 'grade'])
s = student(name='Alice', age=20, grade='A')
print(s.name, s.age, s.grade)
```





#### **冻结集合（frozenset）**

冻结集合不可重复不可变



**特点：**

​	•	**无序**：元素没有固定顺序。

​	•	**唯一性**：不允许重复元素。

​	•	**不可变**：创建后无法修改。

​	•	**可哈希**：可以作为字典的键或其他集合的元素。

​	•	**与集合类似**：支持集合的所有运算，如并集、交集等。

​	•	**作为键使用**：由于不可变，可以用作字典的键或其他集合的成员。

```python
fs = frozenset([1, 2, 3, 2])
print(fs)  # frozenset({1, 2, 3})

# 集合运算
s = {3, 4, 5}
print(fs & s)  # {3}
print(fs | s)  # frozenset({1, 2, 3, 4, 5})

# 用作字典键
d = {fs: "value"}
print(d[fs])  # value
```





#### **默认字典 （defaultdict）**

**特点：**

​	•	**默认值**：当访问不存在的键时，自动创建一个默认值。

​	•	**简化代码**：避免手动检查键是否存在。

​	•	**计数器**：使用 defaultdict(int) 可以方便地进行计数。

​	•	**分组**：使用 defaultdict(list) 可以方便地对元素进行分组。



```python
from collections import defaultdict

# 计数器
counts = defaultdict(int)	 # 指定默认值的数据类型
for item in ['a', 'b', 'a']:
    counts[item] += 1
print(counts)  # defaultdict(<class 'int'>, {'a': 2, 'b': 1})

# 分组
groups = defaultdict(list)
for key, value in [('a', 1), ('b', 2), ('a', 3)]:
    groups[key].append(value)
print(groups)  # defaultdict(<class 'list'>, {'a': [1, 3], 'b': [2]})
```











### 变量

​		变量是在程序运行时记录数据所用

​		使用方法： **变量名 = 变量值**



**对象引用比较**

两个引用是否指向同一个对象，可以使用`is`方法。`is not`可以判断两个对象是不同的：

```python
a = [1, 2, 3]

b = a

c = list(a)

a is b
True

a is not c
True	
```







#### 变量的作用域

​	变量的作用域指的是变量的作用范围，主要分为：**局部变量与全局变量**

​	如果要将函数内定义的变量声明为全局变量，可采用 **global** 关键字： global 变量





##### 局部变量

​	定义在函数体内部的变量，只在函数体内部生效，临时保存数据

​	函数调用完之后则销毁对应的局部变量





##### 全局变量

  定义在函数体外部的变量，无论是在函数体内还是函数体外都可以正常使用

  在全局变量与局部变量 名字相同时，采用 **就近原则** ，两个变量没有任何关联





##### 深浅复制/拷贝

**浅拷贝 copy.copy（）**

​	创建一个新的对象，但该对象中的子对象（如列表中的元素、字典中的值等）仍然是原始对象中子对象的引用。浅拷贝只复制了最外层的对象，而内部的嵌套对象仍然共享引用。

​	变量赋值属于引用，共享相同的内存空间

​	





**深拷贝 copy.deepcopy（）**

​	创建一个新的对象，并且递归地复制所有嵌套对象，使得新对象与原始对象完全独立。即使原始对象中的嵌套对象被修改，也不会影响到深拷贝的新对象。



```python
# 修改 a 实际上是创建了一个新的字符串对象，并将其赋值给 a，而 b 仍然引用原来的字符串对象
a = 1
b = a
a = 2


# 变量赋值属于引用，共享相同的内存空间，两者相互影响，所以修改list_a的值也会改变list_b的值
list_a = [1,2,3]
list_b = list_a
list_a[0] = 4
```







### 数据类型转换

#### 	使用场景

​			从文件中读取的数字默认为字符串类型

​			input语句的返回结果默认为字符串类型

​			将数字等类型转换成字符串输出到外部系统



#### 	常见的转换语句

​		**int( x )**：将X转换成一个整数

​				浮点类型数据转换为整数类型会丢失小数部分（ 丢失精度 ）



​		**float( x )**：将X转换成一个浮点数

​				整型转换成浮点型会自带小数点后一个零



​		**str( x )**：将对象X转换成字符串

​				任何类型都可以转换成字符串类型





#### python 数据与 json 数据相互转换

​		**python 中的 json文件就是拥有特殊格式的字符串**



```python
#	导入json模块
import json

# 准备符合json格式要求的python数据
data = [{"name":"老王,"age":16},{"name":"张三","age":21}]
         
# 通过json.dumps（ data ）方法将 python 数据转换为 json 数据
data = json.dumps(data)			# 数据中包含中文时，需添加参数 ensure_ascii = False 
 
# 通过json.loads（ data ）方法将 json 数据转换为 python 数据，返回类型为list（列表类型）
data = json.loads(data)			# 数据中包含中文时，需添加参数 ensure_ascii = False 
          
```









### 标识符

​	用户在编程过程中使用到的一系列名字，用于给变量，类，方法等命名



**注意事项**

​		规则必须遵守，规范建议遵守

#### 		命名规则

##### 		**内容限定**

​			不可以使用关键字作为变量名

​			标识符命名中，只允许出现：**英文，中文，数字，下划线**

​			其余任何内容都不被允许，且不推荐使用中文

​			数字不可以用在开头

​			

##### 		**大小写敏感**

​			关键字中只有：**True，False，None** 为首字母大写，其余关键字均为全小写结构

##### 		**不可使用关键字**



#### 		命名规范

##### 			**变量的命名规范**

​					见名知意，易懂简洁

​					下划线命名法：多个单词组合命名变量名时，需要使用下划线进行分隔

​					英文字母全小写







### 运算符



#### 数字运算符

​		+	加：两个对象相加

​		-	减：得到一个负数或一个数减去另一个数的值

​		*	乘：两个数相乘的值或返回一个被重复若干次的字符串

​		/	除：输出两个数相除的值（默认返回值为浮点型）

​		%	取余：返回除法运算之后的余数

​		**	指数：返回次方运算之后的值

​		//	取整除：返回一个整型的数值，会自动舍去小数部分



#### 赋值运算符

​	=  ：将等号右边的结果赋给左边的变量



#### 复合赋值运算符

​	数字运算符 + =  

​	b ?= a		等效为   b = b ?  a



#### 比较运算符

​	==	判断内容是否相等，满足则为 True，不满足则为 False

​	!=	 判断内容内容是否不想等，满足则为 True，不满足则为 False











### 字符串



#####  **定义格式**

​	**单引号定义法**：name = ' x '

​	**双引号定义法**：name = " x "

​    **三引号定义法**：name = """ x """

​		三引号定义法和多行注释的写法一致，同样支持换行操作

​		使用变量接收时就是字符串，不使用变量接收就作为单行注释使用



**引号嵌套**

​		如果字符串内容中含有 单引号/双引号可使用转译字符 （ \ ）解除引号的效果，变成普通字符串

​		\n：换行符		\t：制表符



##### 字符串拼接

​	1:	**使用 + 拼接**：变量与字符串，字符串与字符串

​		**无法使用 + 号拼接字符串与整数变量**：可使用 str() 函数将数值类型的变量转换为字符串类型



​	2:   **使用字符串格式化**：

```python
xx = " 这个是 %s " %name
yy = " 这个是 %s， %s 岁" %(name,age)
```

​					% ： 表示占位

​					s  ： 表示将变量转换为字符串放入占位的地方

​					d  ：表示将变量转换为整数放入占位的地方

​					f   ：表示将变量转换为浮点型放入占位的地方

​		**此用法支持直接拼接字符串与数字**

​			

##### 数字精度控制

可以使用辅助符号 m .n 控制数据的宽度与精度



m :	控制宽度，要求变量属于数字类型（很少使用），**设置的宽度小于数字自身时不生效，大于数字自身时使用空格补全**

.n ：控制小数点精度，要求是数字，会进行小数部分的四舍五入

 

**示例**

​	%5d : 表示将整数的宽度控制在五位，如：数字11被设置成 5d 输出时为 [ 空格 ] [ 空格 ] [ 空格 ] 11

​	%5.2f : 表示将宽度控制为五位，将小数点精度设置为二，**小数点与小数部分也会被记入宽度计算**

​				如：11.345 设置为 %7.2f 后，输出结果为 [空格] [空格]11.35

​						两个空格补足宽度，小数部分限制两位精度后四舍五入为 .35



3:  **快速格式化**

**语法： f" 内容 { 变量 }"**





##### 常用方法



​	字符串也是数据容器，属于字符的容器，无法修改其中元素，只能修改成为新的字符串

​	一个字符串可以存放任意数量的字符



###### **通过下标索引取值**

**index方法**：返回对应首位匹配子字符串的下标，如果字符串中不存在该子字符串则报错

**find方法：**返回对应首位匹配子字符串的下标，如果字符串中不存在该子字符串则返回 -1

​			可以使用 in 关键字判断子字符串是否存在于字符串中

​	





###### **replace方法**

​	语法：字符串.replace （ 字符串1，字符串2）

​	功能：将字符串中全部的 字符串1 替换为 字符串2

​	注意：该过程并不是修改字符串本身，而是得到一个新的字符串



###### **split方法**

​	语法：字符串.split（分割字符串）

​	功能：按照指定的分割字符串，将字符串分割成多个子字符串，并存入列表对象中

​	注意：字符串本身不变，而是得到了一个列表对象



###### **strip方法**

​	去除字符串前后的空格，规整操作，类似于 SQL 的 TRIM

​	不传参时默认为空格，传入参数则去除字符串前后的参数字符，字符串尾部去除的是参数的倒序字符串

​	

​	strip() 	 移除两侧空白字符

​	lstrip() 	移除左侧空白字符

​	rstrip() 	移除右侧空白字符





**count : 统计字符串中某字符出现的次数**

​	字符串.count ( "子字符串")



**len : 统计字符串的长度**

​	len ( 字符串 )：返回一个数值类型





**推导式**

```python
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']

[x.upper() for x in strings if len(x) > 2]
# ['BAT', 'CAR', 'DOVE', 'PYTHON']

unique_lengths = {len(x) for x in strings}
# {1, 2, 3, 4, 6}
```





### 输入输出

#### 	**数据输出： print**



##### 参数列表

​	 **value**：可以接收任意多个变量或值，因此print（）函数可以输出多个值。 

​	 **sep**：使用print（）函数输出多个变量时，默认以空格隔开多个变量，如果希望改变默认的分隔符，可通过sep参数进行设置。 

​	 **end**：用来设置以什么结尾，默认值是换行符\n，可以换成其他字符串，如\t、" "等。 

​	 **file**：表示设置输出设备，把print（）函数中的值输出到什么地方，默认输出到标准端（sys.stdout）。 

​	 **flush**：该参数只有两个选项True或False。True表示强制清除[…]





##### 输出方式

1. 简单字符串拼接：

   ```python
   name = "Alice"
   age = 30
   print("Name:", name, "Age:", age)
   # Name: Alice Age: 30
   ```

   这里直接将变量与字符串用逗号分隔传给 print 函数，print 会将它们以空格分隔的方式输出。这种方式简单直观，但不适合需要精确控制输出格式的场景。

   

2. 使用 + 进行字符串拼接：

   ```python
   print("Name: " + name + ", Age: " + str(age))
   # Name: Alice, Age: 30
   ```

   这种方法通过 + 运算符将字符串和变量拼接成一个完整的字符串后再输出。

   **注意**：如果变量不是字符串类型，需要先使用 str() 函数将其转换为字符串。

   

3. 使用 f-string（格式化字符串字面值）：print(f"Name: {name}, Age: {age}")
   输出：Name: Alice, Age: 30
   f-string 是 Python 3.6 及更高版本引入的一种简洁易用的字符串格式化方式。在字符串前加 f 或 F，并在花括号 {} 内插入变量名或表达式，它们的值将被自动替换到相应位置。f-string 支持丰富的格式化选项，如对齐、精度控制、千分位分隔等。

   

4. 使用 % 操作符：

   ```python
   print("Name: %s, Age: %d" % (name, age))
   # Name: Alice, Age: 30
   ```

   这是 Python 中传统的字符串格式化方法，使用 % 操作符和占位符（如 %s 表示字符串，%d 表示整数）来插入变量。变量值通过元组与 % 操作符结合传递。

   

5. 使用 str.format() 方法：

   ```python
   print("Name: {}, Age: {}".format(name, age))
   # Name: Alice, Age: 30
   ```

   str.format() 方法通过 {} 作为占位符，然后通过方法调用时的参数按顺序替换这些占位符。也可以使用关键字参数或位置索引来指定占位符的值。

   

   

   总结：

   ​		• 简单字符串拼接：直观，但不易控制输出格式。

   ​		• 使用 + 进行字符串拼接：需要手动类型转换，代码稍显繁琐。

   ​		• 使用 f-string：简洁、高效、功能丰富，推荐使用。

   ​		• 使用 % 操作符：传统格式化方式，较直观，但不如 f-string 灵活。

   ​		• 使用 str.format() 方法：较为通用，支持复杂格式化，但不如 f-string 简洁。

   








#### 	**数据输入：input**		

​		接收到的数据都统一转换成字符串处理

​		input 中可以放入提示字符串

```python
f = input("xxxxx")
```





### 判断/循环语句



#### 产生随机数

 

```python
import random
num = random.randint(1,10)	# 产生指定范围内的一个随机数
```





#### if 判断语句

​	**if	判断条件 :**

​	[四个空格的缩进]	  条件成立时需执行的语句

​	**elif 判断条件 :**

​	[四个空格的缩进]	  条件成立时需执行的语句

 	**else 判断条件 :**

​	[四个空格的缩进]	  条件成立时需执行的语句



​	**( 需注意 冒号 还有 空格缩进)**







#### while 循环语句

​	**while 条件：**

​		 **条件成立时需执行的语句**



1:	while 语句的条件需要得到布尔类型，True 表示继续循环，False 表示结束循环

2:	需要注意设置循环终止的条件，否则会无限循环下去

3:	空格缩紧格式与 if 判断语句一致



```python
i = 0
while i < 100:
	print(f"这是输出的第 {i} 次")
	i += 1
```







#### for 循环语句

​	**for 临时变量 in 待处理数据集（序列）:**

​	  	**条件成立时需执行的语句**			



1:	无法定义循环条件，只能被动取出数据进行处理

2:	循环内的执行语句需注意空格缩进

3:	字符串可以单独取出里面的每一个字符



待处理数据集为**序列类型**，包括：**字符串，元组，列表**等

其内容可以一个个依次取出



```python
name  = "buzhi"
count = 0
for x in name:
    if x == 'a':
        count += 1
    
print(f"name中存在字母 a 的个数为 {count}")



# 配合 if 判断语句 快速生成偶数的平方
numbers = [1, 2, 3, 4, 5]
squares = [x**2 for x in numbers if x % 2 == 0] 

[4, 16]


# 配合 enumerate： 在遍历序列的同时获取元素的索引，无需单独计数
words = ['apple', 'banana', 'cherry']
for index, word in enumerate(words):
    print(f'Index: {index}, Word: {word}')
    
    
Index: 0, Word: apple
Index: 1, Word: banana
Index: 2, Word: cherry    
    
    

# 配合 zip 和 unpacking： 同时遍历多个序列：
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]
for name, age in zip(names, ages):
    print(f'{name} is {age} years old.')    
    
    
Alice is 25 years old.
Bob is 30 years old.
Charlie is 35 years old.   



# 配合 range 取出容器（有序/存在下标索引）中的内容：
x = ['apple', 'banana', 'cherry','Alice', 'Bob', 'Charlie']
for i in range(len(x)):
    print(x[i])
    
for i in x:
    print(i)
    
    
    
# with-as 语句结合 for 循环处理文件： 安全、高效地遍历文件内容：
with open('file.txt', 'r') as f:
    for line in f:
        process_line(line)  # 对每一行进行处理    
        
        
        
# continue 和 break： 在循环体内快速跳出或继续下一轮循环：
for num in range(10):
    if num % 2 == 0:  # 若num是偶数
        continue  # 跳过当前循环，直接进行下一次循环
    print(num)  # 输出奇数

for num in range(10):
    if num == 5:
        break  # 当num等于5时，退出整个循环
    print(num)  # 输出小于5的数        
    
    
    
    
# 假设我们有一个包含元组的列表，每个元组有三个元素
data = [(1, 'apple', True), (2, 'banana', False), (3, 'cherry', True)]

# 使用for循环和元组解包来迭代这个列表
for a, b, c in data:
    print(f"a: {a}, b: {b}, c: {c}")

# 输出：
# a: 1, b: apple, c: True
# a: 2, b: banana, c: False
# a: 3, b: cherry, c: True    
    
```





##### range

​	返回一个可迭代对象，可以使用循环结构（如 for 循环）对其进行迭代

​	虽然range可以产生任意大的数，但任意时刻耗用的内存却很小



​		类似于numpy中的arange



​	**range（ num ）**

​			获取一个从零开始，到 num 结束的数字序列（不包含 num 本身）

例： list(range(5))  =>	[0,1,2,3,4]



​	**range( num1, num2)**

获取一个从 num1 开始，到 num2 结束的数字序列（不包含 num2 本身）

例： range(5, 10)  =>	[5,6,7,8,9]



​	**range( num1, num2, step)  /  range( num,step)**

**默认起始值为 0**

**step**： 步长参数



```python
# 生成随机IP地址
ip_address = '.'.join(str(random.randint(0, 255)) for _ in range(4))
# '11.97.135.148'

# 在循环结构中使用
for i in range(5):
    print(i) # 打印出从0到4的整数
    
# 打印五遍 xxx    
[print("xxx") for _ in range(5)] 

for _ in range(5):
    print("xxx") 
# for _ in range(x) : 该结构只用于不需要使用循环变量的场景
```





##### 中断循环：	continue - break



**continue：** 临时中断，用于中断本次循环，直接进入下一次循环

​					 可用于 for 循环与 while 循环，效果一致



**break：**	  直接结束循环，退出循环体

​				  	可用于 for 循环与 while 循环，效果一致







#### for 循环与 while 循环对比

​	for循环更简单，while循环更灵活

​	

​	for 用于从容器内依次取出元素并处理，while 用于任意需要循环的场景









### 函数

#### **定义语法**

**def 函数名( 传入参数 ):**

​			**函数体**

**return  返回值** 



参数与返回值在不需要的时候可以省略，但参数的括号不能省略

如果函数没有使用 return 语句返回数据，则返回值属于 None 类型，实际上就是返回 None 字面量







**函数**： 组织好且可重复使用，用来实现特定功能的代码段

​			 定义在 class/类 内，属于class成员的函数称之为**方法**

​			函数可以直接使用，而方法需要先定义对应类的对象，再由该对象调用对应方法



**特点：**

​	•	**一等公民**：函数可以作为参数传递、作为返回值、赋值给变量。

​	•	**闭包**：函数可以记住并访问其定义时的环境。



**使用技巧：**

​	•	**高阶函数**：使用 map(), filter(), reduce() 等高阶函数处理数据。

​	•	**匿名函数**：使用 lambda 表达式创建短小的匿名函数。

​	•	**装饰器**：使用装饰器增强或修改函数的行为。

```python
# 高阶函数
def square(x):
    return x**2

squares = list(map(square, [1, 2, 3, 4]))

# 匿名函数
squares = list(map(lambda x: x**2, [1, 2, 3, 4]))

# 装饰器
def decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@decorator
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
```







#### 函数说明文档

  函数添加说明文档可以辅助理解函数的作用

  通过多行注释的形式对函数进行说明解释

  在pyCharm 中编写代码时，可以通过鼠标悬停查看调用函数的说明文档

```python
def func(x,y):
  """
  函数说明
  :param x: 形参 x 的说明
  :param y: 形参 y 的说明
  :return : 返回值的说明
  """
```





#### 函数进阶

##### 多个返回值

```python
def test_return():
		return 1,2		# 支持不同数据类型的return
		
x,y = test_return()	
print(x)		# 结果一
print(y)		#	结果二
```



##### 多种参数使用形式

**关键字参数**：在传参时指定某一参数，可不按照参数的定义顺序传参



**缺省参数（默认值）**：在定义参数时定义该参数的默认值（需放置在参数定义列表的最后），可通过后期传参覆盖参数的默认值



**不定长参数/可变参数**：用于不确定调用时传递参数的个数（包括不传参）的场景

​	作用：当调用函数不确定参数个数时，可以使用不定长参数

​	类型：

​				位置传递：def user_info ( *args )，传进的所有参数都会被args变量收集，根据传进参数的位置合并成一个元组，args属于元组类型

​				关键字传递：def user_info ( **kwargs )，参数需要属于 键值对类型，同时args属于字典类型

​							







##### lambda表达式

​		在函数的定义中

​		def 关键字，可以定义带有名称的函数，可以基于名称重复使用

​		lambda关键字，可以定义匿名函数，只可临时使用一次



​		**语法：** lambda 传入参数 ：函数体（一行代码）

​		**注意事项：**匿名函数用于临时构建一个函数，只用一次的场景

​						   且函数体只能写一行代码！





#### 迭代器



​	迭代器是一种特殊类型的对象，它实现了迭代协议，即提供了 __iter__() 和 __next__() 两个特殊方法，使得对象能够被用于迭代，开发者可以以统一且高效的方式遍历各种容器（如列表、元组、字典、集合等）以及自定义数据结构。



​		

​	**优势**

​			内存效率：迭代器仅在需要时生成下一个元素，避免一次性加载整个数据集到内存中，这对于处理大型数据集至关重要。

​			通用性：任何实现了迭代协议的对象都可以用相同的语法进行迭代，提高了代码的可复用性和抽象层次。

​			延迟计算：对于生成器等动态生成值的迭代器，能够在运行时按需计算，适用于处理无限序列或依赖于外部状态的数据。

​			易于并行化：由于迭代器提供了对数据的有序访问，它们自然地适用于并行处理框架，如通过分块或分区进行分布式计算。





•__iter__()：此方法返回迭代器对象自身。当一个对象被用于 for 循环或其它需要迭代器的上下文中时，Python 会自动调用此方法。对于迭代器对象来说，通常返回 self 即可。



•__next__()：此方法返回迭代器的下一个元素。每次调用 next() 函数或在 for 循环中前进到下一个元素时，都会调用此方法。当没有更多元素可返回时，应抛出 StopIteration 异常以表明迭代已结束。



```python
fruits = ["Apple", "Banana", "Cherry"]

# 直接使用 for 循环迭代容器
for fruit in fruits:
    print(fruit)

# 显式创建迭代器对象并使用 next() 函数
it = iter(fruits)
while True:
    try:
        fruit = next(it)
        print(fruit)
    except StopIteration:
        break
        
"""
在上述代码中，第一段使用 for 循环直接遍历列表，Python 自动调用了 fruits.__iter__() 获取迭代器，并在每次循环中调用 it.__next__()。
第二段则显式创建了迭代器对象 it，并通过 next() 函数手动控制迭代过程。
"""        
```







##### **生成器（Generator）**

```python
squares = (x ** 2 for x in range(10))  # 生成器表达式
for square in squares:
    print(square)
   
```



##### reversed函数

``reversed``可以从后向前迭代一个序列：

```python
list(reversed(range(10)))
# [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
```

要记住``reversed``是一个生成器（后面详细介绍），只有实体化（即列表或for循环）之后才能创建翻转的序列。







#### 序列函数

Python有一些有用的序列函数。



##### enumerate函数

迭代一个序列时，你可能想跟踪当前项的序号。手动的方法可能是下面这样：

```python
i = 0
for value in collection:
   # do something with value
   i += 1
```



因为这么做很常见，Python内建了一个 ``enumerate`` 函数，可以返回``(i, value)``元组序列：

```python
for i, value in enumerate(collection):
   # do something with value
```



当索引数据时，使用``enumerate``的一个好方法是计算序列（唯一的）``dict``映射到位置的值：

```python
some_list = ['foo', 'bar', 'baz']

# 将列表转换为字典数据
mapping = {}

for i, v in enumerate(some_list):
     mapping[v] = i

mapping
{'bar': 1, 'baz': 2, 'foo': 0}
```







##### sorted函数

``sorted``函数可以接受和``sort``相同的参数（效果相同）。

``sorted``函数可以从任意序列的元素返回一个新的排好序的列表：

```python
sorted([7, 1, 2, 6, 0, 3, 2])
[0, 1, 2, 2, 3, 6, 7]

sorted('horse race')
[' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's']
```







##### zip函数

``zip``可以将多个列表、元组或其它序列成对组合成一个元组列表：

```python
seq1 = ['foo', 'bar', 'baz']

seq2 = ['one', 'two', 'three']

zipped = zip(seq1, seq2)

list(zipped)
# [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
```



``zip``可以处理任意多的序列，元素的个数取决于最短的序列：

```python
seq3 = [False, True]

list(zip(seq1, seq2, seq3))
# [('foo', 'one', False), ('bar', 'two', True)] 多余的直接舍弃
```



``zip``的常见用法之一是同时迭代多个序列，可能结合``enumerate``使用：

```python
for i, (a, b) in enumerate(zip(seq1, seq2)):
     print('{0}: {1}, {2}'.format(i, a, b))

    
0: foo, one
1: bar, two
2: baz, three
```





给出一个“被压缩的”序列，``zip``可以被用来解压序列。也可以当作把行的列表转换为列的列表。这个方法看起来有点神奇：

```python
pitchers = [('Nolan', 'Ryan'), ('Roger', 'Clemens'),
             ('Schilling', 'Curt')]

first_names, last_names = zip(*pitchers)

first_names
('Nolan', 'Roger', 'Schilling')

last_names
('Ryan', 'Clemens', 'Curt')
```











### 基础函数



Python 提供了许多内置函数，用于执行各种常见操作，如数据类型转换、数学计算、字符串处理等。以下是一些常见的 Python 内置函数及其用途：

数据类型转换函数

	•	int(): 将一个值转换为整数。
	•	float(): 将一个值转换为浮点数。
	•	str(): 将一个值转换为字符串。
	•	bool(): 将一个值转换为布尔类型。
	•	list(): 将一个值转换为列表。
	•	tuple(): 将一个值转换为元组。
	•	dict(): 将一个值转换为字典。
	•	set(): 将一个值转换为集合。

#### 数学计算函数

	•	abs(): 返回数字的绝对值。
	•	round(): 对数字进行四舍五入。
	•	max(): 返回最大值。
	•	min(): 返回最小值。
	•	sum(): 返回序列的总和。
	•	pow(): 返回 x 的 y 次幂，等同于 x ** y。
	•	divmod(): 返回两个数字的商和余数。

#### 序列处理函数

	•	len(): 返回对象的长度（元素个数）。
	•	range(): 生成一个序列，常用于循环。
	•	sorted(): 返回一个排序后的列表。
	•	reversed(): 返回一个反转的迭代器。
	•	enumerate(): 返回一个枚举对象，将序列中的元素和其索引一起返回。
	•	zip(): 将两个或多个序列“压缩”成一个元组的序列。

#### 字符串处理函数

	•	format(): 格式化字符串。
	•	chr(): 将 Unicode 码点转换为字符。
	•	ord(): 将字符转换为 Unicode 码点。
	•	str.join(): 将序列中的元素连接成一个字符串。
	•	str.split(): 将字符串拆分为列表。
	•	str.strip(): 移除字符串两端的空白字符。

#### 输入输出函数

	•	print(): 打印输出到控制台。
	•	input(): 从控制台读取输入。

#### 文件处理函数

	•	open(): 打开文件并返回文件对象。
	•	close(): 关闭文件对象。
	•	read(): 读取文件内容。
	•	write(): 向文件写入内容。

#### 其他常见函数

	•	type(): 返回对象的类型。
	•	isinstance(): 检查一个对象是否是指定的类型。
	•	id(): 返回对象的唯一标识符。
	•	help(): 调用内置的帮助系统。
	•	dir(): 返回对象的属性和方法列表。
	•	eval(): 执行字符串表达式并返回结果。
	•	exec(): 执行字符串代码。

#### 示例代码

以下是一些使用这些内置函数的示例代码：

```python
# 数据类型转换

num = int("123")
flt = float("123.45")
string = str(123)
lst = list((1, 2, 3))

# 数学计算

absolute_value = abs(-5)
rounded_value = round(3.14159, 2)
maximum = max(1, 2, 3)
minimum = min(1, 2, 3)
total = sum([1, 2, 3])
power = pow(2, 3)
quotient, remainder = divmod(10, 3)

# 序列处理

length = len([1, 2, 3])
range_list = list(range(5))
sorted_list = sorted([3, 1, 2])
reversed_list = list(reversed([1, 2, 3]))
enumerated_list = list(enumerate(['a', 'b', 'c']))
zipped_list = list(zip([1, 2, 3], ['a', 'b', 'c']))

# 字符串处理

formatted_string = "{} {}".format("Hello", "World")
character = chr(97)
unicode_point = ord('a')
joined_string = "-".join(["a", "b", "c"])
split_string = "a-b-c".split("-")
stripped_string = "  hello  ".strip()

# 输入输出

print("Hello, World!")
user_input = input("Enter something: ")

# 文件处理

with open("example.txt", "w") as f:
    f.write("Hello, World!")

# 其他常见函数

print(type(123))
print(isinstance(123, int))
print(id(123))
help(len)
print(dir(list))
result = eval("3 + 5")
exec("x = 5\nprint(x)")
```















### 文件操作

#### open（name , mode , encoding ）

​	**作用：**		可以打开一个已存在的文件或创建一个新文件



##### 	**参数详解：**

​		**name**：需要打开的目标文件名

​		**mode**：设置打开文件的模式（访问模式）：只读，写入，追加等

​				**r :** 以只读方式打开文件，文件的指针将会放在文件的开头，这是默认模式

​				**w：**打开一个文件只用于写入，如果该文件已存在则打开文件，并从文件的开头开始编辑，原有内容会被删除，如果文件不存在则创建新文件（**覆盖写**）

​				**a：**打开一个文件用于追加，如果该文件已存在则打开文件，并从文件的尾部追加写入新的内容，原有内容不变，如果文件不存在则创建新文件**（追加写）**

​		**encoding**：编码格式（推荐默认使用 UTF-8 ）



```python
f = open('python.txt','r','encoding = UTF - 8')

# encoding的顺序编码不在第三位，所以不能使用位置参数，需要用关键字参数直接指定
# 此处的 f 是 open 函数的文件对象，对象是python中一种特殊的数据类型，拥有属性和方法，可以使用对象.属性或对象.方法对其进行访问
```



##### 	常用方法-读

​			在文件没关闭之前（ close ），读取数据之后都会使得指针定位到被读取数据的尾部，使得以上数据不再被访问



​			**read ( ) 方法**

​					文件对象.read ( num ）：num表示要从文件中读取的数据长度（单位为字节），如果没有传参则默认读取文件中的所有数据，返回一个字符串类型

总结：一次全读，返回一个字符串						

​		

​			**readlines ( ) 方法**

​					readlines可以按照行的形式将整个文件的内容进行一次性读取，并返回一个列表，其中每一行数据作为一个元素

总结：一次全读，返回一个列表，一个元素为一行数据



​			**readline ( ) 方法**

​					一次只读取一行



​			**close ( ) 方法**

​					关闭文件对象，即关闭对该文件的占用，否则会一直被python程序占用		

​				

​			**with open ()  方法**

​					通过在 with open 的语句块中对文件进行操作

​					该方法可以在操作完成之后自动关闭文件（close），避免忘掉close方法

​					

```python
with open("python.txt","r") as f:
	f.readlines()
  
  
path = 'ch02/xxx/yyy/aa.json'
# 默认按行读取返回一个列表，如果文件中是一行数据为一个完整的json对象，则列表中的每一个元素都是完整的json对象
records = [json.loads(line) for line in open(path)]
records[0]['tr'] # 取第一个json对象中tr对应的值
```

 



##### 常用方法-写

​	直接调用 **write** 时，内容并未真正写入到文件中，而是积攒在程序的内存中（缓冲区）



​	当调用 **flush / close** 时才会将内容真正写入到文件中，该做法是为了避免频繁的操作硬盘，导致效率下降



​	可配合 **\n** 手动实现换行操作，	**close 方法自带 flush 功能**

```python
f = open("word.txt", "r", encoding="UTF-8")
num = f.read().count("Hadoop")  # read 方法返回一个字符串
print(f"Hadoop在文件中出现的次数为：{num}")
f.close()

with open("word.txt", "r") as f:  # 该写法可以省略 close
    num_01 = f.read().count("\n")
    print(f"文件中出现换行的次数为：{num_01}")

count = 0
with open("word.txt", "r") as f:  # 该写法可以省略 close
    for line in f:
        new_line = line.strip()  # 去除开头和结尾的换行符以及空格
        words = new_line.split(" ")
        for word in words:
            if word == "Hadoop":
                count += 1

    # 判断单词出现的次数并累计
    print(f"Hadoop在文件中出现的次数为：{count}")

"""
    数据备份 
"""


f = open("ceshi.txt", "r", encoding="UTF-8")
w = open("zhengshi.txt", "w", encoding="UTF-8")

# 这里使用的是 Python 中对文件对象进行迭代的特性，此时的f并不是列表，而是一个文件对象
# Python 中，对文件对象进行迭代时，每次循环都会自动调用 f.readline() 方法读取下一行，并将其作为循环变量（这里是 line）的值。直到文件末尾，没有更多行可读取时，迭代结束。

for line in f:
    if line.strip().split(" ")[4] == "测试":
        continue  # 直接进入下一次循环
    w.write(line)

w.flush()
w.close()
f.close()
```







### 异常操作

#### 		**基本语法**

​			**try :**

​					可能发生错误的代码

​			**except (异常类型...) /   Exception / 不写 （用于捕获全部异常） as e:**

​					如果出现异常执行的代码

​			**else:**

​					未发生异常时执行的代码

​			**finally：**

​					无论是否发生异常都会执行的代码，一般用于关闭资源



#### 		**注意事项：**

​			如果尝试执行的代码所出现的异常类型与要捕获的异常类型不一致，则无法捕获异常

​			一般 try 下方只放一行尝试执行的代码

​			Exception：顶级异常，能取到所有的异常，也可以直接为空/不写



​			**异常具有传递性**





### 模块导入

#### 	**基础语法**

​			模块在使用前需要先导入

​			**[ from ] import [ 模块 | 类  | 变量 | 函数 | * ]  [ as  别名]** 

​	

##### 	常见组合形式：

​			**import	模块名**

​			**from  模块名  import  类，变量，方法等**

​			**from  模块名  import ***

​			**import   模块名  as  别名**

​			**from   模块名   import  功能名  as 别名**



##### 		**_ _  main  _ _  变量的功能**

​			if  _ _ name _ _  =	' _ _  main  _ _ ' 表示只有当该程序是直接运行，而不是被调用时，name的值会变为main

​			才能进入到 if 内部，会运行 if 内部的程序



##### 		_ _  all  _ _   变量的功能

​			可用来控制导入该模块时所用的 * 的导入范围，在该范围内的才可以被 * 取得

​			_ _ all _ _  =  [ ' 方法名 ' , ' 方法名 ']			





#### 	Python 包

​			包就是一个文件夹，里面可以存放许多Python的模块（代码文件），通过包，在逻辑上讲一批模块归为一类，方便使用

​			

##### 		_ _ init _ _.py文件的作用

​			创建 Python 包时会默认自动创建的文件，可以通过该文件表示一个文件夹是 Python 包，而不是普通的文件夹，可以在 _ _ init _ _.py 文件中定义 _ _ all _ _ 变量







### 时间函数

格式字符串示例：

​	•%Y：四位数的年份（如 2024）

​	•%m：两位数的月份（01 到 12）

​	•%d：两位数的日期（01 到 31）

​	•%H：24小时制的小时（00 到 23）

​	•%M：分钟（00 到 59）

​	•%S：秒（00 到 59）

​	• %a（短星期名）、%A（全星期名）、%b（短月份名）、%B（全月份名）等

​	



相关模块：

#### 	

### Pyecharts

##### 	set_global_opts	设置全局配置

```python
# 导入Line功能构建折线图对象
from pyecharts.charts import Line

# 得到折线图对象
line = Line()

line.set_global_opts(
	title_opts = TitleOpts("测试",pos_left = "center",pos_bottom="1%"),
  legend_opts = LegendOpts(is_show = True),
  toolbox_opts = ToolboxOpts(is_show = True),
  visualmap_opts = VisualMapOpts(is_show = True),
  tooltip_opts = TooltipOpts(is_show = True)
)
```



### 杂卉杂花



```python
"""
演示函数综合案例开发
"""

# 定义全局变量 money  name
money = 30000
# name = None  # None 表示该变量的值与类型都处于待定状态
# 定义查询函数
def query(show_header):
    if show_header == 1:
        print("---------- 查询余额 ----------")
    print(f"您的余额剩余： {money}")


def saving(name, num):
    global money  # global 在函数体内定义全局变量
    money += num
    print("---------- 存款操作 ----------")

    print(f"{name} 用户，您本次存款金额为： {num}")
    query(0)


def getmoney(name, num):
    global money  # global 在函数体内定义全局变量
    money -= num
    print("---------- 存款操作 ----------")

    print(f"{name} 用户，您本次取款金额为： {num}")
    query(0)


def main():
    # 要求客户输入姓名
    name = input("请输入您的姓名： ")

    while True:
        print("---------- 主菜单 ----------")
        print("查询余额请 [输入1]")
        print("存款操作请 [输入2]")
        print("取款操作请 [输入3]")
        print("退出系统请 [输入0]")
        flag = int(input("请选择您需进行操作：   "))

        if flag == 1:
            query(1)
        elif flag == 2:
            num = int(input("请输入您的存款金额： "))
            saving(name, num)
        elif flag == 3:
            num = int(input("请输入您的存款金额： "))
            getmoney(name, num)
        else:
            print("--- 退出系统，欢迎下次光临 ---")
            break

```







==================









```python
"""
    面向对象，数据分析案例，主业务逻辑代码
    实现步骤：
        设计一个类，完成数据的封装
        设计一个抽象类，定义文件读取的相关功能，并使用子类实现具体功能
        读取文件，生产数据对象
        进行数据需求的逻辑计算（计算每一天的销售额）
"""

from file_define import FileReader, TextFileReader, JsonFileReader
from data_define import Record

# 传入参数创建相对应的对象实例
text_file = TextFileReader("一月销售数据路径-path")
json_file = JsonFileReader("二月销售数据路径-path")

# 调用对象方法，获取对应路径下数据的列表
jan_data: list[Record] = text_file.read_data()
feb_data: list[Record] = json_file.read_data()

# 将两个月份的销售数据合并为一个 list
all_data: list[Record] = jan_data + feb_data


# 进行数据计算- 每一天的销售额
data_dict = {}
for record in all_data:
    if record.date in data_dict.keys():
        # 当前日期已有记录，与已有值相加即可
        data_dict[record.date] += record.money
    else:
        # 当前日期未被记录，新增该日期与值即可
        data_dict[record.date] = record.money





"""
数据定义的类
"""


class Record:
    # 使用构造方法
    def __init__(self, date, order_id, money, province):
        self.date = date  # 订单日期
        self.order_id = order_id  # 订单ID
        self.money = money  # 订单金额
        self.province = province  # 所属省份

    def __str__(self):
        return f"{self.date},{self.order_id},{self.money},{self.province}"
        
        
        
        
        
"""
与文件相关的类定义
"""
import json

from data_define import Record


# 定义一个抽象类，用于作为顶层设计，确定需要实现的功能
class FileReader:

    def read_data(self) -> list[Record]:
        """" 将读取到的每一条数据都转换成 Record 对象 ，并将这些对象都封装到 list 内返回 """
        pass


# 定义读取文本文件的工具类（ CSV 格式 ）
class TextFileReader(FileReader):
    # 定义成员变量，保存文件的路径
    def __init__(self, path):
        self.path = path

    # 实现父类的抽象方法
    def read_data(self) -> list[Record]:
        f = open(self.path, "r", encoding="UTF-8")

        record_list: list[Record] = []
        for line in f.readlines():
            # print(line)  直接输出会多出换行符，所以需要剔除两边的空格以及末尾的换行符 \n
            data_list = line.strip().split(" ")
            # 构造相应对象
            record = Record(data_list[0], data_list[1], int(data_list[2]), data_list[3])  
            record_list.append(record)

        f.close()
        return record_list


# 定义读取 Json 文件格式的工具类
class JsonFileReader(FileReader):
    # 定义成员变量，保存文件的路径
    def __init__(self, path):
        self.path = path

    # 实现父类的抽象方法
    def read_data(self) -> list[Record]:
        f = open(self.path, "r", encoding="UTF-8")

        record_list: list[Record] = []
        for line in f.readlines():
            data_dict = json.loads(line)
            record = Record(data_dict["date"], data_dict["order_id"], int(data_dict["money"]), data_dict["province"])
            record_list.append(record)

        f.close()
        return record_list


if __name__ == '__main__':
    list01 = TextFileReader("/Users/buzhi/PycharmProjects/pythonProject/primary_demo/word.txt").read_data()
    for i in list01:
        print(i)        

```





### Python面向对象



#### 类的定义和使用

##### 基础语法

​	class 类名称：	class是定义类的关键字

​			类的属性：	定义在类中的变量（成员变量）

​			类的行为：	定义在类中的函数（成员方法）



**创建类对象的语法**：

​	对象名称 = 类名称（）



**成员方法的定义语法：**

​	**def  方法名 ( self , 行参.... ) :** 

​			**方法体**

​	

​	（ 类中的函数称为方法 ）方法定义的参数列表中必须有 self 关键字

​		用于表示类对象自身的意思，传参时可以忽略掉

​		使用类对象调用方法时，self 会自动被python传入

​		在方法内部，想访问类的成员变量必须使用self







#### 魔术方法

​			  属于python内置的方法之一，各自有各自特殊的功能，这些内置方法可称为：魔术方法

​		

##### 常见方法：

​	**if  name == 'main'**

​		常见的惯用语法，用于控制某些代码块仅在直接运行该脚本时执行，而在该脚本被作为模块导入时不执行。

**详细解释**



​	•	**__name__**：这是一个特殊变量，当 Python 解释器运行一个模块时，它会根据执行方式将 __name__ 变量赋值为 '__main__' 或模块的名称。

​	•	**直接运行脚本**：

​	•	如果脚本是被直接运行的（例如，运行命令 python script.py），那么 __name__ 的值将被设置为 '__main__'。

​	•	**作为模块导入**：

​	•	如果脚本是被另一个脚本作为模块导入的，那么 __name__ 的值将被设置为这个脚本的文件名（不包括扩展名）。









​				  **_ _ init _ _( )**  方法（称为构造方法）

​	**功能**：

​				在创建类对象（构造类）的时候会自动执行

​				在创建类对象（构造类）的时候，将传入的参数自动传递给 _ _ init _ _ ( )  方法 使用



​				**_ _  str _ _ ( )  方法**（称为字符串方法）

​	**功能：**		

​				类似于重写 Java 的 **toString** 方法，控制类对象转换为字符串的行为

```python
class Student:
		def __init__(self,name,age):
				self.name = name
				self.age = age
				
		def __str__(self):
				return f"Student类对象,name = {self.name},age = {self.age}"
      
    # 如果该类不定义 __str__  方法，则直接输出该类对象时输出的值为对应地址
```

​			



​				**_ _  lt _ _ ( )  方法**（小于/大于符号比较）

```python
class Student:
		def __init__(self,name,age):
				self.name = name
				self.age = age
        
    def __lt__ (self,other):    
      	return self.age < other.age
      

      
stul = Student("困困",18)
stu2 = Student("壮壮",23)

print(stul < stu2)		# 返回false
print(stul > stu2)		#	返回true
# 如果该类没有定义 lt 方法就直接比较该类的对象则报错


```



​	**_ _  le _ _ ( )  方法** （小于等于/大于等于）

​    **_ _  eq _ _ ( )  方法**（==符号比较）









#### 私有成员

​	**封装的概念：**将现实世界中的事物在类中描述为属性和方法



​	类中提供了私有成员的形式支持不公开的属性和行为

​	私有成员无法被类对象使用，但可以被内部（类中）的其他成员使用



​	**分类：** 

​				私有成员变量：无法直接被类对象使用

​				私有成员方法：无法赋值，也无法获取值 



​	**定义方式：**	

​			私有成员变量：	变量名以 _ _ 开头

​			私有成员方法：	方法名以 _ _ 开头





```python
class Phone
		__current_voltage = 1	    #	当前手机运行电压
		
		def __keep_single_core(self):
				print("使CPU以单核模式运行")
				
		def call_by_5g(self):
				if __current_voltage >= 1:
						print("5G童话模式已开启")
						
				else:
						__keep_single_core
						print("电量不足，无法使用5G通话模式，并已设置单核模式进行省电")
```







#### 继承



​			多继承类中出现同名方法或同名变量时，先继承类的同名方法/变量的优先



##### 		pass 关键字

​		pass是占位语句，用来保证函数（方法）或类定义的完整性，表示无内容/空的意思







##### 		调用父类同名成员

​		在复写父类成员之后类对象调用对应成员时会调用复写后的新成员，如果需要使用被复写的父类成员，则需要特殊的调用方法：



​		调用父类成员

​				使用成员变量：父类名.成员变量

​				使用成员方法：父类名.成员方法（self）

​		使用super （）调用父类成员

​				使用成员变量：super （）.成员变量

​				使用成员方法：super （）.成员方法（）	**此用法不用需要传入self**



​				注意：**只可以在子类内部调用父类的同名成员，子类的实体类对象调用默认是调用子类的复写成员**





#### 多态

​		同一个行为，使用不同的对象获得不同的状态





#### PyMySQL

```python
from pymysql import Connection

# import  pymysql
# 获取到mysql数据库的链接对象
conn = Connection(
    host='localhost',  # 主机名 （IP地址）
    port=3306,
    user='root',
    password='520333',
    autocommit=True    # 将修改数据库确认语句设置为自动提交，默认为false
)


# 获取执行SQL语句的游标对象
cursor = conn.cursor()

# 指定对应操作的数据库 类似于 use 关键字
conn.select_db("buzhi")

# 使用游标对象执行SQL语句 execute:执行
# 执行数据库操作的语句
# cursor.execute("CREATE TABLE test_pymysql(id INT,info VARCHAR(255))")

# 执行查询操作的语句
cursor.execute("select * from data_01 limit 20")
# 获取查询结果
results: tuple = cursor.fetchall()
for i in results:
    print(i)


# 关闭数据库链接
cursor.close()

```







###### 实战三

```python
import json
import random
import string
import datetime
import sys


field_names = [
    "A001", "A002", "A003", "A004", "A005", "A006", "A007", "A008", "A009", "A010", "A011", "A012", "A013",
    "A014", "A015", "A016", "A017", "A018", "A019", "A020", "A021", "A022", "A023", "A024", "A025", "A026",
    "A027", "A028", "A029", "A030", "A031", "A032", "A033", "A034", "A035", "A036", "A037", "A038", "A039",
    "A040", "A041", "A042", "A043", "A044", "A045", "A046", "A047", "A048", "A049", "A050", "A051", "A052",
    "A053", "A054", "A055", "A056", "A057", "A058", "A059", "A060", "A061", "A062", "A063", "A064", "A065",
    "A066", "A067", "A068", "A069", "A070", "A071", "A072", "A073", "A074", "A075", "A076", "A077", "A078",
    "A079", "A080", "A081", "A082", "A083", "A084", "A085", "A086", "A087", "A088", "A089", "A090", "A091",
    "A092", "A093"
]


def get_random_name(filename):
    with open(filename, 'r', encoding='utf-8') as file:
        names = {name.strip() for name in file.readlines()}
    return random.choice(list(names))


def generate_random_date():
    start_date = datetime.datetime(1999, 2, 1)
    end_date = datetime.datetime.now() # 2024, 4, 16, 21, 23, 47, 242311 此时返回的数据格式
    random_date = start_date + datetime.timedelta(days=random.randint(0, (end_date - start_date).days))
    return random_date.strftime("%Y-%m-%d") # '2024-04-16'此时返回的数据格式


# 生成数据函数
def generate_data():
    data = {}
    flag = random.choice(["广东", "湖南", "江西"])
    for field_name in field_names:
        # 根据中文字段名称生成对应的值
        if field_name == "A001":
            value = ''.join(random.choices(string.digits, k=10))  # 生成长度为10的数字字符串
        elif field_name == "A002":
            value = random.choice(["授信申请", "支用申请", "登陆", "续期"])
        elif field_name == "A003":
            value = datetime.datetime.now().strftime("%Y-%m-%d")
        elif field_name in ["A006", "A062"]:
            value = ''.join(random.choices(string.digits, k=18))  # 证件号码
        elif field_name in ["A005", "A030", "A033", "A036", "A039", "A044", "A053"]:  # 获取姓名
            value = get_random_name(
                filename="Make_Data/name.txt")
        elif field_name in ["A007", "A020", "A028", "A031", "A034", "A037", "A055", "A056", "A057", "A065"]:  # 获取电话
            value = get_random_name(
                filename="Make_Data/phonenum.txt")
        elif field_name in ["A008", "A010", "A018", "A021", "A027", "A047", "A058", "A060", "A066", "A070"]:  # 获取地址
            value = get_random_name(
                filename="Make_Data/address.txt")
        elif field_name in ["A012", "A014", "A016", "A063", "A068"]:  # 获取名称
            value = get_random_name(
                filename="Make_Data/unitname.txt")
        elif field_name == "A023":
            value = get_random_name(
                filename="Make_Data/email.txt")
        elif field_name in ["A040", "A041", "A042", "A043"]:
            value = get_random_name(
                filename="Make_Data/bank_id.txt")
        elif field_name == "A045":
            value = get_random_name(
                filename="Make_Data/job_id.txt")
        elif field_name == "A004":
            value = random.choice(["0020030", "0020031", "0020033", "0020035", "FD", "XYK"])
        elif field_name in ["A024", "A025"]:
            value = random.choice(["博士", "硕士", "本科", "中专", "高中", "文盲"])
        elif field_name == "A026":
            value = random.choice(["初级", "中级", "高级", "资深", "专家", "顾问"])
        elif field_name in ["A029", "A032", "A035", "A038"]:
            value = random.choice(["父子", "父女", "母子", "母女", "兄弟", "祖父母", "配偶"])
        elif field_name == "A046":
            value = random.choice(["企业单位", "事业单位", "行政机关", "政府机构", "社会组织", "国际组织"])
        elif field_name in ["A049", "A050", "A051"]:
            value = generate_random_date()
        elif field_name == "A052":
            value = random.choice(["低于一百万", "一百万至五百万", "五百万至一千万", "一千万以上"])
        elif field_name in ["A054", "A082", "A085"]:
            value = random.choice(["True", "False", "-"])
        elif field_name in ["A009", "A011", "A013", "A015", "A017", "A019", "A059", "A061", "A064", "A067", "A069",
                            "A071"]:
            value = ''.join(random.choices(string.digits, k=8))  # 证件号码
        elif field_name == 'A072':
            value = get_random_name(
                filename="Make_Data/ip.txt")
        elif field_name in ["A080", "A081", "A083"]:
            value = random.choice(["00", "01", "04", "02", "03", "00"])
        elif field_name == "A084":
            value = random.randint(1, 3000)
        elif field_name in ["A074", "A086"]:
            value = flag
        elif field_name == "A075":
            if flag == "广东":
                value = get_random_name(
                    filename="Make_Data/guangdong.txt")
            elif flag == "江西":
                value = get_random_name(
                    filename="Make_Data/jiangxi.txt")
            else:
                value = get_random_name(
                    filename="Make_Data/hunan.txt")
        else:
            value = "未知"
        data[field_name] = value
    with open("Nake_Data/demo_data", "w", encoding="UTF-8") as file:
        json.dump(data, file, ensure_ascii=False)
    print(data)
    return data


x = 1
if len(sys.argv) > 1:
    x = int(sys.argv[1])


results = []
datas = ""
for _ in range(x):
    result = generate_data()
    results.append(result)
    datas = ",".join([json.dumps(item, ensure_ascii=False) for item in results])

print('{"input":'+datas+'}')
```







###### 实战四

将本地Excel的数据写入到MySQL中

```python
import pandas as pd

df01 = pd.read_excel('/Users/buzhi/Documents/tableau配套数据/全球超市订单数据.xlsx', sheet_name='订单')
df02 = pd.('/Users/buzhi/Documents/tableau配套数据/全球超市订单数据.xlsx', sheet_name='人员')


# 方法一
import pandas as pd
import mysql.connector

# 创建示例 DataFrame
data = {
    '用户名': ['Alice', 'Bob', 'Charlie', 'David'],
    '数量': [1, -2, 0, -3],
    '单价': [10.5, -5.2, 0, -2.5]
}
df = pd.DataFrame(data)

# 创建数据库连接
conn = mysql.connector.connect(
    host="localhost",
    user="username",
    password="password",
    database="database_name"
)

# 创建游标对象
cursor = conn.cursor()

# 创建表（如果不存在）
create_table_query = """
CREATE TABLE IF NOT EXISTS your_table_name (
    用户名 VARCHAR(255),
    数量 INT,
    单价 FLOAT
)
"""
cursor.execute(create_table_query)

# 插入数据
insert_query = "INSERT INTO your_table_name (用户名, 数量, 单价) VALUES (%s, %s, %s)"
for i, row in df.iterrows():
    cursor.execute(insert_query, tuple(row))

# 提交事务
conn.commit()

# 执行查询并加载结果到 DataFrame
query = "SELECT * FROM your_table_name"
df_query_result = pd.read_sql(query, conn)

# 关闭游标和连接
cursor.close()
conn.close()

# 显示查询结果
print(df_query_result)







# 方法二
from sqlalchemy import create_engine
import pandas as pd

# 创建 SQL Alchemy 引擎
engine = create_engine('mysql+pymysql://username:password@localhost/database_name')

# 创建示例 DataFrame
data = {
    '用户名': ['Alice', 'Bob', 'Charlie', 'David'],
    '数量': [1, -2, 0, -3],
    '单价': [10.5, -5.2, 0, -2.5]
}
df = pd.DataFrame(data)

# 将 DataFrame 数据存储到 MySQL 数据库中的 'your_table_name' 表
df.to_sql('your_table_name', con=engine, if_exists='replace', index=False)

# 执行查询并加载结果到 DataFrame
query = "SELECT * FROM your_table_name"
df_query_result = pd.read_sql_query(query, con=engine)

# 显示查询结果
print(df_query_result)





# 两者差异对比总结

"""
`mysql.connector` 和 `SQLAlchemy` 是两种不同的方法，用于连接和操作 MySQL 数据库。它们各有优点和用途，适合不同的场景。

1. 设计与抽象级别

- **mysql.connector**
  - 这是一个低级别的数据库连接库，直接与 MySQL 进行交互。
  - 提供了对 MySQL 的全面控制，允许进行详细的数据库操作。
  - 需要编写更多的代码来处理连接、游标和 SQL 语句的执行，适合那些需要细粒度控制和优化的场景。

- **SQLAlchemy**
  - 这是一个高级别的 ORM（对象关系映射）库，提供了一个更高层次的抽象。
  - 支持多种数据库（如 MySQL、PostgreSQL、SQLite 等），使得代码具有更好的可移植性。
  - 提供了更简洁的 API，使得与数据库的交互更容易和更具表达力。
  - 适合需要快速开发和维护的项目，尤其是那些需要处理复杂关系和模型的项目。

2. 代码简洁性与易用性

- **mysql.connector**
  - 需要手动处理连接、游标、SQL 语句的执行和事务管理。
  - 更冗长和繁琐，容易出错，需要更多的样板代码。


- **SQLAlchemy**
  - 提供了更高层次的 API，减少了样板代码。
  - 支持直接将 Pandas DataFrame 导入数据库，以及通过 Pandas 直接执行 SQL 查询。



3. 多数据库支持

- **mysql.connector**
  - 专用于 MySQL 数据库，不支持其他数据库。

- **SQLAlchemy**
  - 支持多种数据库，包括 MySQL、PostgreSQL、SQLite、Oracle 等。
  - 提供统一的 API，可以轻松地在不同数据库之间切换。

4. ORM 支持

- **mysql.connector**
  - 不提供 ORM 支持，需要手动编写 SQL 语句和处理结果集。

- **SQLAlchemy**
  - 提供强大的 ORM 功能，允许使用 Python 类和对象来表示数据库中的表和记录。
  - 简化了数据库操作，使得代码更具可读性和维护性。

5. 事务管理

- **mysql.connector**
  - 需要手动管理事务（commit 和 rollback）。

- **SQLAlchemy**
  - 提供自动事务管理，同时也允许手动控制事务。

总结

- **mysql.connector** 适合需要细粒度控制和优化的低级数据库操作。
- **SQLAlchemy** 提供更高级别的抽象和更简洁的 API，适合快速开发和维护，并支持多种数据库和 ORM 功能。
"""
```





