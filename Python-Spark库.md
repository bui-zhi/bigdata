# Python-Spark库



## 概念基础

#### 生态组成

1. **Spark Core**：Spark的核心引擎，提供了分布式任务调度、内存计算和数据抽象功能。它通过弹性分布式数据集（RDD）实现了数据的分布式处理，支持各种类型的操作，包括MapReduce、过滤、聚合等。
2. **Spark SQL**：提供了用于处理结构化数据的SQL接口，使用户可以使用SQL查询语言查询和操作数据。Spark SQL允许将SQL查询与Spark Core的RDD操作结合起来，从而更方便地处理结构化数据。
3. **Spark Streaming**：用于实时数据流处理的模块，支持将持续产生的数据流分成小批次进行处理。它可以与Spark Core无缝集成，使用户能够在实时数据流中执行复杂的数据处理和分析任务。
4. **Spark MLlib**：机器学习库，提供了常见的机器学习算法和工具，包括分类、回归、聚类、推荐等。MLlib利用Spark的分布式计算能力加速了大规模机器学习任务的训练和预测。
5. **Spark GraphX**：用于图形计算和图形处理的模块，提供了构建和操作大规模图形数据的功能。GraphX支持图形算法和图形分析，如PageRank、连通性组件等。



##### 模块对比

Spark Core 和 Spark SQL 是 Spark 中两个核心的模块，两者相辅相成，但在功能和应用场景上有所不同。



​	**Spark Core 支持处理 结构化数据 与 非结构化数据**

​	**Spark SQL  仅支持处理 结构化 数据**





**Spark Core**:

- **功能**：Spark Core 提供了分布式数据处理的基础功能，包括RDD（弹性分布式数据集）的创建、转换和操作，以及任务调度、内存计算等核心功能。
- **应用场景**：适用于需要对大规模数据进行通用性处理的场景，例如MapReduce操作、数据过滤、聚合、排序等。





**Spark SQL**:

- **功能**：Spark SQL 提供了对结构化数据的处理和查询能力，通过SQL语言或DataFrame API来操作数据，底层可以与Spark Core集成，允许在RDD和DataFrame之间进行转换。
- **应用场景**：适用于需要处理结构化数据，进行SQL查询、数据汇总、关联操作等场景，尤其适用于与传统数据库集成或数据仓库查询。





##### **数据类型特点**：

- **Spark Core**：主要处理的是RDD数据类型，RDD是一种分布式的、不可变的数据集，可以包含任意类型的对象。
- **Spark SQL**：主要处理的是DataFrame和DataSet数据类型，DataFrame是一种带有结构的数据集，可以类比于关系型数据库中的表，而DataSet是DataFrame的类型安全版本，提供了更强的类型检查和编译时错误检查。





#### Spark VS Hadoop

由于Apache Spark支持内存计算，并且通过**DAG（有向无环图）**执行引擎支持**无环数据流**，所以官方宣称其在内存中的运算速度要比Hadoop的MapReduce快100倍，在硬盘中要快10倍。



**Spark处理数据与MapReduce处理数据相比，有如下两个不同点：**

​	 Spark处理数据时，可以将中间处理结果数据存储到内存中

​	 Spark 提供了非常丰富的算子(API), 可以做到复杂任务在一个Spark 程序中完成



###### **Hadoop 基于进程计算**

​	**优点**：互相独立，独享资源，监控方便

​	**缺点：**数据不共享，反复加载，浪费内存

​	

​	Hadoop中的MR中每个map/reduce task都是一个java进程方式运行，好处在于进程之间是互相独立的，每个task独享进程资源，没有**互相干扰，监控方便**，但是问题在于task之间**不方便共享数据**，执行效率比较低。比如多个map task读取不同数据源文件需要将数据源加载到每个map task中，造成**重复加载和浪费内存**。





###### **Spark基于线程计算**

​	**优点：**数据共享，计算效率高

​	**缺点**：线程之间存在资源竞争



​	Spark基于线程的方式计算是为了**数据共享和提高执行效率**，Spark采用了线程的最小的执行单位，但缺点是线程之间会有**资源竞争**。







#### StandAlone模式

​	Spark自带的一种集群模式，是真实地在多个机器之间搭建Spark集群的环境，完全可以利用该模式搭建多机器集群，用于实际的大数据处理



​	**StandAlone集群在进程上主要有三类进程**：

​		主节点Master进程：	Master角色, 管理整个集群资源，并托管运行各个任务的Driver	

​		从节点Workers进程： Worker角色, 管理每个机器的资源，分配对应的资源来运行Executor(Task)；每个从节点分配资源信息给Worker管理，资源信息包含内存Memory和CPU Cores核数

​		历史服务器HistoryServer(可选)： Spark Application运行完成以后，保存事件日志数据至HDFS，启动HistoryServer可以查看应用运行相关信息

​			



​	StandAlone 是完整的Spark运行环境,其中:

​		Master角色以Master进程存在, Worker角色以Worker进程存在

​		Driver和Executor运行于Worker进程内, 由Worker提供资源供给它们运行





**application**在集群上运行时，由两部分组成：**Driver Program和Executors**

​	**Driver Program和Executors**

​		**Driver Program：**

​			 相当于AppMaster，整个应用管理者，负责应用中所有Job的调度执行;

​			 运行JVM Process，运行程序的MAIN函数，必须创建SparkContext上下文对象；

​			 一个SparkApplication仅有一个；

​		**Executors：**

​			相当于一个线程池，运行JVM Process，其中有很多线程，每个线程运行一个Task任务，一个Task任务运行需要 1 Core CPU，所有可以认为Executor中线程数就等于CPU Core核数；

​			一个Spark Application可以有多个Executors，可以设置个数和资源信息；













## Spark Core

**spark的三大数据结构**

​	**RDD**：	  	  弹式分布式数据集

​	**累加器**：		分布式共享只写变量

​	**广播变量**：	分布式共享只读变量



​		





### RDD概念知识



​		在Spark中，RDD被表示为对象，通过对象上的方法调用来对RDD进行转换。经过一系列的**transformations（转换算子）**定义RDD之后，就可以调用**actions（行动算子）**触发RDD的计算，action可以是向应用程序返回结果(**count**, **collect**等)，或者是向存储系统保存数据(saveAsTextFile等)。

​		在Spark中，只有遇到action，才会执行RDD的计算(即延迟计算)，这样在运行时可以通过管道的方式传输多个转换。

​	   要使用Spark，开发者需要编写一个Driver程序，它被提交到集群以调度运行Worker。Driver中定义了一个或多个RDD，并调用RDD上的action，Worker则执行RDD分区计算任务。



#### 基础概念



##### 五大特性

​	1:	RDD的分区是RDD数据存储的最小单位，一份RDD数据本质上是分隔成多个分区

​	2:	RDD的方法会作用到其所有的分区上

​	3： RDD之间存在依赖关系（血缘关系）

​	4:	Key- Value型的RDD可以有分区器

​			Key- Value型：RDD中存储的二元元组（只有两个元素的元祖 （"Hadoop",1））

​	5:	RDD在的分区规划，会尽可能靠近数据所在的服务器

​			初始RDD（读取数据）时，分区会尽可能规划到存储数据所在的服务器上，因为这样可以走本地读取，避免网络读取



**本地读取：**Executor所在的服务器，所需读取数据也在同一个Data node上，直接读取硬盘即可

**网络读取：**需要经过网络的传输才能读取到数据   **本地读取性能 > 网络读取性能**



##### 		分区

​	RDD逻辑上是分区的，每个分区的数据是抽象存在的，计算的时候会通过一个compute（计算）函数得到每个分区的数据。

​	如果RDD是通过已有的文件系统构建，则compute函数是读取指定文件系统中的数据，如果RDD是通过其他RDD转换而来，则compute函数是执行转换逻辑将其他RDD的数据进行转换。



##### 		只读

​	**RDD是只读的，要想改变RDD中的数据，只能在现有的RDD基础上创建新的RDD**。

​	



RDD的操作算子包括两类

​	一类叫做transformations（转换算子），它是用来将RDD进行转化，构建RDD的血缘关系；

​	另一类叫做 **actions**   (**count, collect,take**等 行动算子) ，它是用来触发RDD的计算，得到RDD的相关计算结果或者将RDD保存的文件系统中。





#### RDD的特点

​	RDD表示只读分区的数据集，对RDD进行改动，只能通过RDD的转换操作，由一个RDD得到一个新的RDD，新的RDD包含了从其他RDD衍生所必需的信息。

​	RDDs之间存在依赖，RDD的执行是按照血缘关系延时计算的。如果血缘关系较长，可以通过持久化RDD来切断血缘关系。

​	RDD之间进行相互迭代计算（Transformation），当执行开启后，新的RDD生成，代表老的RDD消失

​	RDD的数据是过程数据，只在处理过程中存在，一旦处理完成，被销毁





##### 		弹性

- **存储的弹性**：内存与磁盘的自动切换；
- **容错的弹性**：数据丢失可以自动恢复；
- **计算的弹性**：计算出错重试机制；
- **分片的弹性**：可根据需要重新分片。









##### **运行模式：**

​	本地模式（单机 / local）： 

​			本地模式是以一个独立的进程，通过其内部多个线程模拟整个spark运行时的环境



​	Standalone 模式（集群）：

​			 spark中的各个角色以独立进程的形式存在，并组成了spark集群环境



​	Hadoop YARN 模式 （集群）：

​			Spark中的各个角色运行在YARN的容器内部，并组成Spark集群环境



​	Kubernetes 模式 （容器集群）：

​			Spark中的各个角色运行在Kubernetes 的容器内部，并组成Spark集群环境





##### **Yarn 角色分类：**

​	**资源管理层面**：

​			集群资源管理者（Master）：ResourceManager

​			单机资源管理者（Worker）：Node Manager

​	

​	**任务计算层面**：

​			单任务管理者（Master）：Application Master

​			但任务执行者（Worker）：Task （容器内计算框架的工作角色）





##### 	**PySpark 和 bin/pyspark：**

​		PySpark 			是一个python的类库，提供Spark 的操作API

​		bin/pyspark 	 是一个交互式的程序，可以提供交互式执行Spark 计算

​	









### RDD基本使用



#### 	自定义分区器

​			1：继承 **Partitoner** 类

​			2：重写属性值 **numPartitions**（分区数量）

​			3：重写方法 **getPartition** （分区依据）



```python
from pyspark.conf import SparkConf
from pyspark.context import SparkContext
from pyspark.rdd import RDD, PairwiseRDD
from pyspark import StorageLevel

class MyPartitioner(object):
    def __init__(self, num_partitions=3):
        self.num_partitions = num_partitions

    def numPartitions(self):
        return self.num_partitions

    def getPartition(self, key):
        if key == "nba":
            return 0
        elif key == "wnba":
            return 1
        elif key == "cba":
            return 2
        else:
            return 3

def main():
    sparkConf = SparkConf().setMaster("local").setAppName("-")
    sc = SparkContext(conf=sparkConf)

    rdd_data = [("nba", "xxxxxxxxxx"), ("cba", "xxxxxxxxxx"), ("wnba", "xxxxxxxxxx"), ("xba", "xxxxxxxxxx"), ("pba", "xxxxxxxxxx")]
    rdd: RDD = sc.parallelize(rdd_data)

    # 将RDD转换为K-V格式以便使用分区器
    pair_rdd = rdd.map(lambda x: (x[0], x[1]))

    # 使用自定义分区器
    part_rdd = pair_rdd.partitionBy(MyPartitioner())

    part_rdd.saveAsTextFile("output")

    # 可选：在PySpark 3.0+中，需要显式停止SparkSession
    # 如果使用SparkSession，则改为 spark.stop()
    sc.stop()

if __name__ == "__main__":
    main()

```





#### 程序入口

​	Spark RDD编程的程序入口对象是 **Spark Context** 对象（无论何种编程语言）

​	只有构建出Spark Context，基于它才能执行后续的API调用与计算

​	本质上，Spark Context对编程而言，主要功能就是创建出第一个RDD



``` python
from pyspark import SparkConf,SparkContext


if __name__ == '__main__':
  # 构建 SparkConf 对象
  conf = SparkConf().setAppName("xxx").setMaster("local[*]")
  # 构建 SparkContext 执行环境入口对象
  sc = SparkContext(conf = conf)
```





#### 创建RDD

##### 本地化RDD

​		PySpark 中使用 parallelize 方法创建RDD，将**本地数据**分布化

```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf 类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")

# 基于 SparkConf 类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

rdd1 = sc.parallelize([1, 2, 3, 4, 5])  # 使用列表类型创建RDD
rdd2 = sc.parallelize((1, 2, 3, 4, 5))  # 使用元组类型创建RDD
rdd3 = sc.parallelize({1, 2, 3, 4, 5})  # 使用集合类型创建RDD

# 使用字符串类型创建RDD，此时RDD会将字符串中的字符进行切割，存储每一个单独的字符
rdd4 = sc.parallelize("abcdef")         

# 使用字段类型创建RDD，此时RDD只会保存Key对应的值
rdd5 = sc.parallelize({"key1": "value1", "key2": "value2"})     

# 打印 pyspark 版本
print(sc.version)

# 如需要查看RDD数据集中的内容，则需要使用collect方法收集展示
print(rdd1.collect())
print(rdd2.collect())
print(rdd3.collect())
print(rdd4.collect())
print(rdd5.collect())

# 停止 SparkContext 对象的运行（停止 pyspark 程序）
sc.stop()

```





##### **从外部数据源创建RDD**

PySpark 中使用读取文件的方法创建RDD，textFile从外部数据源创建RDD

textFile / wholeTextFiles / sequenceFile..xxxFile：返回RDD类型数据

read.format('xx').load('xxxxx/xxx/xx.xxx')：返回 Data Frame 类型数据

```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf 类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")

# 基于 SparkConf 类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

rdd = sc.textFile(文件路径)

# textFile(): 读取文本文件，将文件的每一行作为一个字符串元素放入RDD中。
rdd = sc.textFile("hdfs://path/to/file.txt")

# wholeTextFiles(): 读取多个小文本文件，返回键值对RDD，键是文件路径，值是文件内容
rdd = sc.wholeTextFiles("hdfs://path/to/files/*")

# sequenceFile(): 读取Hadoop的SequenceFile，返回键值对RDD，键和值的类型取决于SequenceFile中的实际数据类型。
rdd = sc.sequenceFile("hdfs://path/to/seqfile")

# binaryFiles(): 读取二进制文件，返回键值对RDD，键是文件路径，值是包含文件内容的二进制数据。
rdd = sc.binaryFiles("hdfs://path/to/binaryfiles")





# read方法读取的数据都为 Data Frame 类型
# New in Spark 2.x: DataFrame和Dataset API从多种结构化数据源创建RDD
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName('StructuredDataExample').getOrCreate()

# 读取CSV文件并转换为RDD，读取之后先为Data Frame格式，需通过.rdd方法转换为rdd
csv_df = spark.read.format('csv').load('hdfs://path/to/csvfile.csv')
# 将DataFrame转换为RDD
csv_rdd = csv_df.rdd  

# 其他数据源，例如JSON、Parquet、Avro等
json_df = spark.read.format('json').load('hdfs://path/to/jsonfile.json')
json_rdd = json_df.rdd


# 停止 SparkContext 对象的运行（停止 pyspark 程序）
sc.stop()
```







### RDD具体使用



#### 链式调用

​	对于返回值是新RDD的算子，可以通过链式调用的方法多次调用不同的算子进行计算





#### 迭代器

​	迭代器（Iterator）是一种数据结构，用于遍历集合中的元素，但并不一次性加载所有元素到内存中。在 Spark 中，特别是在处理大规模数据时，迭代器有助于实现高效、内存友好的数据处理。



1. 内存效率： 迭代器允许按需访问集合中的元素，而不是一次性加载所有元素到内存。这对于处理大数据集尤为重要，因为它可以避免一次性加载大量数据导致的内存溢出问题。每次迭代时，仅从数据源获取下一个待处理的元素。
2.  惰性计算： 迭代器遵循“惰性计算”（lazy evaluation）原则，即只有在调用 next() 方法时才会计算并返回下一个元素。这种特性有助于节省计算资源，尤其是当只需访问集合中部分元素时。
3.  不可改变性： 一旦创建了迭代器，它所遍历的集合通常被视为不可变的。这意味着在遍历过程中不能添加、删除或修改元素。这有助于保持数据一致性，尤其是在并发环境下。





#### 算子

##### map算子

 接收一个处理函数，对RDD内的元素逐个处理，并返回一个新的RDD

 可使用 **lambda** 表达式快速编写





```python
# 导包
from pyspark import SparkConf, SparkContext

# 创建SparkConf 类对象
conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")

# 基于 SparkConf 类对象创建SparkContext类对象
sc = SparkContext(conf=conf)

rdd1 = sc.parallelize([1, 2, 3, 4, 5])  # 使用列表类型创建RDD


def func(data):
    return data * 10


rdd2 = rdd1.map(func)
print(rdd2.collect())


# 使用 lambda 表达式实现相同效果
rdd3 = rdd1.map(lambda x: x * 10)
print(rdd3.collect())
```





##### mapPartitions算子

​		mapPartitions ： 对 RDD 的每个分区应用一个用户自定义函数，该函数接收一个迭代器（包含分区内的所有元素）作为输入，并返回一个新的迭代器作为输出。

​		主要用于那些需要在分区级别进行批量处理或者需要在处理过程中维持一些状态的场景。



```python
# 示例场景： 假设有一个包含大量用户点击事件的 RDD，每个元素代表一个点击事件，结构如下：
from collections import namedtuple
# 类似于 Scala 的 case class
ClickEvent = namedtuple('ClickEvent', ['user_id', 'event_time', 'page_url'])

# 示例数据
click_events_rdd = sc.parallelize([
    ClickEvent('user1', '2024-04-13T10:00:00Z', '/home'),
    ClickEvent('user2', '2024-04-13T10:01:00Z', '/about'),
    # ... 更多点击事件 ...
    ClickEvent('user1', '2024-04-13T1.jpg', '/contact'),
])

"""
统计每个用户的最后点击时间，并以 (user_id, last_event_time) 的形式输出。
由于需要在用户层面维护状态（即跟踪每个用户的最新事件时间），而常规的 map 算子无法跨元素保持状态，因此适合使用 mapPartitions 来实现。
"""

from collections import defaultdict
# collections：集合
# defaultdict：默认字典; 字典元素类型初始化

def get_last_click_times(iterator):
    user_last_times = defaultdict(lambda: None)  
# 初始化一个默认字典，用于存储每个用户的最后点击时间

    for event in iterator:
        user_id = event.user_id
        event_time = event.event_time

        if user_last_times[user_id] is None or event_time > user_last_times[user_id]:
            user_last_times[user_id] = event_time

    return [(user_id, last_time) for user_id, last_time in user_last_times.items()]

last_click_times_rdd = click_events_rdd.mapPartitions(get_last_click_times)





"""
函数解析
namedtuple	 
		Python 中的一个内建工具，它位于 collections 模块中，用于创建带有命名字段的简单、轻量级、不可变的数据结构。这种数据结构本质上是一种特殊的元组，但提供了更友好的访问方式和更清晰的语义
		特点：
			命名字段：与普通元组不同，namedtuple 的元素不仅可以通过索引来访问，还可以通过其对应的名称来访问。
			元组的子类：namedtuple 是元组的子类，这意味着它继承了元组的所有基本行为和属性，如不可变性、序列操作（如切片、索引、迭代等）、内置函数（如 len()、hash()、repr() 等）的支持。同时，namedtuple 实例也可以用于任何期望接收元组的地方。
			轻量级：与自定义类相比，namedtuple 不需要定义 __init__、__repr__ 等方法，也不涉及类的实例属性，因此创建和使用 namedtuple 实例通常比使用等效的自定义类更为高效，尤其是在内存和性能敏感的场景下。
			不可变性：如同普通元组，namedtuple 实例一旦创建就不能修改其内容。这确保了数据的安全性，防止意外修改，并使得它们适合作为字典的键或用于并发编程。
			
			
			
	使用方法：创建一个 namedtuple 类型通常通过调用 collections.namedtuple() 工厂函数来完成，该函数接受两个参数：一是要创建的 namedtuple 类型的名称；二是字段名组成的列表或空格分隔的字符串。例如：
	
from collections import namedtuple

# 定义一个名为 ClickEvent 的 namedtuple 类型，包含三个字段：user_id、event_time、page_url
ClickEvent = namedtuple('ClickEvent', ['user_id', 'event_time', 'page_url'])

# 创建 ClickEvent 的实例
click = ClickEvent('user1', '2024-04-13T10:00:00Z', '/home')

# 访问字段
print(click.user_id)  # 输出: user1
print(click[0])       # 输出: user1

# 使用 tuple 的其他操作
print(len(click))      # 输出: 3
print(hash(click))     # 输出: 一个整数，表示 ClickEvent 实例的哈希值
print(repr(click))     
# 输出: ClickEvent(user_id='user1', event_time='2024-04-13T10:00:00Z', page_url='/home')


适用场景： namedtuple 常用于需要表示固定数量、不可变属性的小型数据结构，比如记录、坐标、点、简化的数据库行等。由于其简洁的定义方式、高效的内存使用和易于理解的接口，namedtuple 成为了 Python 编程中的一种常见且实用的数据结构选择。

Python 的 namedtuple 类似于 Scala 的 case class
"""


# 以上创建空字典的方法可以进行简化
def get_last_click_times(iterator):
    user_last_times = {}  # 使用普通的空字典
    
    for event in iterator:
        user_id = event.user_id
        event_time = event.event_time

        # 使用 dict.get() 方法获取用户最后点击时间，如果不存在则返回 None
        if user_last_times.get(user_id) is None or event_time > user_last_times[user_id]:
            user_last_times[user_id] = event_time

    return [(user_id, last_time) for user_id, last_time in user_last_times.items()]
```









##### FlatMap 算子

flatMap：用于对RDD执行Map操作，并解除嵌套操作



```python
# 导包
from pyspark import SparkConf, SparkContext

if __name__ == '__main__':
    # 创建SparkConf 类对象
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    # 基于 SparkConf 类对象创建SparkContext类对象
    sc = SparkContext(conf=conf)

    # flatMap：用于对RDD执行Map操作，并解除嵌套操作
    list1 = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
    # list1为存在嵌套的数据，list2则为解除嵌套后的数据
    list2 = [1, 2, 3, 4, 5, 6, 7, 8, 9]

    rdd = sc.parallelize(["a b c", "a c e", "e c a"])
    # 输出原始数据
    print(rdd.collect())
    # ['a b c', 'a c e', 'e c a']

    # 按照空格切分数据
    print(rdd.map(lambda x: x.split(" ")).collect())
    # [['a', 'b', 'c'], ['a', 'c', 'e'], ['e', 'c', 'a']]

    # 按照空格切分数据后解除嵌套
    print(rdd.flatMap(lambda x: x.split(" ")).collect())
    # ['a', 'b', 'c', 'a', 'c', 'e', 'e', 'c', 'a']

```





##### reduceByKey算子

​	针对 KV 类型的RDD，自动按照 Key 分组，然后根据提供的聚合逻辑，完成 组内数据（value）的聚合操作

​	

​	rdd.reduceByKey(func)			# func: (V,V) -> V，该函数只负责聚合，分组由 By Key自动完成



​	接收两个传入参数（类型需要一致），返回一个返回值，类型需要与传入的参数类型一致



```python
# 导包
from pyspark import SparkConf, SparkContext

if __name__ == '__main__':
    # 创建SparkConf 类对象
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    # 基于 SparkConf 类对象创建SparkContext类对象
    sc = SparkContext(conf=conf)

    rdd = sc.parallelize([('男', 99), ('男', 23), ('男', 19), ('女', 29), ('女', 21)])
    print(rdd.reduceByKey(lambda a, b: a + b).collect())
    # [('男', 141), ('女', 50)]

```











##### groupBy / groupByKey / reduceByKey



**groupBy**：分组的条件可以自定义，**返回值类型为   ( String , Iterable [ (String , Int ) ]  )** 

​					即： 自定义key ， 该 key 同组的集合，集合中为原数据类型，包括 key





**groupByKey** ： 分组的条件为 key - value 类型中的 key，**返回值类型为  ( String , Iterable [ Int ]  )** 

​					即：key 以及相同 key 对应 value 组成的集合，Iterable [ Int ] 其实就是 values 集合中不包含 key





**reduceByKey** ： 分组的条件为 key - value 类型中的 key，**返回值类型为 （ key  ，value ）**

​					即：key 以及对应所有 value 进行合并操作（各种运算）得出的一个 value

​							相同的 key 的数据进行 value 数据的聚合操作，Scala中的聚合操作一般都是俩俩聚合











##### Filter 算子

​	将需要的数据保留，不需要的过滤

​		rdd.filter( func )	# func: ( T ) -> bool

​	传入一个参数（任意类型），但返回值必须是 True / False，返回值为True的原数据被保留，False的则被过滤剔除





```python
# 导包
from pyspark import SparkConf, SparkContext

if __name__ == '__main__':
    # 创建SparkConf 类对象
    conf = SparkConf().setMaster("local[*]").setAppName("test_spark_app")
    # 基于 SparkConf 类对象创建SparkContext类对象
    sc = SparkContext(conf=conf)

    rdd = sc.parallelize([1, 2, 3, 4, 5, 6, 7])

    # 过滤条件为保留奇数
    rdd1 = rdd.filter(lambda x: x % 2 == 1)
    print(rdd1.collect())

```





##### distinct算子

​	 对RDD数据进行去重，返回新RDD

​	 rdd.distinct（） 无需传参





##### sort By算子

​	对RDD数据进行排序，基于指定的排序依据进行排序

​	**rdd.sortBy( func, ascending=Flase, numPartitions=1 )**



​	func: ( T ) -> U  	告知需按照RDD中的哪个数据进行排序

​			例如  lambda x: x[1]  表示按照RDD中的第二列元素进行排序

​	ascending	 True： 升序  False： 降序

​	numPartitions：用多少分区排序





##### collect 算子



​		当对一个分布式数据集调用collect函数时，Spark会触发一个job，将分布在集群各节点上的数据分区结果全部传输回Driver节点。

​		收集完成后，返回一个数组，数组中的元素顺序取决于RDD的分区顺序和每个分区内部的元素顺序。



​	**功能**：将RDD各个分区中的数据统一收集到Driver中，形成一个list 对象，返回值是一个list

​			   但在处理大型数据集时可能会引发性能和内存问题，需谨慎使用



​	在生产环境中，谨慎使用collect处理大规模数据，应优先考虑使用其他更适合大数据处理的动作操作，如take(n)（获取前n个元素）、sample（抽样）或reduce、aggregate等聚合操作。





##### Reduce 算子

​	功能： 将RDD进行俩俩聚合，



##### Take 算子

​	功能： 取出RDD的前N个元素，返回值为 list



##### Count 算子

​	功能： 统计RDD中含有多少条数据，返回值为数字









## Spark SQL

#### 基础概念







### Data Frame

​	实际使用中，使用 DataFrame API 和使用 SQL 语句来实现相同的需求有一些差异：

1. **语法差异**：
   - **DataFrame API**：使用编程语言（如Scala、Python）提供的DataFrame API来编写代码，需要使用DataFrame的各种操作函数（例如`select`、`filter`、`groupBy`等）来对数据进行处理和转换。
   - **SQL**：使用SQL语句来描述数据处理的逻辑，SQL语句更加类似于自然语言，对于熟悉SQL语法的用户来说可能更直观和容易理解。
2. **性能和优化**：
   - **DataFrame API**：由于DataFrame API是基于编程语言构建的，可以更容易地进行代码优化和性能调优，例如使用编译时优化和基于表达式的优化技术。
   - **SQL**：Spark SQL会尝试将SQL查询转换为适合执行的物理执行计划，但是有时候可能会遇到性能问题，尤其是在复杂查询中。
3. **灵活性**：
   - **DataFrame API**：提供了更灵活的数据处理和转换能力，可以使用编程语言的各种控制结构（如循环、条件语句）来实现复杂的逻辑。
   - **SQL**：虽然SQL提供了强大的数据查询和聚合功能，但在某些情况下可能不够灵活，特别是在需要进行复杂逻辑处理时。
4. **可读性和维护性**：
   - **DataFrame API**：由于是使用编程语言编写的，代码可能更容易理解和维护。
   - **SQL**：SQL语句通常更加简洁和直观，对于熟悉SQL语法的人来说，可能更易于理解和阅读。







#### withColumn

​	withColumn 是 Apache Spark（包括 PySpark 和 Scala Spark）中 DataFrame API 的一个重要方法，用于在现有 DataFrame 中添加新列或替换已有列。该方法允许您基于现有 DataFrame 中的列计算新的值，并将结果作为新列添加到 DataFrame 中

```python
Python:	df_new = df.withColumn(column_name, column_expression)
Scala:  val dfNew = df.withColumn(columnName, columnExpression)
```

其中：
		• df: 原始 DataFrame。
		• column_name:  新列的名称，应为字符串类型，表示新列在 DataFrame 中的标识。			

​        • column_expression:   新列的值，通常是一个基于 DataFrame 中其他列的计算表达式。
​		这个表达式可以是简单的列引用、算术运算、字符串操作、内置或自定义函数调用等。
​	

举例：	

```python
extracted_ips = filtered_data.withColumn("ip_prefix",substring(col("ip_address"), 1, 24))
```







### UDF

UDF **可以接受多个列作为输入**，返回一个处理过的值。UDF 是 **逐行处理** 数据的，不涉及聚合操作





```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType, BooleanType

# 创建 SparkSession
spark = SparkSession.builder \
    .appName("PySpark MySQL Example") \
    .config("spark.jars", "/Users/buzhi/PycharmProjects/Spark_Demo/venv/lib/mysql-connector-j-8.2.0.jar") \
    .getOrCreate()

# 读取 MySQL 表到 Spark DataFrame
df = spark.read \
    .format("jdbc") \
    .option("url", "jdbc:mysql://localhost:3306/buzhi") \
    .option("dbtable", "Supermarket_Data_order") \
    .option("user", "root") \
    .option("password", "a1336869185") \
    .option("driver", "com.mysql.cj.jdbc.Driver") \
    .load()
    
# 定义 python函数
def to_upper(name):
    return name.upper()

# 注册函数，并指定返回类型
to_upper_udf = udf(to_upper, StringType())

# 使用 UDF 处理 DataFrame列
df_with_uppercase = df.withColumn("客户名称_Upper", to_upper_udf(df["客户名称"]))
df_with_uppercase.show()


# ==================== 单列

data = [("Alice", 17), ("Bob", 25), ("Cathy", 15)]
df_00 = spark.createDataFrame(data, ["Name", "Age"])


def is_adult(age):
    return age >= 18

# 注册函数，并指定返回类型
is_adult_udf = udf(is_adult, BooleanType())


# 使用 UDF 处理 DataFrame列
df00_with_adult = df_00.withColumn("IsAdult", is_adult_udf(df_00["Age"]))
df00_with_adult.show()



# ==================== 多列

data_01 = [("Alice", 17), ("Bob", 25), ("Cathy", 15)]
df_02 = spark.createDataFrame(data_01, ["Name", "Age"])


# 定义组合姓名和年龄的函数
def combine_name_age(name, age):
    return f"{name} is {age} years old"


# 注册为 UDF，并指定返回类型为 StringType
combine_udf = udf(combine_name_age, StringType())

# 使用 UDF 处理 DataFrame 列
df_with_combined = df_02.withColumn("Description", combine_udf(df_02["Name"], df_02["Age"]))

# 显示结果
df_with_combined.show()



# ==================== 性能优化：使用 Pandas UDF
"""
Spark 提供了一种基于 Apache Arrow 的 Pandas UDF，能够显著提高性能，特别是在处理大型数据集时。使用 Pandas UDF 的方法与常规 UDF 类似，但效率更高。

	•	Pandas UDF: pandas_udf 是 PySpark 中的一种用户自定义函数类型，允许你在 Spark DataFrame 中使用 Pandas 进行向量化操作。与传统的 UDF 相比，Pandas UDF 可以在处理大量数据时提高性能，因为它允许分批处理，而不是逐行处理。
	•	@pandas_udf(StringType()): 这是装饰器，表示定义一个 Pandas UDF，并指定返回类型为 StringType()，表示返回的是字符串数据。
"""


# 创建 DataFrame
data_01 = [("Alice",), ("Bob",), ("Cathy",)]
df_03 = spark.createDataFrame(data_01, ["Name"])

# 定义 Pandas UDF 函数
@pandas_udf(StringType())
def to_upper_pandas(name_series):
    return name_series.str.upper()

# 使用 Pandas UDF
df_with_uppercase = df_03.withColumn("UpperName", to_upper_pandas(df_03["Name"]))

# 显示结果
df_with_uppercase.show()



# 显示结果
# df.createOrReplaceTempView("df_01")
# df_01_data = spark.sql("""
#     SELECT DISTINCT
#         Modeofshipment
#     FROM df_01
# 	""")
#
# df_01_data.show()
```





### UDAF

**UDAF** 是 PySpark 中的用户自定义聚合函数，它用于 **聚合操作**，也就是说，它跨行处理数据并输出一个聚合结果。

**UDAF 的限制**



​	•	在 PySpark 中，**UDAF 目前仅支持单列输入**。你不能创建一个接受多个列的 UDAF 函数。

​	•	UDAF 是用于聚合操作的函数，如 SUM、AVG 等内置聚合函数。



**UDAF 只能接受单列作为输入，并返回聚合结果**，无法像 UDF 一样处理多个列。

```python
from pyspark.sql import SparkSession
from pyspark.sql.expressions import UserDefinedAggregateFunction
from pyspark.sql.types import StructType, StructField, IntegerType, LongType
from pyspark.sql import Row

# 创建 SparkSession
spark = SparkSession.builder.appName("PySpark UDAF Example").getOrCreate()

# 创建 UDAF
class MySum(UserDefinedAggregateFunction):
    # 定义输入的列类型
    def inputSchema(self):
        return StructType([StructField("input", IntegerType())])

    # 定义中间结果的类型，即各分区分别计算后返回的结果
    def bufferSchema(self):
        return StructType([StructField("sum", LongType())])

    # 定义最终输出的类型，即各分区合并计算后返回的结果
    def dataType(self):
        return LongType()

    # 聚合函数是否为确定性的，相同输入值是否能输出相同的值
    def deterministic(self):
        return True

    # 初始化中间结果，定义各分区内计算的初始值
    def initialize(self, buffer):
        buffer[0] = 0

    # 更新聚合结果，定义各分区内每一行数据的计算逻辑
    def update(self, buffer, input):
        buffer[0] += input[0]

    # 合并两个中间结果，定义各分区输出结果汇总时的计算逻辑
    def merge(self, buffer1, buffer2):
        buffer1[0] += buffer2[0]

    # 返回最终的聚合结果
    def evaluate(self, buffer):
        return buffer[0]

# 注册 UDAF
my_sum = MySum()

# 创建 DataFrame
data = [Row(1), Row(2), Row(3)]
df = spark.createDataFrame(data, ["value"])

# 使用 UDAF 进行聚合操作
df.agg(my_sum(df["value"]).alias("sum_value")).show()

# 停止 SparkSession
spark.stop()
```



## 具体使用





#### **使用 视图 / 缓存 创建中间临时表**

使用视图（View）还是缓存（cache/persist）取决于具体的使用场景和需求

##### **视图：**

###### 	优点：

 • 视图是逻辑视图，数据不会立即物化，只有在查询视图时才会执行实际的计算。
 • 适合定义多个中间计算步骤，将复杂的 SQL 查询拆分成多个简单的子查询。
 • 可以灵活地在 SQL 查询中使用，无需担心内存问题。

###### 	场景：

 • 需要在多个 SQL 查询中反复使用相同的中间结果。
 • 中间计算步骤较多，复杂查询需要拆分成多个部分。
 • 数据集较大，缓存可能导致内存不足。



```python
# 创建 SparkSession 并启用 Hive 支持
spark = SparkSession.builder \
    .appName("View Example") \
    .enableHiveSupport() \
    .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
    .getOrCreate()

# 使用 SQL 创建临时视图
spark.sql("""
    CREATE OR REPLACE TEMP VIEW cleaned_orders AS
    SELECT *,
           price * quantity AS order_amount
    FROM your_database_name.orders
    WHERE order_id IS NOT NULL
      AND customer_id IS NOT NULL
      AND price IS NOT NULL
      AND quantity IS NOT NULL
""")

# 使用视图进行聚合分析
customer_order_amount_df = spark.sql("""
    SELECT customer_id,
           SUM(order_amount) AS total_order_amount,
           COUNT(order_id) AS order_count
    FROM cleaned_orders
    GROUP BY customer_id
""")

customer_order_amount_df.show()

```





##### **缓存（cache/persist）**

###### 	优点：

 • 缓存可以将中间结果物化到内存或磁盘中，避免重复计算，提高性能。
 • 适合频繁访问的中间数据，减少重复计算的开销。
 • 可以指定不同的持久化级别（例如：内存、磁盘）。

​	

###### 	场景：

 • 中间结果需要多次访问，缓存可以显著提高性能。
 • 数据集较小，能够在内存中缓存。
 • 需要将中间结果物化，以避免在每次查询时重复计算。



```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col

# 创建 SparkSession 并启用 Hive 支持
spark = SparkSession.builder \
    .appName("Cache Example") \
    .enableHiveSupport() \
    .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
    .getOrCreate()

# 使用 SQL 查询并缓存结果
cleaned_df = spark.sql("""
    SELECT *,
           price * quantity AS order_amount
    FROM your_database_name.orders
    WHERE order_id IS NOT NULL
      AND customer_id IS NOT NULL
      AND price IS NOT NULL
      AND quantity IS NOT NULL
""")

# 缓存 DataFrame
cleaned_df.cache()

# 使用缓存的 DataFrame 进行聚合分析
customer_order_amount_df = cleaned_df.groupBy("customer_id").agg(
    sum("order_amount").alias("total_order_amount"),
    count("order_id").alias("order_count")
)

customer_order_amount_df.show()

# 释放缓存
cleaned_df.unpersist()


```







##### **视图/缓存 混合使用**

```python
from pyspark.sql import SparkSession
from pyspark import StorageLevel

# 创建 SparkSession 并启用 Hive 支持
spark = SparkSession.builder \
    .appName("Hive SQL Caching") \
    .enableHiveSupport() \
    .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
    .getOrCreate()

# 读取 Hive 表数据并创建临时视图
spark.sql("USE your_database_name")
spark.sql("CREATE OR REPLACE TEMP VIEW orders AS SELECT * FROM orders")

# 使用 Spark SQL 进行数据清洗和计算
cleaned_df = spark.sql("""
    SELECT *,
           price * quantity AS order_amount
    FROM orders
    WHERE order_id IS NOT NULL
      AND customer_id IS NOT NULL
      AND price IS NOT NULL
      AND quantity IS NOT NULL
""")

# 缓存 SQL 查询结果
cleaned_df.cache()  # 或者使用 persist 方法指定持久化级别
# cleaned_df.persist(StorageLevel.MEMORY_AND_DISK)

# 进行聚合分析
customer_order_amount_df = spark.sql("""
    SELECT customer_id,
           SUM(order_amount) AS total_order_amount,
           COUNT(order_id) AS order_count
    FROM cleaned_df
    GROUP BY customer_id
""")

# 执行计算并显示结果
customer_order_amount_df.show()

# 释放缓存
cleaned_df.unpersist()

# 将结果保存到新的 Hive 表
customer_order_amount_df.write.mode("overwrite").saveAsTable("customer_order_summary")

# 停止 SparkSession
spark.stop()
```





#### Spark SQL / Data Frame 混用

**Spark SQL可以和 Data Frame API 同时进行数据处理，Spark SQL返回的查询结果底层结构为 DataFrame**

  1. 创建 SparkSession：启用 Hive 支持，并指定 Hive 仓库目录。
  2. 使用 Spark SQL 读取 Hive 表数据：通过 SQL 查询读取 Hive 表 orders，查询结果是一个 DataFrame。
  3. 使用 DataFrame API 进行数据清洗和计算：使用 DataFrame API 对查询结果进行数据清洗和计算，去除缺失值，并添加一个新列 order_amount。
  4. 缓存 DataFrame：使用 cache() 方法将清洗后的 DataFrame 缓存到内存中。
  5. 使用 Spark SQL 进行聚合分析：将清洗后的 DataFrame 注册为临时视图 cleaned_orders，然后使用 SQL 进行聚合分析，计算每个客户的总订单金额和订单数量。
  6. 显示结果：显示聚合分析结果。
  7. 释放缓存：使用 unpersist() 方法释放缓存的 DataFrame，释放内存资源。
  8. 将结果保存到新的 Hive 表：将聚合结果保存到新的 Hive 表中。
  9. 停止 SparkSession：停止 Spark 会话，释放资源。

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum, count

# 创建 SparkSession 并启用 Hive 支持
spark = SparkSession.builder \
    .appName("Mix SQL and DataFrame API") \
    .enableHiveSupport() \
    .config("spark.sql.warehouse.dir", "/user/hive/warehouse") \
    .getOrCreate()

# 使用 Spark SQL 读取 Hive 表数据
orders_df = spark.sql("SELECT * FROM your_database_name.orders")

# 使用 DataFrame API 进行数据清洗和计算
cleaned_df = orders_df.dropna(subset=["order_id", "customer_id", "price", "quantity"]).withColumn("order_amount", col("price") * col("quantity"))

# 缓存 DataFrame
cleaned_df.cache()

# 使用 Spark SQL 进行聚合分析
cleaned_df.createOrReplaceTempView("cleaned_orders")
customer_order_amount_df = spark.sql("""
    SELECT customer_id,
           SUM(order_amount) AS total_order_amount,
           COUNT(order_id) AS order_count
    FROM cleaned_orders
    GROUP BY customer_id
""")

# 显示结果
customer_order_amount_df.show()

# 释放缓存
cleaned_df.unpersist()

# 将结果保存到新的 Hive 表
customer_order_amount_df.write.mode("overwrite").saveAsTable("your_database_name.customer_order_summary")

# 停止 SparkSession
spark.stop()
```











## 代码实例















