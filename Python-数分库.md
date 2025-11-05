#  Python-数分库 

set nu: Linux中vim编辑内显示对应的行数

## Jupyter

### 	常用快捷键

​			shift + enter ： 运行本单元，选中下一单元

​			command + enter ：运行本单元

​			Y：单元转入代码状态

​			M：单元转入markdown 状态

​			A：在上方插入新单元

​			B：在下方插入新单元

​			DD：删除选中的单元



###   魔术命令

  IPython中特殊的命令（Python中没有）被称作“魔术”命令。这些命令可以使普通任务更便捷，更容易控制IPython系统，魔术命令是在指令前添加百分号%前缀



​		%run ： 运行系统命令

​		%time ：测量代码的运行时间（一般用于耗时较长的代码段）

​		%timeit：测试代码的运行时间（一般用于耗时较短的代码段，会多次执行取的最终精确值）

​		%%time：测试多行代码的运行时间

​		



##### 自省

​	1:	在变量前后使用问号？，可以显示对象的信息，可以作为对象的自省

​	2:	如果对象是一个函数或实例方法，定义过的文档字符串，也会显示出信息

​				使用??会显示函数的源码

```python
b = [1, 2, 3]
b?

# 返回以下信息：
Type:       list
String Form:[1, 2, 3]
Length:     3
Docstring:
list() -> new empty list
list(iterable) -> new list initialized from iterable's items


# 这可以作为对象的自省。如果对象是一个函数或实例方法，定义过的文档字符串，也会显示出信息
def add_numbers(a, b):
    """
    Add two numbers together

    Returns
    -------
    the_sum : type of arguments
    """
    return a + b
  
  
  
add_numbers?

# 返回以下信息：
Signature: add_numbers(a, b)
Docstring:
Add two numbers together

Returns
-------
the_sum : type of arguments
File:      <ipython-input-9-6a548a216e27>
Type:      function
  
  
# 使用??会显示函数的源码：
add_numbers??


# 返回以下信息：
Signature: add_numbers(a, b)
Source:
def add_numbers(a, b):
    """
    Add two numbers together

    Returns
    -------
    the_sum : type of arguments
    """
    return a + b
File:      <ipython-input-9-6a548a216e27>
Type:      function
```







##### %run命令

​		可以用 %run 命令运行所有的Python程序

```python
# 假设有一个文件 ipython_script_test.py：
def f(x, y, z):
    return (x + y) / z

a = 5
b = 6
c = 7.5

result = f(a, b, c)



# 这段脚本运行在空的命名空间（没有import和其它定义的变量），因此结果和普通的运行方式
# 文件中所有定义的变量（import、函数和全局变量，除非抛出异常），都可以在IPython shell中随后访问
%run ipython_script_test.py

c
7.5

result
1.4666666666666666

# 如果Python脚本需要命令行参数（在 sys.argv 中查找），则可以在文件路径之后直接传递参数，就像在命令行上运行一样


# %run 是一个 IPython 中特有的魔术命令（magic command）。%run 是 IPython 提供的一个便利功能，旨在增强交互式编程和数据分析的体验，不是 Python 标准库的一部分，不能在非 IPython 环境下使用。

# 在标准的 Python 解释器或普通的 Python 脚本中，%run 命令是不可用的。标准 Python 程序中要运行另一个脚本文件，通常会使用 import 语句来导入模块，或者使用 execfile（在 Python 2 中）或类似 exec(open(filename).read())（在 Python 3 中）的方式。
```







##### %load

​	在Jupyter notebook中，也可以使用 %load 将脚本导入到一个代码格中



```python
>>> %load ipython_script_test.py

    def f(x, y, z):
        return (x + y) / z
    a = 5
    b = 6
    c = 7.5

    result = f(a, b, c)
```





##### %paste /  %cpase

​	如果使用Jupyter notebook，你可以将代码复制粘贴到任意代码格执行。在IPython shell中也可以从剪贴板执行



```python
# 假设在其它应用中复制了如下代码

x = 5
y = 7
if x > 5:
    x += 1

    y = 8
    
    
# %paste 可以直接运行剪贴板中的代码    
%paste
x = 5
y = 7
if x > 5:
    x += 1

    y = 8
## -- End pasted text --


#  %cpaste 功能类似，但会给出一条提示
%cpaste
Pasting code; enter '--' alone on the line to stop or use Ctrl-D.
:x = 5
:y = 7
:if x > 5:
:    x += 1
:
:    y = 8
:--
```















## Numpy

​		plt.imread 读取图片的数据

​		plt.imshow 将数字显示成图片



### 	NumPy数组 - ndarray



ndarray 是一系列同类型数据的集合，以0为下标开始进行集合中元素的索引

​				该对象用于存放**同类型**元素的多维数组，其中每个元素都有**相同存储大小**的区域

​				**python中的list可以存储不同数据类型的元素**



#### 	ndarray常见重要属性

​		**size**：总长度/总数据量

​		**ndim** : 维度

​		**shape**：形状（各维度的长度）

​		**dtype**：元素类型



####  	**ndarray常见数据类型**

​					int:	int8	uint8	**int16	int32**	int64

​					float:	float16	**float32**	float64

​					str：字符串



​		nan：数值类型，表示为空值

​		not a number：不是一个正常的数值，表示空

​		np.nan：float类型



​		numpy 默认 ndarray中所有的元素都具有相同的数据类型

​		如果传进来的列表中包含不同类型的数据，则会自动统一为同一类型（列表[]中可以存放不同数据类型的数据）

​		优先级：str > float > int



#### 		ndarray的创建

​	1:	**使用np.array() 由 python 的 list （列表[] ）创建**

​						**n = np.array(list)**

```python
l = [1,3,6,2,9,5]
n = np.array(l)

array([1, 3, 6, 2, 9, 5])
```



​	2:	**使用np 的常规函数创建**

​				**np.zeros():**	创建一个所有元素都为 0 的多维数组

```python
np.zeros(shape=(3,4),dtype=np.int16)

array([[0, 0, 0, 0],
       [0, 0, 0, 0],
       [0, 0, 0, 0]], dtype=int16)

# 生成一个与传入数组结构相同，但值全为0的数组
np.zeros_like(x)
```

​				

**np.ones(shape, dtype=None,order='C')：**

​		创建一个所有元素都为 1 的多维数组

​		参数说明：

​								shape：形状（元组）

​								dtype=None：元素类型

​								order：C/F	一般默认即可

​									C：是否在内存中以行为主顺序存储多维数据（C-风格）

​									F：是否在内存中以列为主顺序存储多维数据（Fortran-风格）

​						

```python
n=np.ones(shape=(3,4),dtype=np.int16)

array([[1., 1., 1., 1.],
       [1., 1., 1., 1.],
       [1., 1., 1., 1.]])
```

​					

**np.full(shape, fill_value, dtype=None,order='C'):**	

​				创建一个所有元素都为 fill_value 的多维数组



```python
np.full(shape=(3,4), fill_value=6, dtype=None,order='C')

array([[6, 6, 6, 6],
       [6, 6, 6, 6],
       [6, 6, 6, 6]])
```





**np.eye(N,M=None,K=0,dtype=float):**

​				创建一个对角线上值为1，其余位置值为零的二维数组

​							N：行数

​							M：列数，默认为None 表示列数等于行数

​							K=0：对角线向右偏移0个位置（可为负数）

```python
np.eye(N=4,dtype=np.int8)

array([[1, 0, 0, 0],
       [0, 1, 0, 0],
       [0, 0, 1, 0],
       [0, 0, 0, 1]], dtype=int8)
```

​						**单位矩阵：**多行多列矩阵的对角线上的值都为一，其余位置的值都为零，单位矩阵的行数等于列数





**np.linspace(start, stop, num=50,endpoint=True,retstep=False,dtype=None)**

​			创建一个等差数列

​						参数说明：

​							**start**：开始值

​							**stop**：结束值

​							**num** = 50：该数列中默认有五十个数

​							endpoint=True：是否包含结束值

​							retstep=False：是否返回等差值（步长）

​							type=None：元素类型



```python
n = np.linspace(0,100,num=51,dtype=np.int16)

array([  0,   2,   4,   6,   8,  10,  12,  14,  16,  18,  20,  22,  24,
        26,  28,  30,  32,  34,  36,  38,  40,  42,  44,  46,  48,  50,
        52,  54,  56,  58,  60,  62,  64,  66,  68,  70,  72,  74,  76,
        78,  80,  82,  84,  86,  88,  90,  92,  94,  96,  98, 100],
      dtype=int16)
```





**np.arange([start,]stop,[step,]dtype=None)**

​		参数说明：

​							start：开始值（可选，默认为零）

​							stop：结束值（结果不包含该值）

​							step：步长（可选）



​						创建一个数值范围之内的数组，与**python的range**功能类似

​						arange()类似于range()，在给定间隔内返回均匀间隔的数值：

```python
n = np.arange(10)
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

​			



**np.random.randint（low,high=None,size=None,dtype='I'） 常用**

​			创建一个随机整数的多维数组

​					参数说明：

​						low：最小值

​						high=None：最大值

​								high=None时，生成的数值范围在[0，low]区间内

​								当high有具体赋值时，则生成值的范围在[low，high]区间内

​						size=None：数组形状，默认值输出一个随机值

​						dtype=None：元素类型b



```python
np.random.randint(3)
2

np.random.randint(3,10)
8

np.random.randint(3,10,size=6)
array([8, 4, 8, 9, 3, 5])

np.random.randint(3,10,size=(3,4))
array([[8, 8, 4, 6],
       [7, 4, 4, 4],
       [6, 3, 8, 7]])
```





**np.random.random(size=None)**		用于创建小数值，区别于 np.random.randint 的整数

​					创建一个元素为0-1（左闭右开）的随机数多维数组，size=None 数组形状



```python
np.random.random(size=(3,4))

array([[0.13292471, 0.45560473, 0.86434995, 0.55554575],
       [0.27548587, 0.2246425 , 0.16375532, 0.87830494],
       [0.14382681, 0.96140761, 0.90075556, 0.46855921]])
```





**np.random.randn(d0,d1,....dn)**

​					创建一个服从标准正态分布的多维数组

​						标准正态分布又称为U分布/高斯分布 ，时以0为均数，以1为标准差的正态分布，记为N（0，1），标准正态分布出现在0左右的概率最大，离得越远，出现的概率越低

​						

​		

**np.random.normal(loc=0.0,scale=1.0,size=None)**

​					创建一个服从正态分布的多维数组

​					参数说明：

​					 	loc = 0.0 均值：对应着正态分布的中心，标准正态分布的中心是0.0

​						 scale：标准差，对应分布的宽度，scale越大，正态分布的曲线越矮胖，scale越小，则曲线越高瘦

​						size=None：数组形状





**np.random.rand(d0,d1,d2...dn)**

​					创建一个元素为0-1（左闭右开）的随机数多维数组

​					与np.random.random 功能类似，掌握其中之一即可

​					参数说明： dn: 第N个维度的值



​					**shape**

​						n.shape 	# 形状，

​						返回元组中数字的个数代表维度的多少 

​						数组才有该方法，list（列表）没有



**深浅拷贝**

​	np.array()：从现有数组中创建新的数组

​	np.asarray()：相当于索引的形式，并没有真正创建一个新的数组



### 		ndarray 索引 / 切片

​			一维的切片操作与python中的列表完全一致，多维同理

```python
i = [1,2,3,4,5,6,8]
i[2:6]    [3,4,5,6]
i[::-1]		反转



n = np.random.randint(0,10,size=(6,8))
array([[4, 4, 6, 9, 7, 8, 7, 9],
       [0, 5, 1, 3, 7, 7, 0, 3],
       [0, 7, 7, 4, 8, 3, 2, 3],
       [0, 5, 0, 5, 9, 9, 4, 5],
       [3, 9, 0, 5, 7, 5, 5, 1],
       [2, 6, 5, 8, 1, 4, 0, 8]])


# 取一行
print(n[0])
[4 4 6 9 7 8 7 9]


# 取多行
print(n[1:4])
[[0 5 1 3 7 7 0 3]
 [0 7 7 4 8 3 2 3]
 [0 5 0 5 9 9 4 5]]


# 取不连续的多行，可取重复取某一行
print(n[[1,2,4,2]])
[[0 5 1 3 7 7 0 3]
 [0 7 7 4 8 3 2 3]
 [3 9 0 5 7 5 5 1]
 [0 7 7 4 8 3 2 3]]


# 取一列
print(n[:,0])	# 取所有行的第一列[下标为零]
[4 0 0 0 3 2]

print(n[0:4,0])# 取范围行内的第一列
[4 0 0 0]

# 取多行多列
print(n[0:3,2:5])
[[6 9 7]
 [1 3 7]
 [7 4 8]]


# 取不连续的多列，可取重复取某一列，使用[]
print(n[0:3,[0,1,1,1]])
[[4 4 4 4]
 [0 5 5 5]
 [0 7 7 7]]





# 行反转
print(n[::-1])

# 列反转 相当于是对第二个维度做反转
print(n[:,::-1])
```







### Numpy级联操作

​	**np.concatenate()**

​		参数是列表或者元组

​		级联的数组维度必须相同

​		可通过axis参数改变级联的方向（数值为对应的维度）



​		上下合并时，需确保列数一致，axis=0，等效于hstack函数

​		左右合并时，需确保行数一致，axis=1，等效于vstack函数



```python
n1 = np.random.randint(0,100,size=(3,5))
n2 = np.random.randint(0,100,size=(3,5))
display(n1,n2)  # display用于输出多个变量的值


array([[ 0, 34, 42,  7, 51],
       [49,  7, 75,  5, 64],
       [19, 18,  1, 23, 39]])
array([[62, 63, 80, 73, 65],
       [17, 97, 59, 31, 54],
       [72, 49, 37, 24, 69]])




# 参数需为一个由列表组成的元组，默认为上下合并
# axis=0表示第一个维度（行）合并
np.concatenate((n1,n2))  


array([[ 0, 34, 42,  7, 51],
       [49,  7, 75,  5, 64],
       [19, 18,  1, 23, 39],
       [62, 63, 80, 73, 65],
       [17, 97, 59, 31, 54],
       [72, 49, 37, 24, 69]])


# axis=1表示第二个维度（列）合并
np.concatenate((n1,n2),axis=1) 

array([[ 0, 34, 42,  7, 51, 62, 63, 80, 73, 65],
       [49,  7, 75,  5, 64, 17, 97, 59, 31, 54],
       [19, 18,  1, 23, 39, 72, 49, 37, 24, 69]])


```





### 数组的拆分

​	**np.split	可以做水平或垂直拆分**

​		axis=0 行

​		axis=1 列

​		等分时行列需要成均分数的倍数



​		**np.vsplit	按行拆分**

​		**np.hsplit	按列拆分**



```python
n = np.random.randint(0,100,size=(6,4))
array([[33, 17, 15, 55],
       [26, 37, 49, 43],
       [10, 48, 76, 81],
       [25,  5, 17, 66],
       [90,  5, 33, 31],
       [32, 26, 30, 67]])

np.vsplit(n,3)
[array([[33, 17, 15, 55],
        [26, 37, 49, 43]]),
 array([[10, 48, 76, 81],
        [25,  5, 17, 66]]),
 array([[90,  5, 33, 31],
        [32, 26, 30, 67]])]


np.vsplit(n,(1,2,4))
[array([[33, 17, 15, 55]]),
 array([[26, 37, 49, 43]]),
 array([[10, 48, 76, 81],
        [25,  5, 17, 66]]),
 array([[90,  5, 33, 31],
        [32, 26, 30, 67]])]


np.hsplit(n,2)
[array([[33, 17],
        [26, 37],
        [10, 48],
        [25,  5],
        [90,  5],
        [32, 26]]),
 array([[15, 55],
        [49, 43],
        [76, 81],
        [17, 66],
        [33, 31],
        [30, 67]])]

np.hsplit(n,(1,2))
[array([[33],
        [26],
        [10],
        [25],
        [90],
        [32]]),
 array([[17],
        [37],
        [48],
        [ 5],
        [ 5],
        [26]]),
 array([[15, 55],
        [49, 43],
        [76, 81],
        [17, 66],
        [33, 31],
        [30, 67]])]
```



**copy** 

赋值与深拷贝

​	n1 = np.arange(10)

​	n2 = n1	: 此方法为浅拷贝，两个变量用的还是同一个地址，改变任一变量值会导致另一变量值也改变

​	n2 = n1.copy	: 此方法为深拷贝，改变其中一个变量值，不会影响另一变量值





### NumPy聚合操作

​		n.reshape(-1)：降维操作     # **shape：形状 / ʃeɪp /**

​		n.reshape  用于改变数组的形状，其中 -1 作为参数表示一个未知维度，NumPy会根据数组元素的总数和其他已知维度自动计算这个未知维度的大小。

​		当使用 reshape(-1) 时，NumPy会尽可能保持原数组的所有元素不变，并将数组扁平化为一维数组。具体而言，它会计算一个合适的维度值以使新形状的体积（即所有维度的乘积）等于原数组的元素总数。







​		**np.**

​			argmin：最小值对应下标

​			argmax：最大值对应下标

​			argwhere：按条件查找



​			sum：求和	

​			min：最小值

​			max：最大值

​			mean/agerage：平均值，一般主要使用mean

​			median：中位数

​			percentile：百分位数，参数 q = 50时表示中位数

​			std：标准差

​			var：方差

​			power：次方，求幂







```python
n = np.random.randint(0,10,size=(3,5))
n

array([[0, 3, 4, 8, 2],
       [3, 0, 4, 5, 5],
       [5, 8, 6, 4, 3]])

np.sum(n)  # 此时是求所有数值的和
60


np.sum(n,axis=0) # 此时是求行的和（每一列中所有行的值之和）
array([ 8, 11, 14, 17, 10])


np.sum(n,axis=1) # 此时是求列的和（每一行中所有列的值之和）
array([17, 17, 26])

np.nansum(n) # 排除	nan之后剩下的数之和


np.dot(n1,n2)		# 矩阵的乘法运算（第一个矩阵的行数等于第二个矩阵的列数，列数等于行数）
# 相加时需要两矩阵的行数列数都相同
```





### Ndarray数学操作

​	abs：绝对值

​	sqrt：平方根，等效于 n**0.5

​	square：平方，等效于n**2

​	exp：指数 e

​	log：对数（自然对数，以一为底）

​	round：四舍五入

​	ceil：向上取整

​	floor：向下取整

​	cumsum：累加运算





### Ndarray排序操作

​	Ndarray（）排序

​		np.sort()：不改变原数组	

​		ndarray.sort()：改变原数组，不多占用内存空间 



### Ndarray文件操作

​	**保存数组**	

​		**np.save：**保存ndarray到一个spy文件中

​		**np.savez：**将多个array保存到npz文件中

​		

```python
# 以二进制的格式保存 ndarray 数据，通常比文本格式更快且占用空间更少
# x为需要被保存的ndarray数据,保存后的名称为 文件名.npy
np.save('文件名',x)   
np.savez('文件名.npz',xarr=x,yarr=y)

# 存储到csv/txt文件，x为需要被保存的数据
np.savetxt('xx.txt',n,delimiter=' ')  # delimiter为分隔符参数，默认为空格
np.savecsv('xx.csv',n,delimiter=',')
```

 

​	**读取数组**

​			**load：**读取ndarray，ndelimiter：分隔符

```python
np.load('x.npy')
np.load('arr.npz')['xarr']
np.loadtxt('xx.txt',ndelimiter=' ')
np.loadcsv('xx.csv',ndelimiter=',',dtype=np.int16)
```





### NumPy条件判断

#### 逻辑运算

​	直接使用 > / < / = 进行条件判断

```python
# 随机生成十名学生对应的五科成绩
score = np.random.randint(40,100,(10,5))
score > 60
score[score > 80] = 1
```





#### 通用判断函数

​	**np.all()**

​	**np.any()**

```python
# 判断是否全部及格
np.all(score[0:2,:] > 60)

# 判断是否存在优秀
np.any(score[0:2,:] > 90)
```





#### 三元运算符

​	**np.where()**

​		可以通过 np.where() 进行更复杂的运算

```python
# 判断前四名学员的前四门课程成绩，大于60则置为 1 否则为 0
np.where(score[:4,:4] > 60,1,0)
```

​	结合 **np.logical_and   np.logical_or**

```python
temp = score[:4,:4]
np.where(np.logical_and(temp > 60,temp < 90),1,0)
np.where(np.logical_or(temp > 90,temp < 60),1,0)
```





### 矩阵操作

​		+（加）	-（减）	*（乘）	/（除）	//（整除）	**（次方）	%（余数）



##### 	线性代数 

​		**矩阵积 np.dot()**

​		**广播机制：**为方便矩阵运算，numpy会自动补全参与运算的矩阵



### 实际例子

```python
# 导包
import numpy as np


# 创建一个长度为10，值都为0的一维 ndarray 对象，然后让第五个元素值等于1
n = np.zeros(10,dtype=np.int8)
n[4] = 1
n

array([0, 0, 0, 0, 1, 0, 0, 0, 0, 0], dtype=int8)


# 创建一个元素从10到49的一维 ndarray 对象
n = np.arange(10,50)
n

array([10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26,
       27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43,
       44, 45, 46, 47, 48, 49])


# 将上述数据的所有元素位置反转
n = n[::-1]
n


# 使用 np.random.random 创建一个10*10的ndarray 对象，并打印出最大，最小元素
n = np.random.random(size = (10,10))  # random 对应的元素值位于 0.0 - 1.0之间
display(np.max(n),np.min(n))

0.999007910913901
0.0038842757972411546


# 创建一个 10*10 的 ndarray 对象，且矩阵边界全为 1，里面全为 0
n = np.zeros((10,10),dtype = np.int16)
n[[0,-1]] = 1    # 处理第一行和最后一行的数据
n[:,[0,-1]] = 1    # 处理第一列和最后一列的数据
n


# : 为切片
# , 为索引

n = np.ones((10,10),dtype = np.int16)
n[1:-1,1:-1] = 0
n


array([[1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]], dtype=int16)




# 创建一个每行元素的事从0到4的5*5矩阵
l = [0,1,2,3,4]
n = np.array(l*5).reshape((5,5))
n

array([[0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4],
       [0, 1, 2, 3, 4]])



# 创建一个范围在（0，1）之间的长度为12的等差数列
n = np.linspace(0,1,12)
n

array([0., 0.09090909, 0.18181818, 0.27272727, 0.36363636,
       0.45454545, 0.54545455, 0.63636364, 0.72727273, 0.81818182,
       0.90909091, 1.])



# 创建一个长度为10的随机数组，并进行排列
n = np.random.randint(0,10,10)
n.sort()  		 # 改变原数组
n = n[::-1]    # 降序排列

n1 = np.sort(n)   # 不改变原数组
display(n1,n)

array([0, 0, 3, 4, 5, 5, 6, 6, 6, 8])
array([8, 6, 6, 6, 5, 5, 4, 3, 0, 0])



# 

# 创建一个长度为10的随机数组，将最大值替换为0
n = np.random.randint(0,10,size=10)

# 获取最大值的数值
max1 = np.max(n)  
# 获取所有最大值的索引  argwhere：条件筛选，reshape(-1)
max_indexs = np.argwhere(n==max1).reshape(-1) 
# 找出数组中值为最大值的下标，写-1是不需要判断具体数量
n[max_indexs] = 0



# 给定一个四维矩阵，得到最后两维的和
n = np.random.randint(0,10,size=(2,3,4,5),dtype=np.int8)
n.sum(axis=(2,3)) # axis的下标索引值从0开始


array([[ 73,  90,  73],
       [ 86,  84, 110]])




# 给定数组[1，2，3，4，5]，如何得到在这个数组的每个元素之间插入3个0之后的新数组

n = np.arange(1,6)  # 不包含后边界值
n1 = np.zeros(17,dtype=np.int8)
n1[::4]=n  # 4为步伐，切片可以进行对应的赋值操作
n1


array([1, 0, 0, 0, 2, 0, 0, 0, 3, 0, 0, 0, 4, 0, 0, 0, 5], dtype=int8)





# 给定一个二维矩阵，如何交换其中两行元素
n = np.random.randint(1,10,size=(4,4),dtype=np.int8)
display(n)
n[[0,1]] = n [[1,0]]
n[1,0,2,3]   # 也可以直接使用索引值指定对应的行输出


array([[4, 8, 6, 8],
       [1, 1, 8, 6],
       [4, 3, 3, 4],
       [9, 7, 9, 7]], dtype=int8)
array([[1, 1, 8, 6],
       [4, 8, 6, 8],
       [4, 3, 3, 4],
       [9, 7, 9, 7]], dtype=int8)




# 创建一个一万长度的随机数组，使用两种方法对其求三次方，并比较所用时间
n = np.random.randint(0,10,size=100000,dtype=np.int8)
display(n)

%time np.power(n,3)
# %timeit 比较精确的时间，运行多次
# %time 较为粗略的时间

array([9, 0, 3, ..., 5, 8, 3], dtype=int8)
CPU times: user 984 µs, sys: 176 µs, total: 1.16 ms
Wall time: 759 µs
array([-39,   0,  27, ..., 125,   0,  27], dtype=int8)




# 创建一个5*3随机矩阵与一个3*2的随机矩阵，求矩阵积
n1 = np.random.randint(1,10,size=(5,3),dtype=np.int8)
n2 = np.random.randint(1,10,size=(3,2),dtype=np.int8)
display(n1,n2)
# 求矩阵积
np.dot(n1,n2)


array([[9, 3, 3],
       [9, 9, 7],
       [4, 8, 6],
       [3, 4, 9],
       [7, 5, 2]], dtype=int8)
array([[3, 4],
       [7, 4],
       [1, 6]], dtype=int8)
array([[ 51,  66],
       [ 97, 114],
       [ 74,  84],
       [ 46,  82],
       [ 58,  60]], dtype=int8)




# 需求：矩阵的每一行元素都减去该行的平均值
n1 = np.random.randint(1,10,size=(5,5),dtype=np.int8)

n1.mean() 														# 该方法取得的是整个矩阵的平均值
n2 = n1.mean(axis=1).reshape(5,1)		  # 该方法取得的是列维度（每行多列）的平均值
display(n2)  												  # .reshape(5,1)  转换为五行一列的矩阵

n1-n2

array([[9, 6, 8, 4, 1],
       [6, 2, 7, 3, 4],
       [4, 5, 2, 5, 5],
       [2, 7, 2, 1, 1],
       [5, 7, 7, 1, 4]], dtype=int8)

array([[5.6],
       [4.4],
       [4.2],
       [2.6],
       [4.8]])

array([[ 3.4,  0.4,  2.4, -1.6, -4.6],
       [ 1.6, -2.4,  2.6, -1.4, -0.4],
       [-0.2,  0.8, -2.2,  0.8,  0.8],
       [-0.6,  4.4, -0.6, -1.6, -1.6],
       [ 0.2,  2.2,  2.2, -3.8, -0.8]])
```





## Pandas

​	主要数据结构为 **Series（一维数据）**，**Data Frame（）二维数据**

​	pandas ：	用于小规模数量集的处理

​	spark：	    用于大规模数量集的处理







### Series

​	一种类似于一位数组的对象，由两个部分（values，index）组成

​	可以理解为是一种有序的字典结构（既可以通过key-value形式取值，也可以通过索引取值）	

​	数据量较大时推荐使用字典（key-value 方式取值）





​	**values：**一组数据（ndarray类型）

​	**index：**相关的数据索引类型



#### Series 基本属性与方法

##### **基本属性：**

​		**shape**:     形状/维度

​		**size**:	元素个数

​		**index**：  索引

​		**values**：值

​		**name**： 索引名字

```python
s = pd.Series({'Python':130,'Pandas':110,'NumPy':120},name='demo')
s.shape		# 返回类型为元组，注：当元组中只有一个元素时，该元素后面需要单独加一个逗号
(3,)			# 数字的个数代表为维度，数字的数值代表元素个数

s.size
3

s.name
'demo'
```









##### **查看数据：**

​		**head**：   查看前几条数据，默认前五条

​		**tail**:		 查看后几条数据，默认后五条



##### **检测缺失数据**：

​		**isnull/pd.isnull**:  判断索引对应值是否为空，返回下标索引以及对应布尔值

​		**notnull/pd.notnull:**  判断索引对应值是否不为空，返回下标索引以及对应布尔值



```python
# NaN 表示空值，该数值不能再进行运算
s = pd.Series(['张三','李四','王五',np.nan])     
s.isnull()    # 判断索引对应值是否为空，返回下标索引以及对应布尔值
pd.isnull(s)	# 两种方法等效

0    False
1    False
2    False
3     True
dtype: bool

 
s.notnull()    # 判断索引对应值是否不为空，返回下标索引以及对应布尔值
pd.notnull(s)	# 两种方法等效

0     True
1     True
2     True
3     False
dtype: bool
```



##### **通过布尔值索引过滤异常数据：**

```python
# 过滤空值
cond1 = s.isnull()  
cond2 = s.notnull()

s[cond1]    # 通过使用布尔索引过滤出空值

s[cond2]    # 通过使用布尔索引剔除掉空值
s[~cond1]   # 等效上条作用  ～：取反操作
```









#### Series 创建方式

​		1）由列表或NumPy数组创建，默认索引为0到N-1的整数型索引

​		2）由字典创建



```python
import pandas as pd
import numpy as np

# 常用创建方式,name可不写
n = pd.Series(['A','B','C'],index=['a','b','c'],name='测试')  



# 使用数组创建Series
list1 = [1,2,3,4,6]
s = pd.Series(list1)   
# 第一列为索引值，第二列为对应元素值（索引的类型可以是字符等
0    1
1    2
2    3
3    4
4    6
dtype: int64



 
# 使用NumPy数组创建Series
n = np.array(list1)
s = pd.Series(n)        
type(s)

pandas.core.series.Series





s.values                
# array([1, 2, 3, 4, 6]) ndarray类型的一维数组

s.index                
# RangeIndex(start=0, stop=5, step=1)/起始值，终点值，步长

list(s.index)           
# [0, 1, 2, 3, 4] 强制转换成列表进行输出

s.index = ['A','B','C','D','E']    
# 修改索引index，注意索引数量要匹配  
# 等效于 s.index = list(ABCDE) list会将字符串中的每个字符强制转换成一个元素

A    1
B    2
C    3
D    4
E    6
dtype: int64
  
  
s.A,s.C    
# (1, 3) 通过索引获取值，等效于s['A'],s['C']
# 此方法不适用于索引为数字类型时，数字索引需要使用[]  
  
s.values.tolist()   # 递归转换，内部 NumPy 元素也变成 Python list 适合 嵌套结构、结构化数据
# [1, 2, 3, 4, 6]

list(s.values)      # 仅展开最外层，内部仍是 NumPy 元素     适合 普通一维数值型 Series，追求性能 
# [np.int64(1), np.int64(2), np.int64(3), np.int64(4), np.int64(6)]
```





```python
# 通过字典创建 Series

d = {
    'a':11,
    'b':22
}
s = pd.Series(d)  

a    11
b    22
dtype: int64
  
  
d = {
    'a' : np.random.randint(0,10,size=(2,3)),
    'b' : np.random.randint(0,10,size=(2,3))
}

s = pd.Series(d)

a    [[5, 6, 5], [5, 0, 4]]
b    [[4, 4, 4], [9, 1, 1]]
dtype: object
```







#### Series 基础运算



​	适用于 Numpy 的数组运算同样也适用于 Series

​	+ - * / // **  % 加减乘除整除，取平方，求余数 

```python
s = pd.Series(np.random.randint(10,100,size=3))
s1 = s + 10
display(s,s1)

0    89
1    44
2    43
dtype: int64
  
0    99
1    54
2    53
dtype: int64  
```





#### **Series 之间的运算**

```python
# Series 之间的运算
s1 = pd.Series(np.random.randint(10,100,size=3))
s2 = pd.Series(np.random.randint(10,100,size=3))
s3 = pd.Series(np.random.randint(10,100,size=4))
s4 = pd.Series(np.random.randint(10,100,size=5))

s5 = s1 + s2  # 索引数量相同时，同位索引对应的值相加
s6 = s3 + s4  # 索引数量不同时，多余索引对应的值为NaN
# Series没有广播机制，不会自动补充对齐（无法对齐的直接为NaN）

# 该方法能将无法对齐的数据替换为默认值进行计算  保留无法对齐的数据
s7 = s3.add(s4,fill_value=0)  

display(s5,s6,s7)


0    117
1    163
2    137
dtype: int64
  
0    125.0
1    180.0
2    108.0
3     37.0
4      NaN
dtype: float64  
  
  
0    125.0
1    180.0
2    108.0
3     37.0
4     31.0
dtype: float64  
```





#### 显式索引/隐式索引/布尔索引

```python
# 显式索引：使用索引名获取对应的值

s = pd.Series({'Python':130,'Pandas':110,'NumPy':120})
# 一次获取一个数据，返回的数据类型为 Int 类型
s['Python']
s.NumPy

# 一次获取多个数据，返回的数据类型为 Series
s[['Python','NumPy']]
 # 虽然只返回一个数据，但数据类型仍然是 Series 
s[['Python']]  


# 使用 loc[] 获取数据
s.loc['Python']
s.loc[['Python','NumPy']]



# 隐式索引：使用数字下标获取对应的值
s[0]
s[[0]]
s[[0,1]]

# iloc 即 index loc
s.iloc[0]
s.iloc[[0]]
s,iloc[[0,1]]

# s.iloc['Python'] 此方法不可行，与 loc不同！ 不可混用
# s.loc[0]  除非索引值就是0，否则也不可以混用



s = pd.Series({
    '语文':110,
    '数学':125,
    '日语':130,
    'Python':120,
    'Scala':115,
    'Pandas':130,
    'Numpy':133
})


# 布尔索引：通过对应位置的布尔值确定是否取该索引对应的值
s[[True,True,False.....]]

```





#### 隐式切片/显式切片

```python
# 切片

# 隐式切片
s[1:4]      # 左闭右开 不包含右边界  1，2，3
s.iloc[1:4]   # 两种方法等效

# 显式切片    左闭右闭
s['数学':'Python']
s.loc['数学':'Python']
```









### Data Frame

​	Data Frame 是一个表格型的数据结构，可以看作是由 Series 组成的字典，共用同一个索引

​	Data Frame 由按一定顺序排列的多列数据组成，设计初衷是将 Series 的使用场景由一维拓展至多维，所以 Data Frame  既有行索引，也有列索引



​		**行索引：**index

​		**列索引：**columns

​		**形状：**shape

​		**值：**values（NumPy的二维数组）

​		

​		head()：查看前几条数据 （默认五条）

​		tail()：   查看后几条数据 （默认五条）



#### Data Frame 的创建



##### 从字典中创建

​	常见的方法是传递一个字典来创建，以字典的Key值为每一列的列名，字典的Value（数组类型）为一列的元素。

​	此外，Data Frame 会自动加上每一行的索引，在传入值数量不对齐的情况下，值为NaN

​	（与Series一样）



```python
# dataframe 的创建
d = {
    'name':['Hadoop','Spark','Flink','Linux'],
    'age':[8,5,3,18]
}

df = pd.DataFrame(d,index=list('ABCD'))  # 此处可直接定义行索引
df

  name	age
0	Hadoop	8
1	Spark	5
2	Flink	3
3	Linux	18


df.shape     
# (4, 2)

df.columns   
# Index(['name', 'age'], dtype='object')

df.index    
# RangeIndex(start=0, stop=4, step=1)

# 可修改行索引
df.index = list('ABCD')

# 可设置列索引
df.columns = ['name1','age1']
```



##### 其他创建方式

```python
df = pd.DataFrame(
    data = np.random.randint(10,100,size=(4,6)),
    index=['困困','壮壮','多多','咪咪'],
    columns=['语文','数学','英语','化学','物理','生物']
)
```





#### Data Frame 的索引

##### 	**列索引**

```python
# 列索引
# DataFrame 默认情况下 先取列索引 [需特别注意]
# Series 类型：只有一种索引，单维度
#    是一维的数据结构，类似于一列或一个数组  它有一个索引（index）和一组与之对应的值。

# DataFrame 类型：具有行/列索引，双维度
#		是二维的表格型数据结构，可以看作是多个 Series 的组合，它既有行索引（Index），也有列名（Columns）。每一列都是一个 Series，并且所有列共享同一个行索引。

# 索引值都从 0 开始

df.语文
df['语文'] # 与上方法等效  返回值为Series类型

困困    57
壮壮    86
多多    95
咪咪    17
Name: 语文, dtype: int64
    

df[['语文','化学']] # 返回值为 DataFrame 类型

	语文	化学
困困	61	33
壮壮	87	82
多多	51	16
咪咪	37	23

```



##### **行索引**

```python
df.loc['困困']   # 返回值为 Series 类型
df.iloc[0]      # iloc 使用下标进行索引，隐式索引

df.iloc[[0]]    # 返回值为 DataFrame 类型
df.iloc[[0,1]] 
```



##### **索引举例**

```python
# 取某一个元素
df = pd.DataFrame(
    data = np.random.randint(10,100,size=(4,6)),
    index=['困困','壮壮','多多','咪咪'],
    columns=['语文','数学','英语','化学','物理','生物']
)


		语文	数学	英语	化学	物理	生物
困困	57	49	78	40	86	61
壮壮	86	95	18	68	24	39
多多	95	89	93	55	22	79
咪咪	17	45	10	50	61	38




# 方法一：先取列，再取行
df['语文']['困困']
df['语文'][0]
df.语文[0]
df.语文.困困

# 方法二：先取行，再取列
df.iloc[0]['语文']
df.iloc[0][0]
df.iloc[0,0]

df.loc['困困']['语文']
df.loc['困困','语文']
```





#### Data Frame 的切片



【注意】直接使用中括号时

​					索引优先对列进行操作

​					切片优先对行进行操作



##### **行切片**

```python
# 行切片(优先对行进行切片)
df[1:3]   # 左闭右开（包含左边，不包含右边）
df.iloc[1:3]   # 左闭右开（包含左边，不包含右边）

语文	数学	英语	化学	物理	生物
壮壮	86	95	18	68	24	39
多多	95	89	93	55	22	79


df['困困':'多多']   # 左闭右闭（包含左边，右边）
df.loc['困困':'多多']   # 左闭右闭（包含左边，右边）

语文	数学	英语	化学	物理	生物
困困	57	49	78	40	86	61
壮壮	86	95	18	68	24	39
多多	95	89	93	55	22	79
```



##### **列切片**	

```python
# 列切片  多次切片必须使用loc/iloc
# 对列做切片必须先对行做切片，如果没有行切片需求，则可使用 df.iloc[:,x:y]
df.iloc[:,1:3]   # 左闭右开（包含左边，不包含右边）

	数学	英语
困困	83	90
壮壮	27	12
多多	62	17
咪咪	20	89


df.loc[:,'数学':'化学']   # 左闭右闭（包含左边，右边）

数学	英语	化学
困困	49	78	40
壮壮	95	18	68
多多	89	93	55
咪咪	45	10	50
```





##### 切片/索引 混合操作

​	不存在切片时，先取列，存在切片时，先取行（中括号中的顺序
​    

    []
    	使用[]仅进行索引操作时，先列后行
          是否能进行多次操作主要看上一次操作返回的数据类型
    
        [单列索引]							返回 Series
        [[多列索引]] 					 	返回 DataFrame
        [单列索引][单行索引]  		返回 Series 再返回一个值
    
        [[多列索引]][单行索引] 报错 返回 DataFrame 再直接[]是取列索引 故取行索引报错
        [单行索引 / [多行索引]] / [,单行索引 / [多行索引] 报错 []无法在不取列的情况下取行索引，如需实现则须使用 loc / iloc


​      使用[]只能是对行进行切片，无法进行列切片
​        [行切片:行切片] 			返回 DataFrame
​        [行切片:行切片] [列索引]	  返回 Series
​        [单列索引] [行切片:行切片]      返回 Series
​        [多列索引] [行切片:行切片]      返回 DataFrame
​    
​        多次切片必须使用loc/iloc，对列做切片必须先对行做切片，即列切片必须使用loc/iloc
​        [行切片:行切片,列切片:列切片]		报错
​      
​     loc[]
​     	使用 loc[] 进行索引时，先行后列
​      	.loc[单行索引]	返回 Series
​      	.loc[[多行索引]]	返回 DataFrame
​      	.loc[单行索引] [单列索引]	返回一个值
​      	.loc[单行索引] [[多列索引]]	返回多个值  
​      	.loc[[多行索引]] [单列索引]	返回 Series
​       	.loc[[多行索引]] [[多列索引]]	返回 DataFrame 
​    

     	使用 loc[] 进行切片时，先行后列，列索引必须使用loc/iloc 且必须先进行行切片
     	  .loc[行切片]		返回 DataFrame	
     	  .loc[行切片,列切片]			返回 DataFrame	
    		.loc[:,列切片]					 返回 DataFrame	
    		.loc[行切片,[多列索引]] 	 返回 DataFrame	

```python
df.iloc[1:3,1:4]
df.loc['壮壮':'多多','数学':'化学']  # 上下两者等效


# 取一行/一列： 使用行索引/列索引
df['语文']      # 取一列
df.loc['困困']  # 取一行
df.iloc[0]     # 取一行


# 取联系 多行/多列：  使用切片

df.loc['困困':'多多','数学']           # 多行单列
df.iloc[0:3,1]


df.loc['困困','数学':'化学']           # 单行多列
df.iloc[1,1:4]


df.loc['困困':'多多','数学':'化学']     # 多行多列
df.iloc[0:3,2:4]


# 取不连续（指定） 多行/多列：   使用中括号
df.loc[['困困','壮壮'],'数学':'化学']
df.iloc[[0,1],1:4]

```





#### Data Frame 的运算



##### 		行列操作顺序

​	使用[] / [[]] 进行索引取值时，先对列索引进行取值操作

​	使用loc/iloc进行索引取值时，先对行索引进行取值操作



即：

​	• loc： 基于标签索引和切片，行优先（先行后列），包含结束位置。
​	• iloc：基于整数位置索引和切片，行优先（先行后列），不包含结束位置。
​        • []：    默认用于列索引（列优先），也可以用于行切片（行优先）。



​	切片操作只能先进行行切片，再进行列切片，即使使用loc/iloc 也不能改变顺序

​	使用到loc/iloc时都是先取 行 再取 列

​	使用布尔值进行筛选时也是先对行进行操作

		使用 [] 进行布尔索引取值时，取决于布尔索引的 Series 是否对应行或列。具体来说，布尔索引 Series 的索引和 axis 参数的设置决定了它用于筛选行还是列。
		如果没有指定 axis，并且行列数量相同，那么默认情况下 pandas 会将布尔索引 Series 解释为对行进行筛选。这是因为 pandas 默认操作的方向是 axis=0，即按列操作，但在布尔索引中，没有明确指定时，默认会按行进行操作。



​	Data Frame 与 Series 一致，都没有广播机制

​	fill_value : 用于填补**运算中**出现的NaN值，修改未匹配行列的计算默认值，先填充默认值再相加



##### **Data Frame与Data Frame**

```python
df1 = pd.DataFrame(
    data = np.random.randint(10,100,size=(4,6)),
    index=['困困','壮壮','多多','咪咪'],
    columns=['语文','数学','英语','化学','物理','生物']
)


	语文	数学	英语	化学	物理	生物
困困	72	91	96	10	94	51
壮壮	36	81	79	52	38	31
多多	17	49	75	31	32	87
咪咪	43	59	47	70	40	89



df2 = pd.DataFrame(
    data = np.random.randint(10,100,size=(3,5)),
    index=['困困','壮壮','多多'],
    columns=['语文','数学','化学','物理','生物']
)

df2
语文	数学	化学	物理	生物
困困	86	70	22	36	75
壮壮	95	63	58	28	70
多多	55	42	93	82	99


# Data Frame也没有广播机制，未匹配上的行列默认为 NaN 
df1+df2  # 只要运算其中一方不匹配则值为NaN

	化学	数学	物理	生物	英语	语文
咪咪	NaN	NaN	NaN	NaN	NaN	NaN
困困	32.0	161.0	130.0	126.0	NaN	158.0
壮壮	110.0	144.0	66.0	101.0	NaN	131.0
多多	124.0	91.0	114.0	186.0	NaN	72.0


df1.add(df2) 									 # 与上一用法等效
df1.add(df2,fill_value=0)  		 # 修改未匹配行列的计算默认值，先填充默认值再相加
df1.divide(df2,fill_value=1)   # 除法运算

	化学	数学	物理	生物	英语	语文
咪咪	70.000000	59.000000	40.000000	89.000000	47.0	43.000000
困困	0.454545	1.300000	2.611111	0.680000	96.0	0.837209
壮壮	0.896552	1.285714	1.357143	0.442857	79.0	0.378947
多多	0.333333	1.166667	0.390244	0.878788	75.0	0.309091

```





##### **Data Frame与Series 计算**

```python
# 对匹配上的行/列进行运算操作
# axis 默认为 1 / columns
s = pd.Series([1000,100,10],index=df2.index)
困困    1000
壮壮     100
多多      10
dtype: int64
  
  
df2 = pd.DataFrame(
    data = np.random.randint(10,100,size=(3,5)),
    index=['困困','壮壮','多多'],
    columns=['语文','数学','化学','物理','生物']
)

df2


		语文	数学	化学	物理	生物
困困	32	 25		71	 78		45
壮壮	34	 35		61	 18		76
多多	70	 61		34	 87		94


# axis 默认为 1 / columns
df2.add(s,axis=1)   
df2.add(s,axis='columns')


df2.add(s,axis=0)   
df2.add(s,axis='index')


			语文	数学	化学	物理	生物
困困	1086	1070	1022	1036	1075
壮壮	195	163	158	128	170
多多	65	52	103	92	109

```



#### DataFrame的多层索引



##### **隐式构造层次索引**



```python
# 隐式构造层次索引

data1 = np.random.randint(0,100,size=(6,6))
data2 = np.random.randint(0,100,size=6) 


index = [
    ['一班','一班','一班','二班','二班','二班'],
    ['困困','壮壮','咪咪','刀总','多多','蠢萌']
]

columns = [
    ['期中','期中','期中','期末','期末','期末'],
    ['语文','数学','英语','语文','数学','英语']
]

df1 = pd.DataFrame(data = data1, index = index,columns=columns)


df2 = pd.Series(data = data2,index = index)
display(df1,df2)


									期中				期末
					语文	数学	英语	语文	数学	英语
一班	困困	46	53	97	75	57	36
		 壮壮	 66	 43	 94	 76	 82	 20
		 咪咪	 85	 10	 21	 95	 82	 12
二班	刀总  77  43	19  29  10  61
		 多多	 90	 31	 38	 88	 12	 65
		 蠢萌	 20	 42	 92	 30	 78	 42

    
 一班  困困    31
      壮壮    59
      咪咪    37
二班   刀总    30
      多多    94
      蠢萌    18
dtype: int64
```





##### **显式构造层次索引**

```python
# 显式构造层次索引
data = np.random.randint(0,100,size=(6,6))


# 使用数组 arrays
index = pd.MultiIndex.from_arrays([
    ['一班','一班','一班','二班','二班','二班'],
    ['困困','壮壮','咪咪','刀总','多多','蠢萌']
])

columns = pd.MultiIndex.from_arrays([
    ['期中','期中','期中','期末','期末','期末'],
    ['语文','数学','英语','语文','数学','英语']
])

df = pd.DataFrame(data = data, index = index,columns=columns)
df


# 使用元组 tuple
index = pd.MultiIndex.from_tuples(
(
    ('一班','困困'),('一班','壮壮'),('一班','咪咪'),
    ('二班','刀总'),('二班','多多'),('二班','蠢萌')    
)
)

columns = pd.MultiIndex.from_arrays([
    ['期中','期中','期中','期末','期末','期末'],
    ['语文','数学','英语','语文','数学','英语']
])

df = pd.DataFrame(data = data, index = index,columns=columns)
df


# 使用笛卡尔积 product
index = pd.MultiIndex.from_product(
[   # 形成笛卡尔积
    ['一班','二班'],
    ['困困','壮壮','多多']
]
)

columns = pd.MultiIndex.from_arrays([
    ['期中','期中','期中','期末','期末','期末'],
    ['语文','数学','英语','语文','数学','英语']
])

df = pd.DataFrame(data = data, index = index,columns=columns)
df
```



##### **层次化索引 -  Series 操作**

```python
data = np.random.randint(0,100,size=6) 

index = [
    ['一班','一班','一班','二班','二班','二班'],
    ['困困','壮壮','咪咪','刀总','多多','蠢萌']
]

s = pd.Series(data,index)

一班  困困    46
    壮壮    92
    咪咪     2
二班  刀总    46
    多多    16
    蠢萌    21
dtype: int64
  
  
# 显式索引
s.一班 
s['一班'] 
s.loc['一班']       # 只展示第二层（进一层）索引与对应的值

s[['一班']]         # 输出两层索引，保留当前索引
s[['一班','二班']]   # 指定输出多列时需使用[[]]
s[[('一班','困困'),('二班','多多')]]   # 取不同一级索引列中指定二级索引的值

s.一班.困困
s['一班']['困困']       # 取某一值，需按索引级依次指定
s.loc['一班']['困困'] 
s.loc['一班','困困']

# 隐式索引
s[0]        # 直接取值
s.iloc[0]

s.iloc[0:3]   # 切片取多个值
s.iloc[[0,1,2]]  # 索引取多个值

一班  困困    46
     壮壮    92
     咪咪     2
dtype: int64
  
  
  
# 切片
# 显式切片
s['一班':'二班']       # 双闭区间
s.loc['一班':'二班']   # 双闭区间

# 隐式切片
s.iloc[1:8]    # 无视索引等级，任意取   
```





##### **层次化索引 -  DataFrame 操作**

```python
data = np.random.randint(0,100,size=(6,6))

index = [
    ['一班','一班','一班','二班','二班','二班'],
    ['困困','壮壮','咪咪','刀总','多多','蠢萌']
]

columns = [
    ['期中','期中','期中','期末','期末','期末'],
    ['语文','数学','英语','语文','数学','英语']
]

df = pd.DataFrame(data = data, index = index,columns = columns)



# 层次化索引
# Data Frame 操作

# 索引获取元素  先取列索引，再取行索引
df['期中']['数学']['一班']['困困']
df['期中']['数学'][0]

# 索引获取元素，使用 loc/iloc 时先取行再取列
df.iloc[0,1]   
df.loc[('一班','困困'),('期中','数学')]

# 以上四种方法，结果等价
48


df['期中'][['数学']]  # [[]] 保留 Data Frame 格式类型

df['期中']['数学']
df['期中','数学']  
df.期中.数学       # 与上者等效，不保留 Data Frame 格式类型

# 使用 iloc / loc，则先取行索引，再取列索引
df.iloc[:,2]   # 取所有行的下标为2的列索引值
df.iloc[:,[0,1,2,3]]       # 取所有行的下标为0,1,2,3的列索引值
df.iloc[[0,1],[0,1,2,3]]

df.iloc[1]
df.iloc[[1]]
df.iloc[[1,2,4,3]]

df.loc['二班']
df.loc['二班','多多']
df.loc[('二班','多多')]
df.loc['二班'].loc['多多']
df.loc['二班'].loc['多多'][0]

期中  语文    69
    数学    73
    英语    20
期末  语文     1
    数学    62
    英语    62
Name: (二班, 多多), dtype: int64
    
    


# 行切片
# 切片操作建议使用隐式索引（数字下标）

# 切片操作只能先进行行切片，再进行列切片，即使使用loc/iloc 也不能改变顺序
df.iloc[1:5]
# df.loc[('一班','困困'):('二班','多多')] 此用法无法正常实现

# 列切片，必须使用loc/iloc 才能进行列切片
df.iloc[:,1:5]   					# 先对行进行全取，再限定列的切片范围
df.loc[:,'期中':'期末']
# df.loc[:,('期中','数学'):('期末','英语')] 此用法无法正常实现


```







### 函数分类

#### **索引堆叠**

##### **stack - unstack**



```python
# stack:将列索引转换成行索引

									期中				期末
					语文	数学	英语	语文	数学	英语
一班	困困	46	 53		97		75	 57		36
		 壮壮	 66	 	43	 94	 	 76	  82	 20
		 咪咪	 85	 	10	 21	 	 95	  82	 12
二班	刀总  77 	 43	  19 	  29   10   61
		 多多	 90	 	31	 38	   88	  12	 65
		 蠢萌	 20	  42	 92	   30	  78	 42


# stack 默认将最内层的列索引转换成行索引,level（层级）默认值为 -1
df.stack()  



							期中	期末
一班	困困	数学 17	80
		 			英语	34	2
		 			语文	66	91
		 壮壮	 数学	66	56
		 			英语	52	56
		 			语文	49	95
		 咪咪	 数学	21	55
		 			英语	62	25
		 			语文	53	69
二班	刀总	数学	81	88
		 			英语	55	91
		 			语文	53	80
		 多多	 数学	19	34
		 			英语	94	57
		 			语文	12	71
		 蠢萌	 数学	89	75
		 			英语	65	99
		 			语文	48	49





df1 = df.stack(level=0)  # 0为最外层



			数学	英语	语文
一班	困困	期中	14	54	92
期末	36	60	49
壮壮	期中	84	50	70
期末	15	19	83
咪咪	期中	70	36	26
期末	71	61	13
二班	刀总	期中	48	36	94
期末	16	83	97
多多	期中	62	72	43
期末	65	11	84
蠢萌	期中	53	63	68
期末	58	56	75



# 参数：fill_value = 0  设置未匹配项的初始值
# unstack:将行索引转换成列索引
# stack:将列索引转换为行索引
df.unstack()
df1.unstack(level=2)

```





#### 聚合函数

​	求和，平均值（mean），最大值，最小值，中位数等

```python
									期中				期末
					语文	数学	英语	语文	数学	英语
一班	困困	46	 53		97		75	 57		36
		 壮壮	 66	 	43	 94	 	 76	  82	 20
		 咪咪	 85	 	10	 21	 	 95	  82	 12
二班	刀总  77 	 43	  19 	  29   10   61
		 多多	 90	 	31	 38	   88	  12	 65
		 蠢萌	 20	  42	 92	   30	  78	 42


df.loc['一班']['期中']

	  语文	数学	英语
困困	63	49	99
壮壮	54	12	27
咪咪	94	27	82


# axis 默认值为 0 ，将每一列所有的行值SUM
df.loc['一班']['期中'].sum()  

语文    211
数学     88
英语    208
dtype: int64

  
# axis 值为 1，将每一行的所有列值SUM  
df.loc['一班']['期中'].sum(axis = 1)   

困困    211
壮壮     93
咪咪    203
dtype: int64

# 求所有值的总和  
df.values.sum()   



# axis 默认值为 0 ，将每一列所有的行值SUM
df.sum()          

期中  语文    386
     数学    265
     英语    332
期末  语文    136
     数学    357
     英语    331
dtype: int64
  
  
# axis 值为 1，将每一行的所有列值SUM  
df.sum(axis = 1)   

一班  困困    339
     壮壮    279
     咪咪    361
二班  刀总    264
     多多    302
     蠢萌    262
dtype: int64
  
  
# axis 默认值为 0 ,求的每一列的平均值
# 使用 round 函数控制小数分位（四舍五入）
round(df.mean(),2)
```





#### 分组聚合

##### 基本参数

​	**groupby**:        根据指定的键对数据进行分组。

​	**agg**: 		对分组后的数据应用聚合操作，如求和、求平均、计数等。

​	**namedagg：**在 groupby 聚合操作中指定多个聚合函数并为其结果命名

```python
# column：要聚合的列名  
# aggfunc：要应用的聚合函数
df.groupby().agg(
name_x = pandas.NamedAgg(column=None, aggfunc=None),
name_y = pandas.NamedAgg(column=None, aggfunc=None)
)

file_data.groupby('户型',as_index=False)\
    .agg(数量 = pd.NamedAgg(column='区域',aggfunc=pd.Series.count))\
    .sort_values(by='数量',ascending=False)\
    .query('数量 > 50').shape


# 最新方法
file_data.groupby('户型',as_index=False)\
    .agg(数量 = ('区域','sum'))...

```



##### 相关函数

**配合使用的聚合函数**：

​	mean	count	min	max	sum

​	first/last：第一个和最后一个非 Na 值

​	median：非 Na 值的算数中位数

​	prod：非 Na 值的积

​	



##### 具体示例

数据准备

```python
import pandas as pd

data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Alice', 'Bob', 'Charlie'],
    'Subject': ['Math', 'Math', 'Math', 'Science', 'Science', 'Science'],
    'Score': [85, 91, 78, 89, 95, 92]
}

df = pd.DataFrame(data)


"""
      Name  Subject  Score
0    Alice     Math     85
1      Bob     Math     91
2  Charlie     Math     78
3    Alice  Science     89
4      Bob  Science     95
5  Charlie  Science     92
"""
```



根据单列分组并求和

```python
# 按 Name 分组并求 Score 的和
grouped = df.groupby('Name')['Score'].sum()

"""
Name
Alice      174
Bob        186
Charlie    170
Name: Score, dtype: int64
"""
```



根据多列分组并求平均值

```python
# 按 Name 和 Subject 分组并求 Score 的平均值
grouped = df.groupby(['Name', 'Subject'])['Score'].mean()

"""
Name     Subject
Alice    Math       85.0
         Science    89.0
Bob      Math       91.0
         Science    95.0
Charlie  Math       78.0
         Science    92.0
Name: Score, dtype: float64
"""

# 如果不需要按照分组列作为行索引（即恢复正常的Frame格式），则可以设置参数  as_index=False
# 也可以对结果使用 reset_index()
df.groupby(['Name', 'Subject'],as_index=False)['Score'].mean()
df.groupby(['Name', 'Subject'])['Score'].mean().reset_index()

"""
	Name		Subject		Score
0	Alice		Math			85.0
1	Alice		Science		89.0
2	Bob			Math			91.0	
...
"""
```



计算每个组的平均值，并将结果添加为新列

```python
# transform 可以结合 groupby 使用，以便对每个分组内的元素进行操作，并返回一个与原 DataFrame 具有相同形状的结果。
import pandas as pd

data = {
    'Category': ['A', 'A', 'B', 'B', 'C', 'C'],
    'Value': [10, 20, 30, 40, 50, 60]
}
df = pd.DataFrame(data)

# 计算每个组的平均值，并将结果添加为新列
df['Group_Avg'] = df.groupby('Category')['Value'].transform('mean')

"""
  Category  Value  Group_Avg
0        A     10       15.0
1        A     20       15.0
2        B     30       35.0
...
"""
```



求每个分组的某数值前五名

```python
data.groupby('title').apply(lambda x: x.nlargest(5,'rating'))

data.groupby('title').apply(lambda x: x.sort_values('age').head(5))
```

使用每个分组的平均值填充 NA

```python
data.groupby(group_key).apply(lambda x: x.fillna(x.mean()))
```

每一列的空值都有不同的填充值

```python
data.groupby(group_key).apply(lambda x: x.fillna({'East': 0.5,'West': -1}))
```









使用 agg 进行聚合操作	

​	对单列进行单操作时 	agg('xxx')

​	对单列进行多操作时 	agg(['xx','xx']) / agg(x:['xx','xx']) 

​	对多列进行多/单操作	agg({x:['xx','xx'],y:['xx','xx'],z:'xx'})

​	

```python
# 	单一聚合函数
# 按 Name 分组，并求 Score 的平均值
agg_result = df.groupby('Name')['Score'].agg('mean')
print(agg_result)


"""
Name
Alice      87.0
Bob        93.0
Charlie    85.0
Name: Score, dtype: float64
"""


#	多种聚合函数
# 按 Name 分组，分别求 Score 的平均值和总和
agg_result = df.groupby('Name')['Score'].agg(['mean', 'sum'])
print(agg_result)

"""
          mean  sum
Name                
Alice      87.0  174
Bob        93.0  186
Charlie    85.0  170
"""


# 对不同的列应用不同的聚合函数

# 按 Name 分组，对 Score 求平均值和总和，对 Subject 计数
agg_result = df.groupby('Name').agg({
    'Score': ['mean', 'sum'],
    'Subject': 'count'
})

"""
          Score         Subject
           mean  sum    count
Name                           
Alice       87.0  174        2
Bob         93.0  186        2
Charlie     85.0  170        2

"""

# 多种高级用法
# 自定义聚合函数
# 使用自定义函数对分组数据进行聚合
def range_func(x):
    return x.max() - x.min()

agg_result = df.groupby('Name')['Score'].agg(['mean', range_func])
print(agg_result)

"""
          mean  range_func
Name                        
Alice      87.0          4
Bob        93.0          4
Charlie    85.0         14
"""

# 对不同列应用不同的自定义函数

agg_result = df.groupby('Name').agg({
    'Score': ['mean', range_func],
    'Subject': 'count'
})
print(agg_result)

"""
          Score           Subject
           mean  range_func    count
Name                               
Alice      87.0          4        2
Bob        93.0          4        2
Charlie    85.0         14        2
"""

```



通过字典/series进行 索引 分组

```python
people = pd.DataFrame(np.random.randn(5, 5),
                    columns=['a', 'b', 'c', 'd', 'e'],
                    index=['Joe', 'Steve', 'Wes', 'Jim', 'Travi'])

people.iloc[2, [1, 2]] = np.nan

mapping = {'a': 'red', 'b': 'red', 'c': 'blue',
					'd': 'blue', 'e': 'red', 'f' : 'orange'}
					
by_column = people.groupby(mapping, axis=1)
by_column.sum()


"""
			blue			red
Joe		1.340711	2.066031
Steve	1.593862	-2.775582
Wes		0.580141	-0.802790
Jim		-0.369880	0.534285
Travi	1.121880	-0.017979

"""


map_series = pd.Series(mapping)

map_series
"""
a       red
b       red
c      blue
d      blue
e       red
f    orange
dtype: object
"""

people.groupby(map_series, axis=1).count()
"""
			blue	red
Joe		2			3
Steve	2			3
Wes		1			2
Jim		2			3
Travi	2			3

"""
```







使用函数进行分组，

​	比起使用字典或Series，使用Python函数是一种更原生的方法定义分组映射。任何被当做分组键的函数都会在各个索引值上被调用一次，其返回值就会被用作分组名称。

```python
people.groupby(len, axis=0).sum()

"""
		a					b					c					d				 e
3	-0.784115	1.443771	0.117327	1.433645	1.137870
5	-3.188201	0.586747	0.736299	1.979443	-0.192108
"""


people.groupby(len, axis=1).sum()
"""
				1
Joe		3.406741
Steve	-1.181721
Wes		-0.222649
Jim		0.164404
Travi	1.103901
"""
```



```python
tran_data = tran_data.groupby(['to_key']).filter(lambda group : 2<= len(group) <= 50)

"""
	•	在 groupby().filter() 中，lambda 表达式中的参数是每个分组的 DataFrame 子集，因此 len(group) 返回的是该分组的行数。
	filter 需要返回 True 或 False，以保留或丢弃整个组
	
	•	filter(lambda x: 2 <= x <= 50) 这种用法用于逐行过滤时，x 是每个元素，适用于 DataFrame 或 Series 的逐元素操作。
	•	groupby().filter() 的主要用途是基于分组特征进行过滤，例如过滤掉组内行数不符合条件的分组。
"""


# 分组后对每个分组进行行级过滤
filtered_data = tran_data.groupby('to_key').apply(lambda group: group[(group['value'] >= 20) & (group['value'] <= 80)])
print(filtered_data)

# 可用于剔除小于每组某数值平均值的情况
filtered_data = tran_data.groupby('to_key').apply(lambda group: group[(group['value'] >= group['value'].mean())])


# 创建示例数据
data = {
    'to_key': ['A', 'A', 'A', 'B', 'B', 'C', 'D', 'D', 'D', 'D'],
    'value': [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
}
tran_data = pd.DataFrame(data)

# 定义一个自定义过滤函数
def custom_filter(group):
    # 这里可以添加任何复杂的逻辑
    return group[group['value'].between(20, 80)]

# 分组后应用自定义过滤函数
filtered_data = tran_data.groupby('to_key').apply(custom_filter).reset_index(drop=True)
print(filtered_data)

"""
  to_key  value
0      A     20
1      A     30
...
6      D     80
"""
```



使用NamedAgg 命名新生成的列

```python
import pandas as pd

# 创建示例 DataFrame
data = {
    'product': ['A', 'A', 'B', 'B', 'C', 'C'],
    'sales': [100, 150, 200, 250, 300, 350]
}
df = pd.DataFrame(data)

# 使用 groupby 和 NamedAgg 进行聚合
result = df.groupby('product').agg(
    total_sales=pd.NamedAgg(column='sales', aggfunc='sum'),
    average_sales=pd.NamedAgg(column='sales', aggfunc='mean')
)

# 最新方法
file_data.groupby('户型',as_index=False)\
    .agg(数量 = ('区域','sum'))...
```





#### 关联合并

​	**pd.concat()**

​	**pd.append()**

​	**pd.merge()**

​	**join**





join：根据 DataFrame 的索引（index）来对 DataFrame 进行合并



##### **pd.concat()**

```python
"""
df1,df2

   A	B
1	A1	B1
2	A2	B2

	 A	B
3	A3	B3
4	A4	B4
"""

# 默认 axis=0 ,上下垂直合并,等效于SQL中的 union all 
pd.concat([df1,df2],axis=0)

	A		B
1	A1	B1
2	A2	B2
3	A3	B3
4	A4	B4


# axis=1 ,左右水平合并，未匹配的列值为NaN
pd.concat([df1,df2],axis=1)

   A	 B	  A	  B
1	A1	B1	NaN	NaN
2	A2	B2	NaN	NaN
3	NaN	NaN	A3	B3
4	NaN	NaN	A4	B4


# ignore / ɪɡˈnɔːr /：忽视
# 忽略原始行索引 ignore_index，True为重置行索引
# 根据列值，上下合并（axis=0），重置行索引为从零开始的下标值
pd.concat([df1,df2],axis=0,ignore_index=True) 

	 A	B
0	A1	B1
1	A2	B2
2	A3	B3
3	A4	B4



# 使用多层索引 keys，默认索引方向为行索引 由axis(默认值为0)值决定
pd.concat([df1,df2],keys=['x','y'],axis=1) 


  x	      y
	A   B   A	  B
1	A1	B1	NaN	NaN
2	A2	B2	NaN	NaN
3	NaN	NaN	A3	B3
4	NaN	NaN	A4	B4




# 不匹配级联
df3 = make_df([1,2,3,4],list('ABCD'))
df4 = make_df([1,2,3,4],list('ABCE'))
display(df3,df4)



   A	 B	 C	 D
1	A1	B1	C1	D1
2	A2	B2	C2	D2
3	A3	B3	C3	D3
4	A4	B4	C4	D4

	 A	 B	 C	 E
1	A1	B1	C1	E1
2	A2	B2	C2	E2
3	A3	B3	C3	E3
4	A4	B4	C4	E4

# 默认上下合并 对应索引没有值，会自动填充 np.NaN
pd.concat([df3,df4],ignore_index=True)  

	A 	B	   C	 D	E
0	A1	B1	C1	D1	NaN
1	A2	B2	C2	D2	NaN
2	A3	B3	C3	D3	NaN
3	A4	B4	C4	D4	NaN
4	A1	B1	C1	NaN	E1
5	A2	B2	C2	NaN	E2
6	A3	B3	C3	NaN	E3
7	A4	B4	C4	NaN	E4


# 外连接，类似于并集，会显示所有数据
# 默认 join='outer'
pd.concat([df3,df4],join='outer') 

  A	   B	 C	 D	 E
1	A1	B1	C1	D1	NaN
2	A2	B2	C2	D2	NaN
3	A3	B3	C3	D3	NaN
4	A4	B4	C4	D4	NaN
1	A1	B1	C1	NaN	E1
2	A2	B2	C2	NaN	E2
3	A3	B3	C3	NaN	E3
4	A4	B4	C4	NaN	E4


# 内连接，只连接匹配的列
pd.concat([df3,df4],join='inner') 

	 A	 B	 C
1	A1	B1	C1
2	A2	B2	C2
3	A3	B3	C3
4	A4	B4	C4
1	A1	B1	C1
2	A2	B2	C2
3	A3	B3	C3
4	A4	B4	C4
```





##### 	pd.merge() 

​		merge	合并  / mɜːrdʒ /

###### 参数列表

**left:**		参与合并的左侧 DataFrame

**right:**	 参与合并的右侧 DataFrame

**how：** 	inner/outer/left/right  默认为inner

**on：** 		用于连接的列名，必须存在于左右两个 Data Frame对象中

​				  如果未指定，且其他连接键也未指定，则以left/right 列名的交集作为连接键

**left_on / right_on：** 左侧/右侧  Data Frame 中用作连接键的列

**left_index / right_index：**

​				连接键位于索引中，可以传入 left_index = True 或 right_index = True （可以两个都传），以说明索引应该被用作连接键

**sort：**		根据连接键对合并后的数据进行排序，默认为True。

​					有时在处理大数据集时，禁用该选项可获得更好的性能

**suffixes：** 字符串值元组，用于追加到重叠列名的末尾，默认为（'_ x','_ y'）

​					例如左右两个Data Frame对象都有 data 列，则结果中就会出现

​					data_ x	data_ y

**copy：**		设置为False，可以在某些特殊情况下避免将数据复制到结果数据结构中

​					 默认总是复制





###### 示例代码

```python
# 使用 pd.merge 合并
# 类似于mysql中表之间的关联，每一列元素的顺序不要求一致
# merge 与 concat 区别在于：merge需要依据某一共同的行/列进行合并

''' 
使用 merge 合并时，会自动根据两者相同column名称的那一列作为key进行合并,
两边不存在相同列名时，可使用left_on rigth_on分别指定两表中的不同列作为连接字段
单列相同时，该列值匹配即可，多列相同时，默认需要全部相同列值匹配才可连接，可通过 on 指定一列作为连接的字段 
'''


# 内连接 只保留关联行的数据
# 外连接 保留所有数据，未匹配则默认为NaN    可使用fillna()方法或者其他方法来替换这些缺失值
# 左连接 显示左表的所有数据，右边关联上的数据
# 右连接 显示右表的所有数据，左边关联上的数据
# 参数 indicator 默认为false 表示是否生成标明该行来源
# （来自left/right/both）的新列（_merge），列名为 _merge（可通过 suffixes 参数指定名称）
# suffixes：为两表相同字段名的添加指定后缀，默认为_x/_y


"""
df1,df2 

	name	id	age
0	张三	1001	18
1	李四	1002	21
2	王五	1003	24


  id	sex	job
0	1002	男	sailor
1	1003	男	CEO
2	1004	女	programer
"""


# 匹配上相对应列的才保留，默认为inner join/内连接
pd.merge(df1,df2)      

	name	id	age	sex	job
0	李四	1002	21	男	sailor
1	王五	1003	24	男	CEO




# 存在多个相同列时，指定某一列作为连接字段
df1.merge(df2,on = 'id') 

	name	id	age	sex	job
0	李四	1002	21	男	sailor
1	王五	1003	24	男	CEO


# 两边不存在相同列名时，可使用left_on rigth_on分别指定两表中的不同列作为连接字段 
df1.merge(df2,left_on='id',right_on='id') 



# 可使用行索引作为连接的字段
# 左边的行索引值与右边的行索引值相等
df1.merge(df2,left_index=True,right_index=True) 

# 左边的行索引值等于右边的id字段值
df1.merge(df2,left_index=True,right_on='id')    

# how默认为'inner'   inner join/内连接
pd.merge(df1,df2,how='inner')           

# how值为'outer' 外连接 
df1.merge(df2,how='outer')   
pd.merge(df1,df2,how='outer') 



# 根据多个键进行合并，传入一个由列名组成的列表即可
pd.merge(left, right, on=['key1', 'key2'], how='outer')
```





##### join

​	join 是基于索引进行合并的便捷方法，特别适用于两个 DataFrame 有相同或相似索引的情况。通过选择不同的 how 参数（如 ‘left’、‘right’、‘outer’、‘inner’），可以灵活地进行各种连接操作。同时，通过 lsuffix 和 rsuffix 参数处理列名冲突，默认左连接。

​	特点：基于索引进行合并

```python
"""
	•	other：要合并的另一个 DataFrame。
	•	on：用于连接的列。如果没有指定，默认使用索引。
	•	how：连接方式。默认为 ‘left’，可选值有 ‘left’、‘right’、‘outer’、‘inner’。
	•	lsuffix 和 rsuffix：当列名相同时为左、右DataFrame加的后缀。
	•	sort：是否在合并后对结果进行排序。
"""
DataFrame.join(other, on=None, how='left', lsuffix='', rsuffix='', sort=False)


# 基于索引的左连接（默认）

import pandas as pd

# 创建两个示例 DataFrame
df1 = pd.DataFrame({
    'A': ['A0', 'A1', 'A2'],
    'B': ['B0', 'B1', 'B2']
}, index=['K0', 'K1', 'K2'])

df2 = pd.DataFrame({
    'C': ['C0', 'C1', 'C2'],
    'D': ['D0', 'D1', 'D2']
}, index=['K0', 'K2', 'K3'])

# 左连接
result = df1.join(df2)
print(result)

"""
     A   B    C    D
K0  A0  B0   C0   D0
K1  A1  B1  NaN  NaN
K2  A2  B2   C1   D1
"""


# 基于索引的内连接
result = df1.join(df2, how='inner')
print(result)

"""
     A   B   C   D
K0  A0  B0  C0  D0
K2  A2  B2  C1  D1
"""


# 基于索引的外连接
result = df1.join(df2, how='outer')
print(result)

"""
      A    B    C    D
K0   A0   B0   C0   D0
K1   A1   B1  NaN  NaN
K2   A2   B2   C1   D1
K3  NaN  NaN   C2   D2
"""



# 基于索引的右连接
result = df1.join(df2, how='right')
print(result)

"""
      A    B   C   D
K0   A0   B0  C0  D0
K2   A2   B2  C1  D1
K3  NaN  NaN  C2  D2
"""


# 使用 lsuffix 和 rsuffix 处理列名冲突

# 创建两个示例 DataFrame
df1 = pd.DataFrame({
    'A': ['A0', 'A1', 'A2'],
    'B': ['B0', 'B1', 'B2']
}, index=['K0', 'K1', 'K2'])

df2 = pd.DataFrame({
    'A': ['C0', 'C1', 'C2'],
    'D': ['D0', 'D1', 'D2']
}, index=['K0', 'K2', 'K3'])

# 左连接，并处理列名冲突
result = df1.join(df2, lsuffix='_left', rsuffix='_right')
print(result)

"""
     A_left   B   A_right   D
K0      A0   B0      C0   D0
K1      A1   B1     NaN  NaN
K2      A2   B2      C1   D1
"""

```











#### 缺失处理

​		丢失数据（空值）存在两种情况：np.nan  None



**None：**python中自带的空对象，不能参与到任何计算中

​			 object 类型的运算要比 int 类型的运算慢得多

​			 如果函数没有明确的返回值，默认返回 None，也常被作为函数的默认参数



**np.nan：**浮点类型（ Type(np.nan)  float），能参与到计算中，但计算结果总是NaN

​				 可以使用 np.nan*() 函数计算nan ，此时会过滤掉nan





**NaN 和 None 的区别**



​	1.	**数据类型**：

​	•	NaN 是一种特殊的浮点数（float），它来自于 NumPy 库。

​	•	None 是 Python 内置的空值类型，通常用于对象类型（如字符串或其他非数值数据）。

​	2.	**使用场景**：

​	•	NaN 常用于数值型数据的缺失值。

​	•	None 常用于对象型数据的缺失值。

​	3.	**操作行为**：

​	•	大多数数值操作会忽略 NaN，但不会忽略 None。

​	•	NaN 和 None 在 DataFrame 操作（如求和、平均等）中会有不同的表现。



**NaN 和 None 的互换**

```python
# 将 None 转换为 NaN
df['B'] = df['B'].apply(lambda x: np.nan if x is None else x)
print(df)

# 将 NaN 转换为 None
df['A'] = df['A'].apply(lambda x: None if pd.isna(x) else x)
print(df)
```





**在 pandas 中 None 与 np.nan 都会被自动转换成 np.nan**

```python
n = np.array([1,2,3,np.nan,5,6])
np.sum(n)  # 返回值为 nan
np.nansum(n)  # 返回值为17.0 自动过滤nan不参与计算
```





**isna 是 isnull 的别名，notna 是 notnull 的别名 **

​	**isnull()：**判断数据是否为 np.nan，是则返回 true

​	**notnull()：**判断数据是否为 np.nan，是则返回false





​	**all()：**该列/行所有，类似于 and，axis 默认为零，某列

​	**any()：**该列/行存在 ，类似于 or，axis 默认为零，某列



```python
# isnull()/notnull()/any(axis=1/0)/all(axis=1/0)  可灵活搭配使用
df.isnull().any()  # 判断列中是否存在空值元素
df.isnull().all()  # 判断列中是否全为空值元素



# notna() 和  isna()：用于筛选存在非缺失值或缺失值
# notna与notnull，isna与pd.isnull等价，可配合any/all使用
# isnull/pd.isnull:  判断索引对应值是否为空，返回下标索引以及对应布尔值
# notnull/pd.notnull:  判断索引对应值是否不为空，返回下标索引以及对应布尔值


# 对 DataFrame 进行按列求和，得到每列的空值数量
df.isna().sum()
# 对每列的空值数量求和，得到整个 DataFrame 中所有的空值总数量
df.isnull().sum().sum()
```





##### **bool值索引过滤**

```python
# 筛选过滤类似于 切片 操作的行列顺序，无论是[]/loc[]都是先对行进行操作
data = np.random.randint(0,100,size=(5,5))
df01 = pd.DataFrame(data = data,columns=list('ABCDE'))

# 返回一个列索引为 A,B,C,D,E  行索引为默认值 0，1，2，3，4 的DataFrame
	 A	 B	 C	 D	 E
0	51	43	98	63	18
1	96	25	59	76	80
2	54	12	97	71	64
3	24	45	26	28	20
4	61	58	40	64	35

# 将两个值设为 NaN，在 DataFrame 中 np.nan 与 None 都会被转换成 NaN
df01.loc[0,'B'] = np.nan
df01.loc[3,'D'] = None

# 判断 DataFrame 中的每一个值是否为null
df01.isnull()

			A 	 B	    C   	D   	E
0	False	True	False	False	False
1	False	False	False	False	False
2	False	False	False	False	False
3	False	False	False	True	False
4	False	False	False	False	False

# 判断列中是否存在空值元素,any的axis参数值默认为0/列
df01.isnull().any()

A    False
B     True
C    False
D     True
E    False
dtype: bool
  
 
# 判断行中是否存在空值元素
cond = df01.isnull().any(axis=1) 
# type(cond): pandas.core.series.Series
0     True
1    False
2    False
3     True
4    False
dtype: bool
  
  
# 过滤存在空值的行
df01[~cond]  
# 两者等效
df01.loc[~cond]

# 先对不满足列限制的行进行过滤，再对不满足行限制的列去除
df01.loc[df01['A']>50,(df01.loc[1]>30) & (df01.loc[2]>40)]  
```



##### **dropna**

```python
# dropna(subset,axis=0, how='any', inplace=False)

	A			B	  C	   D	  E
0	51	NaN	  98	63.0	18
1	96	25.0	59	76.0	80
2	54	12.0	97	71.0	64
3	24	45.0	26	NaN	  20
4	61	58.0	40	64.0	35

# subset = '' 指定判断的列/行
# inplace = True 直接修改原数据
# 默认情况下 axis = 0 , how = 'any' 删除存在空值的行
# copy 为深拷贝，另开空间，= 为浅拷贝，还是指向同一地址，值会一同改变
# dropna 只能用于过滤空值,默认值： axis=0, how='any', inplace=False
df01.dropna()   

	 A	   B	 C	   D	 E
1	96	25.0	59	76.0	80
2	54	12.0	97	71.0	64
4	61	58.0	40	64.0	35



# axis = 1 , how = 'any'  删除存在空值的列
df01.dropna(axis=1)   

	 A	 C	 E
0	51	98	18
1	96	59	80
2	54	97	64
3	24	26	20
4	61	40	35



# 默认情况下 axis = 0 , how = 'all' 删除所有值都为空值的行
df01.dropna(how = 'all')   

# axis = 1 , how = 'all'  删除所有值都为空值的列
df01.dropna(axis=1,how='all')   

```





##### fillna

```python
# 用于填充替换  Series/DataFrame 中 NaN
# fillna(value=,limit=, inplace=)
# value 为填充数值， limit 为填充次数， inplace 为是否覆盖（修改原数据）
df01.fillna(value = 60,limit = 1) 


# 可以使用fillna()函数或者replace()函数将DataFrame中的NaN值替换为指定值
# 将所有NaN值替换为0
df_filled = df.fillna(0)

# 或者替换特定列的NaN值
df['A'].fillna(0, inplace=True)  # inplace=True 直接在原DataFrame上修改

# 将整个DataFrame中的NaN替换为0
df_replaced = df.replace(np.nan, 0)

# 或者替换特定列的NaN值
df['A'].replace(np.nan, 0, inplace=True) 
```







#### 重复处理



##### 去重

```
    unique(): 返回唯一值的数组（适用于 Series）。可配合 len() 统计数量（包含 NaN）
    drop_duplicates(): 返回去重后的 Series 或 DataFrame，可以指定 DataFrame中的某列/某几列进行去重
```

#####  计算数量

```
    len(): 返回对象的长度，包括 Series/DataFrame/数组 的数据量，包含 NaN
    count(): 返回非 NaN 值的数量（不去重）
    nunique(): 返回唯一值的数量，不包含 NaN   
```

​    

#####  Series 

    	unique：返回唯一值的数组，可配合使用len 统计数量（包含NaN） 
    	drop_duplicates：返回唯一值的Series/DataFrame
    	nunique：返回唯一值的数量，不包含NaN
    	count：返回非NaN的数量（不去重）
    	len：返回 Series 的长度，包括 NaN

#####  DataFrame

```
	drop_duplicates：返回唯一值的Series/DataFrame
	nunique：返回每一列唯一值的数量，不包含NaN
	count：返回每一列非NaN的数量（不去重）
	len：返回 DataFrame 的行数（不包含列名的数量）
```



**duplicated		/ ˈdjʊpləˌket /  重复/复制**

​	duplicated()：检测重复的行

​	drop_duplicated():	删除重复的行

```python
# 判断是否存在重复值/是否与前面的行存在重复

   A	 B	 C	 D
1	A2	B2	C2	D2
2	A2	B2	C2	D2
3	A3	B3	C3	D3
4	A4	B4	C4	D4



df.duplicated()

1    False
2     True
3    False
4    False
dtype: bool
 

# 保留第一行 即：不标识重复值第一次出现位置 
df.duplicated(keep='first') 

1    False
2     True
3    False
4    False
dtype: bool

  
# 保留最后一行 即：不标识重复值最后一次出现位置 
df.duplicated(keep='last') 

1     True
2    False
3    False
4    False
dtype: bool
  
  
# 不保留任一行 即：标识重复值所有出现位置    
df.duplicated(keep=False) 

1     True
2     True
3    False
4    False
dtype: bool
  
  
# 判断指定子集内的数据是否重复    
df.duplicated(subset=['A','B','C'])  
1    False
2     True
3    False
4    False
dtype: bool
  
  
  
  
df.drop_duplicates(subset=['A','B','C'])  # 可配合keep参数
	 A 	 B	 C	 D
1	A2	B2	C2	D2
3	A3	B3	C3	D3
4	A4	B4	C4	D4

```











#### 数据映射



​	**replace：**替换元素   （ inplace 参数用于判断是否修改原本结构 ）

​	**pipe：**将 DataFrame 或 Series 作为输入传递给自定义函数链

​	**map：**处理单独的某一列（重要），对每个元素应用函数，一般用于 Series 数据结构中，不能用于 Data Frame

​	**apply：**既支持 Series，也支持 Data Frame，可以沿行/列方向应用函数，灵活性更高

​	**applymap：**只能用于Data Frame，对每个元素应用函数

​	**transform：**对 DataFrame 或 Series 进行逐元素转换，可以对每个元素、行或列应用一个函数，并返回与输入形状相同的对象



map  针对于 Series 结构进行操作，或者从 DataFrame 中取出某一列/行进行操作

apply  对于 DataFrame 的每一列/行 进行**聚合操作**，也可以单独对 Series 进行操作

replace 对于 Series / DataFrame 进行值替换操作，也可以用于替换缺失值

applymap  对于 DataFrame 中的每一个元素进行操作

transform  对于 Series / DataFrame 进行多次计算处理





##### replace	

```python
index = ['张三','李四','王五','赵六']
columns = ['Java','Python','Scala','Linux']
data = np.random.randint(10,100,size = (4,4),dtype = np.int16)
df = pd.DataFrame(data=data,index=index,columns=columns)


    Java	Python	Scala	Linux
张三	54		88		43		33
李四	68		98		97		99
王五	92		70		32		97
赵六	44		93		89		61


# 将对应的值替换  inplace = True  是否改变原数据
df.replace({54:45,33:63},inplace = True )
df.replace([54,33],[45,63],inplace = True )


# 可同时将多个值替换为同一指定值
df.replace([54,33],np.nan,inplace = True )
```





##### map

​	使用map是一种实现元素级转换以及其他数据清理工作的便捷方式

```python
# map 一般用于 Series 数据结构中，作用于列，不能用于 Data Frame
df1['Python'].map({70:90})     # 使用较少，此该值被替换外，其余值都会被替换为NaN

张三     NaN
李四     NaN
王五    90.0
赵六     NaN
Name: Python, dtype: float64



# map一般用于列中的批量操作
df1['Python']*10
df1['Python'].map(lambda x:x*0.95)

张三    83.60
李四    93.10
王五    66.50
赵六    88.35
Name: Python, dtype: float64


# 将处理过列加入到原始结构中，Pandas为原本不存在的列，若为存在则操作为替换
df1['Pandas'] = df1['Python'].map(lambda x:x*0.95)  # 行索引相同

	  Java	Python	Scala 	Linux 	Pandas
张三	45		88			43		 63				83.60
李四	68		98			97		 99				93.10
王五	92		70			32		 97				66.50
赵六	44		93			89		 61				88.35



# 判断java的成绩是否 > 60 (及格)
# 此处在 lambda 表达式中嵌套使用了 if else ，if 成立时的值放在最前面，else 成立时的值放在最后面
df1['Java是否及格'] = df1['Java'].map(lambda x: '及格' if x >= 60 else '不及格')


		Java	Python	Scala	 Linux	Pandas	Java是否及格
张三	45		88			43		63			83.60			不及格
李四	68		98			97		99			93.10			及格
王五	92		70			32		97			66.50			及格
赵六	44		93			89		61			88.35			不及格


# 使用普通函数
# 判断Scala成绩，60 > n 为不及格，60 <= n <80为及格，n > 80为优秀

def fn(n):
    if n < 60:
        return '不及格'
    elif 60 <= n < 80:
        return '及格'
    elif n >= 80:
        return '优秀'
    

df1['Scala等级'] = df1['Scala'].map(fn)


		Java	Python	Scala	Linux	Scala等级
张三	65		84			30		49		不及格
李四	58		96			36		89		不及格
王五	59		74			95		97		优秀
赵六	68		66			19		68		不及格





# 定义一组与肉类有关的数据
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon','Pastrami', 'corned beef', 'Bacon','pastrami', 'honey ham', 'nova lox'],
'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})

"""
	food			ounces
0	bacon				4.0
1	pulled pork	3.0
2	bacon				12.0
3	Pastrami		6.0
4	corned beef	7.5
5	Bacon				8.0
6	pastrami		3.0
7	honey ham		5.0
8	nova lox		6.0
"""


# “添加一列表示该肉类食物来源的动物类型,先编写一个不同肉类到动物的映射字典”
“meat_to_animal = {
  'bacon': 'pig',
  'pulled pork': 'pig',
  'pastrami': 'cow',
  'corned beef': 'cow',
  'honey ham': 'pig'
}”


# “Series的map方法可以接受一个函数或含有映射关系的字典型对象，但是这里有一个小问题，即有些肉类的首字母大写了，而另一些则没有。因此，我们还需要使用Series的str.lower方法，将各个值转换为小写”

lowercased = data['food'].str.lower()

"""
0          bacon
1    pulled pork
2          bacon
3       pastrami
4    corned beef
5          bacon
6       pastrami
7      honey ham
8       nova lox
Name: food, dtype: object
"""

# 如果map匹配不上字典中的key，则值为NaN
data['animal'] = lowercased.map(meat_to_animal)


"""
	food				ounces	animal
0	bacon				4.0			pig
1	pulled pork	3.0			pig
2	bacon				12.0		pig
3	Pastrami		6.0			cow
4	corned beef	7.5			cow
5	Bacon				8.0			pig
6	pastrami		3.0			cow
7	honey ham		5.0			pig
8	nova lox		6.0			NaN
"""


# 也可以直接一步到位
data['animal'] = data['food'].map(lambda x:meat_to_animal[x.lower()])
```





##### rename



```python
	  Java	Python	Scala	Linux	Scala等级
张三	65	  84			30		49		不及格
李四	58	  96			36		89		不及格
王五	59	  74			95		97		优秀
赵六	68	  66			19		68		不及格


df1.rename({'张三':'Mr Zhang'})  			# 默认 axis=0 修改行索引名
df1.rename(index={'张三':'Mr Zhang'})

					Java	Python	Scala	Linux	Scala等级
Mr Zhang	 65			84			30		49		 不及格
李四				58		 96			 36		 89		  不及格
王五				59		 74			 95		 97			优秀
赵六				68		 66			 19	 	 68			不及格





df1.rename({'Java':'佳哇'},axis=1)
df1.rename(columns={'Java':'佳哇'})


		佳哇	Python	Scala	Linux	Scala等级
张三	65		84			30		49		不及格
李四	58		96			36		89		不及格
王五	59		74			95		97		优秀
赵六	68		66			19		68		不及格



# 重制索引，将索引名还原为数字，并将原索引名称制成 index 名称的一列
df1.reset_index()


		index	Java	Python	Scala	Linux	Scala等级	Java是否及格
0		 张三	 65			84			30		49		不及格			及格
1		 李四	 58			96			36		89		不及格			不及格
2		 王五	 59			74			95		97		优秀			 不及格
3		 赵六	 68			66			19		68		不及格			及格



# 指定某一列的值作为行索引
df1.set_index(keys=['Java'])

	Python	Scala	Linux	Scala等级	Java是否及格
Java					
65	84			30		49		不及格			及格
58	96			36		89		不及格			不及格
59	74			95		97		优秀			 不及格
68	66			19		68		不及格			 及格

```







##### apply / applymap

​		既可用于 Series 也可用于 DataFrame

```python
df = pd.DataFrame(data = np.random.randint(10,100,size=(5,3)),
                index = list('ABCDE'),
                columns = ['Python','Scala','Java']
    )
    


	Python	Scala	Java
A		55			21	52
B		90			37	38
C		24			17	50
D		72			94	54
E		85			44	40


# apply 用于 Series 中(此时作用与 map 相同)，其中 X 表示 Series 中的元素
df['Python'].apply(lambda x: True if x>5 else False)

A    True
B    True
C    True
D    True
E    True
Name: Python, dtype: bool



# apply 用于 DataFrame，其中 x 代表 DataFrame 某列/某行的Series数据
df.apply(lambda x: x.mean(),axis=0)  # 求每一列数据的平均值

Python    65.2
Scala     42.6
Java      46.8
dtype: float64


df.apply(lambda x: x.mean(),axis=1)  # 求每一行数据的平均值

A    42.666667
B    55.000000
C    30.333333
D    73.333333
E    56.333333
dtype: float64



# 自定义方法
def fn2(x):
    return x.mean()


def fn3(x):
    return (np.round(x.mean(),1),x.count())
    
    
df.apply(fn2)  				# 求某一列的平均值

Python    65.2
Scala     42.6
Java      46.8
dtype: float64


df.apply(fn3,axis=1)  # 求某一行的平均值（保留一位小数），以及对应的元素个数

A    (42.7, 3)
B    (55.0, 3)
C    (30.3, 3)
D    (73.3, 3)
E    (56.3, 3)
dtype: object



df.apply(fn3,axis=0)  # 求某一列的平均值（保留一位小数），以及对应的元素个数

	Python	Scala	Java
0		65.2	42.6	46.8
1		5.0		5.0		5.0



# 两种用法等价
def sepal_length_moth(df):
    if df.sepal_length >= 5:
        return "高"
    elif df.sepal_length < 5:
        return "低"
    else:
        return "无法判断"

x = dataset.apply(lambda x:sepal_length_moth(x),axis=1)




def sepal_length_moth(series):
    return series.apply(lambda x: "高" if x >= 5 else "低" if x < 5 else "无法判断")

# 假设 dataset['sepal_length'] 是想要处理的列
y = sepal_length_moth(dataset['sepal_length'])


# applymap： DataFrame 专有的方法，其中 x 为每个元素
df.applymap(lambda x: x + 1)


	Python	Scala	Java
A		56			22	 53
B		91			38	 39
C		25			18	 51
D		73			95	 55
E		86			45	 41
	
  
```



##### pipe

pipe 函数用于将 DataFrame 或 Series 作为输入传递给自定义函数链

```python
# 创建 DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3],
    'B': [10, 20, 30]
})

# 定义自定义函数
def add_one(df):
    return df + 1

# 使用 pipe
result = df.pipe(add_one)
print(result)
```







#### 数据抽样

take	默认取行

permutation		/ ˌpɜːrmjuˈteɪʃ(ə)n /    排列

```python
df = pd.DataFrame(data = np.random.randint(10,100,size=(5,3)),
                index = list('ABCDE'),
                columns = ['Python','Scala','Java']
    )
    

	Python	Scala	Java
A		82			55	18
B		18			58	36
C		38			42	15
D		84			89	67
E		76			74	70

# 行排列，默认 axis=0
df.take([2,1])          

	Python	Scala	Java
C		52			36	63
B		36			47	10


# 列排列
df.take([2,0,1,1],axis=1)

	Java	Python	Scala	Scala
A		84		24			92	92
B		10		36			47	47
C		63		52			36	36
D		14		37			17	17
E		80		74			63	63


 # 进行随机排列
 # np.random.permutation 不会改变输入数组，而是返回一个新的随机排列后的数组 
np.random.permutation([0,1,2]) 
array([1, 0, 2])


# 无放回抽样，依次随机取出，不会有重复值
df.take(np.random.permutation([0,1,2]))   # 模拟随机提取


# 有放回抽样，可能存在重复值
df.take(np.random.randint(0,5,size = 5))
```







#### 数学函数

##### 	**聚合函数**

​			默认求列，可修改参数 axis 的值

​	count()：非空值元素的数量

​	max()：最大值

​	min()：最小值

​	median()：中位数

​	sum()：求和（某一列/行），如需求所有元素之和，则需要 df.values.sum()

​	mean()：平均值



##### 	**其它函数**

​	value_counts()：统计元素出现的次数

​	cumsum()：累加

​	cumprod()：累乘

​	var()：方差，方差越大，数据波动越大

​	std()：标准差，方差的算术平方根

​	cov()：协方差

​	corr()：所有属性相关性系数



​	astype()：数据类型的转换  

```python
chipo.item_price = chipo.item_price.astype('float')
```







#### 数据读写

##### 	csv数据

​		加载CSV数据：.read_csv

​		存储CSV数据：.to_csv



```python
# sep: 						指定分隔符，默认为 ,  read_table 的默认分隔符为 '\t' (制表符) 
#									可使用正则表达式来匹配不同空格数的分隔  sep='\s+'
# skiprows:				用于跳过不需要的取值行
#									skiprows=[0, 2, 3] 0/2/3行的数据不会加载进
# header: 				是否保存列索引，None 时原列索引会变成第一行内容，默认为 [0]
# index_col = 0 : 取第一列为行索引，默认重新生成行索引

pd.read_csv('data.csv',index_col=0,header=None)
# 数据中没有原始索引时
# 可以使用 header=None 赋予数据默认数值索引
pd.read_csv('examples/ex2.csv', header=None)
# 也可以自定义列名
pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
# 指定所读取文件的行/列索引
 names = ['a', 'b', 'c', 'd', 'message']
# 指定数组names作为列索引，指定文件中的某一列为行索引
 pd.read_csv('examples/ex2.csv', names=names, index_col='message')

         a   b   c   d
message               
hello    1   2   3   4
world    5   6   7   8
foo      9  10  11  12



# path_or_buf:		存储路径 + 文件名
# sep: 						指定分隔符，默认为 ,
# header: 				是否保存列索引
# index: 					是否保留行索引,默认为 True

a.to_csv(path_or_buf='data.csv',sep=',',header=True)
```





##### 	excel数据

​			加载excel文件：read_excel

​			存储excel文件：to_excel

​			

```python
# sheet_name: 工作表名称
# header: 是否保存列索引  True/False
# index：是否保存行索引  True/False
# na_rep 输出时将缺失值表示替换为特定的字符串
data.to_excel('data.xlsx',sheet_name='Sheet1',header=True,index=False)




# sheet_name: 指定工作表，可为名称，也可以是索引值（从0开始
# header: 指定某行为列索引,None则新增默认值索引,保留原索引为行数据
# index:  指定某列为行索引,None则新增默认值索引,保留原索引为列数据
# names:  替代列名
# na_values 读取时识别指定字符为缺失值（NaN），可以是一个标量值、字符串、列表或字典
pd.read_excel('data.xlsx',sheet_name=0,header=None)


		0				1			2
0		Python	Scala	Java
1		10	21	25
2		98	71	35
3		96	41	74
4		23	19	31
5		63	40	95
6		79	44	36

# nrows / chunksize 指定读取的行数
pd.read_csv('examples/ex6.csv', nrows=5)
         one       two     three      four key
0  0.467976 -0.038649 -0.295344 -1.824726   L
1 -0.358893  1.404453  0.704965 -0.200638   B
2 -0.501840  0.659254 -0.421691 -0.057688   G
3  0.204886  1.074134  1.388361 -0.982404   R
4  0.354628 -0.133116  0.283763 -0.837063   Q

# 统计Key值对应的数量
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)

tot = pd.Series([])
for piece in chunker:
    tot = tot.add(piece['key'].value_counts(), fill_value=0)

tot = tot.sort_values(ascending=False)

E    368.0
X    364.0
L    346.0
O    343.0
Q    340.0
M    338.0
J    337.0
F    335.0
K    334.0
H    330.0


```





##### MySQL数据

​		**此处复习**

​			使用数据库：									use  数据库名；

​			查询所有数据库：							show databases;

​			查询当前数据库：						    select database（）；

​			查询当前数据库中的所有表：		 show tables;

​			查询表的情况：								desc  表名；

​			查询指定表的建表语句：				 show create table 表名;





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

# 创建 SQLAlchemy 引擎
engine = create_engine('mysql+pymysql://username:password@localhost/database_name')

# 创建示例 DataFrame
data = {
    '用户名': ['Alice', 'Bob', 'Charlie', 'David'],
    '数量': [1, -2, 0, -3],
    '单价': [10.5, -5.2, 0, -2.5]
}
your_table_name = pd.DataFrame(data)

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











#### 类型转换

##### 数值类型转换

​	**floor**:	 将数值向下取整（地板函数）

​	**ceil**:	   将数值向上取整（天花板函数）

​	**trunc**:   将数值截断到整数部分

​	**round**：将数值四舍五入到指定的小数位数

```python
#	decimals：要保留的小数位数，默认为 0

DataFrame.round(decimals=0, *args, **kwargs)
Series.round(decimals=0, *args, **kwargs)
```





##### 变量类型转换

​	**astype**：数据类型的转换  

```python
"""
	•	dtype：要转换的类型或类型字典。
	•	copy：默认为 True，表示返回副本。如果设置为 False，尽量避免复制。
	•	errors：‘raise’ 或 ‘ignore’，默认为 ‘raise’。‘raise’ 会在转换失败时引发错误，‘ignore’ 会忽略错误。
"""
DataFrame.astype(dtype, copy=True, errors='raise')
```



​	**to_numeric**：将一列数据转换为数值类型

```python
"""
	•	arg：要转换的对象。
	•	errors：‘raise’, ‘coerce’, ‘ignore’，默认为 ‘raise’。‘raise’ 会在转换失败时引发错误，‘coerce’ 会将无效值转换为 NaN，‘ignore’ 会忽略错误。
	•	downcast：‘integer’, ‘signed’, ‘unsigned’, ‘float’，将数值类型缩小为最小适用类型。
"""

DataFrame.to_numeric(arg, errors='raise', downcast=None)
```



**to_datetime**:	将一列或多列数据转换为 datetime 类型

```python
"""
	•	arg：要转换的对象。
	•	errors：‘raise’, ‘coerce’, ‘ignore’，默认为 ‘raise’。
	•	其他参数用于处理日期格式和时间单位等。
"""	

DataFrame.to_datetime(arg, errors='raise', dayfirst=False, yearfirst=False, utc=None, format=None, exact=True, unit=None, infer_datetime_format=False, origin='unix', cache=True)
```



**to_timedelta**:	将一列数据转换为 timedelta 类型

```python
"""
	•	arg：要转换的对象。
	•	unit：时间单位，默认为 ‘ns’。
	•	errors：‘raise’, ‘coerce’, ‘ignore’，默认为 ‘raise’。
"""
pd.to_timedelta(arg, unit='ns', errors='raise')
```



**infer_objects**:	将对象类型的列转换为更具体的类型

```python
DataFrame.infer_objects()
```



##### 字符串值转换

​	**str.lower()**：将字符串转换为小写。

​	**str.upper()**：将字符串转换为大写。

​	**str.title()**：将字符串转换为标题大小写。

​	**str.swapcase()**：将字符串中的大写转换为小写，小写转换为大写。

​	**str.capitalize()**：将字符串的第一个字符转换为大写，其他字符转换为小写。













#### 条件筛选

##### np.where 

​	返回的是将元素行索引，列索引分别作为数组的元组		

```python
arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

np.where(arr > 5)
# (array([1, 2, 2, 2]), array([2, 0, 1, 2]))

# 或者直接创建一个新的数组，用True或False替换原数组元素
# 把arr中大于5的元素留下，否则替换为0
result_arr = np.where(arr > 5, arr, 0) 
```

​			

##### np.argwhere 

返回的是将元素的行/列索引结合作为数组的元组

```python
np.argwhere(arr > 5)
# array([[1, 2],
        [2, 0],
        [2, 1],
        [2, 2]])
      

# 可使用 reshape(-1) 操作将数据进行形状整合   
arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
np.argwhere(arr>3)      

array([[3],
       [4],
       [5],
       [6],
       [7],
       [8]])

arr[np.argwhere(arr>3)].reshape(-1)
# array([4, 5, 6, 7, 8, 9])
```



##### np.布尔索引

```python
# 布尔索引
arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
boolean_mask = arr > 5 
arr[boolean_mask]
```







##### pd.loc/iloc/布尔运算

​			•.loc 支持混合索引（包括标签索引和切片）和布尔索引。

​			•.iloc 使用基于位置的索引（整数索引）

​			•直接在DataFrame或Series上进行布尔运算，返回的将是布尔值系列，可用于筛选

```python
import pandas as pd

df = pd.DataFrame(
  data = np.random.randint(10,100,size = (10,5)),
  columns=list('ABCDE')
)

# 筛选出A列值大于30的行
filtered_df_01 = df[df['A'] > 30] 

# 筛选出第一行大于50的列
# 条件过滤类似于切片，对列切片必需使用 loc/iloc，且要先对行切片
filtered_df_02 = df.loc[:,df.loc[0] > 50]
```



​	

##### pd.isin

​	筛选出满足特定值列表的行，用于过滤DataFrame或Series中的数据。

​	可以找到某些值是否存在于指定的列表或集合中，并返回一个布尔型的DataFrame或Series，用于进一步的筛选和分析

```python
"""
注意事项
数据类型：isin函数对数据类型敏感，确保values中的数据类型与DataFrame或Series中的数据类型一致，否则匹配结果可能不正确。
缺失值：NaN值不会被匹配，即使values中包含NaN。
性能：对于大型DataFrame或Series，isin操作可能会比较耗时，注意优化代码以提高性能。
"""


import pandas as pd

# 创建一个示例Series
s = pd.Series(['apple', 'banana', 'cherry', 'date'])

# 查找是否存在于指定列表中
result = s.isin(['apple', 'cherry'])
print(result)

"""
0     True
1    False
2     True
3    False
dtype: bool
"""


# 使用字典在DataFrame中进行列匹配
# 创建一个示例DataFrame
df2 = pd.DataFrame({
    'A': [1, 2, 3, 4],
    'B': ['apple', 'banana', 'cherry', 'date'],
    'C': ['dog', 'cat', 'dog', 'cat']
})

# 使用字典在多个列中查找值
result = df2.isin({'A': [1, 3], 'C': ['cat']})
print(result)

"""
       A      B      C
0   True  False  False
1  False  False   True
2   True  False  False
3  False  False   True
"""
```





##### pd.notna / pd.isna

​		用于筛选存在非缺失值或缺失值

```python
# 筛选出A列存在非缺失值的行
non_missing_df = df[df['A'].notna()]

# 筛选出A列存在缺失值的行
missing_df = df[df['A'].isna()]  
```



##### pd.query

​		使用字符串表达式来查询和过滤DataFrame中的数据

```python
df.loc[(df['数量'] > 0) & (df['单价'] > 0)]
df.query('数量 >0 and 单价 > 0')


"""
注意事项
列名包含空格或特殊字符：如果列名包含空格或特殊字符，需要使用反引号()将列名括起来。例如，df.query('column name > 2')`。
查询字符串中的引号：在查询字符串中使用双引号或单引号时，需要注意转义字符。如果字符串本身包含引号，最好使用不同类型的引号来避免冲突。
性能：query函数在大多数情况下性能较好，但对于非常大的DataFrame，仍需注意性能问题。可以通过优化表达式或使用其他筛选方法来提升性能。
"""


import pandas as pd

# 创建一个示例DataFrame
df = pd.DataFrame({
    'A': [1, 2, 3, 4, 5],
    'B': [10, 20, 30, 40, 50],
    'C': ['foo', 'bar', 'foo', 'bar', 'foo']
})

# 使用query函数查询A列大于2的行
result = df.query('A > 2')

# 使用query函数查询A列大于2且C列等于'foo'的行
result = df.query('A > 2 & C == "foo"')

# 使用局部变量进行查询
threshold = 2
result = df.query('A > @threshold')

# 使用inplace参数直接修改原DataFrame
df.query('A > 2', inplace=True)



# where: 将不满足条件的值替换为 NaN
# mask: 将满足条件的值替换为 NaN


result_where = df.where(df['A'] > 2)
print(result_where)

result_mask = df.mask(df['A'] > 2)
print(result_mask)


"""
     A     B    C
0  NaN   NaN  NaN
1  NaN   NaN  NaN
2  3.0  30.0  foo
3  4.0  40.0  bar
4  5.0  50.0  foo
     A     B    C
0   1.0  10.0  foo
1   2.0  20.0  bar
2   NaN   NaN  NaN
3   NaN   NaN  NaN
4   NaN   NaN  NaN
"""
```













#### 排序操作

##### 	**sort_values**

​			用于对 Series 或 DataFrame 进行排序，可以按指定列或索引进行升序或降序排序

```python
# 创建一个示例DataFrame
df = pd.DataFrame({
    'A': [4, 1, 3, 2],
    'B': [10, 20, 30, 40]
})

# 对 DataFrame 的 'A' 列进行升序排序
sorted_df = df.sort_values(by='A')
sorted_df = df.sort_value(by='A' , ascending=False) # 从高到低
```

​	

##### 	**argsort** 

​			返回数据排序后的索引值，等效  sort_values().index()

```python
# 创建一个示例DataFrame
df = pd.DataFrame({
    'A': [4, 1, 3, 2],
    'B': [10, 20, 30, 40]
})

# 对列 'A' 进行argsort
sorted_indices = df['A'].argsort()

# 根据索引重新排列DataFrame
sorted_df = df.iloc[sorted_indices]
print(sorted_df)


"""
   A   B
1  1  20
3  2  40
2  3  30
0  4  10
"""
```





##### 	**nlargest**

​			从 Series 或 DataFrame 中选择前 n 个最大值

​			等效 sort_values().head()，但 nlargest 性能更优

```python
# 创建一个示例DataFrame
df = pd.DataFrame({
    'A': [4, 1, 3, 2],
    'B': [10, 20, 30, 40]
})

# 使用 nlargest 从 'A' 列中选择前2个最大值的行
top_n_df_nlargest = df.nlargest(2, 'A')
print("nlargest 结果：")
print(top_n_df_nlargest)

"""
nlargest 结果：
   A   B
0  4  10
2  3  30
"""


# 使用 sort_values() 排序并选择 'A' 列中前2个最大的值的行
top_n_df_sort_values = df.sort_values(by='A', ascending=False).head(2)
print("sort_values().head() 结果：")
print(top_n_df_sort_values)

"""
sort_values().head() 结果：
   A   B
0  4  10
2  3  30
"""


# 求每部电影的评分前五名
data.groupby('title').apply(lambda x: x.nlargest(5,'rating'))
# 等效
data.groupby('title').apply(lambda x: x.sort_values('age').head(5))


# 利用分组的平均值填充各自的NA值
fill_mean = lambda g:g.fillna(f.mean())
data.groupby(group_key).apply(fill_mean)
```



##### **nsmallest**

​		nsmallest 方法用于从 Series 或 DataFrame 中选择前 n 个最小值，与 nlargest 类似，但选择的是最小值

```python
# 选择前2个最小的值
bottom_n = data.nsmallest(2)
print(bottom_n)

"""
1    1
3    2
dtype: int64
"""
```



##### **idxmax / idxmin**

​		idxmax 和 idxmin 方法用于返回最大值和最小值的索引



##### **rank**

​		rank 方法用于对 Series 或 DataFrame 中的值进行排名



**参数列表**

​	axis：

​			• 0 或 'index'：按列排名（默认）

​			• 1 或 'columns'：按行排名



​	method：

​			•类型：{'average', 'min', 'max', 'first', 'dense'}

​			•默认值：'average'

​			•描述：定义相同值时的排名处理方式

​					 • **average**：平均排名

​							（例如，如果两个元素并列第2名，则它们的排名都是2.5 1 2.5 2.5 4）类似于SQL中RANK  

​					• **min**：最小排名

​							（例如，并列第2名的两个元素都取2，后续元素为4  1224）类似于SQL中RANK  

​					• **max**：最大排名

​							（例如，并列第2名的两个元素都取3，前面不存在2	1334）类似于SQL中RANK

​					• **first**：根据值出现的顺序分配排名  类似于 SQL 中的 ROW_NUMBER

​							（例如，先出现的取2，后出现的取3  1234）

​					• **dense**：类似于 'min'，但排名之间没有间隙 类似于SQL中 DENSE_RANK

​							（例如，并列第2名的两个元素都取2，下一个元素取3  1223）

​	

​		ascending：

​			•类型：bool

​			•默认值：True

​			•描述：是否升序排名，如果为 False，则降序排名





​		na_option：

​			•类型：{'keep', 'top', 'bottom'}

​			•默认值：'keep'•描述：如何处理 NaN 值

​					 •'keep'：将 NaN 保留为 NaN

​					 •'top'：将 NaN 排在最前面 值为 1

​					 •'bottom'：将 NaN 排在最后面 值为N（LENGTH）

​			









#### 分箱操作

​		分箱操作：将连续型数据离散化，分为等距（等宽）分箱和等频(等深)分箱

​			pd.cut()函数按照预先设定的边界值进行分箱 （常用）

​			pd.qcut()函数则是根据数据的分布情况，自动将数据分为大致相等数量（或比例）的箱。

```python
"""等距分箱（等宽分箱）：pd.cut()
用法：pd.cut(x, bins, right=True, labels=None, retbins=False, precision=3, include_lowest=False)

•参数解释：
•x：待分箱的一维数组或Series。
•bins：分箱的边界列表或数组。可以是整数（代表箱子的数量），也可以是表示边界值的数组。•right：（可选）如果是True（默认），则每个区间右边界的值属于该区间；如果是False，则属于下一个区间。
•labels：（可选）与pd.qcut()相同。
•retbins：（可选）与pd.qcut()相同。
•precision：（可选）与pd.qcut()相同。
•include_lowest：（可选）如果为True，则包含区间下限。
"""

# 实例讲解：
import pandas as pd
import numpy as np

# 创建一个随机数序列
np.random.seed(0)
data = np.random.randn(100)
bins = [-2, -1, 0, 1, 2]  # 设定分箱边界

# 等距分箱
equal_width_bins = pd.cut(data, bins=bins)

# 输出分箱结果
print(equal_width_bins)




"""
等频分箱（等深分箱）：pd.qcut()
用法：pd.qcut(x, q, labels=None, retbins=False, precision=3, duplicates='raise')

•参数解释：
•x：待分箱的一维数组或Series。
•q：分箱的数量或者是每个箱子所包含的数据比例。例如，如果q=[0, 0.25, 0.5, 0.75, 1.0]，则表示将数据分为四个箱，每个箱包含25%的数据。
•labels：（可选）为每个箱子分配标签的列表或布尔值。如果为True，则使用界限作为标签。•retbins：（可选）如果为True，则返回分箱的界限数组以及分箱后的结果。
•precision：（可选）浮点数舍入精度。
•duplicates：（可选）处理边界重复值的方式，'raise'表示遇到重复边界时引发错误，'drop'则忽略重复的边界。
"""

# 实例讲解：
import pandas as pd
import numpy as np

# 创建一个随机数序列
np.random.seed(0)
data = np.random.randn(100)

# 等频分箱，将数据分成4个箱，每个箱包含25%的数据
quantiles = pd.qcut(data, 4)

# 输出分箱结果
print(quantiles)

```









```python
# 等宽分箱
# x: （首个参数，可省略名称），分箱数据
# bins:  分箱个数，可指定区间范围


s = pd.cut(df.Python,bins=8)
s  

# 行索引对应值归宿的分箱区域
0      (75.25, 86.125]
1      (64.375, 75.25]
2       (53.5, 64.375]
3       (53.5, 64.375]
4       (53.5, 64.375]
            ...       
125     (86.125, 97.0]
126    (9.913, 20.875]
127    (9.913, 20.875]
128     (86.125, 97.0]
129     (86.125, 97.0]
Name: Python, Length: 130, dtype: category
# 分箱区间
Categories (8, interval[float64, right]): [(9.913, 20.875] < (20.875, 31.75] < (31.75, 42.625] < (42.625, 53.5] < (53.5, 64.375] < (64.375, 75.25] < (75.25, 86.125] < (86.125, 97.0]] 




s.value_counts()  # 查看每个分箱中的数据个数

(53.5, 64.375]     25
(86.125, 97.0]     24
(42.625, 53.5]     17
(9.913, 20.875]    16
(20.875, 31.75]    14
(75.25, 86.125]    14
(31.75, 42.625]    10
(64.375, 75.25]    10
Name: Python, dtype: int64



t = pd.cut(
    df.Python,                      # 分箱数据
    bins = [0,30,60,90,120],        # 分箱断点
    right = False,                  # 左闭右开 ，默认为左开右闭
    labels = ['C','B','A','S']  # 分箱后的分类标签
)



0      A
1      A
2      A
3      B
4      A
      ..
125    S
126    C
127    C
128    S
129    A
Name: Python, Length: 130, dtype: category
Categories (4, object): ['C' < 'B' < 'A' < 'S']




t.value_counts()

(30, 60]     45
(60, 90]     41
(0, 30]      26
(90, 120]    18
Name: Python, dtype: int64



# 等频分箱
t = pd.qcut(
    df.Python,                      # 分箱数据
    q = 4,   # 分箱个数
    labels = ['C','B','A','S']  # 分箱后的分类标签
)

0      S
1      A
2      A
3      B
4      A
      ..
125    S
126    C
127    C
128    S
129    S
Name: Python, Length: 130, dtype: category
Categories (4, object): ['C' < 'B' < 'A' < 'S']
```





#### 时间序列



```python
# 创建时间戳，Timestamp 时间戳
pd.Timestamp("2024-02-18")  # 时刻数据
# Period('2024-02-18', 'D')



# 时期数据,freq 默认为 D（天），M：月，Y：年
pd.Period("2024-02-18",freq='D')
# Period('2024-02-18', 'D')

pd.Period("2024-02-18",freq='M')
# Period('2024-02', 'M')

pd.Period("2024-02-18",freq='Y') 
# Period('2024', 'A-DEC')




# 批量生成时刻数据
# period 表示需要创建时间值的数量
# freq = 'D' 表示按天作为周期（间隔，M则取每月的最后一天，Y则取每年最后一天
index = pd.date_range("2030.02.13",periods=4,freq = 'D')

# DatetimeIndex(['2030-02-13', '2030-02-14', '2030-02-15', '2030-02-16'], dtype='datetime64[ns]', freq='D')



# 批量生成时期数据
index = pd.period_range("2030.02.13",periods=4,freq = 'D')
# 将对应时间戳作为行索引
pd.Series(np.random.randint(0,10,size=4),index=index)

2030-02-13    3
2030-02-14    7
2030-02-15    4
2030-02-16    1
Freq: D, dtype: int64
    
    
    
    
# 时间格式的转换
pd.to_datetime(['2023.03.14','2030-03-14','2030-3-14','14/3/2030','2030/3/14'])    

DatetimeIndex(['2023-03-14', '2030-03-14', '2030-03-14', '2030-03-14',
               '2030-03-14'],
              dtype='datetime64[ns]', freq=None)




# 时间戳转换为时间，unit='s' 指定单位为 秒
s = pd.to_datetime([1899678987],unit='s')

DatetimeIndex(['2030-03-14 00:36:27'], dtype='datetime64[ns]', freq=None)



# 时间差 DataOffset
f = s + pd.DateOffset(hours = 8)
d = s + pd.DateOffset(days = 8)
j = s - pd.DateOffset(days = 8) 
# s + pd.DateOffset(days = -8) 二者等效

display(s,f,d,j)


DatetimeIndex(['2030-03-14 00:36:27'], dtype='datetime64[ns]', freq=None)
DatetimeIndex(['2030-03-14 08:36:27'], dtype='datetime64[ns]', freq=None)
DatetimeIndex(['2030-03-22 00:36:27'], dtype='datetime64[ns]', freq=None)
DatetimeIndex(['2030-03-06 00:36:27'], dtype='datetime64[ns]', freq=None)




index = pd.date_range('2023-03-14',periods=100,freq='D')
ts = pd.Series(range(len(index)),index=index)

2023-03-14     0
2023-03-15     1
2023-03-16     2
2023-03-17     3
2023-03-18     4
              ..
2023-06-17    95
2023-06-18    96
2023-06-19    97
2023-06-20    98
2023-06-21    99
Freq: D, Length: 100, dtype: int64
      
      
# 时间索引
ts['2023-03-14']  
ts['2023-03']  # 该月所有数据
ts['2023-3']   # 该月所有数据
ts['2023']     # 该年所有数据


# 时间切片
ts['2023-03-14':'2023-03-19']  

2023-03-14    0
2023-03-15    1
2023-03-16    2
2023-03-17    3
2023-03-18    4
2023-03-19    5
Freq: D, dtype: int64
    
    

# 时间戳索引
ts[pd.Timestamp('2023-03-18')]

# 时间戳切片
ts[pd.Timestamp('2023-03-18'):pd.Timestamp('2023-03-28')]

# date范围取值
ts[pd.date_range('2023-03-18',periods=10,freq='D')]    



2023-03-18     4
2023-03-19     5
2023-03-20     6
....
2023-03-26    12
2023-03-27    13
Freq: D, dtype: int64
    
    
    
    
ts.index       			# 取到所有的行索引值
ts.index.year   		# 取到所有行索引的年份信息
ts.index.month  		# 取到所有行索引的月份信息
ts.index.day    		# 取到所有行索引的日信息
ts.index.dayofweek  # 取到星期数    



index = pd.date_range('2024-03-29',periods=365,freq='D')
ts = pd.Series(np.random.randint(0,100,len(index)),index = index)



# 时间移动
ts.shift()   # 默认移动一位

2024-03-29     NaN
2024-03-30    13.0
              ... 
2025-03-24    61.0
2025-03-28    33.0
Freq: D, Length: 365, dtype: float64
      
# periods/期：后/前移的位数       
ts.shift(periods=2)   
ts.shift(periods=-2)        
      
  
  
  
  
# 频率转换 offsets/偏移量
# 数据量由多变少
W = ts.asfreq(pd.tseries.offsets.Week())  # 天 -> 星期 
ME = ts.asfreq(pd.tseries.offsets.MonthEnd())  # 天 -> 月末 
MB = ts.asfreq(pd.tseries.offsets.MonthBegin())  # 天 -> 月初 

display(W,ME,MB)

2023-03-14     0
2023-03-21     7
...
2023-06-13    91
2023-06-20    98
Freq: W, dtype: int64
    
    
2023-03-31    17
2023-04-30    47
2023-05-31    78
Freq: M, dtype: int64
    
    
2023-03-31    17
2023-04-30    47
2023-05-31    78
Freq: M, dtype: int64    
    
    
# 天 -> 小时，数据量由少变多，存在NaN，使用 fill_value 函数进行填充
ts.asfreq(pd.tseries.offsets.Hour(),fill_value=0) 

2023-03-14 00:00:00     0
2023-03-14 01:00:00     0
                       ..
2023-06-20 20:00:00     0
2023-06-21 00:00:00    99
Freq: H, Length: 2377, dtype: int64
      
      
      
      
# resample 重采样，单位可以为 S,T,H,D,W,M,Y（秒/分/时/天/周/月/年
# 后接聚合函数
ts.resample('D').sum()   # 以一天为单位进行汇总
ts.resample('2D').sum()   # 以两天为单位进行汇总
ts.resample('W').sum()   # 以一周为单位进行汇总
ts.resample('M').sum()   # 以一月为单位进行汇总
ts.resample('3M').sum()   # 以季度为单位进行汇总    



# DataFrame 重采样
d = {
    'price':[10,2,21,34,76,43,24,39],
    'score':[10,20,30,40,50,60,70,80],
    'week':pd.date_range('2024-01-01',periods=8,freq='W')
}

df = pd.DataFrame(d)

	price	score	week
0	10	10	2024-01-07
1	20	20	2024-01-14
2	21	30	2024-01-21
3	34	40	2024-01-28
4	76	50	2024-02-04
5	43	60	2024-02-11
6	24	70	2024-02-18
7	39	80	2024-02-25

# 对Week列按月汇总求和
df.resample('M',on='week').sum()
df.resample('M',on='week').apply(np.sum)   # 等效


					price	score
week		
2024-01-31	67	100
2024-02-29	182	260


# 对week列按月进行汇总，对 price 求平均值，对 score 求和
df.resample('M',on='week').agg({'price':np.mean,'score':np.sum})


						price	score
week		
2024-01-31	16.75	100
2024-02-29	45.50	260

```





#### 数据探查

```python
# 用于查看 Data Fram 的前几行
print(df.head(3))


# 用于查看 Data Fram 的后几行
print(df.tail(3))


# 用于查看 Data Fram 的简略信息
print(df.info())


# 用于生成描述性统计信息 （ count / mean / std / min / max / 25/50/75 ）
print(df.describe())


# 查看数据形状（ 几行几列 ）
print(df.shape())



# 随机查看数据（ 默认一条 n=1，数量百分比 frac = 0.001 ）
print(df.sample())
```







#### 索引操作



##### set_index    

用于将 DataFrame 的某一列/多列设置为索引

```python
# 函数签名
DataFrame.set_index(keys, drop=True, append=False, inplace=False, verify_integrity=False)
```

###### **keys**:

​		**类型**: 单列标签、列标签的列表、数组、Series、Index，或多层索引的标签。

​		**作用**: 指定要设置为索引的列名或其他数据结构。

​		**说明**: 可以使用 DataFrame 中的现有列作为索引，或者使用传入的 Series/Index，也可以指定多个列以创建多级索引。

​	

###### **drop**:

​		**类型**: 布尔值 (True 或 False)

​		**默认值**: True

​		**作用**: 是否从 DataFrame 中删除现将用于设置成索引的列。

​		**说明**: 

​				如果为 True，那么现将用作索引的列会从 DataFrame 中移除；

​				如果为 False，这些列将保留在 DataFrame 中

​	

###### **append**:

​		**类型**: 布尔值 (True 或 False)

​		**默认值**: False

​		**作用**: 是否将新的索引附加到现有索引上（从而创建一个多级索引）。

​		**说明**: 如果为 True，则新的索引会被附加到现有的行索引中。如果为 False，将替换现有的索引。

​	

###### **inplace**:

​		**类型**: 布尔值 (True 或 False)

​		**默认值**: False

​		**作用**: 是否直接修改原 DataFrame，而不是返回一个新的 DataFrame。

​		**说明**: 如果为 True，原 DataFrame 会被修改，并且 set_index 不返回任何值；如果为 False，将返回一个新的 DataFrame。

​	

**verify_integrity**:

​		**类型**: 布尔值 (True 或 False)

​		**默认值**: False

​		**作用**: 检查新的索引是否有重复值，并在发现重复值时引发错误。

​		**说明**: 如果为 True，在设置新索引时会检查其唯一性。如果索引中有重复值，会引发 ValueError。







##### reset_index 

用于重置索引，将索引重置为默认的整数索引

##### rename 	 

 用于重命名索引和列的标签

##### rename_axis  

用于修改索引和列的名称

##### set_names	

 用于设置索引或列的名称，可以在行索引和列索引上使用

```python
# 重置为数值类型索引
df_reset = df.reset_index()

# 设置 'A' 列为索引，默认删除 A 列
df_set = df.set_index('A',drop=True)

# 重命名指定索引和列
df_renamed = df.rename(index={'a': 'x'}, columns={'B': 'Beta'})
```











#### 数据透视

##### 	pivot / pivot_table

​		pivot / pivot_table :  根据指定的列，将行数据转为列数据。

​		pivot 要求生成的 DataFrame 不能有重复的索引。

​		如果有重复索引，需使用 pivot_table 函数，pivot_table 支持聚合功能

​		pivot 只是单纯的整理/变形 原始数据，并不进行计算



​		pivot_table 可以指定分组的字段为行索引或列索引，而不是只能作为多级索引	



###### **参数说明**

​	**pivot_table**

```python
pandas.pivot_table(
data, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=False, sort=True)

"""
参数说明
	•	data: DataFrame，必选参数。用于创建透视表的数据源。
	•	values: 列标签或标签列表，可选。要聚合的列。
	•	index: 列标签或标签列表，可选。透视表的行索引。
	•	columns: 列标签或标签列表，可选。透视表的列索引。
	•	aggfunc: 函数或函数列表，可选。用于聚合的函数，默认是 mean。
	•	fill_value: 标量，可选。用于填充缺失值。
	•	margins: 布尔值，可选。添加行/列的总计，默认是 False。
	•	dropna: 布尔值，可选。不要包含所有条目均为 NaN 的列，默认是 True。
	•	margins_name: 字符串，可选。用于总计行/列的名称，默认是 'All'。
	•	observed: 布尔值，可选。当分类数据被聚合时，仅显示观察到的值。
	•	sort: 布尔值，可选。是否对结果进行排序。
"""


```



###### 具体示例

```python
import pandas as pd

data = {
    'Date': ['2021-01-01', '2021-01-01', '2021-01-02', '2021-01-02', '2021-01-03'],
    'Store': ['Store A', 'Store B', 'Store A', 'Store B', 'Store A'],
    'Product': ['Apples', 'Apples', 'Bananas', 'Bananas', 'Apples'],
    'Sales': [10, 20, 15, 25, 10]
}

df = pd.DataFrame(data)

# 创建透视表
# 根据 Store 与 Date 分组求得 Sales 列的SUM值
pivot = pd.pivot_table(df, values='Sales', index='Date', columns='Store', aggfunc='sum')
"""
Store         Store A  Store B
Date                          
2021-01-01       10.0     20.0
2021-01-02       15.0     25.0
2021-01-03       10.0      NaN
"""


# 使用多个聚合函数
pivot = pd.pivot_table(df, values='Sales', index='Date', columns='Store', aggfunc=[sum, len])
"""
          sum           len      
Store  Store A Store B Store A Store B
Date                                   
2021-01-01   10.0    20.0      1      1
2021-01-02   15.0    25.0      1      1
2021-01-03   10.0     NaN      1      0
"""


# 填充缺失值
pivot = pd.pivot_table(df, values='Sales', index='Date', columns='Store', aggfunc='sum', fill_value=0)
"""
Store         Store A  Store B
Date                          
2021-01-01       10.0     20.0
2021-01-02       15.0     25.0
2021-01-03       10.0      0.0
"""



# 添加总计行和列
pivot = pd.pivot_table(df, values='Sales', index='Date', columns='Store', aggfunc='sum', margins=True)
"""
Store         Store A  Store B  All
Date                               
2021-01-01       10.0     20.0  30.0
2021-01-02       15.0     25.0  40.0
2021-01-03       10.0      NaN  10.0
All              35.0     45.0  80.0
"""



# 多级索引和列
pivot = pd.pivot_table(df, values='Sales', index=['Date', 'Store'], columns='Product', aggfunc='sum')
"""
Product             Apples  Bananas
Date       Store                    
2021-01-01 Store A    10.0      NaN
           Store B    20.0      NaN
2021-01-02 Store A     NaN     15.0
           Store B     NaN     25.0
2021-01-03 Store A    10.0      NaN
"""
```





##### **melt**

​	melt 函数与 pivot 相反，它将“宽格式”数据转换为“长格式”数据，相当于把 N 列的数据拼接成两列（列名以及对应的数据 variable	value）。

​	典型应用场景是将多列的值集中到一列，方便进一步分析。



###### 参数说明

```python
pandas.melt(frame, id_vars=None, value_vars=None, var_name=None, value_name=None)
```



​	•	**frame**：需要转换的 DataFrame。

​	•	**id_vars**：保持不变的列，即标识变量。

​	•	**value_vars**：需要进行熔融操作的列。如果没有指定，默认为非 id_vars 的所有列。

​	•	**var_name**：熔融后生成的新列名称，默认是 variable。

​	•	**value_name**：熔融后生成的值的列名称，默认是 value。



 

##### 具体示例

```python
import pandas as pd

data = {
    'Date': ['2023-01-01', '2023-01-01', '2023-01-02', '2023-01-02'],
    'City': ['New York', 'Los Angeles', 'New York', 'Los Angeles'],
    'Temperature': [30, 75, 32, 76]
}
df = pd.DataFrame(data)

"""
Date	City	Temperature
0	2023-01-01	New York	30
1	2023-01-01	Los Angeles	75
2	2023-01-02	New York	32
3	2023-01-02	Los Angeles	76
"""

pd.melt(df,id_vars=['Date'],value_vars=['City','Temperature'])
"""
Date	variable	value
0	2023-01-01	City	New York
1	2023-01-01	City	Los Angeles
2	2023-01-02	City	New York
3	2023-01-02	City	Los Angeles
4	2023-01-01	Temperature	30
5	2023-01-01	Temperature	75
6	2023-01-02	Temperature	32
7	2023-01-02	Temperature	76
"""

pd.melt(df,id_vars=['Date'],value_vars=['Temperature'])
"""	
Date	variable	value
0	2023-01-01	Temperature	30
1	2023-01-01	Temperature	75
2	2023-01-02	Temperature	32
3	2023-01-02	Temperature	76
"""


import pandas as pd
import xlwt

# 读取Excel文件
df = pd.read_excel('/Users/buzhi/Documents/新建 XLS 工作表.xls',sheet_name='Sheet1')
# 去除全为空的行/列
df1 = df.dropna(axis=1,how='all').dropna(axis=0,how='all')   
# 获取所有列索引
course_columns = df1.columns.tolist()  
# 从列表中去除 开课教研室
course_columns.remove('开课教研室')      


df_melted = df1.melt(id_vars=['开课教研室'], value_vars=course_columns,var_name='开课教师',value_name='授课教师')

df_melted_01 = df_melted[['开课教研室','授课教师']].drop_duplicates()
df_melted_02 = df_melted_01[ ~df_melted_01['授课教师'].isnull() ].sort_values(by='开课教研室')
# df_melted_02.to_excel('/Users/buzhi/Documents/开课教研室-授课教师-demo.xls',sheet_name='Sheet1',header=True,index=False)
```













#### to_datetime

##### 函数签名

将各种类型的输入数据转换为 Pandas 的 Timestamp 或 DatetimeIndex 对象的函数

```python
pd.to_datetime(arg, errors='raise', dayfirst=False, yearfirst=False, utc=None, box=True,format=None, exact=True, unit=None, infer_datetime_format=False, origin='unix', cache=False)
```



##### 参数说明

•arg: 输入数据，可以是以下类型之一：

​	•单个值：如字符串、整数、浮点数或 numpy.datetime64 对象。

​	•序列：如列表、数组、Series 或 DataFrame 的一列。

​	•字典：键为索引，值为待转换的时间数据。

​	•其他：如 Period、Timedelta 或 Timestamp 对象。



•errors: 如何处理转换错误，默认为 'raise'。

​	可选值包括：

​		•'raise': 遇到无法转换的值时抛出异常。

​		•'coerce': 将无法转换的值设置为 NaT（Not-a-Time）。

​		•'ignore': 保留无法转换的值不变。



•dayfirst: 是否将日期中的第一位视为日，默认为 False。若为 True，则按照 DD/MM/YYYY 的顺序解析日期，否则默认为 MM/DD/YYYY。



•yearfirst: 是否将日期中的第一位视为年，默认为 False。若为 True，则按照 YYYY/MM/DD 或 YYYY/DD/MM（取决于 dayfirst 设置）的顺序解析日期。



•utc: 是否将解析出的日期时间视为 UTC 时间，默认为 None。若为 True，则将所有日期时间转换为 UTC；若为 False，则视为本地时间。



•box: 控制返回类型，默认为 True。若为 True，返回 Series 或 Index；若为 False，返回 Timestamp 对象的 NumPy 数组。



•format: 如果 arg 是字符串，可以指定日期时间的格式字符串，如 '%Y-%m-%d %H:%M:%S'。当数据有固定格式时，使用此参数可以提高转换速度。



•exact: 是否严格遵照 format 参数指定的格式进行解析。默认为 True，表示必须完全匹配；设为 False 可接受不完全匹配。



•unit: 当 arg 是整数或浮点数时，指定时间单位（如 's'、'ms'、'us' 等）。



•infer_datetime_format: 是否尝试自动检测 arg 中日期时间的格式。若为 True，可能提高解析速度，但可能降低准确性。默认为 False。



•origin: 当 arg 是整数时，指定时间起点。默认为 'unix'，即 1970-01-01 00:00:00（Unix 时间戳的起始时间）。



•cache: 是否缓存 arg 的格式以加速后续调用。默认为 False。































































### 综合实战



**需求：**找出两个Excel表格中存在不同数据的行

```python
# 导入所需库
import pandas as pd

# 假设你有两个Excel文件路径
file1_path = '/Users/buzhi/Desktop/测试01.xlsx'
file2_path = '/Users/buzhi/Desktop/测试02.xlsx'

# 读取Excel文件到DataFrame
df1 = pd.read_excel(file1_path)
df2 = pd.read_excel(file2_path)

# 确保需比较的列名相同，并顺序一致
# 如果不一致，需要调整列顺序或者明确指定列名
# df1 = df1[['column1', 'column2', ...]]
# df2 = df2[['column1', 'column2', ...]]

# 比较两个DataFrame的不同数据

# 这意味着经过isin()操作后的DataFrame，每一行每一个元素都会告诉我们它在df2中是否存在。

# 不同数据在df1中但不在df2中
diff_in_df1_not_in_df2 = df1[df1.isin(df2).all(axis=1) == False]

# 不同数据在df2中但不在df1中
diff_in_df2_not_in_df1 = df2[df2.isin(df1).all(axis=1) == False]


diff_in_df1_not_in_df2.to_excel('/Users/buzhi/Desktop/diff_df1.xlsx', index=False)
diff_in_df2_not_in_df1.to_excel('/Users/buzhi/Desktop/diff_df2.xlsx', index=False)


# 或者，如果你想找出所有不匹配的行（包括部分列不同的行）
# both_diffs = pd.concat([df1.merge(df2, how='outer', indicator=True)['_merge'].loc[lambda x: x == 'left_only'],
#                        df1.merge(df2, how='outer', indicator=True)['_merge'].loc[lambda x: x == 'right_only']])

# both_diffs.to_excel('both_diffs.xlsx', index=False)

```



**具体解释：** isin()   

```python
# isin()函数会逐元素比较df1和df2的数据。
# 返回一个新的DataFrame，其中每个元素为True表示该元素在df2中存在，False则表示不存在。
df1.isin(df2)  

	专业	姓名	科目	成绩
0	True	True	True	False
1	True	True	True	True
2	True	True	True	True

# all(axis=1)：接着，我们对isin()函数返回的DataFrame沿行方向（ axis=1 ）应用all()函数，这一步骤会检查每一行的所有元素是否都为True
# 如果某一行的所有元素都在df2中存在（ 即isin()返回的都是True ），那么all()函数对该行返回True；
# 只要一行中有一个或多个元素不在df2中（ isin()返回False ），all()函数就返回False
# 将结果与False进行比较，选出那些all()函数返回False的行，也就是说，选出的是那些至少有一个元素不在df2中的行，也就是df1独有的行。
df1.isin(df2).all(axis=1) == False

0     True
1    False
2    False
dtype: bool


diff_in_df1_not_in_df2

    专业	姓名	科目	成绩
0	软件工程	叶林生	数据库	88
```









**需求：**传入两个列表【行/列索引值】，返回对应的 DataFrame 结构数据

```python
import pandas as pd
import numpy as np

def make_df(indexs,columns):
    data = [[str(j)+str(i) for j in columns] for i in indexs]
    df = pd.DataFrame(data = data, index = indexs, columns= columns)
    return df
    
    
make_df([1,2,3,4],list('ABCD'))  # list会将字符串自动切分字符

	A		B		C		D
1	A1	B1	C1	D1
2	A2	B2	C2	D2
3	A3	B3	C3	D3
4	A4	B4	C4	D4

```





项目描述：

​	收集并整理一组网店销售数据，进行初步的数据分析，并准备生成一份销售报告。

​	这个项目会用到CSV文件的读写，以及Pandas的各种常用函数。

```python
# 1. 读取数据：•从CSV文件读取数据，假设有两个CSV文件，一个是商品销售记录（sales.csv），另一个是商品信息表（products.csv）。
import pandas as pd

# 读取销售记录
sales_data = pd.read_csv('sales.csv')

# 读取商品信息
product_info = pd.read_csv('products.csv')

# 2. 数据清洗：
# 检查并处理缺失值，假设销售记录中可能存在一些订单ID缺失的情况
# dropna 去除存在空值(np.nan)的行
sales_data = sales_data.dropna(subset=['order_id'])  

# 根据商品ID对商品信息表进行清洗，去除无效的商品ID
# 使用布尔值过滤掉  product_info 中 product_id 为空的行
product_info = product_info[product_info['product_id'].notnull()]

# 3. 数据处理：
# 对销售记录进行分组，计算每种商品的总销售额。
group_sales = sales_data.groupby('product_id')['sale_amount'].sum().reset_index(name='total_sales')  # name 将原本索引设为一列，列名为name的值

# 通过商品ID将销售记录与商品信息表合并。
merged_data = pd.merge(group_sales, product_info, on='product_id')


# 4. 数据操作：•创建一个新的列，计算每种商品的平均售价（总销售额除以销量）。
 merged_data['average_price'] = merged_data['total_sales'] / merged_data['quantity_sold']


# 5. 数据排序和筛选：•按照销售额从高到低排序，并筛选出销售额最高的前10种商品。
top_sales = merged_data.sort_values(by='total_sales', ascending=False).head(10)

# 6. 数据导出：•将处理后的数据导出为一个新的CSV文件，以便后续分析或报告制作。
top_sales.to_csv('top_sales_report.csv', index=False)

```







**需求：**去除（删除/过滤）存在 任一元素绝对值大于三倍标准差 的行

```python
# randn 创建一个服从标准正态分布的多维数组
df = pd.DataFrame(np.random.randn(10000,3))

# 求三倍标准差 
df.std()*3


# 绝对值 
df.abs()

# 判断每一个元素是否大于三倍标准差
cond = df.abs() > df.std()*3

cond.head()

	0			1			2
0	False	False	False
1	False	False	False
2	False	False	False
3	False	False	False
4	False	False	False


# 找出存在元素大于三倍标准差的行
cond2 = cond.any(axis=1)

# 使用bool索引过滤异常值 ~cond2
cond[cond2]
```







###### 实战一

```python
import pandas as pd
import numpy as np

url = 'https://raw.githubusercontent.com/justmarkham/DAT8/master/data/chipotle.tsv'
    
# 获取 CSV 格式数据，分隔符为 tab  
chipo = pd.read_csv(url, sep = '\t')  


	order_id	quantity/数量		item_name/物品名		choice_description/描述	item_price/物品价格
0		1					1								Chips  							NaN						$2.39
1		1					1								Izze							[Clementine]		$3.39


# 查看数据的前十行
chipo.head(10)

# 查看第一个维度/行 数量
chipo.shape[0]

# 查看第二个维度/列 数量
chipo.shape[1]

# 查看数据情况
chipo.info()

# 返回列索引的详情（值/数据类型）
chipo.columns
# 返回行索引的详情（值/数据类型）
chipo.index


# 一共订了多少件商品
chipo['quantity'].sum()
chipo.quantity.sum()


# 检查物品价格类型
chipo.item_price.dtype
# dtype('O')

"""
在Pandas中，dtype('O') 或 dtype=object 表示的数据类型是一种通用的对象类型，它可以容纳任何Python对象，包括但不限于字符串、数字、布尔值、None以及其他自定义对象等。
在Pandas的Series或DataFrame中，当列中的元素类型不一致或者包含非数值类型时，Pandas会将这类列的数据类型标记为object。
"""

# 修改物品价格类型
# x[1:-1] 用于去除价格前的 $符号，并转换为 float 类型  等效：x[1:] 
dollarizer = lambda x: float(x[1:-1])  

# apply 使得 item_price 中的每一个行都经过 dollarizer 函数处理
chipo.item_price = chipo.item_price.apply(dollarizer) 

chipo.item_price.dtype
# dtype('float64')





# 数据集中这段时间的总收入
revenue = (chipo['quantity']* chipo['item_price']).sum()
print('Revenue was: $' + str(np.round(revenue,2)))




# 此期间有多少订单（可能有多条记录属于同一笔订单
# 等效于  chipo.order_id.nunique()
orders = chipo.order_id.value_counts().count()   


# value_counts()：统计元素出现的次数
# count()：统计行数   
chipo.order_id.value_counts()

926     23
1483    14
        ..
94       1
1199     1





# 订购最多的商品是什么?
# 对于订购最多的商品，订购了多少商品?

# 根据 商品名 进行分组 -》 分组求和 -》 根据 quantity 列的SUM值进行倒序排序 -》 取top
chipo.groupby('item_name')
		 .sum().sort_values('quantity',ascending=False)
  	 .head(1)


						order_id	quantity
item_name		
Chicken Bowl	713926		761





# 每笔订单的平均价格

# 数量 * 价格 = 每行数据总收入
chipo['revenue'] = chipo['quantity'] * chipo['item_price']
# 求得每笔订单的总收入
order_grouped = chipo.groupby(by=['order_id']).sum()
# 求所有订单的平均价格
order_grouped.mean()['revenue']



# 有多少产品的价格超过10美元?
# item_price(物品价格) 去除 item_price 字段数据中的 $ 符
chipo.item_price = [float(value[1 : -1]) for value in chipo.item_price]

# 删除 item_name（产品名称），quantity （数量），choice_description（详细介绍）字段重复的数据
chipo_filtered = chipo.drop_duplicates(['item_name','quantity','choice_description'])

# 单价超过10美元的产品总数
chipo_filtered[(chipo_filtered['item_price'] / chipo_filtered['quantity']) > 10].item_name.nunique()

# 单价超过10美元的产品名称 与 详细介绍
chipo_filtered[(chipo_filtered['item_price'] / chipo_filtered['quantity']) > 10][['item_name','choice_description']].drop_duplicates()

# 通过列索引查询指定多列索引时，需通过列表的形式查询 即：[['column1','column2','column3']]

# nunique: 统计去重之后的数据量
# drop_duplicates: 删除重复的数据

# 不知需求
chipo[(chipo['item_name'] == 'Chicken Bowl') & (chipo['quantity'] == 1)]


# 求的每件商品的价格
chipo['flag'] = chipo.item_price / chipo.quantity
chipo[['item_name','flag']].drop_duplicates().sort_values('flag',ascending=False)


# 有多少人点了不止一份汽水
len(chipo[(chipo.item_name == "Canned Soda") & (chipo.quantity > 1)])

```











###### 实战二



```python
import pandas as pd


			age	gender	occupation	zip_code
user_id				
		1	24		M			technician	85711
		2	53		F			other				94043
		3	23		M			writer			32067
		4	24		M			technician	43537
		5	33		F			other				15213
		6	42		M			executive		98101


# sep 指定分隔符，index_col 指定行索引
users = pd.read_csv('https://raw.githubusercontent.com/justmarkham/DAT8/master/data/u.user', sep='|', index_col='user_id')


# 查看前十行，后十行数据
users.head(25)
users.tail(10)
# 查看数据的行列数（一维二维结构
users.shape[0]
users.shape[1]
# 查看行/列索引情况
users.columns
users.index
# 查看每列的数据类型
users.dtypes


# 统计职业的数量
users['occupation'].nunique()
21


users.occupation.value_counts()
student          196
other            105
....
marketing         26
entertainment     18


users.occupation.value_counts().count()
21



# 数量最多/最常见的职业 value_counts:默认排序输出 index[0]:取第一行数据的行索引值
users.occupation.value_counts().index[0]
users.occupation.value_counts().iloc[0]
users.occupation.value_counts().max()



# Notice: By default, only the numeric columns are returned.
# 默认情况下只返回数值类型列的分析数据
users.describe(include = "all")
# 只输出指定列的分析数据
users.occupation.describe()


# 输出平均年纪
round(users.age.mean())


# 输出最小的年龄以及数量
users.age.value_counts().tail()

# 取到倒数第三列的数据
users.iloc[: , :-3]
```



再多介绍一个python的标准库，比如string, json,sys之类的



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

```python
""" 
movies.csv 的数据集，包含以下字段：
•title: 电影标题
•year: 上映年份
•genre: 电影类型（可能包含多个，用逗号分隔）
•rating: 评分（0.0-10.0）
•votes: 投票数
•runtime: 电影时长（分钟）
•revenue: 票房收入（美元）
"""


# 1. 导入所需库并读取数据
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('movies.csv')


# 2. 数据预览与基础统计
# 查看前5行数据
print(df.head())

# 查看数据基本信息
print(df.info())

# 计算描述性统计
print(df.describe(include='all'))


# 3. 数据清洗与处理

# 将 `genre` 列的多个类型拆分为单独行
genres = df['genre'].str.split(', ', expand=True) 
# str.split(', ', expand=True)：使用str的split方法，指定切割符号为 ,   
# expand=True： 将原本返回的 Series（每个元素为一个由分割结果组成的列表）展开为一个新的 DataFrame，其中每个子字符串成为一个新的列。


genres = genres.stack().reset_index(level=1, drop=True).rename('genre')
df = df.drop('genre', axis=1).join(genres)
# stack：将列索引转换为行索引
# reset_index：重置索引  (level=1, drop=True) 重制第一层索引，不保存原始索引
# drop：删除指定索引 
# join：根据 DataFrame 的索引（index）来对 DataFrame 进行合并



# 将 `year` 列转换为整数类型
df['year'] = df['year'].astype(int)

# 处理缺失值：填充票房收入的中位数，删除其他列的缺失值
df['revenue'].fillna(df['revenue'].median(), inplace=True)
df.dropna(inplace=True)
# fillna：空值替换
# dropna：删除存在 NAN 的行


# 4. 数据分析与探索

# 年份分布
sns.histplot(df['year'], bins=range(19.jpg, 2023), discrete=True)
plt.title('上映年份分布')
plt.show()

# 评分分布
sns.displot(df['rating'], kde=True)
plt.title('评分分布')
plt.show()

# 电影类型频数统计
genre_counts = df['genre'].value_counts()
genre_counts.plot(kind='bar', figsize=(10, 5))
plt.title('各类型电影数量')
plt.xlabel('电影类型')
plt.ylabel('数量')
plt.show()

# 评分与投票数的相关性
sns.scatterplot(x='rating', y='votes', data=df)
plt.title('评分与投票数的关系')
plt.xlabel('评分')
plt.ylabel('投票数')
plt.show()

# 平均评分按类型分组
grouped_ratings = df.groupby('genre')['rating'].mean()
grouped_ratings.plot(kind='bar', figsize=(10, 5))
plt.title('各类型电影平均评分')
plt.xlabel('电影类型')
plt.ylabel('平均评分')
plt.show()

# 票房收入箱线图
sns.boxplot(y='revenue', x='genre', data=df)
plt.title('各类型电影票房收入分布')
plt.xlabel('电影类型')
plt.ylabel('票房收入（美元）')
plt.show()

# 票房收入与评分的关系
sns.lmplot(x='rating', y='revenue', data=df, hue='genre', height=8, aspect=1.5)
plt.title('票房收入与评分的关系（按类型区分）')
plt.show()
5. 特征工程# 将票房收入对数化
df['log_revenue'] = np.log(df['revenue'])

# 创建时长类别：短片（<=60分钟）、中片（61-120分钟）、长片（>120分钟）
df['runtime_category'] = pd.cut(df['runtime'], bins=[0, 60, 120, np.inf], labels=['Short', 'Medium', 'Long'])
# 6. 数据导出
df.to_csv('processed_movies.csv', index=False)



"""
这个项目实例涵盖了数据加载、预览、清洗、统计分析、可视化、特征工程等多个环节，使用了大量的 Pandas 函数，包括但不限于：•read_csv•head•info•describe•str.split•stack•reset_index•drop•join•astype•fillna•dropna•value_counts•plot•histplot•displot•scatterplot•groupby•mean•boxplot•lmplot•np.log•cut•to_csv通过完成这个项目，您可以进一步巩固 Pandas 的知识点，并了解如何运用 Pandas 进行实际的数据分析工作。根据实际数据集的大小和复杂性，您可以适当增删或调整分析步骤。
"""
```









###### 实战五



```python
"""项目实例：电子商务网站用户行为数据分析
背景： 某电子商务网站收集了用户在过去一年中的浏览、购买等行为数据，存储在一个名为 user_behavior.csv 的 CSV 文件中。
数据包含以下字段：
	•user_id: 用户唯一标识符
	•visit_date: 用户访问日期
  •product_id: 浏览或购买的产品唯一标识符
  •action_type: 用户行为类型（'view' 表示浏览，'add_to_cart' 表示加入购物车，'purchase' 表示购买）
  •price: 产品价格（仅在购买行为时记录）
  •quantity: 购买数量（仅在购买行为时记录）
   
项目目标：
•清洗和预处理数据，确保数据质量。
•分析用户行为模式，如： 
	•平均每次访问浏览多少商品？•用户从浏览到购买的转化率是多少？•用户在一周内的活跃分布情况如何？•可视化关键发现，以辅助决策。
"""



# 项目步骤：
# 步骤一：数据加载与初步检查import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# 加载数据
df = pd.read_csv('user_behavior.csv')

# 查看数据概览
print(df.head())
print(df.info())


# 步骤二：数据清洗与预处理
# 处理缺失值（假设选择删除含有缺失值的行）
df.dropna(inplace=True)

# 确保 `visit_date` 字段为日期类型
df['visit_date'] = pd.to_datetime(df['visit_date'])

# 对于购买行为，确保 `price` 和 `quantity` 不为负数
df = df[(df['action_type'] == 'purchase') & (df['price'] >= 0) & (df['quantity'] > 0)]


# 步骤三：数据分析
# 计算用户每次访问平均浏览商品数
avg_views_per_visit = df[df['action_type'] == 'view'].groupby('user_id')['product_id'].count().mean()
print(f"平均每次访问浏览 {avg_views_per_visit:.2f} 个商品")

# 计算从浏览到购买的转化率
total_views = df[df['action_type'] == 'view'].shape[0]
total_purchases = df[df['action_type'] == 'purchase'].shape[0]

conversion_rate = total_purchases / total_views * 100
print(f"从浏览到购买的转化率为 {conversion_rate:.2f}%")

# 用户在一周内的活跃分布
day_of_week = df['visit_date'].dt.day_name()
user_activity = df.groupby(day_of_week).size()

print("用户在一周内的活跃分布：")
print(user_activity)


# 步骤四：数据可视化
# 用户在一周内的活跃分布可视化
sns.barplot(x=user_activity.index, y=user_activity.values)
plt.title('用户在一周内的活跃分布')
plt.xlabel('星期')
plt.ylabel('访问次数')
plt.show()


"""
这个项目实例展示了如何使用 Pandas 进行数据加载、清洗、分析和可视化，以解决电子商务网站用户行为数据分析的实际问题。通过对原始数据的处理，提取出有意义的业务指标，并通过图表直观展示用户行为模式，为网站运营策略优化提供数据支持。请注意，实际项目中可能需要根据数据实际情况调整清洗和分析步骤。
"""
```







###### 实战六

```python
"""
求得三十天内X个账户使用的IP地址属于同一网段（网段简化判断规则：32IPV4前24位相同，参考值X=5）的客户，同一客户可以拥有多个账户
"""

df = pd.DataFrame({
    'Account': [...],  # 账户ID列表
    'UseDate': [...],  # 使用日期列表，格式为 datetime 类型
    'IPAddress': [...],  # IP 地址列表，格式为字符串
})
# df = pd.read_csv('your_file.csv')



# 筛选最近三十天的数据： 使用 pd.Timestamp.now() 获取当前时间，然后计算三十天前的日期，过滤出过去三十天内所有账户的使用记录。
thirty_days_ago = pd.Timestamp.now() - pd.DateOffset(days=30)
recent_data = df[df['UseDate'] >= thirty_days_ago]


# 提取 IP 地址前24位： 定义一个辅助函数来提取 IP 地址的前24位（即网络部分），并将其应用到 recent_data 上创建一个新的列。
def extract_network_part(ip_str):
    ip_segments = ip_str.split('.')
    return '.'.join(ip_segments[:3])

recent_data['NetworkPart'] = recent_data['IPAddress'].apply(extract_network_part)


# 按客户和网络部分分组计数： 将数据按照客户ID和提取的网络部分进行分组，计算每个客户在每个网络部分下的账户数。 
# size()：返回每个分组下的数据行数
grouped = recent_data.groupby(['CustomerID', 'NetworkPart']).size().reset_index(name='AccountCount')


# 筛选满足条件的客户： 设置阈值 X（例如 5），找出每个网络部分下有至少 X 个账户的客户。
threshold = 5  # 或者根据实际需求设置X值
qualified_customers = grouped[grouped['AccountCount'] >= threshold]


```







###### 实战七

```python
"""生成实战六中的数据"""
import pandas as pd
import random
import datetime

# 生成唯一的IP地址
def generate_unique_ip_addresses(total_records):
  	 # 创建一个空集合（无序，不重复）
    ip_addresses = set()
    
    
    """
    random.randint(0, 255)：
    for _ in range(4)：执行四次的循环
    str(random.randint(0, 255))：将随机整数转换为字符串类型
    '.'.join(...)：将随机生成的字符串用点号连接起来，形成一个完整的IP地址。
    """
    while len(ip_addresses) < total_records:
        ip_address = '.'.join(str(random.randint(0, 255)) for _ in range(4))  # 生成随机IP地址
        ip_addresses.add(ip_address)
    return list(ip_addresses)
  
  

# 生成数据
def generate_data(num_records):
    ip_addresses = generate_unique_ip_addresses(num_records)
    data = []
    for ip_address in ip_addresses:
        customer_id = ''.join(random.choices('0123456789', k=11))  # 随机生成11位数字作为客户ID
        account_id = ''.join(random.choices('0123456789', k=11))  # 随机生成11位数字作为账户ID
        login_date = datetime.datetime.now() - datetime.timedelta(days=random.randint(0, 365))  # 随机生成一年内的日期
        data.append([customer_id, account_id, login_date.strftime("%Y-%m-%d"), ip_address])

    return data

# 生成数据
num_records = 1000  # 数据记录数
data = generate_data(num_records)

# 创建DataFrame
df = pd.DataFrame(data, columns=['Customer_ID', 'Account_ID', 'Login_Date', 'Login_IP'])

# 保存到CSV文件
df.to_csv('raw_data.csv', index=False)

print("CSV文件已生成：raw_data.csv")

```



###### 实战八

英国零售商分析

```python
# 导入相关库

import pandas as pd 
import numpy as np
import os
import matplotlib.pyplot as plt
import warnings
from sqlalchemy import create_engine
warnings.filterwarnings('ignore')
import plotly as py
import plotly.graph_objs as go
py.offline.init_notebook_mode()
pyplot = py.offline.iplot
%matplotlib inline


# 读取本地数据 encoding指定解码类型
df =pd.read_csv('/Users/buzhi/Downloads/data.csv',encoding='latin-1')

# 修改列索引英文名为中文：
df = df.rename(columns={'InvoiceNo':'发票号','StockCode':'代码','Description':'产品名','Quantity':'数量','InvoiceDate':'发票日期','UnitPrice':'单价','CustomerID':'用户名','Country':'国家'})


# 自定义功能函数
def rstr(df, pred=None):
    # 获取DataFrame的行数
    obs = df.shape[0]
    
    # 获取每列的数据类型
    types = df.dtypes
    
    # 计算每列的非空值数量
    counts = df.apply(lambda x: x.count())
    
    # apply 用于 DataFrame，其中 x 代表 DataFrame 某列/某行的Series数据
    # 获取每列的唯一值（作为数组）
    uniques = df.apply(lambda x: [x.unique()]).T
    
    # 计算每列的缺失值数量
    nulls = df.apply(lambda x: x.isnull().sum())
    
    # 计算每列的不同值的数量
    distincts = df.apply(lambda x: x.unique().shape[0])
    # distincts = df2.apply(lambda x: x.nunique())

    
    # 计算每列的缺失值比例（百分比）
    missing_ratio = (df.isnull().sum() / obs) * 100
    
    # 打印DataFrame的形状
    print('Data shape:', df.shape)
    
    # 定义列名
    cols = ['types', 'counts', 'distincts', 'nulls', 'missing ratio', 'uniques']
    
    # 将各统计信息合并为一个DataFrame
    str = pd.concat([types, counts, distincts, nulls, missing_ratio, uniques], axis=1, sort=True)
    
    # 设置列名
    str.columns = cols
    
    # 返回汇总信息表
    return str

# 调用函数rstr，传入DataFrame dt
details = rstr(df)

# 显示汇总信息表，并按缺失值比例降序排序
display(details.sort_values(by='missing ratio', ascending=False))

# 转换数据类型
df['数量'] = df['数量'].astype(int)
df['单价'] = df['单价'].astype(int)


# 存在多少 购买数量和单价都小于等于零 的数据
# len计算数量时会包含 NaN ，而 nunique，count 计算数量时默认忽略 NaN

df[(df['数量']<=0) & (df['单价']<=0) ].shape[0]
df.query('数量 <= 0 and 单价 <= 0').shape[0]
df.query('数量 <= 0 and 单价 <= 0').count()
df.query('数量 <= 0 and 单价 <= 0').nunique()
len(df.query('数量 <= 0 and 单价 <= 0'))

def filter_func(row):
    return row['数量'] <= 0 and row['单价'] <= 0

df[df.apply(filter_func, axis=1)].shape[0]


# 购买数量 小于零的数据有多少
df.query('数量<0').shape[0]

#  购买数量 小于零的用户有哪些
#  unique() 只能用于 Series 返回一个没有重复数据的数组
df.loc[df['数量']<0,['用户名']]['用户名'].unique()


# 购买数量 小于零，用户占比
df.loc[df['数量']<0,['用户名']].shape[0] / df.shape[0]

# 用户名不为空且购买数量不为空的发票号开头有哪些
df.loc[df['用户名'].notnull() & df['数量']<= 0,'发票号'].apply(lambda x: x[0]).unique()

# 单价小于零的单价以及对应数量
df.loc[df['单价'] <= 0,'单价'].groupby(df['单价']).count()


# 数据清洗

# 筛选出用户名不为空的数据
df1 = df[(df['用户名'].notnull())]

# 剔除重复数据
df2 = df1.drop_duplicates()

# 剔除存在空值的数据 
df2_1 = df2.dropna(how='any',axis='index')


# 提取时间格式
df2['date'] = df2['发票日期'].apply(lambda x: x.split(' ')[0])
df2['time'] = [x.split(' ')[1] for x in df2['发票日期']]

# 提取年月日信息
df2['年份'] =  [x.split('/')[2] for x in df2['date']]
df2['月份'] =  [x.split('/')[0] for x in df2['date']]
df2['天数'] =  [x.split('/')[1] for x in df2['date']]

# 删除不需要的列
df2.drop(['time'],axis=1,inplace = True)

# 数据存储

# 创建mysql引擎
engine = create_engine('mysql+pymysql://root:a1336869185@localhost/buzhi')

df2.to_sql(name='NoReturn_Data', con=engine, if_exists='replace', index=False)

query = 'SELECT * FROM BUZHI.NoReturn_Data LIMIT 100'
df3 = pd.read_sql_query(query,engine)

# 分组求得不同指标
customer =df2.groupby('用户名').agg({'发票号':'nunique','数量':np.sum,'消费额':np.sum})

"""
			发票号	数量	消费额
用户名			
12346.0	1		74215	74215
12347.0	7		1578	3012
12348.0	4		593		944
"""

# 高于80%用户的消费额分界线的用户
df3 = customer[customer['消费额']>customer['消费额'].quantile(q=0.8) ]
```







###### 实战九

```python
now_dt = sys.argv[1]
curr_dt = now_dt.replace("-","")
rela_data_ods = pd.read_csv(f"/xx/xxx/xxx/{curr_dt}/xxxx_xx_{curr_dt}.dat",header=None,sep='|')
rela_data_ods.columns = ['object_key','from_key','to_key','type','L3M_TOT_AMT','L3M_TOT_CNT']
rela_data_ods = rela_data_ods[['object_key','from_key','to_key','type']]

if(len(rela_data_ods) == 0):
    print('数据为空，请检查')
    
    
print('数据量为：',rela_data_ods.shape[0])


# 数据去重辅助函数
def concat_str(dt_type,x,y):
    if x>y:
        return str(dt_type).strip()+str(x)+str(y)
    if y>x
        return str(dt_type).strip()+str(y)+str(x)
    
    

def num01(x,y):
    if x>y:
        return 2
    if y>x:
        return 1
    
rela_data_other = rela_data_ods[~rela_data_ods['type'].str.strip().isin(['tran','tel','addr','mac','equip_fp','work_unit'])]
rela_data = rela_data_ods[rela_data_ods['type'].str.strip().isin(['tran','tel','addr','mac','equip_fp','work_unit'])]


# 数据去重，A-B/B-A 关系只保留一条
rela_data = rela_data[rela_data['to_key'] != rela_data['from_key']]
rela_data = rela_data[['to_key','from_key','type']].drop_duplicates()
rela_data["unique_relat"] = rela_data.apply(lambda x:concat_str(x.from_key,x.to_key),axis=1)
rela_data["num01"] = rela_data.apply(lambda x:num01(x.from_key,x.to_key),axis=1)
rela_data["rank"] = rela_data.groupby(['unique_relat'])['num01'].rank(method='first',ascending=False)
rela_data = rela_data[rela_data['rank']==1][['from_key','to_key','type']]
    
    
# 判断是否为客户
def is_cust_funtion(object_key):
    if object_key.find('indv_cust') != -1 or  object_key.find('corp_cust') != -1 :
        return 1
    else:
        return 0
    
    
# 判断是否为标签
def is_label_funtion(object_key):
    if object_key.find('indv_cust') == -1 and  object_key.find('corp_cust') == -1 :
        return 1
    else:
        return 0
        
        



# 处理转账数据
tran_data = rela_data[rela_data['type'].str.strip()=='tran']        

# 从转账数据 中筛选出from_key为行外账户的数据
swap_data = tran_data[tran_data['from_key'].str.contains('other')]


# 对调 以上数据的 from_key 和 to_key
swap_data['to_key_new'] = swap_data['from_key']
swap_data['from_key'] = swap_data['to_key']
swap_data = swap_data[['from_key','to_key_new','type']]
swap_data.rename(columns = {'to_key_new':'to_key'},inplace = True)

# 再将对调的数据拼接为原先的交易数据，删除原先数据中未对调的部分数据
tran_data = pd.concat([tran_data.drop(swap_data.index,axis=0),swap_data])


tran_data = tran_data.groupby(['to_key']).filter(lambda group : 2<= len(group) <= 50)



# lambda group : 2<= len(group) <= 50
# len(group) 获取该分组的行数

rela_data = rela_data[rela_data['type'].str.strip()!= 'tran']
rela_data = pd.concat([rela_data,tran_data],ignore_index=True)


def edge_weight(x,flag):
    if 'tran' == x:
        return 0.8
    else:
        return 0.2


bla_data = pd.read_csv(f"xx/xx/data/export/xx/{curr_dt}/gwa_opd_t_blklist_cust_alg_{curr_dt},dat",header=None,sep='|')
bla_data.columns = ['object_key','type']
bla_data['flag'] = 1
bla_data.drop_duplicates()

bla_member_data = bla_data[bla_data['type'] == 1][['object_key','flag']] # 黑成员
bla_label_data = bla_data[bla_data['type'] == 2][['object_key','flag']] # 黑标签
bla_entity_data = bla_data[[['object_key','flag']]] # 黑实体

print('数据读取完毕')

rela_data['weight'] = rela_data.apply(lambda x: edge_weight(x.type,x.flag),axis=1)


# 关联出from_key to_key的 flag 值之和
rela_data = pd.merge(rela_data,bla_entity_data,left_on = ['from_key'],right_on=['object_key'],how='left')
rela_data['flag'] = rela_data['flag'].fillna(0)
rela_data.rename(columns={'flag':'from_flag'},inplace = True)
rela_data = rela_data[['from_key','to_key','type','from_flag']]

rela_data = pd.merge(rela_data,bla_entity_data,left_on = ['to_key'],right_on=['object_key'],how='left')
rela_data['flag'] = rela_data['flag'].fillna(0)
rela_data.rename(columns={'flag':'to_flag'},inplace = True)
rela_data['flag'] = rela_data['from_flag'] + rela_data['to_flag']
rela_data = rela_data[['from_key','to_key','type','flag']]



dt = rela_data[['from_key','to_key','weight']].copy()
standValue = 0.5



```







###### 实战十

​	输出 不同系统下 时区数量的前十名

```python
import json
import seaborn as sns
import pandas as pd
import numpy as np


# 读取数据
path = '/Users/buzhi/Documents/学习资料/BigData_MK笔记/Data/example.txt'

records = [json.loads(line) for line in open(path)]
frame = pd.DataFrame(records)

# 查看时区数据的情况
# frame.groupby('tz')['a'].count()
frame['tz'].value_counts()  # 存在空值，需转换



# 清洗时区数据（空值替换）
frame['tz'].fillna('NaN值',inplace=True)

frame.loc[(frame['tz'] == ''),'tz'] = '空字符串'
# frame['tz'] = frame['tz'].replace('', '空字符串')
# frame['tz'] = frame['tz'].apply(lambda x: '空字符串' if x == '' else x)


# 查看 a 列数据情况
frame['a']


# 清洗 a 列数据，判断是否含有 Windows

# 剔除掉 a 列数据为空的数据
frame['a'].dropna
frame = frame.query('a.notnull() or a != ""')

frame['os'] = frame['a'].apply(lambda x: 'Windows' if 'Windows' in x else 'Not Windows')

# 取的每一组对应的数量为 count 列  
# size：返回一个 Series结构
# reset_index：将 Series 的 MultiIndex（多级索引） 转换为 DataFrame 的普通列
# 将原 Series 的值（即计数值）作为一个新的列，并通过 name 参数指定该列的名称
grouped = frame.groupby(['os','tz']).size().reset_index(name='count')



windows_top10 = grouped[grouped['os'] == 'Windows'].sort_values(by='count', ascending=False).head(10)
not_windows_top10 = grouped[grouped['os'] == 'Not Windows'].sort_values(by='count', ascending=False).head(10)
print("Windows Top 10 Combinations:")
print(windows_top10)

print("\nNot Windows Top 10 Combinations:")
print(not_windows_top10)
```







###### 实战十一

​	筛选出评分数量大于250条，且女性评分最高的十部电影

```python
"""
1:读取三张表的数据，并使用merge合并
2:groupby title找出size大于五十的电影（取出满足条件数据的title索引）
3:使用pivot_table函数求的不同性别对每部电影的评分平均值（title作为行索引）
4:根据 F 女性评分排序取前十名，loc[第二步中得到的索引]，即可得出需求所求数据

"""


# 限制每次输出的行数
pd.options.display.max_rows = 10

# 读取数据
unames = ['user_id', 'gender', 'age', 'occupation', 'zip']
users = pd.read_table('/Users/buzhi/Documents/学习资料/BigData_MK笔记/Data/MovieLens/users.dat', sep='::',
                      header=None, names=unames, encoding='latin1',engine='python')

rnames = ['user_id', 'movie_id', 'rating', 'timestamp']
ratings = pd.read_table('/Users/buzhi/Documents/学习资料/BigData_MK笔记/Data/MovieLens/ratings.dat', sep='::',
                        header=None, names=rnames, encoding='latin1',engine='python')
mnames = ['movie_id', 'title', 'genres']
movies = pd.read_table('/Users/buzhi/Documents/学习资料/BigData_MK笔记/Data/MovieLens/movies.dat', sep='::',
                       header=None, names=mnames, encoding='latin1',engine='python')


# 合并三张表的数据
data = pd.merge(pd.merge(ratings, users), movies)


# 筛选出评分数量大于等于250的电影
ratings_by_title = data.groupby('title').size()
titles = ractive_titles[ratings_by_title>=250].index
# 等价于 ratings_by_title.index[ratings_by_title >= 250]


# 求得不同性别对电影的平均评分，title为行索引
mean_ratings = data.pivot_table('rating',index='title',columns='gender',aggfunc='mean')
# 等价于 data.groupby(['gender','title']).agg({'rating':'mean'}).unstack().T


# 筛选出评分数量大于250条，且女性评分最高的十部电影
mean_ratings = mean_ratings.loc[titles]
top_female_ratings = mean_ratings.sort_values(by='F',ascending=False).head(10)

```





###### 实战十二

```python
"""
实例描述

假设你正在处理一个在线零售公司的销售数据集。该数据集包含以下列：

	•	InvoiceNo: 发票编号
	•	StockCode: 商品代码
	•	Description: 商品描述
	•	Quantity: 商品数量
	•	InvoiceDate: 发票日期
	•	UnitPrice: 商品单价
	•	CustomerID: 客户ID
	•	Country: 国家

任务是对数据进行清洗、分析并生成有价值的报告。具体操作包括：

	1.	数据清洗：
	•	处理缺失值，特别是CustomerID。
	•	删除无效的Quantity和UnitPrice（如负值或零值）。
	•	对InvoiceDate进行格式化。
	2.	数据转换：
	•	根据InvoiceNo计算每个订单的总金额。
	•	根据CustomerID计算每个客户的购买行为，如总支出、购买次数。
	3.	数据聚合：
	•	按国家和月份聚合销售数据，计算销售总额。
	•	识别出最常购买的商品（按数量和销售额计算）。
	4.	数据可视化：
	•	可视化按月份和国家的销售趋势。
	•	可视化客户支出的分布情况。
"""


import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# 1. 数据加载
df = pd.read_csv('online_retail.csv')

# 2. 数据清洗

# 去除CustomerID为空的行
df = df.dropna(subset=['CustomerID'])

# 处理负数或零值的Quantity和UnitPrice
df = df[(df['Quantity'] > 0) & (df['UnitPrice'] > 0)]

# 格式化InvoiceDate为日期时间格式
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])

# 3. 数据转换

# 添加一个新的列 TotalAmount 计算每条订单的总金额
df['TotalAmount'] = df['Quantity'] * df['UnitPrice']

# 按CustomerID计算客户的总支出和购买次数
customer_summary = df.groupby('CustomerID').agg({
    'TotalAmount': 'sum',
    'InvoiceNo': 'nunique'
}).rename(columns={'TotalAmount': 'TotalSpent', 'InvoiceNo': 'PurchaseCount'})

# 4. 数据聚合

# 按国家和月份聚合销售数据
df['Month'] = df['InvoiceDate'].dt.to_period('M')
country_sales = df.groupby(['Country', 'Month']).agg({
    'TotalAmount': 'sum'
}).reset_index()

# 找出最受欢迎的商品（按数量和销售额）
product_summary = df.groupby('StockCode').agg({
    'Quantity': 'sum',
    'TotalAmount': 'sum'
}).sort_values(by='TotalAmount', ascending=False)

# 5. 数据可视化

# 按国家和月份的销售趋势
plt.figure(figsize=(14, 7))
for country in df['Country'].unique():
    country_data = country_sales[country_sales['Country'] == country]
    plt.plot(country_data['Month'].astype(str), country_data['TotalAmount'], label=country)
plt.legend()
plt.title('Sales Trend by Country')
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.xticks(rotation=45)
plt.show()

# 可视化客户支出分布
plt.figure(figsize=(10, 6))
plt.hist(customer_summary['TotalSpent'], bins=50, edgecolor='black')
plt.title('Customer Total Spent Distribution')
plt.xlabel('Total Spent')
plt.ylabel('Number of Customers')
plt.show()

# 输出最受欢迎的商品
print("Top 10 products by sales:")
print(product_summary.head(10))

# 可视化最受欢迎的商品
plt.figure(figsize=(10, 6))
top_products = product_summary.head(10)
plt.bar(top_products.index.astype(str), top_products['TotalAmount'], color='blue')
plt.title('Top 10 Products by Sales Amount')
plt.xlabel('StockCode')
plt.ylabel('Total Sales')
plt.xticks(rotation=45)
plt.show()

```





###### 案例十三

```python
# 电商平台的交易数据集，包含客户购买记录的详细信息， 对数据进行处理和分析，包括数据加载、清洗、转换、聚合、筛选和指标计算
"""
	•	OrderID: 订单编号
	•	CustomerID: 客户编号
	•	OrderDate: 订单日期
	•	ProductID: 产品编号
	•	Category: 产品类别
	•	Quantity: 购买数量
	•	Price: 产品单价
	•	TotalAmount: 订单总金额（Quantity * Price）
	•	Country: 客户所在国家
"""


df = pd.read_csv('ecommerce_data.csv')
# 查看数据情况
print(df.info())
print(df.head())

print(df.isnull().sum())   # 返回每列空值的数量,sum 的 axis 默认为0


# 填充缺失值或删除缺失行
df.dropna()      # 默认 axis = 0 , how = 'any' 删除存在空值的行


# 数据类型转换: 确保 OrderDate 是日期格式，Quantity 和 Price 是数值类型
df['OrderDate'] = pd.to_datetime(df['OrderDate'])
df['Quantity'] = df['Quantity'].astype(int)
df['Price'] = df['Price'].astype(float)

# 删除重复数据
df.drop_duplicates()


# 重新计算订单总金额
df['TotalAmount'] = df['Quantity'] * df['Price']

# 按日期分割: 从 OrderDate 中提取年、月、日信息
df['Year'] = df['OrderDate'].dt.year
df['Month'] = df['OrderDate'].dt.month
df['Day'] = df['OrderDate'].dt.day



# 计算每个客户的总购买金额和订单数量
customer_summary = df.groupby('CustomerID').agg(
{
 'OrderID':'nunique',   
 'TotalAmount':'sum'
}).rename(columns={'OrderID':'OrderCount','TotalAmount':'TotalSpent'})



# 筛选高价值客户
high_value_customers = customer_summary[customer_summary['TotalSpent'] > 500]

# 筛选来自美国的订单
us_orders = df[df['Country'] == 'United States']


# 抽取最近6个月的订单
recent_orders = df[df['OrderDate'] >= pd.to_datetime('today') - pd.DateOffset(months=6)]
print(recent_orders.head())


# 计算每个月的销售总额和订单数量
monthly_sales = df.groupby(['Year', 'Month']).agg({
    'TotalAmount': 'sum',
    'OrderID': 'nunique'
}).rename(columns={'OrderID': 'OrderCount'})


# 计算每个产品类别的销售总额和销售数量
category_sales = df.groupby('Category').agg({
    'Quantity': 'sum',
    'TotalAmount': 'sum'
}).sort_values(by='TotalAmount', ascending=False)


# 计算每个客户的平均订单金额和订单频率
customer_loyalty = df.groupby('CustomerID').agg({
    'OrderID': 'nunique',
    'TotalAmount': 'sum'
}).rename(columns={'OrderID': 'OrderCount', 'TotalAmount': 'TotalSpent'})


# 保存高价值客户列表
high_value_customers.to_csv('high_value_customers.csv', index=False)


# 导出月度销售报告
monthly_sales.to_excel('monthly_sales_report.xlsx', sheet_name='SalesReport')


```







###### 案例十四

```python
from time import time
import pandas as pd
import numpy as np
from sklearn.tree import DecisionTreeClassifier

def BinRate(dataSet, Attr_Nm, Resp_Nm):
    group = dataSet.groupby([Attr_Nm])[Resp_Nm].agg(['count','sum'])
    group.columns = ['Tot_Num','Resp_Num']
    group.reset_index(inplace=True)
    group['NResp_Num'] = group['Tot_Num'] - group['Resp_Num']
    group['RespPcnt'] = group['Resp_Num'] / group['Tot_Num']
    return group

def Chi2_Cal_np(dataSet):
    data = dataSet.copy()
    np_data = np.array(data)
    Tot_Num = sum(np_data[:,1])
    Tot_Resp = sum(np_data[:,2])
    
    Resp_Pcnt = Tot_Resp / Tot_Num#Pcnt of Respone sample
    NResp_Pcnt = 1 - Resp_Pcnt#Pcnt of None Response sample
    
    if Resp_Pcnt in [0,1]:
        return 0
    Resp_Exp = np_data[:,1] * Resp_Pcnt
    NResp_Exp = np_data[:,1] * NResp_Pcnt
    
    #chi2_calculation
    RespZip = zip(Resp_Exp,np_data[:,2])
    NRespZip = zip(NResp_Exp,np_data[:,3])
    RespChi2 = [(elem[1]-elem[0])**2 / elem[0] for elem in RespZip]
    NRespChi2 = [(elem[1]-elem[0])**2 / elem[0] for elem in NRespZip]
    chi2 = sum(RespChi2) + sum(NRespChi2)
    return chi2

def IV_Cal(dataSet):
    df = dataSet.copy()
    N_0 = np.sum(df['NResp_Num'])
    N_1 = np.sum(df['Resp_Num'])
    
    df['WOE'] = np.log((df['Resp_Num']/N_1)/(df['NResp_Num']/N_0))
    df['pre_IV'] = (df['Resp_Num']/N_1-df['NResp_Num']/N_0)*df['WOE']    
    return df
    
#Chi2 Merge for numeric attributes
def Chi2_Merge_Num(dataSet,Attr_Nm,Resp_Nm,spe_attr=[],confiVal=3.841,BinMax=10,BinPcntMin=0.05):
    print('Chi2-merge on Attribute:{}'.format(Attr_Nm))
    t0 = time()
    if len(spe_attr) >= 1:
        data_1 = dataSet.loc[dataSet[Attr_Nm].isin(spe_attr)]
        data_2 = dataSet.loc[~dataSet[Attr_Nm].isin(spe_attr)]
        BinMax -= len(set(data_1[Attr_Nm]))
    else:
        data_2 = dataSet.copy()
    
    binRespRate = BinRate(data_2,Attr_Nm,Resp_Nm)
    
    if len(binRespRate) <= 2:
        print('Bins<2, return')
        binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
        binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_num'])
        binRespRate['Attr_Nm'] = Attr_Nm
        binRespRate.rename(columns={Attr_Nm:'levels'},inplace=True)
        binRespRate['Ind'] = 0
        return binRespRate
    
    columns_Nm = binRespRate.columns
    t1 = time()
    binRespRate_np = np.array(binRespRate)
    
    #1.merge adjacent bins with single type samples
    i = 0
    while(i <= binRespRate_np.shape[0] - 2):
        if ((binRespRate_np[i,2]==0 and binRespRate_np[i+1,2]==0) or (binRespRate_np[i,3]==0 and binRespRate_np[i+1,3]==0)):
            binRespRate_np[i,1] = binRespRate_np[i,1] + binRespRate_np[i+1,1]
            binRespRate_np[i,2] = binRespRate_np[i,2] + binRespRate_np[i+1,2]
            binRespRate_np[i,3] = binRespRate_np[i,3] + binRespRate_np[i+1,3]
            binRespRate_np[i,0] = binRespRate_np[i+1,0]
            binRespRate_np = np.delete(binRespRate,i+1,0)
            i = i-1
        i = i+1
    t2 = time()
    
    #2.chi2-merge
    chi2List = []
    for i in range(0 ,len(binRespRate_np)-1):
        col_binRespRate = binRespRate_np[i:i+2,:]
        chi2 = Chi2_Cal_np(col_binRespRate)
        chi2List.append(chi2)
    chi2List = chi2List
    while(1):
        if(len(chi2List)<=(BinMax-1) and min(chi2List)>=confiVal):
            break
        chi_min_index = chi2List.index(min(chi2List))
        combineIndex = chi_min_index
        combine_binRespRate = binRespRate_np[combineIndex+2,:]
        
        binRespRate_np[combineIndex+1,1] = sum(combine_binRespRate[:,1])
        binRespRate_np[combineIndex+1,2] = sum(combine_binRespRate[:,2])
        binRespRate_np[combineIndex+1,3] = sum(combine_binRespRate[:,3])
        binRespRate_np = np.delete(binRespRate, combineIndex,0)
        
        if chi_min_index == 0:
            col_binRespRate_bw = binRespRate_np[combineIndex:combineIndex+2,:]
            chi2_bw = Chi2_Cal_np(col_binRespRate_bw)
            chi2List[chi_min_index+1] = chi2_bw
            chi2List = np.delete(chi2List,chi_min_index,0).tolist()
        elif chi_min_index == len(chi2List) - 1:
            col_binRespRate_fw = binRespRate_np[combineIndex-1:combineIndex+1,:]
            chi2_fw = Chi2_Cal_np(col_binRespRate_fw)
            chi2List[chi_min_index-1] = chi2_fw
            chi2List = np.delete(chi2List,chi_min_index,0).tolist()
        else:
            col_binRespRate_bw = binRespRate_np[combineIndex:combineIndex+2,:]
            chi2_bw = Chi2_Cal_np(col_binRespRate_bw)
            chi2List[chi_min_index+1] = chi2_bw
            col_binRespRate_fw = binRespRate_np[combineIndex-1:combineIndex+1,:]
            chi2_fw = Chi2_Cal_np(col_binRespRate_fw)
            chi2List[chi_min_index-1] = chi2_fw
            chi2List = np.delete(chi2List,chi_min_index,0).to_list()
        t3 = time()
        
        binRespRate = pd.DataFrame(binRespRate_np,columns=columns_Nm)
        binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
        
        #3.check whether each bins contains both Resp.Nreso samples
        minRespRate,maxRespRate = min(binRespRate['RespPcnt']),max(binRespRate['RespPcnt'])
        while minRespRate==0 or maxRespRate==1:
            RespRate_01 = binRespRate[Attr_Nm][binRespRate['RespPcnt'].isin([0,1])]
            index_01 = RespRate_01.index[0]
            if index_01 == 0:
                combineIndex = 0
                combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
                binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
                binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
                binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
                binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
                binRespRate.reset_index(drop=True, inplace=True)
            elif index_01 == binRespRate.shape[0]-1:
                combineIndex = binRespRate.shape[0]-2
                combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
                binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
                binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
                binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
                binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
                binRespRate.reset_index(drop=True, inplace=True)
            else:
                tmp1_binRespRate = binRespRate.loc[index_01-1:index_01,:]
                chi2_1 = Chi2_Cal_np(tmp1_binRespRate)
                
                tmp2_binRespRate = binRespRate.loc[index_01:index_01+1,:]
                chi2_2 = Chi2_Cal_np(tmp2_binRespRate) 
                if chi2_1 < chi2_2:
                    combineIndex = index_01 -1
                    combine_binRespRate = tmp1_binRespRate
                else:
                    combineIndex = index_01 
                    combine_binRespRate = tmp2_binRespRate   
                binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
                binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
                binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
                binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
                binRespRate.reset_index(drop=True, inplace=True)
            
            binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
            minRespRate,maxRespRate = min(binRespRate['RespPcnt']),max(binRespRate['RespPcnt'])
        t4 = time()
        
        #4.check pcnt of samples in bins reaches min threshlod
        binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_Num'])
        
        minPcnt = min(binRespRate['SamplePcnt'])
        while minPcnt < BinPcntMin:
           minPcnt_temp = binRespRate[Attr_Nm][binRespRate['SamplePcnt']==minPcnt]
           index_minPcnt = minPcnt_temp.index[0]
           if index_minPcnt == 0:
               combineIndex = 0
               combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
               binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
               binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
               binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
               binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
               binRespRate.reset_index(drop=True, inplace=True)
           elif index_minPcnt == binRespRate.shape[0]-1:
               combineIndex = binRespRate.shape[0] -2
               combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
               binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
               binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
               binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
               binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
               binRespRate.reset_index(drop=True, inplace=True)
           else:
                tmp1_binRespRate = binRespRate.loc[index_minPcnt-1:index_minPcnt,:]
                chi2_1 = Chi2_Cal_np(tmp1_binRespRate)
                
                tmp2_binRespRate = binRespRate.loc[index_minPcnt:index_minPcnt+1,:]
                chi2_2 = Chi2_Cal_np(tmp2_binRespRate) 
                if chi2_1 < chi2_2:
                    combineIndex = index_minPcnt -1
                    combine_binRespRate = tmp1_binRespRate
                else:
                    combineIndex = index_minPcnt
                    combine_binRespRate = tmp2_binRespRate
                binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
                binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
                binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
                binRespRate = binRespRate.loc[binRespRate.index != combineIndex, :]
                binRespRate.reset_index(drop=True, inplace=True)
           binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
           binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_Num'])
           minPcnt = min(binRespRate['SamplePcnt'])
           t5 = time()         
           
           #5.combine special levels
           if len(spe_attr) > 0:
               binRespRate0 = BinRate(data_1, Attr_Nm, Resp_Nm)
               binRespRate['Ind'] = 1
               binRespRate = pd.concat([binRespRate0,binRespRate])
               binRespRate.reset_index(drop=True, inplace=True)
           binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
           binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_Num'])
           binRespRate['Attr_Nm'] = Attr_Nm
           binRespRate.rename(columns={Attr_Nm:'levels'},inplace=True)
           t6 = time()
           print('Final bins:{},time{}s'.format(len(binRespRate),round(t6-t0),2))
           
           return binRespRate
 

def Chi2_Merge_Cat(dataSet,Attr_Nm,Resp_Nm,spe_attr=[],confiVal=3.841,BinMax=10,BinPcntMin=0.05):
    print('Chi2-merge on Attribute:{}'.format(Attr_Nm))
    t0 = time()
    if len(spe_attr) >= 1:
        data_1 = dataSet.loc[dataSet[Attr_Nm].isin(spe_attr)]
        data_2 = dataSet.loc[~dataSet[Attr_Nm].isin(spe_attr)]
        BinMax -= len(set(data_1[Attr_Nm]))
    else:
        data_1 = pd.DataFrame()
        data_2 = dataSet.copy()
    
    binRespRate = BinRate(data_2,Attr_Nm,Resp_Nm)
    binRespRate = binRespRate.sort_values(by='RespPcnt',ascending=True)
    binRespRate.reset_index(inplace=True,drop=True)
    t1 = time()
    
    binRespRate = binRespRate.drop('RespPcnt',axis=1)
    bin_data = binRespRate.copy()
    
    #1.merge bins next to each other untill number od bins reach 'BinMax' 
    while binRespRate.shape[0]>BinMax:
        chi2List = []
        for i in range(0, binRespRate.shape[0]-1):
            col_binRespRate = binRespRate.loc[i:i+1,:]
            chi2 = Chi2_Cal_np(col_binRespRate)
            chi2List.append(chi2)
        combineIndex = chi2List.index(min(chi2List))
        combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
        
        binRespRate['Tot_Num'][combineIndex+1] = combine_binRespRate['Tot_Num'].sum()
        binRespRate['Resp_Num'][combineIndex+1] = combine_binRespRate['Resp_Num'].sum()
        binRespRate['NPesp_Num'][combineIndex+1] = combine_binRespRate['NResp_Num'].sum()  
        
        binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
        binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
        binRespRate.reset_index(drop=True,inpalce=True)
    t2 = time()
    
    #2.chk whether every bin contain Resp/NResp Ssmple
    binRespRate.loc[0,'RespPcnt'] = 0
    binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
    minRespRate,maxRespRate = min(binRespRate['RespPcnt']), max(binRespRate['RespPcnt'])
    while minRespRate == 0 or maxRespRate == 1:
        RespRate_01 = binRespRate[Attr_Nm][binRespRate['RespPcnt'].isin([0,1])]
        index_01 = RespRate_01.index[0]
        if index_01 == 0:
            combineIndex = 0
            combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]        
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num']) 
            binRespRate['Resp_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
            binRespRate_np = np.delete(binRespRate, combineIndex,0)
        
            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
        elif index_01 == binRespRate.shape[0]-1:
            combineIndex == binRespRate.shape[0]-2
            combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
            combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]        
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num']) 
            binRespRate['Resp_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
        
            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
        else:
            tmp1_binRespRate = binRespRate.loc[index_01-1:index_01,:]
            chi2_1 = Chi2_Cal_np(tmp1_binRespRate)
            
            tmp2_binRespRate = binRespRate.loc[index_01:index_01+1,:]
            chi2_2 = Chi2_Cal_np(tmp2_binRespRate)
            if chi2_1 < chi2_2:
                combineIndex = index_01 -1
                combine_binRespRate = tmp1_binRespRate
            else:
                combineIndex = index_01
                combine_binRespRate = tmp2_binRespRate
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num']) 
            binRespRate['Resp_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
            
            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
        binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
        minRespRate,maxRespRate = min(binRespRate['RespPcnt']),max(binRespRate['RespPcnt'])
    t3 = time()
                
        #3.check pcnt of samples in each bin reach BinPcntMin
    binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_Num'])
    minPcnt = min(binRespRate['SamplePcnt'])
    while minPcnt < BinPcntMin:
        minPcnt_tmp = binRespRate[Attr_Nm][binRespRate['Sample']==minPcnt]
        index_minPcnt = minPcnt_tmp.idnex[0]
        if index_minPcnt == 0:
            combineIndex = 0
            combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
            binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
            
            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
        elif index_minPcnt == binRespRate.shape[0]-1:
            combineIndex = binRespRate.shape[0]-2
            combine_binRespRate = binRespRate.loc[combineIndex:combineIndex+1,:]
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
            binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])
                
            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
        else:
            tmp1_binRespRate = binRespRate.loc[index_minPcnt-1:index_minPcnt,:]
            chi2_1 = Chi2_Cal_np(tmp1_binRespRate)
                
            tmp2_binRespRate = binRespRate.loc[index_minPcnt:index_minPcnt+1,:]
            chi2_2 = Chi2_Cal_np(tmp2_binRespRate) 
            if chi2_1 < chi2_2:
                combineIndex = index_minPcnt -1
                combine_binRespRate = tmp1_binRespRate
            else:
                combineIndex = index_minPcnt 
                combine_binRespRate = tmp2_binRespRate   
            binRespRate['Tot_Num'][combineIndex+1] = sum(combine_binRespRate['Tot_Num'])
            binRespRate['Reps_Num'][combineIndex+1] = sum(combine_binRespRate['Resp_Num'])
            binRespRate['NResp_Num'][combineIndex+1] = sum(combine_binRespRate['NResp_Num'])

            binRespRate[Attr_Nm][combineIndex+1] = u'%s,%s'%(binRespRate[Attr_Nm][combineIndex],binRespRate[Attr_Nm][combineIndex+1])
            binRespRate = binRespRate.loc[binRespRate.index != combineIndex,:]
            binRespRate.reset_index(drop=True,inpalce=True)
 
        binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
        binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / sum(binRespRate['Tot_Num'])
        minPcnt = min(binRespRate['SamplePcnt'])
    t4 = time()
        
    if len(data_1) >0:
        binRespRate0 = BinRate(data_1,Attr_Nm,Resp_Nm)
        binRespRate = pd.concat([binRespRate0,binRespRate])
        binRespRate.reset_index(drop=True,inpalce=True)
        
    binRespRate['SamplePcnt'] = binRespRate['Tot_Num'] / binRespRate['Tot_Num'].sum()
    binRespRate['RespPcnt'] = binRespRate['Resp_Num'] / binRespRate['Tot_Num']
    binRespRate['bin'] = range(1,len(binRespRate)+1)
        
    for i in binRespRate.index:
        bin_data.loc[bin_data[Attr_Nm].map(lambda x: x in binRespRate[Attr_Nm][i].split(',')),'bin']=binRespRate['bin'][i]
        bin_data.loc[bin_data[Attr_Nm].map(lambda x: x in binRespRate[Attr_Nm][i].split(',')),'SamplePcnt']=binRespRate['SamplePcnt'][i]
        bin_data.loc[bin_data[Attr_Nm].map(lambda x: x in binRespRate[Attr_Nm][i].split(',')),'RespPcnt']=binRespRate['RespPcnt'][i]
    t5 = time()
    binRespRate['Attr_Nm'] = Attr_Nm
    binRespRate.rename(columns = {Attr_Nm:'levels'},inplace=True)
    bin_data['Attr_Nm'] = Attr_Nm
    bin_data.rename(columns = {Attr_Nm:'levels'},inplace=True)
    binRespRate['Ind'] = 3
    print('Final bins{},Time{}s'.format(len(binRespRate), round(t5-t0,2)))
        
    return bin_data,binRespRate

def multi_IV(dataSet,feature_list,Resp_Nm,sp=False,sp_value=[]):
    ivMat = pd.DataFrame()
    flag = 1
    for i in range(len(feature_list)):
        if sp == True:
            TestChi2Merge = Chi2_Merge_Num(dataSet,feature_list[i],Resp_Nm,spe_attr=sp_value[i])
        else:
            TestChi2Merge = Chi2_Merge_Num(dataSet, feature_list[i], Resp_Nm)
        IV_data = IV_Cal(TestChi2Merge)
        ivMat = pd.concat([IV_data,ivMat])
        print('{}/{} completes'.format(i, len(feature_list)))
        flag += flag
        
    return ivMat
    
def IV_mat(dataSet,Resp_Nm,col_spnum,sp_value,col_spcat):
    df  = dataSet.copy()
    col1 = df.select_dtypes(include=['int32','int64','float64']).columns
    #1.Binning for special treatment numeric features
    ivMat1 = multi_IV(df, col_spnum, Resp_Nm,sp=True,sp_value=sp_value)
    
    #2.Binning for the rest of numeric features
    col_num = list(set(col1) - set(col_spnum))
    ivMat2 = multi_IV(df, col_num, Resp_Nm)
    
    #3.chi2-merge for categorical features
    col2 = df.select_dtypes(include=['O']).columns
    woe_Mat3 = pd.DataFrame()
    iv_Mat3 = pd.DataFrame()
    for feature in col_spcat:
        bin_Mat3_,woe_Mat3_ = Chi2_Merge_Cat(df,feature,Resp_Nm)
        iv_Mat3_ = IV_Cal(woe_Mat3_)
        woe_Mat3 = pd.concat([woe_Mat3,woe_Mat3_],axis=0)
        iv_Mat3 = pd.concat([iv_Mat3,iv_Mat3_],axis=0)
    #4.categprical features need not merge
    col_cat = list(set(col2) - set(col_spcat))
    ivMat4 = pd.DataFrame()
    for feature in col_cat:
        TestBin = BinRate(df, feature, Attr_Nm)
        TestIVmat = IV_Cal(TestBin)
        TestIVmat['SamplePcnt'] = TestIVmat['Tot_Num'] / TestIVmat['Tot_Num'].sum()
        TestIVmat['Attr_Nm'] = features
        TestIVmat['Ind'] = 1
        TestIVmat.rename(columns = {feature:'levels'},inplace=True)
        ivMat4 = pd.cancat([ivMat4,TestIVmat])
    
    ivMat = pd.DataFrame()
    ivMat = pd.concat([ivMat1,ivMat2,ivMat3,ivMat4],axis=0)
    ivMat = ivMat[ivMat['Attr_Nm'] != Resp_Nm]
    return ivMat
        
def tree_bin(dataSet,Attr_Nm,Resp_Nm):
    import itertools
    df  = dataSet.copy()
    print('Tree-Binning on : {}'.format(Attr_Nm))
    X = df[[Attr_Nm]]
    Y = df[Resp_Nm]
    clf = DecisionTreeClassifier(max_depth=3,
                                 min_samples_leaf=0.05,
                                 class_weight='balanced')
    clf.fit(X,Y)
    cut = []
    cut.append(clf.tree_.threshold)
    cut = list(itertools.chain.from_iterable(cut))
    cut = [x for x in cut if x != -2]
    cut = [round(i,4) for i in cut]
    cut = list(set(cut))
    cut.sort()
    cut.append(float('inf'))
    cut.insert(0, float('-inf'))
    if len(cut)<4:
        print('Change to Chi2_Merge')
        binRespRate = Chi2_Merge_Num(df, Attr_Nm, Resp_Nm)
    else:
        Attr_Nm_ = Attr_Nm + '_cut'
        df_out = pd.DataFrame()
        df_out[Attr_Nm] = pd.cut(df[Attr_Nm],cut)
        df_out[Resp_Nm] = df[Resp_Nm]
        binRespRate = BinRate(df_out, Attr_Nm, Resp_Nm)
        binRespRate = Get_Frame(binRespRate,Attr_Nm)
    print('Number of bins:{}'.format(len(binRespRate)))
    return binRespRate

def Get_Frame(dataSet,Attr_Nm):
    dataSet['RespPcnt'] = dataSet['Resp_Num'] / dataSet['Tot_Num']
    dataSet['SamplePcnt'] = dataSet['Tot_Num'] / sum(dataSet['Tot_Num'])  
    dataSet['Attr_Nm'] = Attr_Nm
    dataSet.rename(columns={Attr_Nm:'levels'},inplace=True)
    return dataSet



def IV_Chi2(dataSet,Resp_Nm,col_List=None):
    df = dataSet.copy()
    if col_List == None:
        df_select = df
    else:
        df_select = pd.DataFrame(df[col_List])
    ll = ['float64','float32','float16','int64','int32','int16','int8']
    num_List = set(df_select.select_dtypes(include=ll).columns)-set([Resp_Nm])
    cat_List = set(df_select.select_dtypes(include=['object']).columns)
    ivMat = pd.DataFrame()
    pro_List = []
    for i ,feature in enumerate(num_List,start=1):
        try:
            df[feature] = df[feature].fillna(-9999)
            num_ChiBin = tree_bin(df, feature, 'Y')
            ivMat_num = IV_Cal(num_ChiBin)
            ivMat = pd.concat([ivMat,ivMat_num])
            print('Numeric {}/{} completed'.format(i, len(num_List)))
        except Exception as e:
            print(e)
            print("{} can't process binning!".format(feature))
            pro_List.append(feature)
    ivCatBin = pd.DataFrame()
    for i,feature in enumerate(cat_List, start = 1):
        if len(df[feature].unique()) > 100:
            print("warning:levels of '{}'more than 100".format(feature))
            pro_List.append(feature)
            continue
        try:
            df[feature] = df[feature].fillna(-9999)
            cat_bin,cat_ChiBin = Chi2_Merge_Cat(df, feature, 'Y',spe_attr=[])
            ivMat_cat = IV_Cal(cat_ChiBin)
            ivMat = pd.concat([ivMat,ivMat_cat])
            ivCatBin = pd.concat([ivCatBin,cat_bin])
            print('Categorical {}/{} completed'.format(i, len(cat_List)))
        except Exception as e:
            print(e)
            print("{} can't process binning!".format(feature))
            pro_List.append(feature)
    return ivMat,ivCatBin,pro_List

    

ivMat,ivCatBin,Attr_problem = IV_Chi2(data,'Y')






```



#### 海致-AML

##### CODE_TOOL

###### BasicFunc

```python
import numpy as np
import pandas as pd
import scipy.stats as st


class BasicFunc(object):
    """
    功能说明：统一的基键函数接口
    """

    def __init__(self):
        pass

    ##平均值
    @staticmethod
    def get_avg(li):
        return float(np.mean(li))

    ##10%分位值
    @staticmethod
    def get_10_per(li):
        return float(np.percentile(li, 10))

    ##25%分位值
    @staticmethod
    def get_25_per(li):
        return float(np.percentile(li, 25))

    ##射中位数
    @staticmethod
    def get_50_per(li):
        return float(np.median(li))

    ##75%分位值
    @staticmethod
    def get_75_per(li):
        return float(np.percentile(li, 75))

    ##90%分位值
    @staticmethod
    def get_90_per(li):
        return float(np.percentile(li, 90))

    ##四分位距
    @staticmethod
    def get_4_dis(li):
        return float(np.percentile(li, 75) - np.percentile(li, 25))

    ##最大值
    @staticmethod
    def get_max(li):
        return float(np.max(li))

    ##最小值
    @staticmethod
    def get_min(li):
        return float(np.min(li))

    ##极差
    @staticmethod
    def get_max_gap(li):
        return float(np.max(li) - np.min(li))

    ##标准差
    @staticmethod
    def get_std(li):
        return float(np.std(li))

    ##变异系数
    @staticmethod
    def get_cv(li):
        return float(np.std(li) / np.mean(li))

    ###偏度
    @staticmethod
    def get_skew(li):
        return float(st.skew(li))

    ## 峰度
    @staticmethod
    def get_kur(li):
        return float(st.kurtosis(li))

    ## 平均偏差
    @staticmethod
    def get_avg_gap(li):
        return float(np.mean(abs(li - np.mean(li))))

    ##信息熵
    # def get_ent(li)

    ##按连续相同交易方向分组
    @staticmethod
    def generate_group_id(li):
        li = li.tolist()
        lag_li = [li[0]] + li[:-1]
        return abs(np.array(li) - np.array(lag_li)).cumsum()

    ##构建差分序列
    @staticmethod
    def generate_diff(li):
        li = li.tolist()
        return np.diff(li, axis=0, prepend=li[0])

    ##构建元素比值序列
    @staticmethod
    def generate_pct(li):
        li = li.tolist()
        last_li = li[1:] + [li[-1]]
        return np.array(last_li) / np.array(li)

    ##生成凹凸性趋势
    @staticmethod
    def generate_ser(li):
        li = li.tolist()
        ser = []
        sum_li = np.array(li).cumsum()
        for i in range(len(li)):
            ser.append(sum_li[:i + 1])
        return (sum_li + sum_li[0]) / 2 - list(map(np.median, ser))

    ##获取众数个数
    @staticmethod
    def a_get_mode_size(li):
        return len(pd.Series(data=li).mode())

    ##获取众数占比
    @staticmethod
    def a_get_mode_frac(li):
        li = li.tolist()
        return st.mode(li)[1][0] / sum(li)

    ##信息熵
    @staticmethod
    def a_get_infor(li):
        a = np.array(li) / sum(li)
        return sum(np.log2(a) * a * (-1))

    def all_func(self):
        return list(filter(lambda x: x.startswith('get') and callable(getattr(self.x)), dir(self)))

    def all_func_a(self):
        return list(filter(lambda x: x.startswith('a_get') and callable(getattr(self.x)).dir(self)))

```





###### MyUtils

```python
import pandas as pd
from functools import reduce
import numpy as np


class MyUtils(object):
    """工具类，封装常用方法
    """

    def __init__(self):
        pass

    @staticmethod
    def data_process_1(raw_pd: pd.DataFrame, col_name: str):
        """根据指定col_name进行过滤，然后根据客户号cst_id 与 指定col_name 进行分组
        Args:
            raw_pd (pd.DataFrame): 客户号cst_id与指定
            col_name进行分组
            col_name (string): 指定的col_name列名
            return(pd.DataFrame):返回分组的交易次数、交易金额和账户余额
        """
        raw_pd_1 = raw_pd[['cst_id', col_name, 'tran_amount', 'tran_banlance']]
        # 逻辑值：raw_pd_1[0] 为cst_id  而raw_pd_1[1]为列名
        # 去除 col_name 为NaN或空字符的数据
        raw_filter_pd = raw_pd_1[(raw_pd_1[col_name] != ' ') & (raw_pd_1[col_name].notna())]
        res_pd_temp = raw_filter_pd.groupby(['cst_id', col_name]).agg(
            {'tran_amount': ['count', 'sum'], 'tran_banlance': ['sum']})
        res_pd_1 = res_pd_temp.loc(axis=1)['tran_amount'].rename(
            columns={'count': 'tran_amount_count', 'sum': 'tran_banlance_sum'}).reset_index()

        res_pd_2 = res_pd_temp.loc(axis=1)['tran_banlance'].rename(columns={'sum': 'tran_banlance_sum'}).reset_index()

        res_pd = pd.merge(res_pd_1, res_pd_2, on=['cst_id', col_name], how='inner')
        return res_pd



    @staticmethod
    def data_process_2(raw_pd: pd.DataFrame, col_name: str, sum_col: str, base_func_list: list):
        """根据指定的col_name 进行过滤，然后根据客户号cst_id与指定col_name进行分组

        Args:
            raw_pd (pd.DataFrame): 客户号，指定col_name以及sunm_col
            col_name (str): 指定的列名
            sum_col (str): 指定的统计列
            base_func_list (list): 对统计列进行统计的函数
            return(pd.DataFrame)
        """
        raw_pd_1 = raw_pd[['partyid_casedate', col_name, sum_col]]
        raw_filter_pd = raw_pd_1[(raw_pd_1[col_name] != ' ') & (raw_pd_1[col_name].notna())]
        res_pd_temp = raw_filter_pd.groupby(['partyid_casedate', col_name]).agg({sum_col: base_func_list})
        rename_dict = {}
        for i in base_func_list:
            rename_dict[i] = sum_col + '_' + i
        res_pd = res_pd_temp.loc(axis=1)[sum_col].rename(columns=rename_dict).reset_index()
        return res_pd

    @classmethod
    def get_feature_4_basefunc(cls, raw_pd: pd.DataFrame, func_list: list, feature_name_prefix: str):
        """根据生成的交易金额与交易次数序列生成特征

        Args:
            raw_pd (pd.DataFrame): 按照客户号与对应交易特征种类生成的交易金额与交易次数序列，账户余额序列。列名tran_amount_count,tran_amount_sum,tran_banlance_sum
            func_list (list): list需要对序列处理的基础函数列表
            feature_name_prefix (str): 最后生成的特征名称前缀

        Returns:
            pd.DataFrame: 返回带客户号的特征列表
        """
        # 按基础函数聚合
        raw_pd_2 = raw_pd.groupby('cst_id') \
            .agg({'tran_amount_count': func_list \
                     , 'tran_amount_sum': func_list \
                     , 'tran_banlance_sum': func_list})
        ##调整pd.DataFrame并重命名列名

        rename_dict = {}
        for col in raw_pd_2.loc(axis=1)['tran_amount_count'].columns:
            rename_dict[col] = feature_name_prefix + '_tran_amount_count_' + col
        res_pd_1 = raw_pd_2.loc(axis=1)['tran_amount_count'].rename(columns=rename_dict).reset_index()

        rename_dict = {}
        for col in raw_pd_2.loc(axis=1)['tran_amount_sum'].columns:
            rename_dict[col] = feature_name_prefix + '_tran_amount_sum_' + col
        res_pd_2 = raw_pd_2.loc(axis=1)['tran_amount_sum'].rename(columns=rename_dict).reset_index()

        rename_dict = {}
        for col in raw_pd_2.loc(axis=1)['tran_banlance_sum'].columns:
            rename_dict[col] = feature_name_prefix + 'tran_banlance_sum' + col
        res_pd_3 = raw_pd_2.loc(axis=1)['tran_banlance_sum'].rename(columns=rename_dict).reset_index()
        res_pd = cls.merge_pd_4_list([res_pd_1, res_pd_2, res_pd_3])
        return res_pd

    @classmethod
    def get_feature_4_col_1(cls, raw_pd, func_list, col_name, feature_name_prefix='col_name'):
        """根据指定的col_name进行过滤，然后根据客户号cst_id与 指定col_name 进行分组后，根据生成的序列生成特征

        Args:
            raw_pd (pd.DataFrame): 客户号 与 指定col_name 以及 交易金额字段 tran_amount
            func_list (list): 需要对序列处理的基础函数列表
            col_name (string): 指定的col_name
            feature_name_prefix (str, optional): 最后生成的特征名称前缀. Defaults to 'col_name'.

        Returns:
            _type_: _description_
        """
        process_pd = cls.data_process_1(raw_pd, col_name)
        if feature_name_prefix == 'col_name':
            feature_name_prefix = col_name
        res_pd = cls.get_feature_4_basefunc(process_pd, func_list, feature_name_prefix)
        return res_pd

    @staticmethod
    def merge_pd_4_list(pd_list, merge_method='left', on='partyid_casedate'):
        """根据pd.DataFrame list  merge所以pd

        Args:
            pd_list (list): 需要merge的pd_list，第一个元素为主表
            merge_method (str, optional): merge的方法. Defaults to 'left'.
            on (str, optional): 主键. Defaults to 'cst_id'.

        Returns:
            pd.DataFrame: _description_
        """
        if len(pd_list) < 2:
            print('pd1ist 长度必须大于 1，当前长度%s' % str(len(pd_list)))
            return
        res_pd = reduce(lambda x, y: pd.merge(x, y, on=on, how=merge_method), pd_list)
        return res_pd

    @classmethod
    def get_feature_4_base(cls, raw_pd, func_list, feature_name_prefix, col_list=None):
        # 特按基础函数聚合
        base_col_dict = {}
        if col_list:
            for col in col_list:
                base_col_dict[col] = func_list
        else:
            col_list = list(raw_pd.columns)[1:]
            for col in col_list:
                base_col_dict[col] = func_list

        raw_pd_2 = raw_pd.groupby('cst_id') \
            .agg(base_col_dict)

        # 调整pd.DataFrame并重命名列名
        rename_dict = {}
        res_pd_list = []
        for agg_col in col_list:
            for col in raw_pd_2.loc(axis=1)[agg_col].columns:
                rename_dict[col] = feature_name_prefix + '_' + agg_col + '_' + col
            res_pd_list.append(raw_pd_2.loc(axis=1)[agg_col].rename(columns=rename_dict).reset_index())
            if len(res_pd_list) > 1:
                res_pd = cls.merge_pd_4_list(res_pd_list)
                return res_pd
            else:
                return res_pd_list[0]

    @classmethod
    def get_feature_4_col_2(cls, raw_pd, func_list, col_name, feature_name_prefix='col_name'):
        """根据指定的col_name进行过滤，然后根据客户号khh与指定col_nane进行分组后，根据生成的序列生成特征，只对交易次数进行衍生

        Args:
            raw_pd (pd.DataFrame): 客户号cst_id与指定col_name以及交易金额字段tran_amount
            func_list (1ist): 1ist需要对序列处理的基础函数列表
            col_name (string): string指定的col_name列名
            feature_name_prefix (str, optional): 最后生成的特征名称前缀，默认使用col_name
        Returns:
            pd.DataFrame: 返回带客户号的特征列表
        """
        process_pd = cls.data_process_1(raw_pd, col_name)
        if feature_name_prefix == 'col_name':
            feature_name_prefix = col_name
        res_pd = cls.get_feature_4_base(process_pd[['cst_id', 'tran_amount']], func_list, feature_name_prefix)
        return res_pd

    @classmethod
    def get_feature_4_group_gap(cls, raw_pd, func_list, feature_name_prefix, group_id, gap_col):
        """根据roup_id生成组间间距序列，并基于基础函数序列生成特征。
        @param raw_pd:pd.DataFrame按照客户号与对应特征种类生成的基础数据序列，必须包含分组group.1d与计算间距的列
        @param func_1ist:1ist需要对序列处理的基础函数列表
        eparam feature-.name_prefix:string最后生成的特征名称前缀
        @param group_id:string分组ID
        @param gap._col:string组间间距初始序列
        @return res_.pdpd.DataFrame返回带客户号的特征列表
        """
        raw_pd['lag_%s' % group_id] = raw_pd[group_id].shift(1)
        raw_pd['lag_%s' % gap_col] = raw_pd[gap_col].shift(1)
        # 生成序列
        raw_filter_pd = raw_pd[
            (raw_pd['lag_%s' % group_id].notna()) & (raw_pd[group_id] != raw_pd['lag_%s' % group_id])]
        raw_filter_pd['%s_gap' % feature_name_prefix] = raw_filter_pd['lag_%s' % gap_col] - raw_filter_pd[gap_col]
        raw_filter_pd = raw_filter_pd.drop(['lag_%s' % group_id, 'lag_%s' % gap_col], axis=1)
        res_pd = cls.get_feature_4_base(raw_filter_pd, func_list, feature_name_prefix, ['%s_gap' % feature_name_prefix])
        return res_pd

    @staticmethod
    def reduce_mem_usage(df):
        """功能说明：降低Pandas DataFrame内存使用率
        """
        start_mem = df.memory_usage().sum() / 1024 ** 2
        numerics = ['int16', 'int32', 'int64', 'float16', 'float32', 'float64']
        for col in df.columns:
            col_type = df[col].dtypes
            if col_type in numerics:
                c_min = df[col].min()
                c_max = df[col].max()
                if str(col_type)[:3] == 'int':
                    if c_min > np.iinfo(np.int8).min and c_max < np.iinfo(np.int8).max:
                        df[col] = df[col].astype(np.int8)
                    elif c_min > np.iinfo(np.int16).min and c_max < np.iinfo(np.int16).max:
                        df[col] = df[col].astype(np.int16)
                    elif c_min > np.iinfo(np.int32).min and c_max < np.iinfo(np.int32).max:
                        df[col] = df[col].astype(np.int32)
                    elif c_min > np.iinfo(np.int64).min and c_max < np.iinfo(np.int64).max:
                        df[col] = df[col].astype(np.int64)
            else:
                if c_min > np.finfo(np.float16).min and c_max < np.finfo(np.float16).max:
                    df[col] = df[col].astype(np.float16)
                elif c_min > np.finfo(np.float32).min and c_max < np.finfo(np.float32).max:
                    df[col] = df[col].astype(np.float32)
                else:
                    df[col] = df[col].astype(np.float64)

        end_mem = df.memory_usage().sum() / 1024 ** 2
        print('Memory usage after optimization is : {:.2f}MB'.format(end_mem))
        print('Decreased by {:.1f}%'.format(100 * (start_mem - end_mem) / start_mem))
        return df

    @staticmethod
    def check_col(columns, col_list):
        """功能说明：检查columns里面是否包含col_list里面的名称
        """
        return len(set(columns) & set(col_list)) == len(set(col_list))

    @classmethod
    def get_top_n_recall(cls, res_df, top_n):
        """ 功能说明：获取res_df top_n的召回率，精准率
            @param res_.dfpd.DataFrame模型预测的结果表必须包含的列[cst_id,score,
            label']
            label上报或排除  score 模型预测得分
            @param top_nint/float f1oat时必须小于1表示top占比，int时表示前多少行
            @return pd.DataFrame top_n,recall,precision
        """
        if cls.check_col(res_df.columns.tolist(), ['cst_id', 'score', 'label']) == False:
            print("必须包含【'cst_id','score','label']!")
            return pd.DataFrame()
        res_reindex_df = res_df.sort_values(['score'], ascending=False).reset_index(drop=True)
        top_n_index = 0
        if top_n > 1:
            top_n_index = int(top_n)
        else:
            top_n_index = int(len(res_df.index) * top_n)
        top_n_pd = res_reindex_df.iloc[:top_n_index, :]
        recall = round(len(top_n_pd[top_n_pd['label'].isin([1])]) / len(res_df[res_df['label'].isin([1])]), 4)
        precision = round(len(top_n_pd[top_n_pd['label'].isin([1])]) / len(top_n_pd), 4)
        return pd.DataFrame.from_dict({'top_n': [top_n], 'recall': [recall], 'precision': [precision]})

    @classmethod
    def get_khh_jyls(cls, spark, dt, khh_li, jyls_cumsum_bool, select_cols, table_dict):
        """ 
        功能说明：获取所给客户列表和字段列表的90天交易流水
        @param spark SparkSession初始化的SparkSession
        @param dt抽取的交易流水表中间表的时间分区
        @param khh_li list所需的客户列表
        @param jyls_.cumsum_bool:boo1客户列表累计的历史交易数据量  是否超过最大行数
        @param select_cols: list所需字段列表
        @param table_dict: dict sql语句中需要替换的表名字典
        @return res_pd: pd.DataFrame所需交易流水表
        """
        cust_pd = pd.DataFrame()
        cust_pd['cst_id'] = khh_li
        cust_df = spark.createDataFrame(cust_pd)
        cust_df.registerTempTable("cust_tb")
        select_cols_str = ','.join(select_cols)
        sql_str_2 = '''
        select a.* from
            (select cst_id,%s from priv_app_aicloud.kypf_report_zs_90_jyls where dt ='%s') as a
        inner join cust_tb as b
        on a.khh = b.khh
        ''' % (select_cols_str, dt)

        sql_str_2 = cls.table_replace(sql_str_2, table_dict)
        if jyls_cumsum_bool:
            res_pd = cls.toPandas(spark.sql(sql_str_2), 30)
        else:
            res_pd = spark.sql(sql_str_2).toPandas()
        return res_pd

    # 分布式优化toPandas
    @staticmethod
    def _map_to_pandas(rdds):
        return [pd.DataFrame(list(rdds))]

    @classmethod
    def toPandas(cls, df, n_partitions=None):
        if n_partitions is not None: df = df.repartition(n_partitions)
        df_pand = df.rdd.mapPartitions(cls._map_to_pandas).collect()
        df_pand = pd.concat(df_pand)
        df_pand.columns = df.columns
        return df_pand

    @staticmethod
    def table_replace(sql_str, table_dict):
        """功能说明：替换表名
        Args:
            sql_str (string): 需要替换的SQL语句
            tablie_dict :SQL语句中需要替换的表名字典
            return：sql_str_r(string) ： 替换完表名的sql语句
        """
        for k, v in table_dict.items():
            sql_str = sql_str.replace(k, v)
        return sql_str

```



##### CODE_SPLIT

######  features_order

```python
import sys
import pandas as pd
from sklearn.model_selection import train_test_split, RandomizedSearchCV
import lightgbm as lgb

'''
使用sys.argv获取命令行输入的参数，将其作为文件路径，
通过pandas读取CSV格式的数据文件，将其存储在名为data的变量中。
'''
path = sys.argv
data = pd.read_csv(path[1])

# data=pd.read_csv("../data/train_feature.csv")

'''
使用sklearn.model_selection中的train_test_split函数，
将数据集data按照7:3的比例分割成训练集和测试集。
其中，x_train和x_test分别为训练集和测试集的特征矩阵，
y_train和y_test分别为训练集和测试集的标签。
data.iloc[:, 2:]表示选取data数据集的第3列至最后一列作为特征矩阵，
data[['label']]表示选取data数据集的label列作为标签。
'''
x_train, x_test, y_train, y_test = train_test_split(data.iloc[:, 2:], data[['label']], test_size=0.3, random_state=1)

'''
定义了一个字典search_params，包含了待优化的超参数及其范围。
num_leaves、n_estimators和min_data_in_leaf分别表示决策树的叶子节点数、弱学习器数量和叶子节点最少包含的样本数,learning_rate则表示学习率
'''
search_params = {
    'num_leaves': range(200, 500, 20),
    'n_estimators': range(200, 500, 20),
    'min_data_in_leaf': range(100, 200, 20),
    'learning_rate': [0.01, 0.05, 0.1, 0.2, 0.3]
}

'''
定义了一个字典other_params，包含了模型的其他超参数。
具体来说，objective表示模型的目标函数为二分类问题；
boosting_type表示使用GOSS算法提升模型性能；
max_depth表示每棵决策树的最大深度为4；
is_unbalance则表示数据不均衡。
'''
other_params = {
    'objecttive': 'binary',
    'boosting_type': 'goss',
    'max_depth': 4,
    'is_unbalance': 'True'
}

'''
这段代码使用lightgbm模型库中的LGBMClassifier函数，
创建了一个基础模型gbm，并使用随机搜索法（RandomizedSearchCV）对其超参数进行优化。
具体来说，n_iter=30表示随机搜索迭代30次；
scoring='precision'表示评价指标为精确率；
cv=3表示使用3折交叉验证。最后，将最优超参数打印输出。
'''
gbm = lgb.LGBMClassifier(**other_params)
optimized_GBM = RandomizedSearchCV(gbm, search_params, n_iter=30, scoring='precision', cv=3, n_jobs=12)
optimized_GBM.fit(x_train, y_train)
optimized_GBM.best_params_

'''
使用字典params将基础模型的超参数与最优超参数合并，
并使用LGBMClassifier函数创建最优模型best_gbm。
然后，使用fit函数将训练集x_train和y_train输入到最优模型中进行训练，生成模型model。
'''
params = dict(other_params, **optimized_GBM.best_params_)
best_gbm = lgb.LGBMClassifier(**params)
model = best_gbm.fit(x_train, y_train)

'''
创建一个名为feature_importance_pd的DataFrame对象，
其中col_name列为特征名称，importance列为对应特征的重要性值。
然后，将训练好的模型model中的特征重要性值按从大到小排序，
并将结果保存在feature_importance_pd中。
最后，将feature_importance_pd保存为CSV格式的文件，存储在../data/feature_level.csv路径下。
'''
feature_importance_pd = pd.DataFrame()
feature_importance_pd['col_name'] = x_train.columns
feature_importance_pd['importance'] = model.feature_importances_
feature_importance_pd.sort_values('importance', ascending=False)

feature_importance_pd.to_csv('../data/feature_level.csv', index=False)

```



###### Get_features

```python
from code_tool.BasicFunc import BasicFunc
from code_tool.MyUtils import MyUtils
import time
from datetime import datetime


class Getfeatures(object):
    def __init__(self) -> None:
        pass

    def get_features_100(data):
        res_raw_pd = data[['partyid_casedate', 'receive_pay_cd', 'opp_acct_num', 'cnt_amt',
                           'opp_name', 'channel', 'cash_trans_flag']]
        res_raw_pd['je_num'] = res_raw_pd['cnt_amt'].astype(float)
        khh_pd = res_raw_pd[['partyid_casedate']].drop_duplicates()

        # dfkhmc对方客户名称
        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 1]
        res_pd_temp_1 = MyUtils.data_process_2(raw_in_pd, 'opp_name', 'je_num', ['sum'])
        res_pd_1_1 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'dfkhmc_je_num_sum_get_max_y'}). \
            reset_index()
        # 全量
        res_pd_temp_1 = MyUtils.data_process_2(res_raw_pd, 'opp_name', 'je_num', ['sum'])
        res_pd_1_2 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'dfkhmc_je_num_sum_get_max_x'}). \
            reset_index()

        # channel交易渠道出帐
        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        res_pd_temp_1 = MyUtils.data_process_2(raw_in_pd, 'channel', 'je_num', ['sum'])
        res_pd_1_3 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'channel_je_num_sum_get_max_y'}). \
            reset_index()
        # channel交易渠道
        res_pd_temp_1 = MyUtils.data_process_2(res_raw_pd, 'channel', 'je_num', ['sum'])
        res_pd_1_4 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'channel_je_num_sum_get_max_x'}). \
            reset_index()

        # cash_trans_flag现金转账出帐
        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        res_pd_temp_1 = MyUtils.data_process_2(raw_in_pd, 'cash_trans_flag', 'je_num', ['sum'])
        res_pd_1_5 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'cash_trans_flag_je_num_sum_get_max_y'}). \
            reset_index()
        # cash_trans_flag现金转账出帐
        res_pd_temp_1 = MyUtils.data_process_2(res_raw_pd, 'cash_trans_flag', 'je_num', ['sum'])
        res_pd_1_6 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_max]}).loc(axis=1)['je_num_sum']. \
            rename(columns={'get_max': 'cash_trans_flag_je_num_sum_get_max_x'}). \
            reset_index()

        res_pd_1 = MyUtils.merge_pd_4_list(
            [khh_pd, res_pd_1_1, res_pd_1_2, res_pd_1_3, res_pd_1_4, res_pd_1_5, res_pd_1_6])

        # dfzh_je_num_count_get_25_per_x
        #
        #
        res_raw_pd = data[['partyid_casedate', 'receive_pay_cd', 'opp_acct_num', 'cnt_amt', 'amt_val', 'opp_isparty']]
        res_raw_pd['je_num'] = res_raw_pd['cnt_amt'].astype(float)
        res_raw_pd['zhye_num'] = res_raw_pd['amt_val'].astype(float)

        # 全量数据
        res_pd_temp_1 = MyUtils.data_process_2(res_raw_pd, 'opp_acct_num', 'je_num', ['count'])
        # aaa13 19
        res_pd_6_1 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_25_per, BasicFunc.get_50_per]}).loc(axis=1)['je_num_count']. \
            rename(
            columns={'get_25_per': 'dfzh_je_num_count_get_25_per_x', 'get_50_per': 'dfzh_je_num_count_get_50_per_x'}). \
            reset_index()

        # 出帐数据
        raw_out_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        res_pd_temp_1 = MyUtils.data_process_2(raw_out_pd, 'opp_acct_num', 'je_num', ['count'])
        res_pd_6_2 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_50_per]}).loc(axis=1)['je_num_count']. \
            rename(columns={'get_50_per': 'dfzh_je_num_count_get_50_per'}). \
            reset_index()
        res_pd_6 = MyUtils.merge_pd_4_list([khh_pd, res_pd_6_1, res_pd_6_2])

        # 入账数据
        # wh_zhye_num_get_kur_y 
        #
        #

        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 1]
        # 我行客户
        res_pd_temp_3 = res_raw_pd[res_raw_pd['opp_isparty'] == 1]
        res_pd_temp_4 = res_pd_temp_3.groupby('partyid_casedate'). \
            agg({'zhye_num': [BasicFunc.get_kur], 'je_num': [BasicFunc.get_max, BasicFunc.get_max_gap]})

        res_pd_7_4 = res_pd_temp_4.loc(axis=1)['je_num'].rename(
            columns={'get_amx': 'wh_je_num_get_max_y', 'get_mx_gap': 'wh_je_num_get_max_gap_y'}).reset_index()

        # 非我行客户
        """ 
        以下两行可能出现错误
        """
        res_pd_temp_5 = res_raw_pd[res_raw_pd['opp_isparty'] != 1]
        res_pd_temp_6 = res_pd_temp_5.groupby('partyid_casedate'). \
            agg({'zhye_num': [BasicFunc.get_25_per], 'je_num': [BasicFunc.get_25_per]})

        res_pd_7_5 = res_pd_temp_6.loc(axis=1)['zhye_num'].rename(
            columns={'get_25_per': 'fwh_zhye_num_get_25_per_y'}).reset_index()
        res_pd_7_6 = res_pd_temp_6.loc(axis=1)['je_num'].rename(
            columns={'get_25_per': 'fwh_je_num_get_25_per_y'}).reset_index()

        # 出帐数据
        # wh_zhye_num_get_kur
        #
        #
        raw_out_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        # 我行客户
        res_pd_temp_7 = res_raw_pd[res_raw_pd['opp_isparty'] == 1]
        res_pd_temp_8 = res_pd_temp_7.groupby('partyid_casedate'). \
            agg({'zhye_num': [BasicFunc.get_kur], 'je_num': [BasicFunc.get_skew, BasicFunc.get_kur]})

        res_pd_7_7 = res_pd_temp_8.loc(axis=1)['zhye_num'].rename(
            columns={'get_kur': 'wh_zhye_num_get_kur'}).reset_index()
        res_pd_7_8 = res_pd_temp_8.loc(axis=1)['je_num'].rename(
            columns={'get_skew': 'wh_je_num_get_skew', 'get_kur': 'wh_je_num_get_kur'}).reset_index()

        # 非我行客户
        res_pd_temp_9 = res_raw_pd[res_raw_pd['opp_isparty'] != 1]
        res_pd_temp_10 = res_pd_temp_9.groupby('partyid_casedate'). \
            agg({'je_num': [BasicFunc.get_75_per, BasicFunc.get_4_dis, BasicFunc.get_avg]})

        res_pd_7_9 = res_pd_temp_10.loc(axis=1)['je_num']. \
            rename(columns={'get_75_per': 'fwh_je_num_get_75_per', 'get_4_dis': 'fwh_je_num_get_4_dis',
                            'get_avg': 'fwh_je_num_get_avg'}). \
            reset_index()

        res_pd_7 = MyUtils.merge_pd_4_list(
            [khh_pd, res_pd_7_4, res_pd_7_5, res_pd_7_6, res_pd_7_7, res_pd_7_8, res_pd_7_9])

        # dg_je_num_get_max_y
        #
        #
        #
        #
        res_raw_pd = data[['partyid_casedate', 'receive_pay_cd', 'opp_party_class_cd', 'cnt_amt', 'amt_val']]
        res_raw_pd['je_num'] = res_raw_pd['cnt_amt'].astype(float)
        res_raw_pd['zhye_num'] = res_raw_pd['amt_val'].astype(float)

        # 出帐数据
        raw_out_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        ## 对公客户
        res_pd_temp_1 = raw_out_pd[raw_out_pd['opp_party_class_cd'].isin(['C'])]
        # aaa
        res_pd_8_1 = res_pd_temp_1.groupby('partyid_casedate'). \
            agg({'je_num': [BasicFunc.get_max, BasicFunc.get_4_dis]}).loc(axis=1)['je_num']. \
            rename(columns={'get_max': 'dg_je_num_get_max', 'get_4_dis': 'dg_je_num_get_4_dis'}). \
            reset_index()

        # 入账数据
        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 1]
        ## 对公客户
        res_pd_temp_4 = raw_in_pd[raw_in_pd['opp_party_class_cd'].isin(['C'])]
        # aaa
        res_pd_8_4 = res_pd_temp_4.groupby('partyid_casedate'). \
            agg({'je_num': [BasicFunc.get_max, BasicFunc.get_max_gap]}).loc(axis=1)['je_num']. \
            rename(columns={'get_max': 'dg_je_num_get_max_y', 'get_max_gap': 'dg_je_num_get_max_gap_y'}). \
            reset_index()

        res_pd_8 = MyUtils.merge_pd_4_list([khh_pd, res_pd_8_1, res_pd_8_4])

        # date_group_ser_je_num_sum_get_avg
        #
        #
        #
        res_raw_pd = data[['partyid_casedate', 'receive_pay_cd', 'cnt_amt', 'amt_val', 'tx_dt', 'dt_time', 'case_date']]
        res_raw_pd['je_num'] = res_raw_pd['cnt_amt'].astype(float)
        res_raw_pd['zhye_num'] = res_raw_pd['amt_val'].astype(float)
        res_raw_pd['receive_pay_cd'] = res_raw_pd['receive_pay_cd'].astype(int)

        # 基于交易方向分组
        raw_sort_pd = res_raw_pd.sort_values(['partyid_casedate', 'tx_dt', 'dt_time'], ascending=True)
        raw_sort_pd['group_id'] = raw_sort_pd.groupby('partyid_casedate')['receive_pay_cd'].transform(
            BasicFunc.generate_group_id)
        raw_sort_pd['date_group'] = raw_sort_pd['tx_dt'] + raw_sort_pd['group_id'].apply(lambda x: '_' + str(x))

        raw_group_pd = MyUtils.data_process_2(raw_sort_pd[['partyid_casedate', 'date_group', 'je_num']], 'date_group',
                                              'je_num', ['sum'])
        raw_group_pd = raw_group_pd.sort_values(['partyid_casedate', 'date_group'], ascending=True)
        raw_ser_pd = raw_group_pd[['partyid_casedate', 'je_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_ser)
        raw_ser_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_1_tmp = raw_ser_pd.groupby('partyid_casedate').agg({'je_num_sum': [BasicFunc.get_avg]})
        res_pd_16_1 = res_pd_16_1_tmp.loc(axis=1)['je_num_sum'].rename(
            columns={'get_avg': 'date_group_ser_je_num_get_avg'}).reset_index()

        # 入账数据
        raw_in_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 1]
        ##获取交易时间戳
        raw_in_pd['ts'] = raw_in_pd[['dt_time']].apply(
            lambda x: time.mktime(time.strptime(x['dt_time'], '%Y/%m/%d %H:%M:%S')), axis=1)
        raw_in_pd['days'] = raw_in_pd[['tx_dt', 'case_date']]. \
            apply(
            lambda x: (datetime.strptime(x['case_date'], '%Y/%m/%d') - datetime.strptime(x['tx_dt'], '%Y/%m/%d')).days,
            axis=1)

        # 按小时分组
        raw_in_pd['hour'] = raw_in_pd['dt_time'].apply(lambda x: int(x[11:13]))
        # 每小时分组
        raw_in_pd['hours_1_group'] = raw_in_pd[['partyid_casedate', 'days', 'hour']]. \
            apply(lambda x: str(x['partyid_casedate']) + "_" + str(x['days']) + "_" + str(x['hour']), axis=1)
        #
        gap_group = raw_in_pd[['partyid_casedate', 'hours_1_group', 'ts']]. \
            groupby(['partyid_casedate', 'hours_1_group'])['ts']. \
            agg(BasicFunc.get_max_gap). \
            reset_index()
        res_pd_16_4 = gap_group.groupby('partyid_casedate'). \
            agg({'ts': [BasicFunc.get_90_per]}).loc(axis=1)['ts']. \
            rename(columns={'get_90_per': 'hours_1_group_gap_ts_get_90_per_y'}). \
            reset_index()

        # 获取交易时间戳
        res_raw_pd['ts'] = res_raw_pd[['dt_time']].apply(
            lambda x: time.mktime(time.strptime(x['dt_time'], '%Y/%m/%d %H:%M:%S')), axis=1)
        ##计算交易日期与dt之间的时间差
        res_raw_pd['days'] = raw_in_pd[['tx_dt', 'case_date']]. \
            apply(
            lambda x: (datetime.strptime(x['case_date'], '%Y/%m/%d') - datetime.strptime(x['tx_dt'], '%Y/%m/%d')).days,
            axis=1)
        ##每15天分组
        res_raw_pd['days_15_group'] = res_raw_pd[['partyid_casedate', 'days']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days'] // 15), axis=1)
        res_raw_pd = res_raw_pd.sort_values(['partyid_casedate', 'days_15_group'], ascending=True)
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'days_15_group', 'je_num']],
                                              'days_15_group', 'je_num', ['count'])
        res_pd_16_7 = raw_group_pd.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_cv]}).loc(axis=1)['je_num_count']. \
            rename(columns={'get_cv': 'days_15_group_je_num_count_get_cv_x'}). \
            reset_index()

        raw_diff_pd = raw_group_pd[['partyid_casedate', 'je_num_count']].groupby('partyid_casedate').transform(
            BasicFunc.generate_diff)
        raw_diff_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_8 = raw_diff_pd.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_avg]}). \
            loc(axis=1)['je_num_count']. \
            rename(columns={'get_avg': 'days_15_group_diff_je_num_count_get_avg_x'}). \
            reset_index()

        raw_pct_pd = raw_group_pd[['partyid_casedate', 'je_num_count']].groupby('partyid_casedate').transform(
            BasicFunc.generate_pct)
        raw_pct_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_9_tmp = raw_pct_pd.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_10_per, BasicFunc.get_75_per, BasicFunc.get_avg]})
        res_pd_16_9 = res_pd_16_9_tmp.loc(axis=1)['je_num_count']. \
            rename(columns={'get_10_per': 'days_15_group_pct_je_num_count_get_10_per',
                            'get_75_per': 'days_15_group_pct_je_num_count_get_75_per',
                            'get_avg': 'days_15_group_pct_je_num_count_get_avg'}). \
            reset_index()

        #
        #
        res_raw_pd['hour'] = res_raw_pd['dt_time'].apply(lambda x: int(x[11:13]))
        res_raw_pd['minute'] = res_raw_pd['dt_time'].apply(lambda x: int(x[14:16]))
        # 每1分钟分组
        res_raw_pd['minute_1_group'] = res_raw_pd[['partyid_casedate', 'days', 'hour', 'minute']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days']) + '_' + str(x['hour']) + str(x['minute']),
                  axis=1)
        res_raw_pd = res_raw_pd.sort_values(['partyid_casedate', 'minute_1_group'], ascending=True)
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'minute_1_group', 'zhye_num']],
                                              'minute_1_group', 'zhye_num', ['sum'])
        raw_diff_pd = raw_group_pd[['partyid_casedate', 'zhye_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_diff)
        raw_diff_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_12 = raw_diff_pd.groupby('partyid_casedate'). \
            agg({'zhye_num_sum': [BasicFunc.get_avg_gap, BasicFunc.get_max_gap, BasicFunc.get_std]}). \
            loc(axis=1)['zhye_num_sum']. \
            rename(columns={'get_avg_gap': 'minute_1_group_diff_zhye_num_sum_get_avg_gap_x',
                            'get_max_gap': 'minute_1_group_diff_zhye_num_sum_get_max_gap_x',
                            'get_std': 'minute_1_group_diff_zhye_num_sum_get_std_x'}). \
            reset_index()

        # 每1天分组
        res_raw_pd['day_1_group'] = res_raw_pd[['partyid_casedate', 'days']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days']), axis=1)
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'day_1_group', 'zhye_num']],
                                              'day_1_group', 'zhye_num', ['sum'])
        res_pd_16_15 = raw_group_pd.groupby('partyid_casedate'). \
            agg({'zhye_num_sum': [BasicFunc.get_min]}). \
            loc(axis=1)['zhye_num_sum']. \
            rename(columns={'get_avg_gap': 'day_1_group_zhye_num_sum_get_min_x'}). \
            reset_index()

        # 每小时分组
        res_raw_pd['hours_1_group'] = res_raw_pd[['partyid_casedate', 'days', 'hour']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days']) + '_' + str(x['hour']), axis=1)
        gap_group = res_raw_pd[['partyid_casedate', 'hours_1_group', 'ts']]. \
            groupby(['partyid_casedate', 'hours_1_group'])['ts'].agg(BasicFunc.get_max_gap).reset_index()
        res_pd_16_17 = gap_group.groupby('partyid_casedate'). \
            agg({'ts': [BasicFunc.get_75_per, BasicFunc.get_90_per, BasicFunc.get_4_dis]}). \
            loc(axis=1)['ts']. \
            rename(columns={'get_75_per': 'hours_1_group_gap_ts_get_75_per_x',
                            'get_90_per': 'hours_1_group_gap_ts_get_90_per_x',
                            'get_4_dis': 'hours_1_group_gap_ts_get_4_dis_x'}). \
            reset_index()

        # 每三天分组
        res_raw_pd['days_3_group'] = res_raw_pd[['partyid_casedate', 'days']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days'] // 3), axis=1)
        res_raw_pd = res_raw_pd.sort_values(['partyid_casedate', 'days_3_group'], ascending=True)
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'days_3_group', 'je_num']],
                                              'days_3_group', 'je_num', ['sum'])
        raw_ser_pd = raw_group_pd[['partyid_casedate', 'je_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_ser)
        raw_ser_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_19 = raw_ser_pd.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_75_per]}). \
            loc(axis=1)['je_num_sum']. \
            rename(columns={'get_75_per': 'days_3_group_ser_je_num_sum_get_75_per_x'}). \
            reset_index()

        # 每7天分组
        res_raw_pd['days_7_group'] = res_raw_pd[['partyid_casedate', 'days']]. \
            apply(lambda x: x['partyid_casedate'] + '_' + str(x['days'] // 7), axis=1)
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'days_7_group', 'je_num']],
                                              'days_7_group', 'je_num', ['sum'])
        raw_ser_pd = raw_group_pd[['partyid_casedate', 'je_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_ser)
        raw_ser_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_23 = raw_ser_pd.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_75_per, BasicFunc.get_50_per, BasicFunc.get_4_dis]}). \
            loc(axis=1)['je_num_sum']. \
            rename(columns={'get_75_per': 'days_7_group_ser_je_num_sum_get_75_per_x',
                            'get_50_per': 'days_7_group_ser_je_num_sum_get_50_per_x',
                            'get_4_dis': 'days_7_group_ser_je_num_sum_get_4_dis_x'}). \
            reset_index()
        raw_diff_pd = raw_group_pd[['partyid_casedate', 'je_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_diff)
        raw_diff_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_24 = raw_diff_pd.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_avg]}). \
            loc(axis=1)['je_num_sum']. \
            rename(columns={'get_avg': 'days_7_group_diff_je_num_sum_get_avg_x'}). \
            reset_index()

        # 出帐数据
        raw_out_pd = res_raw_pd[res_raw_pd['receive_pay_cd'] == 2]
        raw_out_pd = raw_out_pd.sort_values(['partyid_casedate', 'days_15_group'], ascending=True)
        raw_group_pd = MyUtils.data_process_2(raw_out_pd[['partyid_casedate', 'days_15_group', 'je_num']],
                                              'days_15_group', 'je_num', ['sum'])
        raw_diff_pd = raw_group_pd[['partyid_casedate', 'je_num_sum']].groupby('partyid_casedate').transform(
            BasicFunc.generate_diff)
        raw_diff_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_28 = raw_diff_pd.groupby('partyid_casedate'). \
            agg({'je_num_sum': [BasicFunc.get_kur, BasicFunc.get_avg]}). \
            loc(axis=1)['je_num_sum']. \
            rename(columns={'get_kur': 'days_15_group_diff_je_num_sum_get_kur',
                            'get_avg': 'days_15_group_diff_je_num_sum_get_avg'}). \
            reset_index()

        #
        raw_group_pd = MyUtils.data_process_2(res_raw_pd[['partyid_casedate', 'minute_1_group', 'je_num']],
                                              'minute_1_group', 'je_num', ['count'])
        raw_diff_pd = raw_group_pd[['partyid_casedate', 'je_num_count']].groupby('partyid_casedate').transform(
            BasicFunc.generate_diff)
        raw_diff_pd['partyid_casedate'] = raw_group_pd['partyid_casedate']
        res_pd_16_30 = raw_diff_pd.groupby('partyid_casedate'). \
            agg({'je_num_count': [BasicFunc.get_avg_gap, BasicFunc.get_std]}). \
            loc(axis=1)['je_num_count']. \
            rename(columns={'get_avg_gap': 'minute_1_group_diff_je_num_count_get_avg_gap_x',
                            'get_std': 'minute_1_group_diff_je_num_count_get_std_x'}). \
            reset_index()
        #
        gap_group = res_raw_pd[['partyid_casedate', 'minute_1_group', 'ts']]. \
            groupby(['partyid_casedate', 'minute_1_group'])['ts'].agg(BasicFunc.get_max_gap).reset_index()
        res_pd_16_33 = gap_group.groupby('partyid_casedate'). \
            agg({'ts': [BasicFunc.get_std]}). \
            loc(axis=1)['ts']. \
            rename(columns={'get_75_per': 'minute_1_group_gap_ts_get_std_x'}). \
            reset_index()

        # 合并res_16结果
        res_16_li = [res_pd_16_1, res_pd_16_4, res_pd_16_7, res_pd_16_8, res_pd_16_9, res_pd_16_9, res_pd_16_12,
                     res_pd_16_15, res_pd_16_17,
                     res_pd_16_19, res_pd_16_23, res_pd_16_24, res_pd_16_28, res_pd_16_30, res_pd_16_33]

        res_pd_16 = MyUtils.merge_pd_4_list([khh_pd] + res_16_li)

        # 合并所以结果
        res_li = [res_pd_1, res_pd_6, res_pd_7, res_pd_8, res_pd_16]
        res_pd = MyUtils.merge_pd_4_list([khh_pd] + res_li)

        return res_pd

```



###### get_features_for_model

```python
import pandas as pd
import numpy as np

from Get_Features import Getfeatures
import warnings

warnings.filterwarnings('ignore')
# os.environb['NUMEXPR_MAX_THREADS']='8'


in_data = pd.read_csv("../data/train.csv", sep=',')
label = in_data['label']
in_data = in_data.drop(columns='label')
in_data['partyid_casedate'] = in_data['case_date'] + '_' + in_data['party_id']
in_data = in_data[['partyid_casedate', 'party_id', 'receive_pay_cd', 'opp_acct_num', 'cnt_amt',
                   'opp_name', 'amt_val', 'opp_isparty', 'opp_party_class_cd', 'tx_dt', 'dt_time',
                   'channel', 'cash_trans_flag', 'app_state_cd', 'case_date']]
in_data['partyid_casedate'] = in_data['partyid_casedate'].astype(str)
in_data = in_data.replace('(null)', np.nan).replace('\\N', np.nan)
in_data['amt_val'] = in_data['amt_val'].replace('(null)', np.nan).replace('\\N', np.nan).astype(float)
in_data['cnt_amt'] = in_data['cnt_amt'].replace('(null)', np.nan).replace('\\N', np.nan).astype(float)
in_data['receive_pay_cd'] = in_data['receive_pay_cd'].replace(np.nan, -99999).astype(int)
in_data['opp_isparty'] = in_data['opp_isparty'].replace(np.nan, -99999).astype(int)

khh_list = in_data[['partyid_casedate']].drop_duplicates()

feature_pd = pd.DataFrame()

for i in range(0, len(khh_list), 5000):
    data = pd.merge(khh_list[i:i + 5000], in_data)
    feature_pd_s = Getfeatures.get_features_100(data)
    feature_pd = feature_pd.append(feature_pd_s)

feature_pd.loc[:, 'label'] = label
feature_pd.to_csv('../data/train_feature.csv', index=False)

```



###### get_features_for_pre

```python
import pandas as pd
import numpy as np

from Get_Features import Getfeatures
import warnings
warnings.filterwarnings('ignore')
# os.environb['NUMEXPR_MAX_THREADS']='8'


in_data=pd.read_csv("../data/test.csv")
in_data['partyid_casedate']=in_data['case_date']+'_'+in_data['party_id']
in_data=in_data[['partyid_casedate','party_id', 'receive_pay_cd', 'opp_acct_num', 'cnt_amt',
                'opp_name','amt_val', 'opp_isparty', 'opp_party_class_cd', 'tx_dt', 'dt_time',
                'channel', 'cash_trans_flag', 'app_state_cd', 'case_date']]
in_data['partyid_casedate']=in_data['partyid_casedate'].astype(str)
in_data=in_data.replace('(null)',np.nan).replace('\\N',np.nan)
in_data['amt_val']=in_data['amt_val'].replace('(null)',np.nan).replace('\\N',np.nan).astype(float)
in_data['cnt_amt']=in_data['cnt_amt'].replace('(null)',np.nan).replace('\\N',np.nan).astype(float)
in_data['receive_pay_cd']=in_data['receive_pay_cd'].replace(np.nan,-99999).astype(int)
in_data['opp_isparty']=in_data['opp_isparty'].replace(np.nan,-99999).astype(int)


khh_list=in_data[['partyid_casedate']].drop_duplicates()

feature_pd=pd.DataFrame()

for i in range(0,len(khh_list),10000):
    data=pd.merge(khh_list[i:i+10000],in_data)
    feature_pd_s=Getfeatures.get_features_100(data)
    feature_pd=feature_pd.append(feature_pd_s)

feature_pd.to_csv('../data/test_feature.csv',index=False)
```





