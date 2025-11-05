# Spark Core





## 底层原理

- 待学习





## 基础概念





**spark的三大数据结构**

​	**RDD**：	弹式分布式数据集

​	**累加器**：	分布式共享只写变量

​	**广播变量**：	分布式共享只读变量









### 分区概念

#### 	自定义分区器

​			1：继承 **Partitoner** 类

​			2：重写属性值 **numPartitions**（分区数量）

​			3：重写方法 **getPartition** （分区依据）



```scala
	  def main(args: Array[String]): Unit = {
    val sparkConf: SparkConf = new SparkConf().setMaster("local").setAppName("-")
    val sc = new SparkContext(sparkConf)

    val rdd: RDD[(String, String)] = sc.makeRDD(List(
      ("nba", "xxxxxxxxxx"),
      ("cba", "xxxxxxxxxx"),
      ("wnba","xxxxxxxxxx"),
      ("xba", "xxxxxxxxxx"),
      ("pba", "xxxxxxxxxx")
    ))

    val partRDD: RDD[(String, String)] = rdd.partitionBy(new MyPartitioner)

    partRDD.saveAsTextFile("output")
    
    sc.stop()
  }


  //自定义分区器
  //1：继承 Partitoner 类
  //2：重写属性值 numPartitions（分区数量）
  //3：重写方法 getPartition （分区依据）
  class MyPartitioner extends Partitioner{
    // 分区数量
    override def numPartitions: Int = 3

    // 根据数据的Key 返回数据所在的分区索引 ( 从零开始 )
    override def getPartition(key: Any): Int = {

        if( key == "nba")  {0}
        else if ( key == "wnba") {1}
        else if ( key == "cba") {2}
        else {3}


      // 优化上述判断语句为模式匹配
      key match {
        case "nba" => 0
        case "wnba" => 1
        case "cab" => 2
        case  _ => 3
      }
    }
  }

```





### 累加器概念

​		用来把 Executor 端变量信息聚合到 Driver 端，在 Driver 程序中定义的变量，在 Executor 端的每个 Task 都会得到这个变量的一个新副本，每个 task 更新这个变量副本的值后传回给 Driver 端进行 merge 







## RDD概述

​	

​	RDD（Resilient Distributed Dataset）叫做**弹性分布式数据集**，是Spark中最基本的数据抽象。



代码中是一个抽象类，它代表一个弹性的、不可变、可分区、里面的元素可并行计算的集合。

RDD的数据只有在调用 **actions** 类算子方法时，才会真正执行业务逻辑操作



**collect**() : 	将数据返回驱动器程序中最简单常见的操作，通常在单元测试中使用

​					  数据量不能太大，因为返回的数据存放在内存中，数据量太大会造成内存溢出





### RDD的属性



**Partition**：分区，数据集的基本组成单位

**Option [ Partitioner ] = None**：Option代表可能有也可能没有的参数，没有参数时默认为None





### RDD的特点

​	RDD表示只读的分区的数据集，对RDD进行改动，只能通过RDD的转换操作，由一个RDD得到一个新的RDD，新的RDD包含了从其他RDD衍生所必需的信息。

​	RDDs之间存在依赖，RDD的执行是按照血缘关系延时计算的。如果血缘关系较长，可以通过持久化RDD来切断血缘关系。

​	RDD之间进行相互迭代计算（Transformation），当执行开启后，新的RDD生成，代表老的RDD消失

​	RDD的数据是过程数据，只在处理过程中存在，一旦处理完成，被销毁



#### 		弹性

- **存储的弹性**：内存与磁盘的自动切换；
- **容错的弹性**：数据丢失可以自动恢复；
- **计算的弹性**：计算出错重试机制；
- **分片的弹性**：可根据需要重新分片。



#### 		分区

​	RDD逻辑上是分区的，每个分区的数据是抽象存在的，计算的时候会通过一个compute（计算）函数得到每个分区的数据。

​	如果RDD是通过已有的文件系统构建，则compute函数是读取指定文件系统中的数据，如果RDD是通过其他RDD转换而来，则compute函数是执行转换逻辑将其他RDD的数据进行转换。



#### 		只读

​	**RDD是只读的，要想改变RDD中的数据，只能在现有的RDD基础上创建新的RDD**。

​	由一个RDD转换到另一个RDD，可以通过丰富的操作算子实现，不再像MapReduce那样只能写map和reduce了。



RDD的操作算子包括两类

​	一类叫做transformations，它是用来将RDD进行转化，构建RDD的血缘关系；

​	另一类叫做 **actions**   (**count, collect,take**等) ，它是用来触发RDD的计算，得到RDD的相关计算结果或者将RDD保存的文件系统中。







#### 		算子

RDD 的方法与 scala 集合对象的方法不一样

集合对象的方法都是在同一个节点的内存中完成

RDD 的方法可以将计算逻辑发送到Executor端（分布式节点）执行

RDD方法外部的操作都是在driver端执行，而方法内部的逻辑代码都是在Executor端执行，scala的函数式编程会导致算子内部经常会利用到算子外部的数据，这样就形成了闭包的效果，当RDD算子中传递函数时会自动包含闭包操作，执行检测功能（如果使用的算子外数据无法进行序列化，则意味着无法传值给Executor端执行），所以在执行任务计算前，需要检测闭包内的对象是否可以进行序列化，该操作称为闭包检测



**闭包**：函数将外部变量引入至内部，形成闭合的效果，改变变量的生命周期



## RDD编程

### 	编程模型

​	在Spark中，RDD被表示为对象，通过对象上的方法调用来对RDD进行转换。经过一系列的transformations定义RDD之后，就可以调用**actions**触发RDD的计算，action可以是向应用程序返回结果(**count**, **collect**等)，或者是向存储系统保存数据(saveAsTextFile等)。在Spark中，只有遇到action，才会执行RDD的计算(即延迟计算)，这样在运行时可以通过管道的方式传输多个转换。

  要使用Spark，开发者需要编写一个Driver程序，它被提交到集群以调度运行Worker。Driver中定义了一个或多个RDD，并调用RDD上的action，Worker则执行RDD分区计算任务。



### 	RDD的创建

spark中创建RDD的方式可以分为三种：从集合中创建，从外部存储中创建以及从其他RDD中创建



#### 从集合中创建

spark为在函数中创建RDD提供了两种主要的函数：parallelize，makeRDD



**使用parallellize（）从集合中创建RDD**

```scala
var rdd = sc.parallelize(Array(1 to 8))
```



**使用makeRDD（）从集合中创建RDD**

```scala
val rdd = sc.makeRDD(Array(1 to 8))
```





#### 外部存储系统的数据集创建

存储系统的种类包括本地的文件系统，以及所有hadoop支持的数据集，比如hdfs，Cassandra，hbase等

```scala
val rdd = sc.textFile("hdfs://hadoop01:9000/release")
```





## RDD转换



### 	Value类型



#### 		map（func）：

**作用**：返回一个新的RDD，该RDD由每一个输入元素经过func函数转换后组成

**需求**：创建一个1-10的数组RDD，将所有元素*2之后形成新的RDD



```scala
val source = sc.parallelize(1 to 10)	//创建
source.collect()	//打印

val mapadd = source.map(_*2)			//所有元素*2
mapadd.collect()	//打印最终结果
```









#### 		mapPartitions(func)：

**作用**：

​	1:	可以以分区为单位进行数据转换操作，但会将整个分区的数据加载到内存中进行引用

​	2:	处理完的数据不会被释放，因为还存在对象的引用

​	3:	在内存较小，数据量较大的情景下，容易出现内存溢出





​	类似于 map，但独立地在RDD的每一个分片上运行，因此在类型为T的RDD上运行时，func的函数类型必须是 Iterator[T]  =>  Iterator[U]  （传入一个迭代器，返回一个迭代器）

​	假设有N个元素，有M个分区，那么map的函数的将被调用N次，而mapPartitions被调用M次，一个函数一次处理所有分区。





**需求**：创建一个RDD，使每个元素*2组成新的RDD

 

```scala
// 函数签名

def mapPartitions[U:ClassTag](

f:Iterator[T] => iterator[U]

preservesPartitoning: Boolean = false) : RDD[U]

)





val rdd = sc.parallelize(Array(1,2,3,4))	//创建
//【1,2】，【3,4】

val rdd2 =  rdd.mapPartitions(x=>x.map(_*2))	//所有元素*2
 rdd2.collect()		//打印最终结果



// 需求：取出每个分区的最大值

val rdd1 = makeRDD(List(1,2,3,4),2)

val mprdd = rdd1.mapPartitions(
 {
  	iter =>{
        List(iter.max).iterator		
      // 返回值需为迭代器模式，而最大值是一个数，所以需要构建迭代器
    	}   
 	} 
)

mprdd.collet().foreach(println)

```





**其余例子**

```scala
// 计算每个分区的平均值和总和
val numbersRDD: RDD[Int] = sc.parallelize(Seq(1, 2, 3, 4, 5, 6, 7, 8, 9, 10), 2)

// 计算每个分区的总和与平均值
val sumsAndAverages = numbersRDD.mapPartitions { partition =>
  val sum = partition.sum
  val count = partition.size
  Iterator((sum, count, sum.toDouble / count))
}

sumsAndAverages.foreach(println)
// 在上述例子中，mapPartitions函数对RDD的每个分区进行操作，分别计算每个分区内的元素总和、元素数量以及平均值。


//过滤并转换数据假设有一个包含JSON字符串的RDD，需要将每个分区中的JSON对象解析为Case Class，并过滤出符合特定条件的对象。
case class User(id: Int, name: String, age: Int)

val jsonStringsRDD: RDD[String] = sc.textFile("users.json")

// 解析JSON并过滤年龄大于18岁的用户
val usersRDD = jsonStringsRDD.mapPartitions { partition =>
  import org.json4s._
  import org.json4s.jackson.JsonMethods._

  implicit val formats = DefaultFormats

  partition.flatMap { jsonString =>
    try {
      parse(jsonString).extractOpt[User].filter(_.age > 18)
    } catch {
      case _: Throwable => None
    }
  }
}

//在这个场景下，mapPartitions被用来处理每个分区的数据，将JSON字符串解析成用户对象，并仅保留年龄超过18岁的用户。



//按组聚合数据如果需要在每个分区内部按照某个字段分组并对另一个字段进行求和，可以利用mapPartitions结合本地集合的操作来实现。
case class Record(key: String, value: Double)

val recordsRDD: RDD[Record] = ...

// 在每个分区内部按key进行聚合
val groupSumsRDD = recordsRDD.mapPartitions { partition =>
  val localMap = scala.collection.mutable.Map.empty[String, Double]
  partition.foreach(record => localMap.update(record.key, localMap.getOrElse(record.key, 0.0) + record.value))
  localMap.iterator.map { case (k, v) => (k, v) }
}
// 这个示例中，我们使用mapPartitions在每个分区内部创建一个本地mutable map，然后遍历分区中的每一条记录，对相同键（key）的value做累加，最后返回一个迭代器，包含了每个键及其对应的累加值。
```











​	**map与mapPartition（）的区别**



**map**（）：每次处理一条数据，是分区内一个数据一个数据的执行，类似于**串行操作**，性能较低

**mapPartition**（）：每次处理一个分区的数据，是以分区为单位进行**批处理操作**，当该分区中整个分区的数据都处理完之后，原RDD中对应分区的数据才能释放，有可能导致oom，性能较高，但占内存（整个分区数据量可能很大）

mapPartition算子需要传递一个迭代器，并返回一个迭代器，没有要求元素的个数保持不变，所以可以增加或减少数据



开发指导：当内存空间比较大的时候建议使用mapPartition，以提高处理效率，**完成比完美更重要**











#### 		 mapPartitionsWithIndex(func)：

**作用**：类似于mapPartitions，但func带有一个整数参数表示分片的索引值，因此在类型为T的RDD上运行时，func的函数类型必须是(Int, Iterator[ T ]) => Iterator[ U ]；

​	将待处理的数据以分区为单位发送到计算节点进行处理，这里的处理可以是任意的处理，哪怕是过滤数据，在处理时同样可以获取当前分区索引



**需求**：创建一个RDD，使每个元素跟所在分区形成一个元组组成一个新的RDD

```scala
val add = sc.parallelize(Array(1,2,3,4))	//创建RDD

//使每个元素与其所在的分区组成一个新的RDD
val indexRdd = add.mapPartitionsWithIndex(
  (index,items) =>(
    items.map((index,_))
  )
)		

indexRdd.collect().foreach(println)	

// 返回结果
//(0,1)
//(0,2)
//(0,3)
//(0,4)


val add = sc.makeRDD(List(1,2,3,4),2)	//创建RDD

// 只保留第二个分区的数据 
val mpiRDD = rdd.mapPartitionsWithIndex(
	(index,iter) => {
    if(index == 1){
      iter
    }else{
      Nil.iterator		// Nil 为空集合
    }
  }
)

mpiRDD.collect().foreach(println)
```









#### 	flatMap（func）:

**作用**：类似于map，但每一个输入元素都可以被映射为零或多个输出元素，所以func应该返回一个序列，而不是单一元素

**需求**：创建一个元素为-5的RDD，运用 flatMap 创建一个新的RDD，新的RDD为原RDD的每一个元素的扩展



```scala
val sourceFlat = sc.parallelize(1 to 5)

val flatMap = sourceFlat.flatMap(1 to _)	//根据原来的RDD生成新的RDD

flatMap.collect()		//打印新的RDD
//(1,1,1,2,1,2,3,1,2,3,4,1,2,3,4,5)


val rdd1: RDD[List[Int]] = sc.makeRDD(List(List(1, 2), List(3,4)))
val rdd2: RDD[Int] = rdd1.flatMap( list => list)


val rdd3: RDD[String] = sc.makeRDD(List("Hello Scala","Hello Hadoop"))
val rdd4: RDD[String] = rdd3.flatMap(
  s => {s.split(" ")}
)



val rdd5: RDD[Any] = sc.makeRDD(List(List(1, 2), 3, List(3, 4, 5)))
val rdd6 = rdd5.flatMap{
      // 此处为从模式匹配优化后的匿名函数
       case list: List[_] => list
       case dat => List(dat)
  }

//    此用法为模式匹配
//    (
//      data => {
//        data match {
//          case list: List[_] => list
//          case dat => List(dat)
//        }
//      }
//    )


```











#### 	glom

**作用**：将每一个分区形成一个数组，形成新的RDD类型RDD[Array[T]]

​			将同一个分区的数据直接转换成相同类型的内存数组进行处理，分区数量不变



```scala
val rdd = sc.parallelize(1 to 16,4)		//创建一个含有4个分区的RDD

rdd.glom().collect()	//将每个分区的数据都放到一个数组中，并收集到Driver端进行打印

//Array(Array(1,2,3,4),Array(5,6,7,8),Array(9,10,11,12).Array(13,14,15,16))


// TODO 求的每个分区中最大值之和

val rdd = sc.makeRDD(List(1, 2, 3, 4, 5, 6), 2)


// glom 将每个分区中的数据分别作为一个数组返回
val glomRDD: RDD[Array[Int]] = rdd.glom()

// 取出每个数组（分区）中的最大值
val maxRDD: RDD[Int] = glomRDD.map(_.max)

// collect操作是将该RDD的数据都收集
println(maxRDD.collect().sum)
```



#### 	groupBy(func)

**作用**：分组，按照传入函数的返回值进行分组，将相同key对应的值放入到一个迭代器

**shuffle**：将数据根据指定的规则进行分组，分区默认不变，但数据会被打乱重新组合，该操作称为shuffle，极限情况下，数据可能会被分在同一个分区中



**分区与分组没有必然的关系**

**一个组的数据在同一个分区中，但并不代表一个分区中只有一个组**



```scala
val rdd = sc.parallelize(1 to 4)	//创建RDD
val group = rdd.groupBy(_%2)	//按照元素模以2的值进行分组 _%2 x => x%2

group.collect		//打印结果
//Array((0,CompactBuffer(2,4)),(1,CompactBuffer(1,3)))


//根据首字母进行分组
val add = sc.makeRDD(List("Hello","Spark","Scala","Hadoop"),2)
val groupRdd = rdd.groupBy(_.charAt(0))

//_.charAt(0):字符串首字母
//_.contains("xxx"):判断字符串中是否包含该字符
//.substring(satrt,stop):用于切割字符串
//.startsWith("xxx"):判断字符串是否以该字母开头
// rdd.partitions.size:查看该RDD的分区数量

groupRdd.collect().foreach(println)
```









#### 	filter(func)

**作用**：过滤，返回一个新的RDD，该RDD由经过func函数计算后返回值为true的输入元素组成。将数据根据指定的规则进行筛选，符合条件的数据留下，不符合规则的数据则丢弃

**注意**：数据进行筛选过滤后，分区数量不变，但分区内的数据可能会不均衡，生产环境中可能会出现数据倾斜的情况

**需求**：创建一个RDD（由字符串组成），过滤出一个新的RDD（包含“ye"字串）

```scala
var sourceFilter = sc.parallelize(Array("buzhi","yelinsheng","yebukun","yetongyi","zengyiyang"))

val filter = sourceFilter.filter(_.contains("ye"))
// 筛选出字符串中包含的字符
// _.contains("xxx"): 判断字符串中是否包含该字符

filter.collect()

// 过滤得到奇数
val rdd = sc.makeRDD(List(2,3454,1,3423,13,5,336,08))

val dataRdd = rdd.filter(_%2 == 0)
```



##### 小功能：

```scala
//初始化配置信息及SparkContext
  val sparkConf: SparkConf = new SparkConf().setAppName("Test").setMaster("local[*]")
  val sc = new SparkContext(sparkConf)

  val rdd = sc.textFile("datas/Apache.log")

  val values: RDD[String] = rdd.filter(
    line => {
      val datas = line.split(" ")
      val time = datas(3)
      time.startsWith("17/05/2015")
    }
  )    

  values.map(
    line => {
      val strings = line.split(" ")
      strings(6)
    }
  ).collect().foreach(println)

  sc.stop()
```





#### sample( withReplacement , fraction , seed )



**withReplacement**：Boolean 	\抽取数据后是否放回 true（放回），false（丢弃）

**fraction** ：double  					 \比率，小数

**seed** ：long 								 \抽取数据时随机算法的种子，该参数为空时使用当前的系统时间



```scala
// 不放回不重复抽取
val rdd = sc.makeRDD(List(1,2,3,4,5,6,7,8,9))

println( rdd.sample(false,0.4,1).collect().mkString(",") )
```



#### distinct

##### 	函数说明：

将数据集中重复的元素去除，默认情况下，只有8个并行任务来操作，但是可以传入一个可选的numTasks参数改变它。



```scala
val rdd = sc.makeRDD (List(1,2,3,4,2,3,4))

val rdd1 = rdd.distinct()


// 底层算法

map(x => (x ,null)).reduceByKey((x,_) =>x, numPartitions).map(_._1)

// 过程
// map(x => (x,null)) :形成（1，null）的元组结构
// reduceByKey((x,_) => numPartitions):根据键进行分组之后再操作，两个null返回一个null ，numPartitions是第二个参数
// 两个相同KEY的VALUES 返回其中一个： (null,null) => null 去掉多余重复的数据 (1,null),(2,null)
// map.(_._1):返回元组的第一个字值
```





#### coalesce( numPartitions ：Int , Shuffle : Boolean（默认为false） )

**作用**：修改分区数量

​			根据数据量缩减分区，用于大数据集过滤后，提高小数据集的执行效率

​			可以配合shuffle选项增加分区，减少每个分区中的数据量，**如果扩大分区时不进行shuffle操作，则会出现空分区，无意义**

​			当spark程序中存在过多的小任务时，可以通过 coalesce 方法收缩合并分区，减少分区个数，减小任务调度成本



​			减少分区时只会将被减少分区中的数据整体塞到其他的某个分区中，并不会将被减少分区中的数据拆分至多个分区中，存在数据倾斜的风险

​			如果想要避免数据倾斜，可以使用第二个参数进行 **Shuffle** 操作

```
		1，2				1,2,3,4

		3，4		->		5,6

		5，6		->		7,8

		7，8			
```

​	



**举个例子**：

```scala
var rdd	= sc.makeRDD(List(1,2,3,4,5,6),3)	//分为四个分区
val newrdd = rdd.coalesce(2，true)		//合并缩减分区,默认情况下不会将分区内的数据打乱进行重新组合，可能会出现数据倾斜，可进行shuffle处理以避免数据情况的出现
```



```scala
val rdd = sc.makeRDD(List(1,2,3,4,5,6,7,8,9,0),2)
val newrdd = rdd.coalesce(4,true)		//增加分区数量必须配合shuffle，否则新增分区为空，无意义

```



#### repartitions(numPartitions)

**作用**：根据分区数量，通过网络重新洗牌所有数据，扩大分区



- coalesce重新分区，可以选择是否进行shuffle过程。由参数shuffle: Boolean = false/true决定。

-  repartition实际上是调用的coalesce，进行shuffle。





#### sortBy(func,[ascending],[numTasks])

**作用**：使用func先对数据进行处理，按照处理后的数据结果排序，第二个参数为排序选项，默认为正序

​			底层也有shuffle的操作



**举个例子**：

```scala
val rdd = sc.parallelize(List(2,1,3,4))
rdd.sortBy(x => x).collect()	//按照自身大小进行排序

rdd.sortBy(x => x%3 )  //按照与3余数的大小排序
```



```scala
val rdd = sc.makeRDD(List(("1",1),("11",2),("2",3)),2)
val newrdd = rdd.sortBy( t => t._1) /(_._1)

newrdd.collect().foreach(println)
```





#### pipe( command,[envVars] )

**作用**：管道，针对每一个分区都执行一个shell脚本，返回输出的RDD

**注意**：编写的shell脚本需要放在worker节点可以访问到的位置



**举个例子：**



```shell
#shell脚本
#!/bin/sh
echo "AA"
while read LINE ;
	do
		echo ">>>" ${LINE}
	done
```



### 双Value类型交互

**union（rdd）**:交集

**intersection（rdd）**：并集

**subtract（rdd）**：差集



交并差集的两个数据源需要是相同类型的数据，而拉链的数据源类型可以不一致，但分区个数以及每个分区的数据量都需要相同

**Zip（rdd）**：拉链



```scala
    
    val rdd1 = sc.makeRDD(List(1,2,3,4))
    val rdd2 = sc.makeRDD(List(3,4,5,6))

    // 交集 3，4
    val rdd3 : RDD[Int] = rdd1.intersection(rdd2)
    println(rdd3.collect().mkString(","))


    // 并集 1，2，3，4，3，4，5，6 （不去重）
    val rdd4: RDD[Int] = rdd1.union(rdd2)
    println(rdd4.collect().mkString(","))


    // 差集 1，2
    val rdd5: RDD[Int] = rdd1.subtract(rdd2)
    println(rdd5.collect().mkString(","))


    // 拉链 (1,3),(2,4),(3,5),(4,6)
    val rdd6: RDD[(Int,Int)] = rdd1.zip(rdd2)
    println(rdd6.collect().mkString(","))
```





### Key-Value类型



#### partitionBy

**函数签名**：def partitionBy( partitioner : Partutioner ) : RDD[(K,V)]



**函数说明**：将数据按照指定的Partitioner重新进行分区，Spark默认使用的分区器是HashPartitioner



**举个例子**：

```scala
val rdd = sc.makeRDD(Array((1,"aaa"),(2,"bbb"),(3,"ccc")),3)
import org.apache.spark.HashPartitioner

val maprdd = rdd.partitionBy(new HashPartitioner(2))
```



**注意事项**：

当重分区的分区器与当前RDD的分区器类型以及分区数量都一样时，重分区不会进行任何操作

spark的分区器：HashPartitioner，RangePartitioner（一般是在排序时使用）





#### reduceByKey

##### 	概念说明：

reduceByKey : 相同的key的数据进行value数据的聚合操作



scala语言中的聚合操作一般都是两两聚合，而spark基于scala开发，所以它的聚合一般也是两两聚合

reduceByKey = groupByKey + map



reduceByKey支持分区内预聚合功能，可以有效减少shuffle落盘时的数据量

#####  举个例子:

```scala
val rdd = sc.makeRDD(List(("a",1),("a",2),("a",3),("b",2),("b",4),("c",1)))

rdd.reduceBykey((x:int,y:int) => { x + y})
//[1,2,3] > [3,3] > [6]

rdd.reduceByKey(prinln(s"x = ${x},y= ${y}) )
//x = 1,y = 2   x = 3,y = 3 如果reduceByKey中Key对应的value只有一个，则该value不会参与运算，比如：("c",1)

rdd.collect().foreach(println)
```







#### groupByKey

##### **函数说明**：

​	将分区的数据直接转换成相同类型的内存数据进行后续处理，将数据源中相同key的数据分在同一个组中，形成一个对偶元组，元组中的第一个元素为key，第二个元素为相同key的value集合



​	groupByKey：会将数据打乱重组，但分区数量不变，即存在shuffle操作





##### **举个例子**：

```scala
val rdd = sc.makeRDD(List(("a",1),("a",2),("a",3),("b",2),("b",4),("c",1)))

//groupByKey
val grouprdd : RDD[(String,Iterable[Int])] = rdd.groupByKey()
grouprdd.collect().foreach(println)

//(a,CompactBuffer(1,2,3))
//(b,CompactBuffer(2,4))
//(c,CompactBuffer(1))

//groupBy
val group1rdd : RDD[(String,Iterable[(String,Int)])] = rdd.groupBy(_._1)
group1rdd.collect().foreach(println)
//groupBy会将每一个key value单独拿出来组成元组，而groupByKey会将key与对应的所有value组成一个集合

//(a,CopactBuffer((a,1),(a,2),(a,3)))
//(b,CopactBuffer((b,2),(b,4)))
//(c,CopactBuffer((c,1)))


```



#### -- 对比 --

**reduceByKey 与 groupByKey**



**从shuffle的角度**：

​	reduceByKey  和 groupByKey 都存在 shuffle 操作，但 reduceByKey 可以在 shuffle 操作前对分区内相同key的数据进行预聚合操作（ combine ），以减少 shuffle 落盘时的数据量，而 groupByKey 只是进行分组，不能减少落盘时数据量，reduceByKey 的性能比 groupByKey 的性能高



**从功能的角度**：

​	reduceByKey包含分组与聚合的功能，而groupByKey只有分组功能，无法聚合，所以在需要同时使用分组聚合的场合下，推荐使用reduceByKey，如果仅仅只需要分组而不需要聚合，则只能使用groupByKey





#### aggregateByKey



##### 作用：

​	可以将数据按照相同的key和value进行聚合，分为分区间与分区内

##### 说明：

aggregateByKey 存在函数柯里化，有两个参数列表

​	第一个参数列表：传递一个参数（初始值），默认为零，用于与第一个value进行分区内计算

​	第二个参数列表：需要传递两个参数，第一个参数为分区内的计算规则，第二个参数为分区间的计算规则



最终返回值类型结果应该与初始值的类型保持一致

##### 举个例子：



```scala
//求各分区最大值之和
rdd.aggregateByKey()(
    
	(x,y) => math.max(x,y)		//找出分区内的最大值
	(x,y) => x + y						//将各分区的最大值相加	

)

//获取相同key的数据的平均值
val newrdd =  rdd.aggregateByKey((0,0)，
( (t,v)  => {						//分区内操作
               (t._1 + v , t._2 + 1)		//t代表初始值 （0，0），v为相同key对应的value
           }     
 
  (t1,t2) => {					//分区间操作，每个key对应的value的第一个元素，第二个元素分别进行相加
                (t1._1 + t2._1 , t1._2 + t2._2 )
            }          
         )              
      )
                
 val maprdd = newrdd.mapValues{
     case(num ,cnt) => {
         num / cnt
     }
 }      
```



#### combineByKey



##### 说明：

该方法需要三个参数

参数一：将相同key的第一个数据进行结构的转换，实现操作，即相同key的第一条数据进行的操作

参数二：分区内的计算规则

参数三：分区间的计算规则



##### 举个例子：

```scala

//获取相同key的数据的平均值
val newrdd =      rdd.combineByKey((
					v => (v,1)			//参数一
			)，                           
(					                  	
      (t,v) => {						//分区内操作
               (t._1 + v , t._2 + 1)		//t代表初始值，v为相同key对应的value
         }     
                  
       (t1,t2) => {					//分区间操作，每个key对应的value的第一个元素，第二个元素分别进行相加
                (t1._1 + t2._1 , t1._2 + t2._2 )
            }                  
          )              
      )
                
 val maprdd = newrdd.mapValues{
     case(num ,cnt) => {
         num / cnt
     }
 }      
```









#### join

##### 	作用：

​	将两个不同数据源的数据中相同key的value连接在一起，形成元组。

​    如果两个数据源中的key没有匹配上，则该数据不会出现在结果中



​	如果两个数据源中有多个相同的key，则会依次进行匹配，可能会出现笛卡尔积，数据量将会几何倍增，导致性能降低

**谨慎使用join**



#### leftOuterJoin / rightOuterJoin



​			以主表为本体进行关联，没有连接上的次表字段使用 **None** 代替



#####   举个例子：

```scala
val rdd1 = sc.makeRDD(List(("a",1),("b",2),("c",3)))
val rdd2 = sc.makeRDD(List(("d",4),("a",4),("a",5),("c",6)))


val joinrdd = rdd1.join(rdd2)
println(joinrdd.collect().mkString(","))
//(a,(1,4)),(a,(1,5)),(c,(3,6))


val leftadd = rdd1.leftOuterJoin(rdd2)
println(leftadd.collect().mkString(","))
//(a,(1,Some(4))),(a,(1,Some(5))),(b,(2,None)),(c,(3,Some(6)))


val rightadd = rdd1.rightOuterJoin(rdd2)
println(rightadd.collect().mkString(","))
//(d,(None,4)),(a,(Some(1),4)),(a,(Some(1),5)),(c,(Some(3),6))


```









#### cogroup  

​	对两个RDD中的KV元素，每个RDD中相同key中的元素分别聚合成一个集合



**举个例子**

```scala
val rdd1 = sc.makeRDD(List(("a",1),("b",2),("c",3)))
val rdd2 = sc.makeRDD(List(("d",4),("a",4),("a",5),("c",6)))

val cgrdd = rdd1.cogroup(rdd2)
//(a,(CompactBuffer(1),CompactBuffer(4,5)))
//(b,(CompactBuffer(2),CompactBuffer()))
//(c,(CompactBuffer(3),CompactBuffer(6)))
//(d,(CompactBuffer(),CompactBuffer(4)))
```







### groupBy / groupByKey / reduceByKey



**groupBy**：分组的条件可以自定义，**返回值类型为   ( String , Iterable [ (String , Int ) ]  )** 

​					即： 自定义key ， 该 key 同组的集合，集合中为原数据类型，包括 key



**groupByKey** ： 分组的条件为 key - value 类型中的 key，**返回值类型为  ( String , Iterable [ Int ]  )** 

​					即：key 以及相同 key 对应 value 组成的集合，Iterable [ Int ] 其实就是 values 集合中不包含 key



**reduceByKey** ： 分组的条件为 key - value 类型中的 key，**返回值类型为 （ key  ，value ）**

​					即：key 以及对应所有 value 进行合并操作（各种运算）得出的一个 value

​							相同的 key 的数据进行 value 数据的聚合操作，Scala中的聚合操作一般都是俩俩聚合









## 常见算子



### 行动算子

​	**行动算子：触发作业（ job ）执行的方法**

​						底层代表调用的是环境对象 （ SC ）的run job 方法

​						底层代码中会创建 ActiveJob ，并提交执行





#### reduce函数

函数签名： def reduce ( f : ( T , T ) => T ) : T

函数说明： 聚集RDD中的所有元素，先聚合分区内的数据，再聚合分区间的数据





#### collect算子

**作用**：

​		collect 操作是将不同分区的数据按照分区顺序都采集到 driver 端内存中形成数组进行处理 （以分区为单位采集数据到driver端，其实是driver端内存数据打印 ）

​		collect()方法的作用是收集分布在各个worker的数据到driver节点，如果不使用这个方法，每个worker的数据只在自己本地显示，并不会在driver节点显示。





#### count算子

**作用**：

​	返回数据源中的数据个数



##### countByKey算子

**作用：**

​		表示 Key 出现的个数，与 value 无关

```
val rdd = sc.makeRDD(List(("a",1),("a",2),("a",1),("b",4))
val stringToLong: : collection.Map[String,Long] = rdd.countByKey()
// Map(a -> 3,b -> 1)
```





##### countByValue算子

**作用：**

​		表示 Value 出现的个数

```scala
val rdd = sc.makeRDD(List(1,1,1,4))
val intToLong: collection.Map[Int,Long] = rdd.countByValue()
// Map(4 -> 1,1 -> 3)
```





#### first算子

**作用：**

​	获取数据源中的第一个数据





#### tack算子

**作用：**

​	按顺序获取 N 个数据





#### take Ordered算子

**作用：**

​	将数据排序后取最后 N 个数据





#### aggregate算子

**作用：**

​	先进行分区内的运算，再进行分区间的运算，也可以设置初始值

​	aggregateByKey的初始值只会参与分区内的计算

​	aggregate的初始值会参与分区内以及分区间的计算



当分区内与分区间的计算规则相同时可以使用 **fold** 函数	

```scala
val rdd = sc.makeRDD(List(1,2,3,4))
val result = rdd.aggregate( 0 )( _ + _ , _ + _ )  // 10		1+2  +  3+4
val result1 = rdd.aggregate( 10 )( _ + _ , _ + _ )  // 40  10+1+2  + 10 +  10+3+4
// aggregateByKey -> 30	 10+1+2  +   10+3+4

val result2 = rdd.fold( 10 )( _ + _ )  // 40  10+1+2  + 10 +  10+3+4

```

​		



### foreach算子

#### 	作用：



是在executor端内存集合执行的循环遍历方法，属于分布式操作

foreach算子中的方法都在executor端执行，如果在 foreach 前面加了 collect 则是在 Driver 端打印



```scala
val rdd = sc.makeRDD(List(1,2,3,4))

// 此处的 foreach 其实是Driver 端内存集合的循环遍历方法，因为 collect 会以分区为单位采集数据到driver端，输出的数据按照分区顺序
rdd.collect().foreach(println)

// 此处的 foreach 其实是在 Executor 端内存打印，分布式打印，没有顺序
rdd.foreach( println )
```





RDD 的方法与 Scala 集合对象的方法不一样

集合对象的方法都是在同一个内存中完成

RDD的方法可以将计算逻辑发送到 Excutor 端（分布式节点）执行

为了分区分不同的处理效果，将RDD的方法称为算子

**RDD的方法外部操作都是在 Driver 执行，而方法内部的逻辑代码是在 Excutor**









### 大小写转换



​	toLowerCase：转小写

​	toUpperCase:   转大写





## 常见函数使用

#### 函数大全

##### 常见算子/函数

1. **map**：对 RDD 中每个元素都应用一个指定函数。例如，可以对存储商品价格列表的 RDD 应用 `map` 函数，从而生成具有更高价值的新 RDD。

   

2. **filter**：创建一个只包含满足指定条件的 RDD 的新 RDD。例如，可以使用 `filter` 操作从产品分类的 RDD 中过滤特定类别的产品。

   

3. **reduceByKey**：将 RDD 中具有相同键的元素聚合成单个元素。例如，可以使用 `reduceByKey` 操作计算每个客户的总订单金额。

   

4. **groupByKey**：按照 RDD 中每个元素的键将元素进行分组。例如，可以按照地理区域将顾客分组，以方便后续分析。

   

5. **join**：利用两个RDD之间共有的键执行基于连接的操作。例如，可以将客户信息的 RDD 和订单信息的 RDD 进行连接，并输出包含全部客户和其订单的新 RDD。

   

6. **distinct**：创建一个包含 RDD 所有不同元素的新 RDD。例如，可以使用 `distinct` 操作获取员工名字列表中的唯一值。

   

7. **count**：返回 RDD 元素数目。例如，可以使用 `count` 操作计算 RDD 中订单记录的数量。

   

8. **take**：获取 RDD 中前 N 个元素。例如，可以使用 `take` 抽样操作从 RDD 中提取顾客列表中的前 10 名。

   

9. **aggregate**：使用一个初始值和组合操作来对RDD进行聚合。例如，可以使用 `aggregate` 操作计算订单 RDD 中的平均订单价值。

   

10. **sortByKey**：按照键对RDD进行排序。例如，可以根据产品ID对存储产品价格列表的 RDD 进行排序，以实现更高效的搜索匹配。

    

11. **mapPartitions**：对 RDD 的所有元素进行一次性操作。例如，可以使用 `mapPartitions` 操作为特定类型的客户生成推荐商品。

    

12. **flatMap**：将每个输入元素转换为零个、一个或多个输出元素，然后将这些结果连接成一个（新）RDD。例如，可以使用 `flatMap` 操作将包含每个产品的标签列表的 RDD 转换为单个包含所有标签的 RDD。

    

13. **fold**：将给定聚合函数应用于 RDD 元素序列，返回最终聚合结果并指定初始值。例如，可以使用 `fold` 操作计算每种产品的总销售额。

    

14. **intersection**：创建仅包含两个 RDD 共有元素的新 RDD。例如，可以使用 `intersection` 操作找出位于两个市场范围内的重叠顾客。

    

15. **sample**：从 RDD 中抽取随机样本。例如，可以使用 `sample` 操作获取订单记录的统计数据样本。

    

16. **union**：将两个 RDD 连接成一个 RDD。例如，可以使用 `union` 操作将两个产品列表 RDD 进行组合。

    

17. **cache / persist**：通过将 RDD 缓存在内存中来提高性能。例如，可以使用 `cache` 操作持久化存储复杂计算的结果。

    

18. **coalesce**：将较小的RDD合并到较大的RDD分区中，从而获得更高的效率。例如，可以使用 `coalesce` 操作减少产品成本数据集的分区数量。

    

19. **repartition**：重新分区 RDD 以使其适应指定分区数。例如，可以使用 `repartition` 操作对顾客列表的 RDD 进行重分区，以便进行更具有针对性的筛选操作。

    

20. **distinctByKey**：创建仅包含唯一键的新 RDD。例如，可以使用 `distinctByKey` 操作获取订单记录中每个客户的独特订单计数。

    

21. **combineByKey**：使用自定义初始和聚合函数来转换某些类型的 RDD。例如，可以使用 `combineByKey` 操作为客户 ID 创建订单历史记录的集合。（此操作类似于 reduceByKey()，但前者允许您指定初始值）。

    

22. **foldByKey**：对具有共同键的元素执行折叠/缩小操作。例如，可以使用 `foldByKey` 操作计算每个员工的平均销售额。

    

23. **top-n**：使用RDD数据集返回前 N 个元素。例如，可以使用 `top-n` 操作获取最昂贵产品列表 RDD 中的前10个产品。

    

24. **countByKey**：对RDD中每个键出现的次数进行计数。例如，可以使用 `countByKey` 操作实现为客户创建词云的自定义主题探索。

    

25. **foreach**：对 RDD 进行迭代操作并应用给定函数。例如，可以使用 `foreach` 操作将产品价格下调5％，以刺激销售。

    

26. **takeSample**：从 RDD 中随机提取 N 个样本元素。例如，可以使用 `takeSample` 操作分析订单集数据，并评估当前市场趋势。





##### 字符串的灵活使用

```scala
//_.charAt(0):字符串首字母
//_.contains("xxx"):判断字符串中是否包含该字符
//.substring(satrt,stop):用于切割字符串
//.startsWith("xxx"):判断字符串是否以该字母开头
// rdd.partitions.size:查看该RDD的分区数量
```





##### 集合/迭代器的灵活使用

```
迭代器 it 的两个基本操作是 **next** 和 **hasNext**。

​	调用 **it.next()** 会返回迭代器的下一个元素，并且更新迭代器的状态。

​	调用 **it.hasNext()** 用于检测集合中是否还有元素，如果还有可返回的元素，返回true。

​	可以使用 **it.min** 和 **it.max** 方法从迭代器中查找最大与最小元素

​	可以使用 **it.size** 或 **it.length** 方法来查看迭代器中的元素个数

​	可以使用**it.contains** 方法检测迭代器中释放包含指定元素
```







#### get函数

在Scala中，get函数是一种可用于访问元组、列表或映射等数据类型中元素的函数。该函数接受一个索引或键作为参数，并返回对应的值。

以下示例演示了如何使用get函数获取元组、列表和映射中的元素：

```Scala
// 访问元组中的第一个元素
val myTuple = (1, "hello")
val firstElement = myTuple.get(0) // 1

// 访问列表元素
val myList = List(1, 2, 3, 4)
val thirdElement = myList.get(2) // 3

// 访问映射元素
val myMap = Map("a" -> 1, "b" -> 2, "c" -> 3)
val valueForB = myMap.get("b") // Some(2)
val valueForD = myMap.get("d") // None

```

在以上示例中，我们分别定义了一个元组、一个列表和一个映射，并使用get函数来访问它们中的元素。对于元组和列表，我们可以通过索引来获取对应的元素值；对于映射，我们可以使用键来获取对应的值。当映射中不存在指定的键时，get函数会返回None。

需要注意的是，结果类型是Option[T]，即可能有一个值（Some），也可能没有值（None）。当元素存在时，结果是Some类型的包装值，我们可以通过Some的get方法来访问其中的元素。而当元素不存在时，结果是None类型的值。

总之，在Scala中，使用get函数可以轻松地获取元组、列表或映射等数据类型中指定元素的值。由于返回的结果是Option类型，因此我们需要以安全的方式处理其结果，这通常需要在代码中使用模式匹配来检查Option是否存在。





#### Option[T]类型



在Spark中，Option 类型是一种用于表示值可能存在或不存在的类型。Option[ T ] 类型可以包含以下两种结果：

- Some [ T ]：代表存在一个类型为T的值；
- None：代表不存在值。

Option[ T ] 被广泛应用于 Spark API 中的许多函数和方法，特别是在处理缺少数据时，方便开发者进行安全性检查、错误处理等操作。

以下示例演示了如何在Spark中使用Option类型：



```Scala
val myMap = Map("a" -> 1, "b" -> 2, "c" -> 3)
val valueForB = myMap.get("b") // Some(2)
val valueForD = myMap.get("d") // None

// 检查Option是否存在值
if (valueForB.isDefined) {
  val v = valueForB.get // 获取存在的值
  println(s"Value for key 'b' is $v")
} else {
  println("Key 'b' does not exist.")
}

// 使用模式匹配处理Option类型结果
valueForD match {
  case Some(v) => println(s"Value for key 'd' is $v")
  case None => println("Key 'd' does not exist.")
}




  def average(list: List[Double]): Option[Double] = {
    if (list.nonEmpty) Some(list.sum / list.length)
    else None
  }

/*
这段代码定义了一个名为average的函数，它接收一个包含Double类型元素的列表作为输入，并返回一个Option[Double]类型的结果。该函数将根据列表中是否存在元素来计算平均值，并使用Some返回具有平均值的Option对象，或使用None将其转换为表示缺少数据的Option值。

在以上函数中，我们可以看到Option类型的两种情况，一种是当list为空的时候None，还有就是计算出的平均数利用 Some(avg) 进行返回。*/
```



以上示例中，我们使用了Spark中Map类的get方法来获取键对应的值，并返回Option[T]类型的结果。在接收到Option类型的值之后，进行了下述操作：

- 使用isDefined函数来检查Option对象中是否存在值。如果存在，就使用get方法来获取值；
- 使用模式匹配来检查Option对象中是否存在值。如果存在，我们可以将其绑定到变量v并输出；否则输出一个消息，提示用户不存在相应的键。

总之，在Spark中，Option类型是一种非常重要的数据结构，它可以帮助开发人员更好地处理缺少数据或未知数据场景下的代码编写。使用Option类型还可以提高代码的可读性，减少运行时错误，并简化代码的异常处理逻辑。





#### emptyRDD

在Spark中，emptyRDD是一种用于表示一个空数据集（即无元素 RDD）的特殊 RDD。

emptyRDD 通常用于初始化或累积值，被广泛使用在 Spark 中的转换操作中。



可以使用以下代码创建一个 emptyRDD：

```scala
val sc = new SparkContext(...)
val emptyRdd = sc.emptyRDD[Int]
```

以上代码中，我们使用 SparkContext 类的 `emptyRDD[T]` 方法来创建一个空的 `RDD[Int]`对象，其中类型参数 T 可以改变为任何其他类型。

emptyRDD 实际上还有许多其他用途。例如，当运行聚合函数并没有输入数据时，我们可以传递 emptyRDD 而不是没有输入。另外，如果要与 DataFrame 或 DataSet 进行 join 操作时，也可以使用 emptyRDD 来解决 Schema 不匹配问题。

另外，需要注意的是，emptyRDD 并不真正是一个空的 RDD 对象。实际上，在 Spark 中创建了一个包含零个分区的 RDD 对象，而没有任何元素。这意味着所有被应用到该 RDD 的操作将什么都不会发生，并且不会消耗任何计算资源。

总之，在Spark中，emptyRDD是一种无元素 RDD，通常用于初始化或与其他 RDD 合并或转换。它在 Spark 的数据分析工作中非常实用，特别是在处理大数据集合或处理一些极端情况下。



## 数据读取与保存



### 	基础概念

spark的数据读取与数据读取可以从两个维度进行区分：文件格式以及文件系统

**文件格式**：text文件 ，json文件，CSV文件，sequence文件以及object文件

**文件系统**：本地文件系统，HDFS，HBASE以及数据库



- **文件读取**：sc.textFile(String)
- **文件保存**：sc.saveAsTestFile(String)



### Save算子

#### 	作用：

将数据保存至不同格式的文件中

#### 	类型：

​	**saveAsTextFile**

​	**saveAsObjectFile**

​	**saveAsSequenceFile**：要求数据的类型必须是K-V类型





### 	json文件

#### 	概念

使用RDD读取JSON文件处理很复杂，同时SparkSQL集成了很好的处理JSON文件的方式，所以应用中多是采用SparkSQL处理JSON文件。

需先导入解析json所需的包： import scala.util.parsing.json.JSON





#### 	举个例子

```scala
import scala.util.parsing,json.JSON		//导入解析json所需的包

hadoop fs -put ./examples/src/main/resources/people.json/
//上传json文件到hdfs

val json = sc.textFile("/people.json")	//读取文件

val result = json.map(JSON.parseFull)	//解析json数据

result.collect		//打印

```



### Sequence文件

#### 	概念：

SequenceFile文件是[Hadoop](http://lib.csdn.net/base/hadoop)用来存储二进制形式的key-value对而设计的一种平面文件(Flat File)。Spark 有专门用来读取 SequenceFile 的接口。在 SparkContext 中，可以调用 sequenceFile[ keyClass, valueClass](path）

**SequenceFile文件只针对PairRDD**







## 常见示例



`split("\\s+")`是Spark中常用的字符串分割方法，它使用正则表达式`\s+`作为分隔符将字符串切分成多个子串。在正则表达式中，`\s`代表任意一个空白字符，而`+`表示匹配前面的字符一次或多次





```Scala
val newNames = List.range(0, 17).mkString(",").split(",")
```

这段代码使用 Scala 中的 List.range 函数生成从 0 到 16 的整数列表，其中 `List.range(0, 17)` 表示生成一个包含从 0 到 16（因为上界被排除在外）共 17 个元素的整数列表。

接下来，`.mkString(",")` 将列表中的所有元素用逗号分隔，并将它们转换成一个字符串。因此，这一部分代码返回的是字符串 `"0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16"`。

最后，`.split(",")` 根据逗号分割上述字符串，并将其转换为一个字符串数组。因此，这段代码返回的是一个 `Array[String]` 类型的变量 newNames，该变量包含了从 "0" 到 "16" 的 17 个字符串元素。





```Scala
val df2 = dfRename.withColumn("newColumn", dfRename("2") * 2)
```

dfRename("2")`：这个操作通过列名称 "2" 从 DataFrame `dfRename` 中选择了一个包含所有 "2" 列数值的 Column 对象。这个对象实际上是 Scala 语言中的一个表达式，但因为 Spark SQL API 中的隐式转换机制，可以直接作为算术运算符或者其他列转换方法的输入参数。

`dfRename("2") * 2`：这个操作使用箭头符号（`*`）对 Column 表示的列进行乘法操作，与任何常规整数一样。结果是一个新的 Column 类型的对象，等于 "2" 列原始数据乘以 2 的结果。根据上下文可知，命名为 newColumn。

`dfRename.withColumn("newColumn", dfRename("2") * 2)`：这个操作使得 `dfRename` DataFrame 上加入一个名为 "newColumn" 的新列，其内容等于第二列的值乘以 2 的结果。

最后将结果赋值给新的 `DataFrame` 对象 `df2`。由此可见，该代码会对 Spark DataFrame 进行转换，并且使用了 DataFrame API 结合了函数式编程的风格来处理大规模数据集。



`df2("newColumn").cast("int")`：这个操作调用 Column 类型对象上的 cast 方法，将它的类型转换为 Int 型。





```scala
private def mineRule7(graph: Graph[Byte,Int]) : RDD[(Long,Long,Long,String)] ={
    
    val verts = Array(isPerson _, isCompany _, isListedCompany _)
    val edges = Array((0,1, isHoldingMoreThan(3000)_),(0,2,isHolding(0) _),(1,2,isHolding(0) _))
    
    SubGraphQueryWithFunction.query(graph, verts, edges)
    .map(x => (x(0), x(1), x(2),"Rule7")).persist(StorageLevel.MEMORY_AND_DISK) 
}
```



## 	常见概念

#### 		血缘关系：

​	多个连续的RDD之间的依赖关系被称为血缘关系



**OneToOne 依赖/窄依赖**

​	新RDD的一个分区中的所有数据都依赖于旧RDD的一个分区



**Shuffle依赖/宽依赖**

​	新的RDD的一个分区中的数据依赖于旧RDD的多个分区



**窄依赖**：表示每一个父（上游）RDD的 partition 最多被子（下游）RDD的一个 partition 依赖

**宽依赖**：表示每一个父（上游）RDD的 partition 被多（下游）RDD的多个 partition 依赖





#### 	RDD任务划分

​	RDD 任务切分中间分为 **Application**，**Job**，**Stage** 和 **Task**



**Application**：初始化一个SparkContext 即生成一个Application

**Job**：一个Action算子就会生成一个job

**Stage**：Stage等于宽依赖（ShuffleDependency）的个数加一

**Task**：一个Stage阶段中，最后一个RDD的分区个数就是Task的个数







#### 	RDD持久化



RDD对象可以重用，但数据无法重用

RDD中不存储数据，如果一个RDD需要重复使用，则需要从头再次执行运算以获取数据

持久化操作不一定是为了重用，在数据执行过程较长或数据较为重要的场合时也可以采用持久化操作



**缓存特点**：

​	保留RDD之间的血缘关系，即使缓存丢失也可以基于血缘关系进行重新计算



**缓存方式**：

​	RDD是将自己分区的数据，每个分区自行将其数据保存在其所在的Executor内存和硬盘上，属于 **分散存储**



##### 	RDD Cache / Persise 缓存

​	RDD通过 **Cache** 或者 **Persise** 方法将前面的计算结果缓存，默认情况下会把数据缓存在JVM的堆内存中

​	但并不是在这两个方法被调用时就立刻缓存，而是在触发后面的action算子时，该RDD才会被缓存在计算节点的内存中，并供后面重用。

​	Cache 方法在底层调用 Persise 方法实现缓存，**Cache 默认将数据缓存在内存中** 

​	Persise 方法缓存数据使用后	释放缓存：unPersise ( )



**Cache** 或者 **Persise** 方法无论是缓存在内存还是磁盘，都是临时保存，也存在丢失风险

​			内存中，缓存是不安全的，比如断电\计算任务内存不足，把缓存清理给计算让路；

​			硬盘中，因为硬盘损坏丢失的可能性也是存在的





**参数**：

 NONE    :    什么类型都不是

 SER		： 序列化（不加该参数默认为反序列化）

 DISK	  ：  磁盘

 MEMORY 	：内存



    DISK_ONLY     ：     磁盘
    
    DISK_ONLY_2    ：    磁盘    双副本
    
    MEMORY_ONLY    ：    内存    反序列化    把RDD作为反序列化的方式存储，假如RDD的内容存不下，剩余的分区在以后需要时会重新计算，不会刷到磁盘上。（大不了不存..）
    
    MEMORY_ONLY_2    ：  内存    反序列化    双副本
    
    MEMORY_ONLY_SER     ：    内存    序列化     这种序列化方式，每一个partition以字节数据存储，好处是能带来更好的空间存储，但CPU耗费高
    
    MEMORY_ONLY_SER_2   :     内存    序列化    双副本
    
    MEMORY_AND_DISK     ：    内存 + 磁盘    反序列化   RDD以反序列化的方式存内存，假如RDD的内容存不下，剩余的会存到磁盘
    
    MEMORY_AND_DISK_2    :    内存 + 磁盘    反序列化    双副本
    
    MEMORY_AND_DISK_SER    ：    内存 + 磁盘    序列化  
    
    MEMORY_AND_DISK_SER_2    ：    内存 + 磁盘    序列化     双副本
    
    *********** 序列化能有效减少存储空间，默认MEMORY_ONLY







##### Checkpoint 检查点

​	**Checkpoint** 技术：将RDD的数据永久保存（区分于缓存），但**仅支持硬盘存储，不保留血缘关系**，被设计认为安全，保存过程中集中收集各个分区的数据进行存储，属于**集中存储**。



​	checkpoint需要落盘，需要先指定 检查点保存路径 ，检查点路径保存的文件在作业执行完毕后不会被删除



​	可以把数据持久化写入到hdfs上。后续要触发checkpoint持久化操作，需要有一个action操作，后续会开启新的job执行checkpoint操作。



​	它会改变rdd的依赖关系，后续数据丢失了不能够在通过血统进行数据的恢复。

​	

​	为了保证数据安全，一般情况下会单独再执行一次作业

​	为了提高效率，一般情况下需要与Cache联合使用，先 Cache 再 checkpoint



##### 缓存与 Checkpoint 对比

​	Checkpoint 不管分区数量多少，风险都一样，但缓存的分区越多，风险也越高

​	Checkpoint 支持写入 HDFS，但缓存不行，因为HDFS具有高可靠存储，所以 Checkpoint 被认为是安全的

​	Checkpoint 不支持写入内存，但缓存可以，在缓存写入内存时，性能要比 Checkpoint 好一些

​	Checkpoint 属于重量级保存，只能存储在硬盘上（本地 / HDFS），属于集中存储，Cache 属于轻量化保护 RDD数据，可以存储在硬盘与内存中，属于分散存储

​	因为 Checkpoint 被认为安全，所以不保留血缘关系，而缓存在设计上被认为不安全，所以保留血缘关系



cache与 checkpoint 也有区别。

​	前者虽然可以将 RDD 的 partition 持久化到磁盘，一旦 driver program 执行结束，也就是 executor 所在进程 ，被 cache 到磁盘上的 RDD 也会被清空。

​	而 checkpoint 将 RDD 持久化到 HDFS 或本地文件夹，如果不被手动 remove 掉，是一直存在的，也就是说可以被下一个 driver program 使用，而 cached RDD 不能被其他 dirver program 使用。



**性能对比：**

​	**Cache性能更好，因为是分散存储，各个Executor并行执行，效率高**

​	**CheckPoint比较慢，性能消耗更大，因为是集中存储涉及到网络IO，但是存储到HDFS更加安全（多副本机制）**





##### idea实际使用

```Scala
    val sc: SparkContext = {
          // 创建SparkConf对象，设置应用的配置信息
          val sparkConf: SparkConf = new SparkConf()
            .setAppName(this.getClass.getSimpleName.stripSuffix("$")) //设计应用程序的名称
            .setMaster("local[2]") //设置运行模式为本地，且CPU核心数为两核
          // 传递SparkConf对象，构建Context实例
          new SparkContext(sparkConf)
        }
        sc.setLogLevel("WARN")

```







#### shuffle

spark的shuffle操作必须进行“落盘处理”（将数据先存储至磁盘，待分组完毕后再写入内存，不能在内存中等待数据，以防止内存溢出），该操作需要与磁盘交互，使得shuffle性能非常低



shuffle会产生大量的中间磁盘文件，进而由大量的磁盘IO影响性能 



spark中会导致shuffle操作的有以下几种算子：

1，	**repartition**类的操作：比如 repartition、repartitionAndSortWithinPartitions、coalesce(默认不会，第二个参数shuffle:（Boolean ）：为Ture时会；少分区向多分区转换会)等

2，	**byKey**类的操作：比如 **reduceByKey、groupByKey、sortByKey** 等

3，	**join**类的操作：比如join、cogroup等

4，	去重，聚合，排序，重分区，集合或表操作



**重分区**: 一般会shuffle，因为需要在整个集群中，对之前所有的分区的数据进行随机，均匀的打乱，然后把数据放入下游新的指定数量的分区内



**byKey类操作**：因为你要对一个key，进行聚合操作，那么肯定要保证集群中，所有节点上的，相同的key，一定是到同一个节点上进行处理



**join类操作**：两个RDD进行join，就必须将相同join key的数据，shuffle到同一个节点上，然后进行相同key的两个rdd数据的笛卡尔乘积







#### 序列化



​	继承 **Serializable** 接口：extends Serializable

​	如果需要在RDD算子中使用到具体的对象，则需要将该对象序列化，该对象对应的类继承 Serializable 接口

因为在 **spark** 程序中 RDD方法外部的操作都是在 **driver** 端执行，而方法内部的逻辑代码都是在 **Executor** 端执行，driver 端定义生成的对象想要在 RDD算子的Executor 端执行需要进行序列化之后从 driver 端传输至 Executor端



​	使用case修饰类形成样例类：case class name()，样例类在编译时会自动混入序列化特质（实现可序列化接口）



#### **闭包检测**：

​	传入RDD中的对象 / 对象属性 需要检测是否已经进行过序列化，若没有序列化则报错，其他类型的变量则不需要（字符串本身就已经经历过序列化）

类的构造参数其实是类的属性，RDD使用构造参数时需要通过闭包检测





Spark 出于性能考虑，在 Spark2.0 中开始支持另一种序列化机制 （ Kryo ），该机制的速度是Serializable的十倍，当 RDD 在 Shuffle 数据时，简单数据类型/数组/字符串类型都已经在 Spark 内部使用 Kryo 进行序列化



**注意：**即使对象使用了 Kryo 序列化，也需要继承 Serializable 接口









## 实际案例



##### 小功能：

从服务器日志数据Apache.log中获取每个时间段的访问量

```scala
 //初始化配置信息及SparkContext
    val sparkConf: SparkConf = new SparkConf().setAppName("bbb").setMaster("local[*]")
    val sc = new SparkContext(sparkConf)

    val rdd = sc.textFile("datas/Apache.log")		//从本地读取文件

    val timeRDD = rdd.map(
      line => {
        val datas = line.split(" ")
        val time = datas(3)	
         //记录类型：83.148.9.213 - - 17/05/2015:10:05:03 +0000 GET /datas/logday/input/buzhi-bigdata/

        //  获取hours的方法一：   time.substring(10,12) //用于截取字符串
        val sdf = new SimpleDateFormat("dd/MM/yyyy:HH:mm:ss")	 //按照时间格式存储
        val date = sdf.parse(time) 	//获取时间格式的数据
        
        //  获取hours的方法二：    date.getHours
        val sdf1 = new SimpleDateFormat("HH")
        val hour = sdf1.format(date)

        (hour, 1) //返回该时间点作为键的元组，value值为1

      }
    ).groupBy(_._1)   //.reduceByKey( _+_)

//.groupBy(_._1):按集合中每个元组的第一个元素进行分组：Array((03,CompactBuffer((03,1),(03,1)，(03,1)), (10,CompactBuffer((10,1),(10,1),(10,1),(10,1))))

//.reduceByKey( _+_ )：按元组中的键进行分组，将相同键的值进行相加，后续不需要再继续运算，结果为(03,3),(10,5)

    timeRDD.map {
      case (hour, iter) => {
        (hour, iter.size)			//求出每个键（houre）出现的次数  (03,3),(10,5)
      }
    }.collect().foreach(println)



// 拓展对比 ===========================================================================

// groupBy 与 groupByKey 返回值对比
//groupBy会将每一个key value单独拿出来组成元组，而groupByKey会将key与对应的所有value组成一个集合


// groupByKey
//(a,CompactBuffer(1,2,3))
//(b,CompactBuffer(2,4))
//(c,CompactBuffer(1))

//groupBy
//(a,CopactBuffer((a,1),(a,2),(a,3)))
//(b,CopactBuffer((b,2),(b,4)))
//(c,CopactBuffer((c,1)))




    val rdd = sc.parallelize(List(("a",1),("a",2),("a",3),("b",1),("b",2),("c",1)))
    val rdd1: RDD[(String, Iterable[(String, Int)])] = rdd.groupBy(_._1)
    val rdd2: RDD[(String, Iterable[Int])] = rdd.groupByKey()


    rdd1.collect().foreach(print)
    println()
    rdd2.collect().foreach(print)


    println()

    rdd1.map {
      case (flag, num) => {
        (flag, num.size)
      }
    }.collect().foreach(print)


    println()

    rdd2.map {
      case (flag, num) => {
        (flag, num.size)
      }
    }.collect().foreach(print)
    

// 对应输出结果

//(a,CompactBuffer((a,1), (a,2), (a,3)))(b,CompactBuffer((b,1), (b,2)))(c,CompactBuffer((c,1)))
//(a,CompactBuffer(1, 2, 3))(b,CompactBuffer(1, 2))(c,CompactBuffer(1))
//(a,3)(b,2)(c,1)
//(a,3)(b,2)(c,1)

```







**取每个城市的广告播放量的前三名**

```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}
// TODO 案例实操 - 01

object Req_01 {
  def main(args: Array[String]): Unit = {

    val sparkConf: SparkConf = new SparkConf().setMaster("local").setAppName("-")
    val sc = new SparkContext(sparkConf)


    // 获取原始数据： 时间戳，省份，城市，用户，广告
    val dataRDD = sc.textFile("data")


    // 数据转换： 将原始数据进行结构转换，剔除掉不需要的字段，方便统计
    // 时间戳，省份，城市，用户，广告 => ( (省份，广告)，1 )
    val mapRDD = dataRDD.map {
      line => {
        val datas = line.split(" ")
        ((datas(1), datas(4)), 1)
      }
    }


    //  将转换结构后的数据进行分组聚合，即需要分组又需要聚合：groupByKey
    //  ( (省份，广告)，1 ) => ( (省份，广告)，SUM )
    val reduceRDD = mapRDD.reduceByKey( _ + _)


    // 将聚合结果进行结构转换
    // ( (省份，广告)，SUM ) => ( 省份，（ 广告，SUM ） )
    val newmapRDD = reduceRDD.map {
      case ((prv, ad), sum) =>
        (prv, (ad, sum))
    }


    // 将转换结构后的数据根据省份进行分组
    // ( 省份，（ 广告-A，SUM-A ）, （ 广告-B，SUM-B ） )
    val groupRDD: RDD[(String, Iterable[(String, Int)])] = newmapRDD.groupByKey()


    // 将分组后的数据进行组内排序（降序 (Ordering.Int.reverse) 默认为升序），取前三名（ take(3) ）
    val resultRDD: RDD[(String, List[(String, Int)])] = groupRDD.mapValues(
      iter => {
        iter.toList.sortBy(_._2)(Ordering.Int.reverse).take(3)
      }
    )


    // 采集数据，输出至控制台
    resultRDD.collect().foreach(println)

    //(4,List((12,25),(2,22),(16,22)))
    //(8,List((2,27),(20,23),(19,20)))
  }
}

```









```scala
import org.apache.spark.rdd.RDD
import org.apache.spark.{SparkConf, SparkContext}

object Req_03 {
  def main(args: Array[String]): Unit = {
    val sparkConf: SparkConf = new SparkConf().setMaster("local").setAppName("-")
    val sc = new SparkContext(sparkConf)


    // 读取原始的日志信息
    val actionRDD: RDD[String] = sc.textFile("datas/user_visit_action.txt")


    // 统计品类的点击数量  （品类ID，点击数量）
    val clickActionRDD = actionRDD.filter(
      action => {
        val datas = action.split("_")
        datas(6) != "-1"
      }
    )

    val clickCountRDD: RDD[(String, Int)] = clickActionRDD.map(
      action => {
        val datas = action.split("_")
        (datas(6), 1)
      }
    ).reduceByKey(_ + _)



    // 统计品类的下单数量  （品类ID，下单数量）
    val orderActionRDD = actionRDD.filter(
      action => {
        val datas = action.split("_")
        datas(8) != "null"
      }
    )


    val orderCountRDD = orderActionRDD.flatMap(
      action  => {
        val datas = action.split("_")
        val cid = datas(8)

        // cid = 1,2,3,4
        val cids = cid.split(",")

        //[(1,1),(2,1),(3,1)]
        cids.map( id => ( id,1))
      }
    ).reduceByKey( _ + _)



    // 统计品类的支付数量  （品类ID，支付数量）
    val payActionRDD = actionRDD.filter(
      action => {
        val datas = action.split("_")
        datas(10) != "null"
      }
    )


    val payCountRDD = payActionRDD.flatMap(
      action => {
        val datas = action.split("_")
        val cid = datas(10)

        // cid = 1,2,3,4
        val cids = cid.split(",")

        //[(1,1),(2,1),(3,1)]
        cids.map(id => (id, 1))
      }
    ).reduceByKey(_ + _)


    // 将品类进行排序，并且前十名
    // 点击数量排序，下单数量排序，支付数量排序
    // 元组排序：先比较第一个，再比较第二个，第三个，依次排序
    // （品类ID，（点击数量，下单数量，支付数量））

    // cogroup 此处是根据 key 值进行关联，并在每个小范围内进行group
    // 例：第一个范围内存在(a,1)，第二个范围内存在 (a,1)(a,2)，两个相同key的情况，则group成（1,2)，第三个范围内存在(a,3)
    // one.cogroup(two,three)
    // 最终输出格式为 (a,[1],[1,2],[3])

    val cogroupRDD: RDD[(String, (Iterable[Int], Iterable[Int], Iterable[Int]))] =
        clickCountRDD.cogroup(orderCountRDD, payCountRDD)

    // mapValues 对值进行map处理，将未匹配上的 null 字段转为 0
    // 且将集合格式 (Iterable[Int], Iterable[Int], Iterable[Int]) 转换为 (Int, Int, Int) 格式
    // iterator.hasNext  用于检测集合中是否还有元素，如果还有可返回的元素，返回true
    // iterator.next     返回迭代器的下一个元素，并且更新迭代器的状态
    // 此处iterator的结合使用是先判断是否存在null值，如果没有则输出原来的值，如果有则转换为数值 0

    val analysisRDD: RDD[(String, (Int, Int, Int))] = cogroupRDD.mapValues {
      case (clickIter, orderIter, payIter) =>
        var clickCnt = 0
        val iter1 = clickIter.iterator
        if (iter1.hasNext) {
          clickCnt = iter1.next()
        }

        var orderCnt = 0
        val iter2 = orderIter.iterator
        if (iter2.hasNext) {
          orderCnt = iter2.next()
        }

        var payCnt = 0
        val iter3 = payIter.iterator
        if (iter3.hasNext) {
          payCnt = iter3.next()
        }

        (clickCnt, orderCnt, payCnt)
    }

    // 排序
    // false 降序排序  take 取前十行
    // 元组排序：先比较第一个，再比较第二个，第三个，依次排序
    val resultRDD = analysisRDD.sortBy(_._2, ascending = false).take(10)

    // 将结果采集到统计台打印
    resultRDD.foreach(println)
  }
}

```







以上代码的优化点：	

​	actionRDD 被反复使用：加入缓存  actionRDD.cache()

   cogroup 可能存在shuffle操作





​	

# Spark SQL

Spark SQL是Spark用来处理结构化数据的一个模块，提供了两个编程抽象：**DataFrame** 和 **DataSet**，能将Spark SQL转换成RDD



​	在idea中开发程序时，如果需要 RDD 与 DF，DS之间进行转换，则需要引入 **import spark.implicits._**

​	此处的 **Spark** 不是 Scala 中的包名，而是创建的 **sparkSession** 对象的变量名称，所以必须先创建 sparkSession 对象再导入，且这里的 Spark 对象不能使用 **Var** 声明，因为 Scala 只支持 val 修饰的对象的引入。	**spark-shell** 中无需导入，自动完成此操作





**RDD 关注于 数据本身**	：

​			 1，张三，30

**Data Frame 关注于 数据结构**	：

​			id, name, age

**Data Set 关注于 数据类型**	：

​			User





## 基础概念

### SparkSession 与 SparkContext

**SparkSession**是Spark最新的SQL查询起始点

​		实质上是**SQLContext**和**HiveContext**的组合，所以在SQLContext和HiveContext上可用的API在SparkSession上同样是可以使用的

​		**SparkSession内部封装了sparkContext，所以计算实际上是由sparkContext完成的**。







### DataSet 与 DataFrame  

**DataFrame**

​	**DataFrame = RDD + 泛型 + Schema（约束）信息 + 方便的SQL操作 + 优化**

​	DataFrame 是一个分布式数据容器，且更像传统数据库的二维表格，除了数据以外，还记录数据的结构信息，即schema。同与Hive类似，DataFrame也支持嵌套数据类型（struct、array和map）

​	DataFrame 是一种以 RDD 为基础的分布式数据集，类似于传统数据库中的二维表格。

​	DataFrame 与 RDD 的主要区别在于，前者带有schema元信息，是一种特殊的RDD，一个分布式的表

​	DataFrame 为数据提供了Schema 的视图，可以当作数据库中的一张表来对待

​	DataFrame 属于懒执行，性能要比RDD高（优化执行计划）



**DataSet**是 **DataFrame API** 的一个扩展，是SparkSQL最新的数据抽象

​		**DataSet = DataFrame + 泛型**

​	DataFrame只知道字段，但不知道字段的类型，而DataSet不仅仅知道字段，而且知道字段类型，有更严格的错误检查

​	DataSet 是特殊的 DataFrame，是一个分布式的表，DataFrame是特殊的RDD，可以通过 AS 方法将 DataFrame 转换成 DataSet

​	

​	针对RDD、DataFrame 与 Dataset 三者编程比较来说

​			Dataset API无论语法错误和分析错误在编译时都能发现

​			然而RDD和DataFrame有的需要在运行时才能发现

​			此外RDD与Dataset相比较而言，由于Dataset数据使用特殊编码，所以在存储数据时更加节省内存。







### **spark.read.textFile()**  与 spark.sparkContext.textFile()

**spark.sparkContext.textFile()**   和  **spark.read.textFile()** 是 Spark 中两种不同的读取文件的方式，有以下差别：

1. 返回类型不同：

   ​			spark.sparkContext.textFile()  返回一个 RDD（弹性分布式数据集）

   ​			spark.read.textFile()  返回一个 **DataFrame**

   

2. 调用方式不同：

   ​			spark.sparkContext.textFile()  是通过 **SparkContext** 对象进行调用，对应的返回类型为无结构类型的 RDD 数据

   ​			spark.read.textFile()  是通过 **SparkSession** 对象进行调用，对应的返回类型为具有结构类型的 DataFrame 数据

   

   ```scala
   val sparkConf: SparkConf = new SparkConf().setMaster("local").setAppName("WordCount")
   val sc = new SparkContext(sparkConf)
   val lines: RDD[String] = sc.textFile("datas")
   
   val spark =  SparkSession.builder().config(sparkConf).getOrCreate()   // SparkSession 底层封装了 SparkContext
   val df: DataFrame = spark.read.json("datas/user.json")
   ```

   

3. API 不同：

   ​			spark.sparkContext.textFile()  是使用基于 RDD 的 API 进行操作和转换

   ​			spark.read.textFile() 是使用基于 **DataFrame** 的 API 进行操作和转换

   





### 对比RDD，Data Frame，Data Set

**共性**

​		RDD，Data Frame，Data Set 都是spark平台下的分布式弹性数据集

​		

​		三者都具有惰性机制，在创建，转换时都不会立即执行，只有遇到Action（foreach等行动算子）才会开始遍历运算

​		

​		三者都会根据 spark 的内存情况自动缓存运算，即使数据量很大也不需要担心内存溢出

​		

​		三者具有许多共同的函数，如：filter 等，也都具备 **partition** 的概念

​		

​		Data Frame，Data Set  进行操作时需导入： **import spark.implicits._** （在创建好 **Spark Session**对象后尽量直接导入）

​		

​		Data Frame，Data Set  都可以使用模式匹配获取各个字段的值和类型



#### **区别**

​		**RDD**

​				RDD 一般和 spark mllib 同时使用

​				RDD 不支持 spark SQL 操作



​		**Data Frame**

​				Data Frame 每一行的类型固定为 ROW ，每一列的值无法直接访问，只有通过解析才能获得各个字段的值	（与 Data Set 以及 RDD 不同）

​				Data Frame ， Data Set 一般不和 spark mllib 同时使用

​				Data Frame ， Data Set  均支持 spark SQL 操作，还能注册临时表/视窗，进行SQL语句操作

​				Data Frame ， Data Set  支持一些特别方便的保存方式（比如保存为 CSV），可以带上表头，使得每一列的字段名一目了然



​		 **Data Set**

​				Data Frame ， Data Set 拥有完全相同的成员函数，区别只是每一行的数据类型不同，Data Frame 其实就是 Data Set 的一个特例 type DataFrame = Dataset [Row]

​				Data Frame  也可以称为 Data Set  [Row] ，每一行的数据类型都是Row，但Row不解析，每一行存在哪些字段，字段又属于什么类型都不得而知，只能通过 getAS 或 模式匹配拿出特定字段	  





```
工作业绩
	1.完成反欺诈POC测试项目的部分内容
	2.参与需求的初始分析，负责原数据的提取，用户数据的探索与处理开发，验证开发逻辑建立图本体模型，并通过搭建的挖掘模块将处理后的数据接入到图平台对应的实体与边，从数据层面实现出图效率优化
	3.完成图平台挖掘模块的配置与测试，并完成相关SQL开发
    参与图决策模块中的场景搭建工作，包括各项指标的读取配置与计算抽数输出
 	4.优化挖掘场景中各层节点的spark任务并行数，调整运行资源        
 	5.通过SQL优化提高鲁文的运行效率，进而缩短整个场景的运行时间      
 	6.配合整理POC项目中所需的产品说明文档
 	7.完成招商银行ai实验室交易链场景边表和下载表的开发，涉及代码逻辑梳理，脚本优化，以及数据验证，最终转化为pyspark形式，并完成初步上线。
	8.招商银行ai实验室票据链场景的相关实体表，边表以及下载表的开发，涉及到与行方的对接沟通，需求确认，底表跟进以及脚本语言的转化与优化。
	9.理解票据链业务背景以及业务逻辑，独立完成招商银行ai实验室票据链四大输出场景（页面首页，票据挖掘，票据链明细，分析界面）的数据开发工作。
	10.完成东莞银行反洗钱数据字典整理，直销反欺诈的图本体模型的搭建
	
心得体会
	1: 通过实际项目的数据开发积累了银行业务的开发经验，为后续同类型项目的开发奠定了基础。在与行方沟通对接，明确需求以及进度跟进方面积累了一定的方法与经验。
	2: 逐渐熟练pysprak的实际使用，能做到独立 编写执行脚本，定位修改报错，优化执行效率。
	3: 对行方的业务类型与需求场景，技术平台有了更多的了解，进一步沉淀银行业务知识。
	4: 对Scala以及python的实际使用以及熟练程度都有了明显的提升，熟悉Spark的资源监控与相关日志排查。
	5: 对图平台的实际使用有了更加深的了解，也有了一定的经验。

	
```









### 常见函数

#### **stripMargin**

 stripMargin是 Scala 中的一个方法，用于去除多行字符串中每行末尾的空格和分隔符。
		在 Spark SQL 中，stripMargin 方法常用于定义 SQL 语句的多行字符串。



#### **persist** （spark的缓存）

一种可用于将中间计算结果缓存进内存以提高性能的函数。

它可以在从磁盘、内存等存储介质读取数据时，将数据存储在内存或磁盘中，以避免反复计算并加快查询速度。

最终，我们通过  **unpersist()**  方法来删除缓存





#### 临时表

 **createOrReplaceTempView**：

​			临时视图/表，存在则会覆盖替换，更换链接失效

  

**createGlobalTempView**：

​			全局临时视图/表，可供所有会话共享，并在整个spark程序中保持存在，直到应用程序关闭

​			访问对应表时需在表名前增加前缀 **global_表名**



#### 屏蔽日志

```scala
import org.apache.log4j.{Level, Logger}
Logger.getLogger("org.apache.spark").setLevel(Level.WARN)
Logger.getLogger("org.eclipse.jetty.server").setLevel(Level.WARN)
```



#### **反射类型推断**

当RDD中数据类型CaseClass样例类时，通过反射Reflecttion获取属性名称和类型，构建Schema，应用到RDD数据集，将其转换为DataFrame



```Scala
/**
* 封装电影评分数据
*
* @param userId 用户ID
* @param itemId 电影ID
* @param rating 用户对电影评分
* @param timestamp 评分时间戳
*/
case class MovieRating(
	userId: String,
	itemId: String,
	rating: Double,
	timestamp: Long
)



import org.apache.spark.rdd.RDD
import org.apache.spark.sql.{DataFrame, SparkSession}
/**
* 采用反射的方式将RDD转换为DataFrame和Dataset
*/
object SparkRDDInferring {
	def main(args: Array[String]): Unit = {
        
	// 构建SparkSession实例对象
	val spark: SparkSession = SparkSession
				.builder() 				// 使用建造者模式构建对象
				.appName(this.getClass.getSimpleName.stripSuffix("$"))
				.master("local[*]")
				.getOrCreate()

    // import spark.implicits._ ：导入所需要的部分类
    // import spark.implicits.* ：导入所有支持的类与方法  
     import spark.implicits._
        
	// 读取电影评分数据u.data, 每行数据有四个字段，使用制表符分割
	// user id | item id | rating | timestamp
        
    //定义了一个 ratingsRDD 变量，并将其初始化为一个由原始评分数据创建的 RDD    
	val rawRatingsRDD: RDD[String] = spark.sparkContext
		.textFile("datas/ml-100k/u.data", minPartitions = 2)
        
        
     // 转换数据	使用 filter() 方法过滤掉空行和格式不正确的行，并使mapPartitions() 方法对每个分区应用一个映射函数。
    val raringsRDD: RDD[MovieRating] = rawRatingsRDD.filter(x => x != null && x.trim.split("\t").length == 4)
      .mapPartitions{ x =>
      x.map { x =>
          // 拆箱操作, Python中常用
          val Array(userID, itemID, rating, timestamp) = x.trim.split("\t")
          // 返回MovieRating实例对象
         MovieRating(userID,itemID,rating.toDouble,timestamp.toLong)
        }
      }
//    peopleRDD.map{  
//        x => val para = x.split(",");
//       ( para(0), para(1).trim.toInt )
//    }.toDF("name","age")
    
//    peopleRDD.map{
//          x => val para = x.split(",");
//			    People(para(0),para(1).trim.toInt)
//      }.toDF         需创建people case class 实例（类例类的特性）

        
        
/**
        mapPartitions() 方法将每个分区中的元素应用于映射函数，该函数将每行评分数据转换为一个 MovieRating 对象。
        在该映射函数中，我们首先使用 split() 方法按制表符切割每行数据，然后使用 Array() 方法将结果存储为数组。
        接着，我们在 MovieRating 类型的构造函数中，根据数组中的元素创建一个包含四个属性（即 userId、itemId、rating 和 timestamp）的对象。
 **/
        
        
     // 将RDD转换为DataFrame和Dataset
	val ratingsDF: DataFrame = ratingsRDD.toDF()	
        
    ratingsDF.printSchema()
        /*
     printSchema() 方法是 DataFrame API 中的一个方法，用于打印出 DataFrame 的结构信息。它会显示 DataFrame 中每个列的名称、数据类型和是否为可空字段等信息。
	root
	|-- userId: string (nullable = true)
	|-- itemId: string (nullable = true)
	|-- rating: double (nullable = false)
	|-- timestamp: long (nullable = false)
		*/
        
	ratingsDF.show(10)
        
    spark.stop()
```



#### 详细流程

```scala
package SparkDemo

import java.util.Properties

import org.apache.spark.sql.{DataFrame, Dataset, SparkSession}
import org.apache.spark.storage.StorageLevel

object Demo_05 {


  def main(args: Array[String]): Unit = {

    val spark: SparkSession = SparkSession.builder()
      .master("local[*]") //设置运行模式
      .appName("*") //设置进程名称
      .config("spark.sql.shuffle.partitions", "4") //设置shuffle时分区数目
      .getOrCreate()


    //导入隐式转换
    // import spark.implicits._ ：导入所需要的部分类
    // import spark.implicits.* ：导入所有支持的类与方法
    import spark.implicits._

    //通过 SparkSession 对象进行调用，返回一个 DataFrame
    val rawRatingDS: Dataset[String] = spark.read.textFile("ratings.dat")

    val ratingsDF: DataFrame = rawRatingDS
      //过滤数据
      .filter(x => null != x && x.trim.split("::").length == 4)
      //提取转换数据
      .mapPartitions { x =>
        x.map { x =>
          //按照分隔符进行分割，拆箱到变量中
          val Array = x.trim.split("::")
          //最后一行为返回值，返回一个四元组
          (Array(0), Array(1), Array(2), Array(3))
        }
          //指定列名添加Schema
      }.toDF("userID", "movieID", "rating", "timestamp")

          /*
          root
          |-- userId: string (nullable = true)
          |-- movieId: string (nullable = true)
          |-- rating: double (nullable = false)
          |-- timestamp: long (nullable = false)
          */
          //ratingsDF.printSchema()
          /*
          +------+-------+------+---------+
          |userId|movieId|rating|timestamp|
          +------+-------+------+---------+
          | 1| 1193| 5.0|978300760|
          | 1| 661| 3.0|978302109|
          | 1| 594| 4.0|978302268|
          | 1| 919| 4.0|978301368|
          +------+-------+------+---------+
          */

//    基于SQL方式进行分析
//    第一步：注册dataframe为临时视图
//    createOrReplaceTempView：临时视图/表，存在则会覆盖替换
//    createGlobalTempView：全局临时视图/表，可供所有会话共享，并在整个spark程序中保持存在，知道应用程序关闭
//    表名称必须以global_temp. 开头
    ratingsDF.createOrReplaceTempView("ratings")

//    第二步：编写脚本
//    stripMargin是 Scala 中的一个方法，用于去除多行字符串中每行末尾的空格和分隔符。
//    在 Spark SQL 中，stripMargin 方法常用于定义 SQL 语句的多行字符串。
    val top10MovieDF : DataFrame = spark.sql(
      """
        |select
        |movieId, ROUND(AVG(rating), 2) AS avg_rating, COUNT(movieId) AS cnt_rating
        |from
        | ratings
        |group by
        | movieID
        |having
        | cnt_rating > 2000
        |order by
        | avf_rating desc,cnt_rating desc
        | limit 10
        |""".stripMargin)
//    truncate表示是否对过长的字符串进行截断
    top10MovieDF.show(10,truncate = false)


//    基于DSL = Domain Special Language（特定领域语言） 分析

    import org.apache.spark.sql.functions._
    val resultDF : DataFrame = ratingsDF
//    选取字段
      .select($"movieID",$"rating")
//    分组：按照电影id，获取平均评分和评分次数
      .groupBy($"movieID")
      .agg(
        round(avg($"rating"),2).as("avg_rating"),
        count($"movieID").as("cnt_rating")
      )
//    过滤：评分次数大于2000
      .filter($"cnt_rating" > 2000)
//    排序：先按照评分降序，再按照次数降序
      .orderBy($"avg_rating".desc,$"cnt_rating".desc)
//    获取前十
      .limit(10)

//   显示结构
    resultDF.printSchema()
//    输出前十行数据
    resultDF.show(10)

//    将分析的结果数据保存至MySQL数据库与生成csv文件
//    结果dataframe被使用多次，进行缓存
    resultDF.persist(StorageLevel.MEMORY_AND_DISK)


//    保存至MySQL数据库表汇总
    resultDF.coalesce(1)  //考虑降低分区数目
      .write
      .mode("overwrite")    //覆盖写
      .option("driver","com.mysql.cj.jdbc.Driver")
      .option("user","root")
      .option("password","520333").jdbc(
      "jdbc:mysql://node1.itcast.cn:3306/?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true",
        "db_test.tb_top10_movies",
        new Properties()
        )

//    保存CSV文件，每行数据中各字段之间用逗号分隔
    resultDF
      .coalesce(1)
      .write.mode("overwrite")
      .csv("data/top10-movies")

//    释放缓存数据
    resultDF.unpersist()

//    应用结束，关闭资源
    Thread.sleep(10000000)
    spark.stop()


  }
}

```



## Data Frame



### **创建方式**

​	1:	通过spark的数据源进行创建

​	2:	从一个存在的RDD进行转换

​	3:	从 Hive Table 进行查询返回



#### 通过spark的数据源进行创建

​			**spark.read.数据格式( “路径” )**

​		如果是从内存中获取数据，则可以得知具体的数据类型，数字默认为Int

​		如果是从文件中读取的数据（数字），则不能确定数据类型，直接使用big int接收，可以和Long类型转换，但不能和Int进行转换



**对字段的值进行操作**：	

​		将 age 字段对应的值 + 1	:  	使用 $"xxx" 或 ' xxx 的方式引用字段

```scala
df.select($"age" + 1 )
df.select('age + 1)

df.select(S"username",$"age" + 1)
df.select('username,'age + 1)
```



**为运算结果起别名**

```scala
df.select('username,'age + 1 as "newage").show
```



**其余函数操作**

```scala
df.filter('age > 20).show		//筛选大于二十的数据
df.groupBy("age").count.show		//计算年龄的分布数量
```







## Data Set

  **DataSet** 是具有强类型的数据集合，需要提供对应的类型信息



### 创建方式

#### 		通过 Data Frame 转换

​			DF.as ( 样例类)  可直接将 Data Frame 转化为 Data Set （样例类中属性的数据类型需对应 Data Frame  ）

​			Data Set 类型也可以通过 **toDF** 方法转换为 Data Frame 





```scala
    val sparkConf = new SparkConf().setMaster("local[*]").setAppName("SparkSQL")
    val spark: SparkSession = SparkSession.builder().config(sparkConf).getOrCreate()
    import  spark.implicits._
    
    
    val data_02: DataFrame = spark.sql(" select * from data_01 where Pclass = 3")

    val data_set: Dataset[tmp] = data_02.as[tmp]
    
    
    case class tmp(PassengerId:Int,Survived:Int,Name:String)
```



#### 		通过 RDD 转换	

​			当 RDD 中的数据为具有类型（样例类对象）的数据时，可通过 RDD.toDS 函数直接转换为 Data Set

​		

```scala
case class User ( name:String,age:Int)
sc.makeRDD(List(("zhangsan",18),("lisi",22))).map(t => User(t._1,t._2)).toDS
```





### 读取与保存

#### 		读取数据

​			加载数据的通用方法：	spark.read.json/csv/parquet/orc/jdbc( 路径/文件名) **（常用）**

​			如果需要读取不同格式的数据，可以对不同数据格式进行设定：	spark.read.format( 数据类型 )[ .option( "读取数据需要的参数" ) ]. load（ path ）

​			**format**：指定加载的数据类型，包括“ csv , jdbc, json, orc, parquet” 

​			**load** :   	在 “ csv , jdbc, json, orc, parquet”  以及 text File 格式下需要传入加载数据的路径

​			**option** :   在 JDBC 格式下需要传入的相应参数 （url, user, password, dabble）

​			



##### 		读取 CSV 格式数据 

##### **spark.read.format("csv").option("sep",";").option("inferSchema","ture").option("header","true").load("data/user.csv")**

​		**sep** :	指定单个字符分割字段和值， 默认是 ,  

​		**inferSchema** :	自动推断列的类型，需要对数据进行一次额外的传递，默认为false

​		**header** :	是否保留表头，将第一行作为列名，默认是  false

​		

​	

```scala

import org.apache.spark.sql.{DataFrame, Row, SaveMode, SparkSession}
import org.junit.Test
 
import java.util.Properties
 
class $02_Reader {
  val spark: SparkSession = SparkSession.builder().appName("appName").master("local[4]").getOrCreate()
 
   @Test
  def readFile(): Unit ={
     //TODO 读取文件第一种方式
     val df: DataFrame = spark.read.format("text").load("data/wc.txt")
     df.show
 
 
      //TODO 读取文件第二种方式
      val df2: DataFrame = spark.read.text("data/wc.txt")
     df2.show
 
     //TODO 读取csv文件
     // csv 常用的 option:
     //   seq:指定字段之间的分隔符
     //   header: 是否以第一行作为列名
     //   inferSchema: 自动推断列的类型
//     val df3: DataFrame = spark.read.option("header", "ture").option("inferSchema", "true").csv("data/xxx.csv")
//     df3.printSchema()
 
 
 
     //TODO 读取 parquet
     df2.write.mode(SaveMode.Overwrite).parquet("data/parquet")
     spark.read.parquet("data/parquet").show
   }
 
 
  /**
   * 读取 mysql
   */
  @Test
  def readMysql(): Unit ={
    // 指定mysql url地址
    val url = "jdbc:mysql://hadoop102:3306/gmall"
 
    // TODO 拉取mysql整表数据
    val tableName = "user_info"
    //TODO 拉取mysql指定数据
    val tableName1 = "(select * from user_info where id>50) t1"
 
    // 指定读取mysql需要的参数封装
    val props = new Properties()
    props.setProperty("user","root")
    props.setProperty("password","123456")
    // TODO 读取mysql第一种方式 : 此种方式获取 mysql 生成的 DataFrame的分区数 = 1
    // TODO 此种方式获一般只用于小数据量场景
    val df: DataFrame = spark.read.jdbc(url, tableName, props)
    println(df.rdd.getNumPartitions)
 
 
    // TODO 读取mysql第二种方式: 此种方式读取mysql生成的DataFrame的分区数 = conditions数组中元素个数 <不用>
    //每个分区拉取数据的 where 条件
    val conditions: Array[String] = Array("id<=50", "id>50 and id <= 300", "id>300 and id<500", "id>=500")
    val df2: DataFrame = spark.read.jdbc(url, tableName, conditions, props)
 
 
    // TODO 读取mysql第三种方式: 此种式读取mysql 生成的DataFrame的分区数 = (upperBound - lowerBound) < numPartitions ? (upperBound - lowerBound) : nunPartitions
    // TODO 此方式一般用于大数据量场景
    val tableName2 = "(select min(id) min_id, max(id) max_id froam user_info) user"
    val minMaxIdDF: DataFrame = spark.read.jdbc(url, tableName2, props)
    val row: Row = minMaxIdDF.first()
    val min_id: Long = row.getAs[Long]("min_id")
    val max_id: Long = row.getAs[Long]("max_id")
 
    // 用于分区的mysql字段名，必须是数字、日期、时间戳, 建议用主键
    val columnName = "id"
 
    // 用于决定分区数据间距的下限, 一般设置为 columnName字段的最小值
    val lowerBound: Int = 1
 
    // 用于决定分区数据间距的上限, 一般设置为 columnName字段的最大值
    val upperBound: Int = 1
 
    val df4: DataFrame = spark.read.jdbc(url, tableName, columnName, lowerBound, upperBound, 5, props)
 
    println(df4.rdd.getNumPartitions)
    
  }


```







#### 		保存数据

​		 	保存数据的通用方法：	df.write.mode( 保存方式 ).json/csv/parquet/orc/jdbc( 路径/文件名) **（常用）**

​		 	如果需要保存不同格式的数据，可以对不同数据格式进行设定： df.write.mode( 保存类型 ).format( 文件格式 ).save( 路径/文件名 )					

​				**mode**： 保存的类型	默认为error / 文件存在则抛出异常

​														append	    文件存在则追加

​														overwrite	文件存在则覆盖

​														ignore		  文件存在则忽略（不做操作）





#### 		Parquet

​			Parquet 是 Spark SQL 中默认的数据源格式，能有效存储嵌套数据的列式存储格式

​			数据源为 Parquet 文件时，Spark SQL 可以方便的执行所有操作，不需使用 format

​			可通过修改配置项  **spark.sql.sources.default**  修改默认数据源格式







### 连接MySQL数据库

```scala
import org.apache.spark.SparkConf
import org.apache.spark.sql.{DataFrame, SaveMode, SparkSession}


/***
*	实际使用中的步骤
*	环境准备 -> 数据获取 -> 转换为对应格式 （ DataFrame -> 保存成临时表） 
*				  -> 使用spark.sql进行数据处理 -> 数据存储
***/
object DemoSQL_02 {
  def main(args: Array[String]): Unit = {
    val sparkConf = new SparkConf().setMaster("local[*]").setAppName("SparkSQL")
    val spark: SparkSession = SparkSession.builder().config(sparkConf).getOrCreate()

    //TODO 链接mysql 读取数据
    val df: DataFrame = spark.read.format("jdbc")
      .option("url", "jdbc:mysql://localhost:3306/buzhi")
      .option("driver", "com.mysql.cj.jdbc.Driver")
      .option("user", "root")
      .option("password", "520333")
      .option("dbtable", "data_01")
      .load()

    df.createOrReplaceTempView("data_01")

    // stripMargin 去除空白
    val data_02: DataFrame = spark.sql(" select PassengerId,Survived,Name from data_01 where Pclass = 3".stripMargin)

    
    // persist 缓存
    val data_set: Dataset[tmp] = data_02.as[tmp].persist()
    data_02.createOrReplaceTempView("data_02")

    spark.sql("select * from data_02 where Survived != 0".stripMargin).show()

    // unpersist 释放缓存
    data_set.unpersist()

    // 保存数据
//    df.write.format("jdbc")
//      .option("url", "jdbc:mysql://localhost:3306/buzhi")
//      .option("driver", "com.mysql.cj.jdbc.Driver")
//      .option("user", "root")
//      .option("password", "520333")
//      .option("dbtable", "new_data_01")
//      .mode(SaveMode.Append)
//      .save()

    spark.close()
  }
}



// 注意 本机使用的mysql版本为 8.0.20
// 对应依赖jar 包：
	      <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.32</version>
        </dependency>
```





## 项目举例

##### 集团派系

```Scala
 
// 读取每一行的文件进行切割，最终返回一个元组
// val：常量，var：变量
val totalLines = sc.textFile(path).map(line =>
          val jv = parse(line)
          val from = getJsonValue(jv,"_from")
          val to = getJsonValue(jv,"_to")
          val attr = getJsonValue(jv,"_attr")                            
          val key = md5Hash( from + to)
          ( key, from, to, attr, line)                             
                                      )


// toLowerCase:转小写，  toUpperCase:转大写，	||：或
//过滤（符合条件留下，不符合剔除）出元组的第四个元素（attr）为officer或belong的数据
val totalOfficers = totalLines.filter(f => {
           val attr = f._4                          
           attr.toLowerCase.equals("officer") || attr.toLowerCase.equals("belong") } )
//批量操作
	.flatMap{ 
        case (key,from,to,att,_) => {
            
        val srcType = from.split("/").head
        val dstType = to.split("/").head
        val knownListed = false            
        val Type = if (att.toLowerCase.equals("legal") || att.toLowerCase.equals("officer")) Officer else Belong
        
//当dstType =  GraphUtil.Type_Company 且 （srcType = GraphUtil.Type_Company / GraphUtil.Type_Person）         
        if (dstType.toUpperCase.equals(GraphUtil.Type_Company) && (srcType.toUpperCase.equals(GraphUtil.Type_Company) || srcType.toUpperCase.equals(GraphUtil.Type_Person))){
            
            val attr = if (Type == Officer) FLAG_Officer << numberOfBitsToShift else FLAG_Belong << numberOfBitsToShift         
            iterator(key -> ((from, knownListed),(to, knownListed),attr))
        }else
        Iterator.empty            
    }   
  }.repartition(numPartitiom)


println("totalOfficers:" + totalOfficers.count)
totalOfficers.take(10).foreach(x => println("totalOfficers:" + x._2))




val notListedCompanyInvest = sc.textFile(investRatioOfNotListedCompanies).map(line => {
          val jv = parse(line.trim)
          val from = getJsonValue(jv,"_from")
          val to = getJsonValue(jv,"_to")                                                
          val key = md5Hash( from + to)
          val amount = try {
              (getJsonValue(jv,"invest_ratio").toDoble * 10000).toInt
          }  catch{
              case _: Throwable => 0
          }                                                                  
            (key, ((from,false),(to,false),amount + (FLAG_Hold << numberOfBitsToShift)))                                                  } ).repartition(numPartition)



println("notListedCompanyInvest:" + notListedCompanyInvest.count)
notListedCompanyInvest.take(10).foreach(x => println("notListedCompanyInvest:" + x._2))



```



```scala
def legalName(n:String):Boolean = {
    val inlegalName = Array("自然人股东","人民政府","省政府","委员会","国资委","国务院")
    inlegalName.map(name => !n.contains(name)).reduce(_ && _)    
}


def main(args: Array[String]) :Unit = {
    
    val sparkConf = new SparkConf().setAppName("GroupFaction")
    sparkConf.set("spark.driver.maxResultSize","20G")
    sparkConf.set("spark,yarn.executor.memoryOverhead","2048")
    
    val sc = new SparkContext(sparkConf)
    
   	val confFile = if(args(0).toInt == 0) "conf/graph_miner.poperties" else
    "graph_miner.properties"
    
    val prop: Properties = new Properties
    prop.load(new FileInputStream(confFile))
    
    val data_dt = args(1).trim
    
    val sqlContext = new HiveContext(sc)
    
    //企业名称
    val Cust_Nm = prop.getProperty("Cust_Nm")
    //组织性质代码
    val ORG_CHAR_CD = prop.getProperty("ORG_CHAR_CD")
    //经营状态代码 --去除非正常营业的企业
    val OPRT_STCD = prop.getProperty("OPRT_STCD")
    val companyRaw = sc.textFile(company_new_path)
    
    val company = companyRaw.filter(
    line => {
        val jv = parse(line)
        
           //第一行代码使用 json4s 库提供的 \ 方法取得 jv 对象下名称为 Cust_Nm 的字段值，并将其转换成字符串类型。由于该函数运行时可能会抛出异常，需要在 try/catch 语句块中处理，防止程序崩溃。

					//如果上述代码执行成功，则接着去掉字符串两端的任何空格并移除其中的 , 字符。最后，该函数将返回格式良好的字符串 name。如果异常被捕获或者无法从 JSON 中取得指定名称的值，该函数将返回一个 "empty" 字符串
        
        val name = try{
            (jv \ Cust_Nm).values.toString.trim.replace(",", "")
        }
        catch{
            case e: Exception => "empty"
        }
        
        
        val state = try {
            (jv \ OPRT_STCD).values.toString
        }
        catch{
            case e: Exception => "not_key"
        }
        
        
        val entType = try {
            (jv \ ORG_CHAR_CD).values.toString
        }
        catch{
            case e: Exception => "个体工商户"
        }
        
        //长度>4 且 不包含特殊部门 且 值不为empty
        name.length > 4 && legalName(name) && !name.equals("empty")       
    }    
  ).repartition(numPartition)
    
    
    if(Debug_01 == 1) {
        print("company_count: " + company.count())
        company.take(10).foreach(println)
    }
    
    
    
    val parent_id = prop.getProperty("parent_id")
    val parent_label = prop.getProperty("parent_label")
    
    
    //查询结果按照 JSON 格式进行序列化，然后再分成多个 Partition（在这里是 500）以便进行 MapReduce 类型的计算，Scala中对变量的调用与shell相似
    val pageRanks : RDD[(String,Double)] = sqlContext.sql(s"select * from ${kgs_base_entity_company_parent}").toJSON.rdd.repartition(500).map(
    line => {
        val jv = parse(line)
        val id = (jv \ parent_id).values.toString
        val pr = (jv \ parent_label).values.toString.toDouble
        id -> pr
       //->符号是Scala中用于表示键值对的运算符，可以将一个键和一个值合成一个二元组
       //在这里，(id, pr) 就是一个由 id 和 pr 组成的二元组，其中 -> 运算符就代表了它们之间的映射。这种写法相当于调用了Tuple2.apply(id, pr) 方法，并生成包含两个元素的元组 (id, pr)。最终，该操作返回的结果就是一个包含多个这样的 (id, pr) 数据的 RDD。
    }   
  ).repartition(numPartiton)
   
    
    
    val Cst_UUID = prop.getProperty("Cst_UUID")
    val companyIdName = company.map(
    line => {
        val jv = parse(line)
        val key = (jv \ Cst_UUID).values.toSting
        val name = (jv \ Cust_Nm).values.toSting
        s"company/$key" -> name
    })
    
   
    
}
```





# Spark Streaming





## 基础概念



数据处理的方式：

​		流式数据处理：来一条处理一条

​		批量数据处理：积攒一批统一处理



数据处理延迟的长短：

​		实时数据处理：毫秒级别

​		离线数据处理：小时/天

















# Scala

### 基础知识

Windows cmd退出Scala： (  :quit   需要加:)



scala注释使用与java完全一致

分支语句的返回值：最后一行表达式的值

动态绑定 = 多态





**object**：关键字，声明一个单例对象（伴生对象），全局只有唯一一个

**main**：从外部可以直接调用执行的方法，只能被定义在object单例对象内

**def**：方法名称(参数名称：参数类型)： 方法返回值类型 = { 方法体 }



​	伴生对象与伴生类的名称必须一样，且需要放在同一个文件中

​	伴生对象与伴生类可以互相访问对方所有的私有成员



**惰性赋值**

​	使用关键字：lazy  在使用到时才会执行，延迟变量的加载时机

​	只能使用常量 val 接收，不能使用var接收

​	该函数称为**惰性函数**，加载方式称为**惰性加载**，**懒加载**



##### **标识符**

​	规则必须遵守，规范约定俗成

 	**命名规则**：
 		必须由 大小写字母，数字，下划线，美元符 组成

​	且不能以数字开头，不能和Scala关键字重名

​	**命名规范**：

​		变量或方法：从第二个单词开始，每个单词的首字母都大写，其它字母都小写（小驼峰命名法）

```scala
zhangSanAge, student_Country
```

​		类或特质（Trait）：每个单词的首字母都大写，其它字母都需小写（大驼峰命名法）

```scala
Person,StudentDemo
```

​		包：全部小写，一般都是公司的域名反写，多级包之间用 . 分割

```scala
com.itheima.add,cn.itcast.update
```









### 变量与数据类型



#### 代码规范：

（1）使用一次 tab 操作，实现缩进，默认整体向右边移动，用 shift+tab 整体向左移

（2）或者使用 ctrl + alt + L 来进行格式化

（3）运算符两边习惯性各加一个空格。比如：2 + 4 * 5。 





#### 常量与变量：



**var 变量名 [: 变量类型] = 初始值  		var i : int =10**

**val 常量名 [: 常量类型] = 初始值		   val j : int = 20**



常量：在程序执行过程中，值不会改变。

能使用常量的地方就尽量不使用变量



#### 注意事项：

- 声明变量时，类型可以省略，编译器会自动推导，即**类型推导**
- 类型确定后，不能再更改，说明scala是强数据类型语言
- 变量声明时必须要有初始值，在声明/定义一个变量时，可以使用var/val修饰变量，var所修饰的变量可以改变，而val修饰的变量不可变，但对象的状态（值）却可以改变，比如：自定义对象，数组，集合等



#### 数据类型：

- scala中一切数据都是对象，都是any的子类，而java的基本数据类型并不是真正意义上的对象，java中的基本类型与引用类型都没有共同的祖先

  

- scala的数据类型分为两大类：数值类型（AnyVal）与引用类型（AnyRef），两种类型都是对象

  

- scala的数据类型仍然遵守低精度类型向高精度数据类型的自动转换（隐式转换）

  

- Unit：对应java中的void，用于方法返回值的位置，表示该方法没有返回值。Unit是一个数据类型，只有一个对象（），而void不是数据类型，只是一个关键字

  

- Null是一个类型，只有一个对象：null，它是所有引用类型的子类

  

- Nothing是所有数据类型的子类，主要用在一个函数没有明确返回值时使用，这样可以将抛出的返回值返回给任何变量或函数



​	**Any**：所有类型的父类，相当于java中的object

​	

**AnyVal**：所有数值类型的父类

​				Double	Boolean	Int	Short	Unit（类似于Java中的void关键字，当方法没有返回值时使用该类型）....



​	**AnyRef**：所有引用类型的父类

​				String 	数组	对象类型

​	

**Null**：所有**引用类型**的子类，它的值只有一个 null，无法赋值给数值类型的变量

​	

**Nothing**：所有数据类型的字类，所有的数据类型都是它的父类，一般需要结合异常使用





Scala的数据类型与Java的数据类型绝大部分都相同

**区别**

​	1: Scala中所有的类型都使用大写字母开头

​	2: 整型使用Int 而不是 integer，默认的整型为 Int，默认的浮点型为Double

​	3: Scala中定义变量可以不写类型，让Scala编译器自动推断





##### 整数类型：

Byte：

​	8位有符号补码整数，数值区间为-128-127

Short：

​	16 位有符号补码整数。数值区间为 -32768 到 32767

Int：

​	32 位有符号补码整数。数值区间为 -2147483648 到 2147483647

Long：

​	64 位有符号补码整数。数值区间为 -9223372036854775808 到9223372036854775807 = 2 的(64-1)次方-1 



**Scala 的整型，默认为 Int 型，声明 Long 型，须后加‘l’或‘L’**

**Scala 程序中变量常声明为 Int 型，除非不足以表示大数，才使用 Long**



##### 浮点类型（float，double）：

Float：32 位, IEEE 754 标准的单精度浮点数

Double：64 位 IEEE 754 标准的双精度浮点数

**Scala 的浮点型常量默认为 Double 型，声明 Float 型常量，须后加‘f’或‘F’。**



##### 字符类型（Char）：

基本说明：

字符类型可以表示单个字符，字符常量是用单引号括起来的单个字符

\t：一个制表位，实现对齐功能	

\n：换行符

\ \：表示 \ 符号

\ "：表示 '' 符号



##### **布尔类型（Boolean）：**

Boolean类型数据只允许取值true跟false

Boolean类型只占一个字节





##### **Unit** **类型、Null 类型和** Nothing **类型**：

###### 		基本说明

1. Unit：表示无值，与java中的void关键字一样，用作不返回任何类型的方法结果中。Unit只有一个实例值，写作：（）

   

2. Null：null只有一个实例值 ：null

   null可以赋值给任意引用类型，但不能赋值给值类（AnyVal）

   

3. Nothing：Nothing在scala的类层级最低端，是任何其他类型的子类型。当一个函数未确定返回值类型时，可使用Nothing来指定返回值类型，使得我们可以将返回的值（异常）赋给其他函数或变量（兼容性）



#### 类型转换：

##### 数值类型自动转换：

范围小的数据类型会自动转换成范围大的数据类型值

​		从小到大：Byte,	Short,	Char,	Int,	Long,	Float,	Double





（1）自动提升原则：有多种类型的数据混合运算时，系统首先自动将所有数据转换成精度大的那种数据类型，然后再进行计算。



（2）把精度大的数值类型赋值给精度小的数值类型时，就会报错，反之就会进行自动类型转换。



（3）（byte，short）和 char 之间不会相互自动转换。



（4）byte，short，char 他们三者可以计算，在计算时首先转换为 int 类型



（5）char类型可以转换成int类型





##### 强制类型转换：

​			将范围大的数据类型强制转换为范围小的数据类型

​			强制类型转换可能会造成精度缺失



​			val / var 变量名: 数据类型 = 具体值.toXxx		Xxx表示需要转换成的数据类型

​			(Xxx 数据类型的关键字首字母大写)

​	

**数值类型与string类型之间的互相转换**

​				数值类型转换为string类型

​					格式一： val / var 变量名: String = 值数据类型 + ""

​					格式二：val / var 变量名:  String = 值数据类型.toString



​			    string类型转换为数值类型

​					val / var 变量名: 数据类型 = 具体值.toXxx		Xxx表示需要转换成的数据类型

​					String类型转换为Char类型的方式有点特殊，并不是调用toChar  而是toCharArray



**基本说明**：自动类型转换的逆过程，将精度大的数值类型转换为精度小的数值类型。使用时要加上强制转函数，但可能造成精度降低或溢出，格外要注意。



**注意事项**：

（1）将数据由高精度转换为低精度，就需要使用到强制转换

（2）强转符号只针对于最近的操作数有效，往往会使用小括号提升优先级

**举个例子：**

```scala
object TestForceTransfer {
 def main(args: Array[String]): Unit = {
 //（1）将数据由高精度转换为低精度，就需要使用到强制转换
 var n1: Int = 2.5.toInt	 // 这个存在精度损失
 
 //（2）强转符号只针对于最近的操作数有效，往往会使用小括号提升优先级
 var r1: Int = 10 * 3.5.toInt + 6 * 1.5.toInt 	// 10 *3 + 6*1 
= 36
 var r2: Int = (10 * 3.5 + 6 * 1.5).toInt	 // 44.0.toInt = 
44
 println("r1=" + r1 + " r2=" + r2)
 } }
```







### 字符串

#### 	字符串输出：

##### 			基本语法：

1:	通过双引号使用，多行输出时使用“ | ”号作为换行连接符。

​		字符串之间，字符串与变量之间通过+号连接

2:	通过插值表达式使用

​		s"xxx = ${变量名} ， yyy = ${变量名}"，也可以使用  s" $name "

​		**s" ${ name +  num}"：该格式可以进行变量的值运算操作**

3:通过三引号来引用长字符串/大段文本，例如大段的SQL，可保留空格与换行







#### 	键盘输入：

使用步骤：

​			**导包** : import scala.io.StdIn

​			**通过 StdIn.readXxx()  接收用户键盘录入的数据**



##### 			基本语法：

​			StdIn.readLine（）：接收字符串数据类型

​			StdIn.readInt（）：接收整型数据类型

​			StdIn.readShort（）

​			StdIn.readDouble（）



数组转为字符串类型进行输出：xxx.mkString()

字符串转化为数组进行操作：String.toBuffer



```scala
import scala.io._
object Test {
   def main(args: Array[String]) {
      print("请输入菜鸟教程官网 : " )
      val line = StdIn.readLine()

      println("谢谢，你输入的是: " + line)
   }
}
```



#### 写入文件：

​	Scala 进行文件写操作，直接用的都是 java中 的 I/O 类 （**java.io.File**)

```scala
import java.io._

object Test {
   def main(args: Array[String]) {
      val writer = new PrintWriter(new File("test.txt" ))

      writer.write("菜鸟教程")
      writer.close()
   }
}
```



#### 读取文件：

​	可以使用 Scala 的 **Source** 类及伴生对象来读取文件

```scala
import scala.io.Source

object Test {
   def main(args: Array[String]) {
      println("文件内容为:" )

      Source.fromFile("test.txt" ).foreach{
         print
      }
   }
}
```





### 运算符

分类：算术运算符，赋值运算符，关系运算符，逻辑运算符，位运算符

​    ·	Scala中没有三元运算符，已被if- else取代，也没有++ --操作

​    ·	整数相除的结果还是整数，只有浮点型数据参与计算才会出现小数

​	·	任意类型的数据与字符串拼接之后都将是一个新的字符串





Scala 中其实是没有运算符的，所有运算符都是方法。

1）当调用对象的方法时，点.可以省略

2）如果函数参数只有一个，或者没有参数，()可以省略





##### 		==：

Java：  ==比较两个变量本身的值，即两个对象在内存中的首地址

scala：==更加类似于 Java 中的 equals，比较字符串中所包含的内容是否相同





#### **逻辑运算符**：

##### 	基本语法：

用于连接多个条件（一般来讲就是关系表达式），逻辑表达式不管简单还是复杂，最终的结果也是一个 Boolean 值。

Scala代码中不能对一个boolean类型的数据反复进行取反操作，但Java中可以

假定：变量 A 为 true，B 为 false



&& 	逻辑与 	(A && B) 运算结果为 false

|| 	 逻辑或	 (A || B) 运算结果为 true

  !		逻辑非 	!(A && B) 运算结果为 true



#### 赋值运算符：

##### 	基本语法：

赋值运算符就是将某个运算后的值，赋给指定的变量。



**Scala 中没有++、--操作符，可以通过+=、-=来实现同样的效果；**



+：表示正数，普通的加法操作，字符串的拼接

-： 表示负数，普通的减法操作

*：乘号 用于获取两个数据的乘积

/：除法 用于获取两个数据的商

%：取余（求模），用于获取两个数据的余数 （**底层原理  a  -  a/b  *  b**）

= 	简单的赋值运算符，将一个表达式的值赋给一个左值	C = A + B 将 A + B 表达式结果赋值给 C

+= 	相加后再赋值 	C += A 等于 C = C + A	

-= 	相减后再赋值 	C -= A 等于 C = C - A

*=	相乘后再赋值 	C *= A 等于 C = C * A

/= 	相除后再赋值 	C /= A 等于 C = C / A

%= 	求余后再赋值 	C %= A 等于 C = C % A

<<= 	左移后赋值 	C <<= 2 等于 C = C << 2

\>>= 	右移后赋值 	C >>= 2 等于 C = C >> 2

&= 	按位与后赋值 	C &= 2 等于 C = C & 2

^=	  按位异或后赋值 	C ^= 2 等于 C = C ^ 2

|= 	 按位或后赋值 	C |= 2 等于 C = C | 2





**关系运算符**

​			>，>=，<，<=，==，!=

​			·	关系表达式不管简单还是复杂，最终结果一定是boolean类型，要么true，要么false

​			·	== 比较的是数值，eq 比较的是变量地址值 （与Java中的使用效果相反）





### 流程控制

#### 	分支控制**if-else**：

​	分支控制有三种：单分支、双分支、多分支



##### 	单分支：

**基本语法**:

​	if (条件表达式) {

​					执行代码块

​					}

**说明：当条件表达式为 ture 时，就会执行{ }的代码。**



##### 	双分支：

**基本语法**：

​		if (条件表达式) {

​				执行代码块 1

​			} else {

​				执行代码块 2 

​			} 



##### 	多分支：

**基本语法**：

​		if (条件表达式 1) {

​				执行代码块 1 

​			}

​			else if (条件表达式 2) {

​				执行代码块 2 

​			}

​			else {

​				执行代码块 n 

​			} 



**Scala 中 if else 表达式其实是有返回值的，具体返回值取决于满足条件的代码体的最后一行内容。**



**举个例子**：

```scala
 def main(args: Array[String]): Unit = {
     
 println("input age")
 var age = StdIn.readInt()
 val res :String = if (age < 18){
 		"童年"
 }else if(age>=18 && age<30){
 		"中年"
 }else{
		 "老年"
 }
		 println(res)
	 }
}
```





#### 嵌套分支：

##### **概念**：

​	在一个分支结构中又完整的嵌套了另一个完整的分支结构，里面的分支的结构称为内层。

分支外面的分支结构称为外层分支。

​	**嵌套分支不要超过 3 层**

##### 举个例子：

```scala
object TestIfElse {
 def main(args: Array[String]): Unit = {
 	println("input age")
	var age = StdIn.readInt()
 	val res :String = if (age < 18){
		 "童年"
 	}else {
 		if(age>=18 && age<30){
 			"中年"
			 }else{
 				"老年"
 			 }
		 }
 		println(res)
	 }
}
```



##### 引入变量：

```scala
for(i <- 1 to 3; j = 4 - i) {
 println("i=" + i + " j=" + j)
}
```



##### 循环返回值：

将遍历过程中处理的结果返回到一个新 Vector 集合中，使用 yield 关键字

**注意：开发中很少使用**

```scala
object TestFor {
    
 def main(args: Array[String]): Unit = {
 	var res = for(i <-1 to 10) yield {
			 i * 2
 		}
			 println(res)	//Vector(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
 }
```



##### 倒序打印：

如果想倒序打印一组数据，可以用 reverse

```scala
for(i <- 1 to 10 reverse){
 		println(i)
}
```



#### for循环

##### 基础使用

```Scala
范围遍历 to是方法调用，等价于 for(i <- 1.to(10))，包含双边
  for(i <- 0 to 9){
    println(i + ".print")
  }

Range遍历，不包含右边界（上限）,可以传入第三个参数"步长"，默认为步长值为 一
  for(i <- Range(1,10)) {
    println(i + ".print")
  } //不包含右边界（上限）

等价于
  for(i <- 1 until 10)) {
    println(i + ".print")
  }	 // 不包含右边界（上限）

遍历集合,也可以遍历其他类型的集合（Array，List，Set）
  for(i <- Array(12,34,55,78,96)) {
    println(i + ".print")
  }

如果想倒序打印一组数据，可以用 reverse
	for(i <- 1 to 10 reverse){
 		println(i)
	}

将遍历过程中处理的结果返回到一个新 Vector 集合中，使用 yield 关键字
 	var res = for(i <-1 to 10) yield {
			 i * 2
 		}
  println(res)	//Vector(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)
 

```





##### 循环守卫


```Scala
 for(i <- 1 to 10 if i != 2){
    println(i + ".print")
  }

以上代码等价于
 for(i <- 1 to 10){
   if(i != 2){
    println(i + ".print")     
   }
 }

	等价于Java
 for(int i=0;i <= 10;i++){
   if(i == 2){
   		continue
   }else{
    println(i + ".print")     
   }
 }		
```



##### 循环步长

```Scala
for(i <- 1 to 10 by 2){
     println(i + ".print")     
}

for(i <- 30 to 13 by -2){
     println(i + ".print")     
}  //倒序输出

// 倒序输出
	for(i <- 1 to 10 reverse){
 		println(i)
	}
//等价于
	for(i <- 10 to 1 by -1){
     println(i)     
	}  


//循环步长可以为小数，前提是前面的范围也是浮点型
//循环步长为负数时，开始值必须小于结束值，否则报错
for(data <- 1.0 to 10.0 by 0.5){
     println(i)     
	}  
```





#####  引入变量

说明：

​		1: for 推导式一行中有多个表达式式，使用 ； 符号进行分割

​		2: for 推导式有一个约定，当推导式仅包含单一表达式时使用圆括号，当包含多个表达式时，一般每行一个表达式，并使用花括号代替圆括号

```scala

	for(i <- 1 to 3; j = 4 - i) {
		 println("i=" + i + " j=" + j)
	}
  

		// 练习-乘法表
    for(i <- 1 to 9){
      for(j <- 1 to i){
        print(s"$j * $i = ${i * j}\t")
      }
      println()
    }

    // 简化版
    for{i <- 1 to 9;
        j <- 1 to i}{
        print(s"$j * $i = ${i * j}\t")
        if(j == i) println()
    }
```



#### while与do..while循环：

**While 和 do..While 的使用和 Java 语言中用法相同。**



##### while方法：

​			循环变量初始化

​			while (循环条件) {

 					循环体(语句)
 	
 					循环变量迭代

​				}



##### 说明：

（1）循环条件是返回一个布尔值的表达式

（2）while 循环是先判断再执行语句

（3）与 for 语句不同，while 语句没有返回值，即整个 while 语句的结果是 Unit 类型()

（4）因为 while 中没有返回值，所以当要用该语句来计算并返回结果时，就不可避免

的使用变量，而变量需要声明在 while 循环的外部，那么就等同于循环的内部对外部的变量

造成了影响，所以不推荐使用，而是推荐使用 for 循环。 







##### do..while循环方法：

 循环变量初始化

 do{

 	循环体(语句)
 	
 	循环变量迭代

 } while(循环条件)



##### 说明：

（1）循环条件是返回一个布尔值的表达式

（2）do..while 循环是先执行，再判断





#### 循环中断：

##### 	概念

Scala 内置控制结构特地**去掉了** **break** **和** **continue**，是为了更好的适应**函数式编程**，推荐使用函数式的风格解决break和continue的功能，而不是一个关键字。

Scala中使用breakable控制结构来实现 break 和 continue 功能。



##### 	实际操作

​	**采用异常的方式退出循环**

```scala
def main(args: Array[String]): Unit = {
 	try {
 		for (elem <- 1 to 10) {
 			println(elem)
 			if (elem == 5) throw new RuntimeException
 			}
 		}catch {
 			case e =>
 		}
 			println("正常结束循环")
	}
```



**采用 Scala 自带的函数退出循环**

**说明**：导入相关的包后使用Breaks.breakable(）将循环语句包裹，在退出语句后加Breaks.break()

```scala
import scala.util.control.Breaks

	def main(args: Array[String]): Unit = {
 		Breaks.breakable(
			 for (elem <- 1 to 10) {
			 println(elem)
 			 if (elem == 5) Breaks.break()
 			}
 		)
			 println("正常结束循环")
}
```



**对break进行省略**

```scala
import scala.util.control.Breaks._
	object TestBreak {
 		def main(args: Array[String]): Unit = {
 
			 breakable {
				 for (elem <- 1 to 10) {
 				 println(elem)
 				 if (elem == 5) break
				 }
 			}
 
 			println("正常结束循环")
 	} 
 }
```



#### 多重循环：

##### 	基本说明

（1）将一个循环放在另一个循环体内，就形成了嵌套循环。其中，for，while，do…while均可以作为外层循环和内层循环。【**建议一般使用两层，最多不要超过** **3** **层**】 



（2）设外层循环次数为 m 次，内层为 n 次，则内层循环体实际上需要执行 **m\*n** 次。





### 函数式编程

#### 	概念：

Scala 中的函数式编程和面向对象编程做到了完美融合



​	面向对象编程：

​			解决问题，分解对象，行为，属性，然后通过对象的关系以及行为的调用来解决问题。

​			Scala 语言是一个完全面向对象编程语言。万物皆对象

​			对象的本质：对数据和行为的一个封装



​	函数式编程：

​			解决问题时，将问题分解成一个一个的步骤，将每个步骤进行封装（函数）通过调用这些封装好的步骤，解决问题。

​			Scala 语言是一个完全函数式编程语言。万物皆函数。

​			**函数的本质**：函数可以当做一个值进行传递



#### 	函数基础：

##### 			函数基本语法

​	scala中对函数/方法的定义：main函数内称为（狭义上的）函数，main函数外称为方法，两者的作用域不同。	

```scala
def sum ( x:int , y:int ):int={
    x + y
}
//def：定义函数的关键字
//sum：函数名
//x:int,y:int ：参数名与参数类型
//:int ：函数返回值类型
//x + y：函数体

```



​		

##### 		函数与方法的区别

​	（1）为完成某一功能的程序语句的集合，称为函数。

​	（2）类中的函数称之方法。

​	（3）Scala 语言可以在任何的语法结构中声明任何的语法

​	（4）函数没有重载和重写的概念；方法可以进行重载和重写

​	（5）Scala 中函数可以嵌套定义



**函数定义**

​	（1）函数 1：无参，无返回值	def test1(): Unit ={}

​	（2）函数 2：无参，有返回值	def test2():String={}

​	（3）函数 3：有参，无返回值	def test3(s:String):Unit={}

​	（4）函数 4：有参，有返回值	def test4(s:String):String={}

​	（5）函数 5：多参，无返回值	def test5(name:String, age:Int):Unit={}

​	（6）函数 6：多参，有返回值     def tset6(name:String,age:lnt):String={}



**可变参数与默认值**

​	（1）可变参数	

​	（2）如果参数列表中存在多个参数，那么可变参数一般放置在最后

​	（3）参数默认值，一般将有默认值的参数放置在参数列表的后面

​				age : Int = 30 ，s：String*



##### 			函数至简原则

​		（1）return 可以省略，Scala 会使用函数体的最后一行代码作为返回值

​		（2）如果函数体只有一行代码，可以省略花括号

​		（3）返回值类型如果能够推断出来，那么可以省略（:和返回值类型一起省略）

​		（4）如果有 return，则不能省略返回值类型，必须指定

​		（5）如果函数明确声明 unit，那么即使函数体中使用 return 关键字也不起作用

​		（6）Scala 如果期望是无返回值类型，可以省略等号

​		（7）如果函数无参，但是声明了参数列表，那么调用时，小括号，可加可不加

​		（8）如果函数没有参数列表，那么小括号可以省略，调用时小括号必须省略

​		（9）如果不关心名称，只关心逻辑处理，那么函数名（def）可以省略



```scala
object TestFunction {
    
 def main(args: Array[String]): Unit = {
 // （0）函数标准写法
 	def f( s : String ): String = {
 			return s + " jinlian"
 		}
 			println(f("Hello"))
 
     // 至简原则:能省则省
 
     //（1） return 可以省略,Scala 会使用函数体的最后一行代码作为返回值
 	def f1( s : String ): String = {
 			s + " jinlian"
 		}
 			println(f1("Hello"))
 
     //（2）如果函数体只有一行代码，可以省略花括号
 	def f2(s:String):String = s + " jinlian"
 
     
     //（3）返回值类型如果能够推断出来，那么可以省略（:和返回值类型一起省略）
 		def f3( s : String ) = s + " jinlian"
 			println(f3("Hello3"))
 
     
     //（4）如果有 return，则不能省略返回值类型，必须指定。
 		def f4() :String = {
 			return "ximenqing4"
       	}
 			println(f4())

     
     //（5）如果函数明确声明 unit，那么即使函数体中使用 return 关键字也不起作用
 		def f5(): Unit = {
 			return "dalang5"
 		}
 			println(f5())
	
     
     //（6）Scala 如果期望是无返回值类型,可以省略等号
 	// 将无返回值的函数称之为过程
 		def f6() {
 			"dalang6"
		 }
			 println(f6())
 
     
     //（7）如果函数无参，但是声明了参数列表，那么调用时，小括号，可加可不加
 		def f7() = "dalang7"
 			println(f7())
 			println(f7)
 
     
     //（8）如果函数没有参数列表，那么小括号可以省略,调用时小括号必须省略
		 def f8 = "dalang"
 			//println(f8())
 			println(f8)
 
     
     //（9）如果不关心名称，只关心逻辑处理，那么函数名（def）可以省略
 		def f9 = (x:String)=>{println("wusong")
 		def f10(f:String=>Unit) = {
 				f("")
			 }
 				f10(f9)
 			println(f10((x:String)=>{println("wusong")}))
 }
}
```



##### scala中“ =>”的四种使用场景

1：

​	表示函数的返回类型，左边是参数类型，右边是方法返回值类型。

**备注：** 当函数只有一个参数的时候，函数类型里面括起来函数参数的括号是可以省略的。



2：

​	表示匿名函数，匿名函数定义， 左边是参数 右边是函数实现体 （x: Int）=>{}



3：

​	case语句：在模式匹配 match 和 try-catch 都用 “=>” 表示输出的结果或返回的值



4：

​	By-Name Parameters(传名参数)，传名参数在函数调用前表达式不会被求值，而是会被包裹成一个匿名函数作为函数参数传递下去，例如参数类型为无参函数的参数就是传名参数。即A函数作为B函数的参数



#### 匿名函数：

**概念**：没有名字的函数称为匿名函数

​			(x: Int) => { 函数体 }

x：表示输入参数名称， Int：表述输出参数类型，函数体：表示具体代码逻辑



**匿名函数可以作为参数传递**





#### 高阶函数：

##### 1）函数可以作为值进行传递

```scala
object TestFunction {
 	def main(args: Array[String]): Unit = {
 //（1）调用 foo 函数，把返回值给变量 f
 //val f = foo()
		 val f = foo
		 println(f)
 
 //（2）在被调用函数 foo 后面加上 _，相当于把函数 foo 当成一个整体传递给变量 f1
		 val f1 = foo _
 			foo()
 			f1()
        
//（3）如果明确变量类型，那么不使用下划线也可以将函数作为整体传递给变量
		var f2:()=>Int = foo 
 
    	def foo():Int = {
 			println("foo...")
 				1
			 } 
}
```





##### 2）函数可以作为参数进行传递

```Scala
def main(args: Array[String]): Unit = {
 
 // （1）定义一个函数，函数参数还是一个函数签名；f 表示函数名称;(Int,Int)表示输入两个 Int 参数；Int 表示函数返回值
 	def f1(f: (Int, Int) => Int): Int = {
 				f(2, 4)
 			}
 
 // （2）定义一个函数，参数和返回值类型和 f1 的输入参数一致
		 def add(a: Int, b: Int): Int = a + b
 
 // （3）将 add 函数作为参数传递给 f1 函数，如果能够推断出来不是调用，_可以省略
 		println(f1(add))
		println(f1(add _))

}
```





##### 3）函数可以作为函数返回值返回

```scala
def main(args: Array[String]): Unit = {
	 def f1() = {
		def f2() = {
		 【-】f2 _
		}
	}
		val f = f1()
// 因为 f1 函数的返回值依然为函数，所以可以变量 f 可以作为函数继续调用
			f()
// 上面的代码可以简化为
		f1()()
}
```





##### 4）匿名函数简化原则

```scala
 //   可以将匿名函数赋值给变量，使得该变量的类型为函数，则该变量名就是匿名函数的名称，以实现调用
      val f6 = (name : String) => {println(name)}
//   此时 f6的变量类型为   String => Unit   即：一个参数为string类型返回值为unit的函数
      f6("buzhi")

//   定义一个函数，以函数作为参数输入
      def f7(func : String => Unit) : Unit ={
        func("不知")    // 此时可以将f6作为参数传入
      }

      def f8(func : String => Unit,x : String) = {
        func(x)
      }

      f7(f6)
      f7((name : String) => {println(name)})
      f8(f6,"buzhi")



/***   匿名函数的至简原则    ***/

//  1：参数类型可以省略，会根据形参进行自动推导
      f7 ( (name)  =>{ println( name ) } )

//  2：省略类型之后，如果只有一个参数，则可以将圆括号也进行省略
//     没有参数或参数量大于一时不能省略圆括号
      f7( name => { println(name) } )

//  3： 如果匿名函数只有一行，则大括号也可以省略
      f7( name => println(name) )

//  4：如果参数只出现一次，则参数省略后后面调用参数时可以使用 _ 代替
      f7( _ => println(_) )
      f7( println(_) )

//  5: 如果可以推断出，当前传入的println是一个函数体，而不是调用语句，则可以省略下划线
      f7( println )
```



#### 特用函数：

##### 闭包

​	如果一个函数，访问到了它的外部（局部）变量的值，那这个函数和它所处的环境称为闭包







##### 柯里化（Currying）

柯里化(Currying)指的是将原来接受两个参数的函数变成新的接受一个参数的函数的过程。新的函数返回一个与原有第二个参数为参数的函数。



将一个参数列表的多个参数，变成多个参数列表

```scala
def fun02 (i:Int): String => Char => Boolean = {
  def f1(s:String): Char => Boolean ={
    def  f2(c: Char): Boolean = {
      if (i == 0 && s == "" && c == '0') false
      else true
    }
    f2
  }
  f1
}    


def fun03 (i:Int): String => Char => Boolean = {
      s => c => if (i == 0 && s == "" && c == '0') false else true}

def fun04 (i:Int)(s:String)(c:Char):Boolean = {
      if (i == 0 && s == "" && c == '0') false else true
    } 

println(fun03(0)("")('0'))
```



##### 部分参数应用函数

在待传入参数前加：  _:

```scala
 

def log(date: Date, message: String)  = {
      println(date + "----" + message)
   }

//普通函数
      log(date, "message1" )

//部分应用函数

     val logWithDateBound = log(date, _ : String)

     logWithDateBound("message2" )





def Demo(s1: String,s2: String) ={
  println(s1 + ":\t"+s2)
}
    var s = Demo("name",_:String)
	//相当于是创了一个含有参数值的新函数
    s("kunkun")
    s("buzhi")

  }

```



##### 模式匹配

一个模式匹配包含了一系列备选项，每个备选项都开始与关键字 case ，且每个备选项都包含一个模式及一到多个表达式。箭头符号 => 隔开模式和表达式



```scala
// 简单模式匹配
变量 match {
  
  case "常量1" => 表达式一
  case "变量2" => 表达式二
  case "变量3" => 表达式三
  case  _ =>	表达式四		// 默认匹配项
  
 }


    // 提示用户录入字符串并接收
    println("请录入一个字符串： ")
    var str = StdIn.readLine()

    // 判断字符串是否是指定的内容，并接受结果
    val result = str match {

      case "Hadoop" => "大数据分布式存储和计算框架"
      case "zookeeper" => "大数据分布式协调服务框架"
      case "spark" => "大数据分布式内存计算机框架"
      case _ => "未匹配"

    }


    // 打印结果
    println(result)
    println("-" * 15)


    // 简写形式
    str match {
      case "hadoop" => println("大数据分布式存储和计算框架")
      case "zookeeper" => println("大数据分布式协调服务框架")
      case "spark" => println("大数据分布式内存计算机框架")
      case _ => println("未匹配")
    }



```







```Scala
// 匹配类型
// 除了匹配数据之外，match 表达式还可以进行类型匹配
// 如果我们要根据不同的数据类型执行不同的逻辑，也可以使用 match 表达式来实现
对象名 match {
  
  case 变量名1：类型1 => 表达式1
  case 变量名2：类型2 => 表达式2
  case 变量名3: 类型3 => 表达式3
  ...
  case _ => 表达式4
  
}


    // 定义一个变量为 Any 类型，然后分别给其赋值 "Hadoop"，1，1，0
    val a:Any = 1.0

    // 定义模式匹配，然后分别打印类型的名称
    val result_01 = a match{
      case x: String => s"${x}是String类型的数据"
      case x: Double =>  s"${x}是Double类型的数据"
      case x: Int =>  s"${x}是Int类型的数据"
      case _ => "未匹配"
    }

    println(result_01)

    val result_02 = a match {
      case _: String => "String"
      case _: Double => "Double"
      case _: Int => "Int"
      case _ => "未匹配"
    }

    println(result_02)
```





**守卫**

在 case 语句中添加 if 条件判断，可以让我们的代码更加简洁

```scala
// 守卫
变量 match {
  case 变量名 if 条件-1 => 表达式1
  case 变量名 if 条件-2 => 表达式2
  case 变量名 if 条件-3 => 表达式3
  ...
  case _ => 表达式4
}




println("请录入一个整数：")
var num = StdIn.readInt()

num match {
  // 如果 a >= 0 而且 a <= 3 则打印 [0-3]
   case a if a >= 0 && a <= 3 => println("[0-3]")
  // 如果 a >= 4 而且 a <= 8 则打印 [4-8]
   case a if a >= 4 && a <= 8 => println("[4-8]")
  // 否则打印：未匹配
   case _ => println("未匹配")
    }
```



**匹配样例类**

​	可以使用模式匹配来匹配样例类，从而实现快速回去样例类中的数据成员



1:  样例类型后的小括号里编写的字段个数需要和该样例类的字段个数保持一致



```scala
对象名 match {
		case 样例类型1(字段1，字段2，字段n) => 表达式1
		case 样例类型2(字段1，字段2，字段n) => 表达式2
		case 样例类型3(字段1，字段2，字段n) => 表达式3
		case 样例类型4(字段1，字段2，字段n) => 表达式4
		...
    case _ => 表达式5
}


// 创建两个样例类 Customer Order
// Customer 包含姓名，年龄字段
case class Customer(var name: String, var age: Int)

// Order 包含 id字段
case class Order(var id: Int)


def main(args: Array[String]): Unit = {

    // 分别定义两个案例类的对象，并指定为Any类型
val c: Any = Customer("buzhi",18)
val o: Any = Order(345)
val arr: Any = Array(0,1)

    // 使用模式匹配这两个对象，并分别打印它们的成员变量值
    c match {
      case Customer(a,b) => println(s"Customer类型的对象，name = ${a},age = ${b}")
      case Order(c) => println(s"Order类型，id = ${c}")
      case _ => println("未匹配")
    }
}    
```







**匹配集合**

可以用来匹配数组，元组，集合（列表，集，映射）



```scala
// 数组

// 定义三个数组
val arr1 = Array(1,2,3)
val arr2 = Array(0)
val arr3 = Array(1,2,3,4,5)


// 通过模式匹配找到指定的数组
arr1 match {
	case Array(1,x,y) => println(s"匹配长度为3，首元素为1，后两个元素为：${x},${y}")
	case Array(0) => println("匹配只有一个元素，且值为零的数组")
	case Array(1,_*) => println("匹配第一个元素值为一，其余元素个数与元素值都没有限制")
	case _ => println("未匹配") 
    }

// 通过模式匹配找到指定的列表 List

list match{
  case List(0) => println("匹配只有一个元素，且值为零的列表")
  case List(1,_*) => println("匹配第一个元素值为一，其余元素个数与元素值都没有限制")
  case List(x,y) => println(s"匹配只有两个元素的列表，元素为${x},${y}")
  case _ => println("未匹配") 
}

		// 采用 Nil  Tail关键字进行优化

list match{
  case 0 :: Nil => println("匹配只有一个元素，且值为零的列表")
  case 0 :: tail => println("匹配第一个元素值为一，其余元素个数与元素值都没有限制")
  case x :: y :: Nil => println(s"匹配只有两个元素的列表，元素为${x},${y}")
  case _ => println("未匹配") 
}


// 通过模式匹配找到指定的元组

a match {
  case (1,x,y) => println(s"匹配长度为三，且以一开头，后两个元素为 ${x},${y}")
  case (x,y,5) => println(s"匹配长度为三，且以五结尾，前两个元素为 ${x},${y}")
  case _ => println("未匹配")   
}


 val map = Map("张三" -> 23 ,"李四" -> 18,"王五" -> 22)
for((k,v) <- map if v == 18) println(s"${k} = ${v}")
```



**执行流程**

​	1:	 先执行第一个 case，看变量值和该case对应的常量值是否一致

​	2:	如果一致则执行该 case 对应的表达式

​	3:	如果不一致则往后执行下一个 case ，看变量值和该 case 对应的变量值是否一致

​	4：如此类推，如果所有的 case 都不匹配，则执行 case _  对应的表达式





##### 偏函数

​	偏函数常用于模式匹配

**基本概念**

def	function: PartialFunction[A，B]

A：代表函数的参数类型

B：代表函数的返回类型



**举个例子**

```scala
object PartialFuncDemo {
def func1: PartialFunction[String, Int] = { 
		case "one" => 1
		case "two" => 2
		case _ => -1
	}

def main(args: Array[String]) {
       println(func1("one"))
	}
}
```







##### 样例类



样例类Case Class

​		Case Class一般被翻译成样例类，它是一种特殊的类，能够被优化以用于模式匹配。



* 当一个类被声名为case class的时候，scala会帮助我们做下面几件事情：

  1、构造器中的参数如果不被声明为var的话，它默认的是val类型的，但一般不推荐将构造器中的参数声明为var。

  

  2、自动创建伴生对象，同时在里面给我们实现子apply方法，使我们在使用的时候可以不直接使用new创建对象。

  

  3、伴生对象中同样会帮我们实现unapply方法，从而可以将case class应用于模式匹配。

  

  4、实现自己的toString、hashCode、copy、equals方法，定义case class便会自动生成对应的toString,hashCode,equals,copy等方法

  

  除此之此，case class与其它普通的scala类没有区别

```scala
//样例类Student
case class Student(name: String, age: Int, stuNo: String) extends Person

//case class会自动生成 **apply** 方法，创建对象时无需用new
val p: Person = Student(“lisi”, 20, “101”)



object Test {
   def main(args: Array[String]) {
       val alice = new Person("Alice", 25)
    val bob = new Person("Bob", 32)
       val charlie = new Person("Charlie", 32)

   for (person <- List(alice, bob, charlie)) {
        person match {
            case Person("Alice", 25) => println("Hi Alice!")
            case Person("Bob", 32) => println("Hi Bob!")
            case Person(name, age) =>
               println("Age: " + age + " year, name: " + name + "?")
         }
      }
   }
   // 样例类
   case class Person(name: String, age: Int)
}


```



### 集合的使用

##### **集合的类型**：

Scala 的集合有三大类:**序列 Seq、集 Set、映射 Map**

所有的集合都扩展自 **Iterable** 特质

 Scala 中集合有可变( mutable )和不可变( immutable )两种类型，immutable 类型的集合 初始化后就不能改变了(注意与 val 修饰的变量进行区别



数组，列表，set集合都只能存放相同类型的元素

元组可以存放不同类型的元素，但不可变





##### **定长数组和变长数组**

​	定长数组： val arr = new Array [ Int ] ( 8 )

​	可变数组： val arr = new ArrayBuffer[ Int ] ()





##### **Seq（列表）**：

List的特征是其元素以线性方式存储，集合中可以存放重复对象。

在 Scala 中列表要么为空(Nil 表示空列表)要么是一个 head 元素加上一个 tail 列表。



​	不可变序列：

​		val list = List(1,2,3)

​		在list前面插入一个元素以生成一个新的列表

​		val list1 = 0  ::  list



​	可变序列：

​		val list = ListBuffer[Int] (1,2,3)

​		在list后面插入一个元素，不需要生成一个新的列表

​		list.append（4）

​		

##### 	Set集合

Set是最简单的一种集合。集合中的对象不按特定的方式排序，并且没有重复对象。

**Set是无序且没有重复元素的集合**



不可变的Set集合

 val set = new HashSet[Int] ()



可变的Set集合

val set = new mutable.HashSet [  Int  ] ()



##### 元组

Scala 元组将固定数量的项目组合在一起，以便它们可以作为一个整体传递。 元组是不同类型的值的集合，与数组或列表不同，元组可以容纳不同类型的对象，但它们也是不可变的。





#### 迭代器

Scala Iterator（迭代器）不是一个集合，而是一种用于访问集合的方法。



迭代器 it 的两个基本操作是 **next** 和 **hasNext**。

​	调用 **it.next()** 会返回迭代器的下一个元素，并且更新迭代器的状态。

​	调用 **it.hasNext()** 用于检测集合中是否还有元素，如果还有可返回的元素，返回true。



​	可以使用 **it.min** 和 **it.max** 方法从迭代器中查找最大与最小元素

​	可以使用 **it.size** 或 **it.length** 方法来查看迭代器中的元素个数

​	可以使用**it.contains** 方法检测迭代器中释放包含指定元素

```scala
object Test {
   def main(args: Array[String]) {
      val it = Iterator("Baidu", "Google", "Runoob", "Taobao")
      
      while (it.hasNext){
         println(it.next())
      }
     
      val ita = Iterator(20,40,2,50,69, 90)
      val itb = Iterator(20,40,2,50,69, 90)
     
      println("最大元素是：" + ita.max )
      println("最小元素是：" + itb.min )
      println("ita.size 的值: " + ita.size )
      println("itb.length 的值: " + itb.length )
     
   }
}
```



##### **迭代器常用方法**

| 1    | **def hasNext: Boolean**如果还有可返回的元素，返回true。     |
| ---- | :----------------------------------------------------------- |
| 2    | **def next(): A**返回迭代器的下一个元素，并且更新迭代器的状态 |
| 3    | **def ++(that: => Iterator[A]): Iterator[A]**合并两个迭代器  |
| 4    | **def ++[B >: A](that :=> GenTraversableOnce[B]): Iterator[B]**合并两个迭代器 |
| 5    | **def addString(b: StringBuilder): StringBuilder**添加一个字符串到 StringBuilder b |
| 6    | **def addString(b: StringBuilder, sep: String): StringBuilder**添加一个字符串到 StringBuilder b，并指定分隔符 |
| 7    | **def buffered: BufferedIterator[A]**迭代器都转换成 BufferedIterator |
| 8    | **def contains(elem: Any): Boolean**检测迭代器中是否包含指定元素 |
| 9    | **def copyToArray(xs: Array[A], start: Int, len: Int): Unit**将迭代器中选定的值传给数组 |
| 10   | **def count(p: (A) => Boolean): Int**返回迭代器元素中满足条件p的元素总数。 |
| 11   | **def drop(n: Int): Iterator[A]**返回丢弃前n个元素新集合     |
| 12   | **def dropWhile(p: (A) => Boolean): Iterator[A]**从左向右丢弃元素，直到条件p不成立 |
| 13   | **def duplicate: (Iterator[A], Iterator[A])**生成两个能分别返回迭代器所有元素的迭代器。 |
| 14   | **def exists(p: (A) => Boolean): Boolean**返回一个布尔值，指明迭代器元素中是否存在满足p的元素。 |
| 15   | **def filter(p: (A) => Boolean): Iterator[A]**返回一个新迭代器 ，指向迭代器元素中所有满足条件p的元素。 |
| 16   | **def filterNot(p: (A) => Boolean): Iterator[A]**返回一个迭代器，指向迭代器元素中不满足条件p的元素。 |
| 17   | **def find(p: (A) => Boolean): Option[A]**返回第一个满足p的元素或None。注意：如果找到满足条件的元素，迭代器会被置于该元素之后；如果没有找到，会被置于终点。 |
| 18   | **def flatMap[B](f: (A) => GenTraversableOnce[B]): Iterator[B]**针对迭代器的序列中的每个元素应用函数f，并返回指向结果序列的迭代器。 |
| 19   | **def forall(p: (A) => Boolean): Boolean**返回一个布尔值，指明 it 所指元素是否都满足p。 |
| 20   | **def foreach(f: (A) => Unit): Unit**在迭代器返回的每个元素上执行指定的程序 f |
| 21   | **def hasDefiniteSize: Boolean**如果迭代器的元素个数有限则返回 true（默认等同于 isEmpty） |
| 22   | **def indexOf(elem: B): Int**返回迭代器的元素中index等于x的第一个元素。注意：迭代器会越过这个元素。 |
| 23   | **def indexWhere(p: (A) => Boolean): Int**返回迭代器的元素中下标满足条件p的元素。注意：迭代器会越过这个元素。 |
| 24   | **def isEmpty: Boolean**检查it是否为空, 为空返回 true，否则返回false（与hasNext相反）。 |
| 25   | **def isTraversableAgain: Boolean**Tests whether this Iterator can be repeatedly traversed. |
| 26   | **def length: Int**返回迭代器元素的数量。                    |
| 27   | **def map [ B ] (f: (A) => B): Iterator[B]**将 it 中的每个元素传入函数 f 后的结果生成新的迭代器。 |
| 28   | **def max: A**返回迭代器迭代器元素中最大的元素。             |
| 29   | **def min: A**返回迭代器迭代器元素中最小的元素。             |
| 30   | **def mkString: String**将迭代器所有元素转换成字符串。       |
| 31   | **def mkString(sep: String): String**将迭代器所有元素转换成字符串，并指定分隔符。 |
| 32   | **def nonEmpty: Boolean**检查容器中是否包含元素（相当于 hasNext）。 |
| 33   | **def padTo(len: Int, elem: A): Iterator[A]**首先返回迭代器所有元素，追加拷贝 elem 直到长度达到 len。 |
| 34   | **def patch(from: Int, patchElems: Iterator[B], replaced: Int): Iterator[B]**返回一个新迭代器，其中自第 from 个元素开始的 replaced 个元素被迭代器所指元素替换。 |
| 35   | **def product: A**返回迭代器所指数值型元素的积。             |
| 36   | **def sameElements(that: Iterator[_]): Boolean**判断迭代器和指定的迭代器参数是否依次返回相同元素 |
| 37   | **def seq: Iterator[A]**返回集合的系列视图                   |
| 38   | **def size: Int**返回迭代器的元素数量                        |
| 39   | **def slice(from: Int, until: Int): Iterator[A]**返回一个新的迭代器，指向迭代器所指向的序列中从开始于第 from 个元素、结束于第 until 个元素的片段。 |
| 40   | **def sum: A**返回迭代器所指数值型元素的和                   |
| 41   | **def take(n: Int): Iterator[A]**返回前 n 个元素的新迭代器。 |
| 42   | **def toArray: Array[A]**将迭代器指向的所有元素归入数组并返回。 |
| 43   | **def toBuffer: Buffer[B]**将迭代器指向的所有元素拷贝至缓冲区 Buffer。 |
| 44   | **def toIterable: Iterable[A]**Returns an Iterable containing all elements of this traversable or iterator. This will not terminate for infinite iterators. |
| 45   | **def toIterator: Iterator[A]**把迭代器的所有元素归入一个Iterator容器并返回。 |
| 46   | **def toList: List[A]**把迭代器的所有元素归入列表并返回      |
| 47   | **def toMap[T, U]: Map[T, U]**将迭代器的所有键值对归入一个Map并返回。 |
| 48   | **def toSeq: Seq[A]**将代器的所有元素归入一个Seq容器并返回。 |
| 49   | **def toString(): String**将迭代器转换为字符串               |
| 50   | **def zip[B](that: Iterator[B]): Iterator[(A, B)**返回一个新迭代器，指向分别由迭代器和指定的迭代器 that 元素一一对应而成的二元组序列 |







##### 集合的常用方法

map, flatten, flatMap, filter, sorted, sortBy, sortWith, grouped,

fold(折叠), foldLeft, foldRight, reduce, reduceLeft, aggregate, union, intersect(交集), diff(差集), head, tail, zip, mkString, foreach, length, slice, sum

map: 对集合中的每个元素进行某种映射操作.

flatten: 扁平化操作

flatMap: 对集合中的每个元素进行某种映射操作并进行压平.

filter: 过滤出符合条件的元素集合

sorted : 集合按照升序方式排序

sortBy: 集合按照某种方式或方法排序

sortWith: 将集合按照某种规则排序  传入参数(函数)返回值类型是布尔类型

grouped: 将集合按照个数进行分组

fold : 折叠操作

foldLeft:从左到右折叠: (x,y)  x代表左边,y代表右边 ,那么从左到右折叠意思就是x作为主要的与y进行操作

foldRight:从右到左折叠: (x,y)  x代表左边,y代表右边 ,那么从右到左折叠意思就是y作为主要的与x进行操作

reduce: 聚合操作

reduceLeft:左聚合

reduceRight: 右聚合

aggregate:对集合进行某种聚合操作

union: 连合多个集合

intersect: 求出两个集合的交集

diff: 求出两个集合的差集

head: 求出集合中的头元素

tail:返回列表中的尾部元素

zip: 拉链操作

mkString: 对集合进行格式化输出

foreach: 遍历集合中的每个元素

length:求出集合的长度

slice(start,end):截取集合中的元素

sum:对集合进行求和操作



### 面向对象



#### 	Scala单例对象

scala所以的object对象都是单例对象，无法使用new，且object中定义的成员变量与方法都是静态的，可以通过对象类名或者是对象类名.方法的形式调用



1:	单例对象采用 **object**关键字声明

2:	单例对象对应的类称之为伴生类，伴生对象的名称必须与伴生类名完全一致，且需要放在同一文件中

3:	单例对象中的属性与方法可以通过伴生对象名（类名）直接调用访问



```scala
object demo10_Object {
  def main(args: Array[String]): Unit = {
//    val student = new Student10("alice",18)
//    通过伴生对象创建伴生类的事例化对象
    val student = Student10.newStudent("alice",18)
        student.printInfo()

    val student1 = Student10.apply("bob", 18)
        student1.printInfo()

//  此处实际调用的是伴生对象中的apply方法创建私有类的实例对象，与上一方法等效
//  看起来像是使用主构造器的形式创建对象，实际上使用的是伴生对象的apply方法，常见用法
    val student2 = Student10("bob", 18)
        student2.printInfo()
  }
}



// 伴生对象
object Student10{
  val school = "xiaosenlin"

//  定义一个类的对象事例的创建方法，伴生对象可以调用伴生类中的私有方法/属性
  def newStudent(name: String,age: Int):Student10 = {
    new Student10(name,age)
  }
// 该方法在使用可以直接使用伴生对象名称后加对应的参数，不需要强调apply方法的名称，此为Scala的特殊函数
  def apply(name: String,age: Int):Student10 = new Student10(name,age)

}


// 定义类，调用伴生对象中定义的属性值

//class Student10(val name: String, val age: Int){
//构造方法私有化，无法直接通过主构造器创建该类对象
class Student10 private (val name: String, val age: Int){
  def printInfo(): Unit = {
    println(s"Student: name = $name,age = $age,school = ${Student10.school}")
  }
}

```









#### Scala包

**基本语法**： package 包名

**三大作用**： 区分相同名字的类，控制范围范围，更好的管理类



#### **Scala包对象**

在Scala中可以为每个包定义一个同名的包对象，定义在包对象中的成员，作为其对应包下所有class和object的共享变量，可以被直接访问



需要在与该包名相同包的统一层级下定义

```scala
package object 包名{
  val commonValue = "buzhi" 
  
  def commonMethod(): Unit = {
    println(s"${buzhi}在学习")
  }
  
}
```



**导包说明**



1:	在顶部使用**import**导入，该文件中的所有类都可使用

2:	局部导入，什么时候使用就什么时候导入，在其作用范围内都可以使用

3:	 _ 通配符导入，import java.util._

4:	使用 => 起别名，import java.util.{ ArrayList => JL }

5:	导入相同包的多个类，import java.util.{ ArrayList ，HashSet }

5:	屏蔽类，import java.util.{ ArrayList => _ . _}



默认导入： java.lang._	scala._	scala.predef._





#### 	Scala类

##### 		定义：

​		Scala没有public（默认为public），一个Scala中可以写多各类

​		Scala中的类使用class关键字修饰

​		定义类默认有一个空参构造方法

​		—	代表任意字符



​		类可以看作为一个模版，对象可以用于表示具体事物

##### 		特点：

​		类的成员变量在没有被val或var修饰的情况下不能被外部所访问，相当于没有提供get/set方法

​		类的成员变量使用var修饰时，相当于提供了get/set方法

​		类的成员变量使用val修饰时，相当于提供了get方法，但不提供set方法





```scala

/***
 * Scala中的public属性的底层实际为private，并通过get，set方法对其进行操作
 * 所以不建议在Scala中用private对属性进行修饰，再为其定义get，set方法
 * 但为了兼容Java的某些框架，可以使用@BeanProperty注解实现为属性设置getXXX，setXXX方法
 */

object demo07_Object {

  def main(args: Array[String]): Unit = {
      val student = new Student()
//    val name = student.name   // error ，不能访问被private所修饰的属性
      println(student.sex)
      student.sex = "male"
      println(student.sex)
  }

}


// 定义一个类
class  Student{

  //定义属性
  // 此处的下划线表示该变量的初值为空，需对应 var 类型的变量

  private  var name: String = _    //  对应null
  var age: Int = _        // 对应0
  // @BeanProperty 用于创建符合Java权限规范的属性/方法，亦省略get/set方法
  @BeanProperty
  var sex: String = _

  // scala中默认是公有，但也可以设为私有
  // private var name : String = "buzhi"
}

```







##### 构造器

​		分为主构造器和辅助构造器

**主构造器**：类中有多少个参数，创建类时就使用多少个参数创建

**辅助构造器**：类似于java中的构造方法，在scala中可以有多个辅助构造器（即各辅助构造器的参数列表不同）



**构造器参数**

1) Scala 类的主构造器的形参未用任何修饰符修饰，那么这个参数是局部变量，纯粹的参数。

2) 如果参数使用 **val** **关键字声明**，那么 Scala 会将参数作为类的私有的只读属性使用 

3) 如果参数使用 **var** **关键字声明**，那么那么 Scala 会将参数作为类的成员属性使用,并会提供属性

对应的 xxx()[类似 getter]/xxx_$eq()[类似 setter]方法，即这时的成员属性是私有的，但是可读写。





- Scala 构造器作用是完成对新对象的初始化，**构造器没有返回值**。

  

- 主构造器的声明直接放置于类名之后 ，没有参数时可以省略小括号

  

- **主构造器会执行类定义中的所有语句**，这里可以体会到 Scala 的函数式编程和面向对象编程融合在一起，即：构造器也是方法（函数），传递参数和使用方法和前面的函数部分内容没有区别

  

- 如果主构造器无参数，小括号可省略，构建对象时调用的构造方法的小括号也可以省略

  

- 辅助构造器名称为 this（这个和 Java 是不一样的），多个辅助构造器通过不同参数列表进行区分， 在底层就是构造器重载

  

- 如果想让主构造器变成私有的，可以在()之前加上 private，这样用户只能通过辅助构造器来构造对象了     class Person private() {}

  

- 辅助构造器的声明不能和主构造器的声明一致,会发生错误(即构造器名重复)

  

-  辅助构造都器需要直接或间接调用主构造器（否则报错），在Scala中类中与类名相同的方法就是普通的方法，而不像Java是构造器





```Scala
class 类名(形参列表) { // 主构造器
	 // 类体
	  def this(形参列表) 
    { // 辅助构造器}
  	def this(形参列表)
    { //辅助构造器可以有多个...}
} 
//1. 辅助构造器 函数的名称 this, 可以有多个，编译器通过不同参数来区分.
        
        
class AA{
 //主构造器无参数，可省略小括号   
}
//构建对象时调用的构造方法的小括号也可以省略
 val a = new AA
 val b = new AA()
        
        
        
object ConDemo03 {
 def main(args: Array[String]): Unit = {
 	val p1 = new Person2()
 		}
	}
//定义了一个 Person类  Person有四个构造器
class Person2() {
    
 var name: String = _
 var age: Int = _
    
 def this(name : String) {
 //辅助构造器无论是直接或间接，最终都一定要调用主构造器，执行主构造器的逻辑
 //而且需要放在辅助构造器的第一行[这点和 java 一样，java 中一个构造器要调用同类的其它构造器，也需要放在第一行]
	 this() 	//直接调用主构造器
	 this.name = name
 }
    
 //辅助构造器
 def this(name : String, age : Int) {
	 this() 		//直接调用主构造器
	 this.name = name
	 this.age = age
 }
    
 def this(age : Int) {
	 this("匿名") 	//调用辅助构造器构造器,间接调用了主构造器，因为 def this(name : String) 中调用了主构造器
	 this.age = age
 }
    
 def showInfo(): Unit = {
 	println("person 信息如下:")
 	println("name=" + this.name)
 	println("age=" + this.age)
	 }
}
```



主构造器中传参的方式不同代表的作用也不同



```scala
 def this(name : String) {
   	student1.Student_01

    val student_02 = new Student_02
    println(s"stundet2: name = ${student_02.name} ,age = ${student_02.age}")


    val student_03 = new Student_03("bob",18)
    println(s"stundet3: name = ${student_03.name} ,age = ${student_03.age}")

 }


//  主构造器无参时，小括号可以省略不写
  class Student_01() {
// 定义属性
  var name: String = _
  var age: Int = _

  println("1: 主构造方法被调用")


// 声明辅助构造方法
   def this(name: String){
      this()  // 此时主构造器空参，所以空括号就能代表显示调用了主构造器
      println("2: 辅助构造方法一被调用")
      this.name = name
      println(s"name: $name age: $age")
    }


//  声明第二个辅助构方法，此处也可以调用第一个辅助构造方法，且需直接或间接调用主构造器
    def this(name: String,age: Int){
      this(name)    //调用辅助构造方法一，相当于间接调用了主构造器
      println("3: 辅助构造方法二被调用")
      this.age = age
      println(s"name: $name age: $age")
    }

//  在Scala中类中与类名相同的方法就是普通的方法，而不像Java是构造器
    def Student_01():Unit ={
      println("一般方法被调用")
    }
  }


class Student_02{   // 主构造器没有内容时可以省略小括号
    // 单独定义属性
    var name: String = _
    var age: Int = _
}

// 上面student02的定义等价于，推荐使用该中方法定义成员变量，给主构造器传参直接定义对应的属性
class Student_03(var name: String, var age: Int)    // 此时是主构造器，定义成员属性

class Student_04(name: String, age: Int)    // 此时只是形参，并不是该类中的属性
```



**注意:给某个属性加入@BeanPropetry** **注解后，会生成** **getXXX** **和** **setXXX** **的方法**

并且对原来底层自动生成类似 xxx(),xxx_$eq()方法，没有冲突，二者可以共存



##### 	访问权限：

​	 **Java权限排序：public ->  protected  -> default ->  private**

​	**override**：用于重写父类方法的关键字



​	Scala中属性和方法的默认访问权限为public，但Scala没有public关键字

​	private为私有权限，只在类的内部以及伴生对象中可用

​	protected 只能在当前类及子类，伴生对象中可以访问，同包无法访问



​	类的前面加上private [ this ]	标识该类在当前包下是可见的，当前包外不可见

​	类的前面加上private [ 包名 ]	标识该类在当前包及其子包下可见

​	private加在构造器前标识该类的构造器是私有的，外部类或外部对象都无法调用，也无法通过主构造器直接创建该对象



```scala
package chapter01

import scala.beans.BeanProperty

/** *
 * Scala中的public属性的底层实际为private，并通过get，set方法对其进行操作
 * 所以不建议在Scala中用private对属性进行修饰，再为其定义get，set方法
 * 但为了兼容Java的某些框架，可以使用@BeanProperty注解实现为属性设置getXXX，setXXX方法
 * scala中默认权限是公有，但也可以设为私有
 * Java权限排序：public protected default private
 * override关键字用于重写方法
 * Scala中类的辅助构造方法都需要直接或间接调用主构造器
 ** */

object demo07_Object {

  def main(args: Array[String]): Unit = {
    val student = new Student()
    //    val name = student.name   // error ，不能访问被private所修饰的属性
    println(student.sex)
    student.sex = "male"
    println(student.sex)

    val person = new Person()
    //    person.idCard 私有属性无法访问

    val student1 = new Student_01
    val student2 = new Student_01("buzhi")
    val student3 = new Student_01("yelinsheng",23)

  }

}


// 定义一个类
 class Student {
  //定义属性
  // 此处的下划线表示该变量的初值为空，需对应 var 类型的变量
  // private var name : String = "buzhi"

  private var name: String = _ 		//  对应null
  var age: Int = _ 								// 对应0

  // @BeanProperty 用于创建符合Java权限规范的属性/方法，亦省略get/set方法
  @BeanProperty
  var sex: String = _
   
}


// 定义一个父类
  class Person {
    private var idCard: String = "12345"
    protected var name: String = "alice"
    var sex: String = "female"

    //类的前面加上private [ 包名 ]	标识该类在当前包及其子包下可见
    private[chapter01] var age: Int = 18

    def printInfo(): Unit = {
      println((s"Person: $idCard $name $sex $age"))
    }
}


//  定义一个子类 继承关键字extends
  class Worker extends Person {
    println("Worker: ")
    //    println(idCard) 无法访问 私有的属性只能在该类本身或半生对象中访问
    name = "bob"
    age = 23
    sex = "male"

    //  重写printInfo方法
    override def printInfo(): Unit = {
      println((s"Worker: $name $sex $age"))
    }
  }

```





#### 属性/成员变量

1) 属性的定义语法同变量，示例：[访问修饰符] var 属性名称 [：类型] = 属性值

2) 属性的定义类型可以为任意类型，包含值类型或引用类型[案例演示]

3) Scala 中声明一个属性,必须显示的初始化，然后根据初始化数据的类型自动推断，属性类型可以省略(这点和 Java 不同)。

4) 如果赋值为 null,则一定要加类型，因为不加类型, 那么该属性的类型就是 Null 类型.

5) 如果在定义属性时，暂时不赋值，也可以使用符号_(下划线)，让系统分配默认值.



_ 对应的值：

Byte Short Int Long ：	0

Float Double：	0.0

String 和 引用类型：	null

Boolean：	false



#### 创建对象

案例

```Scala
class Person {  // 主构造器为无参构造器
 var age: Short = 90
 var name: String = _
 
 def this(n: String, a: Int) {
 	this()		// 直接调用主构造器
 	this.name = n
 	this.age = a
 	}
 }

var p : Person = new Person("不知",20)
```

对应流程分析

1) 加载类的信息(属性信息，方法信息)

2) 在内存中(堆)开辟空间

3) 使用父类的构造器(主和辅助)进行初始

4) 使用主构造器对属性进行初始化 [ age:90, naem null ]

5) 使用辅助构造器对属性进行初始化 [ age:20, naem 不知 ]

6) 将开辟的对象的地址赋给 p 这个引用





#### 伴生类

当单例对象与某个类共享同一名称时，该单例对象称为类的伴生对象，类被称为该单例对象的伴生类，类与其伴生对象可以互相访问彼此的私有成员，但必须在同一个源文件中定义类与其伴生对象。



#### 特质（Trait）

scala的trait相当于java的接口，但trait可以定义属性与方法的实现

一般情况下，scala的类只能继承单一父类，但是如果是trait则可以继承多个，实现多继承（with），一个类可以混入（mixin）多个特质



##### 基本语法

 trait  特质名{

​		trait  主体

​	}



**没有父类**： class	类名	extends	特质1	with	特质2	witch	..

**含有父类**：	 class	类名	extends	父类	witch	特质1	witch	特质2



**如果父类与特质有相同的方法或属性，则继承的子类必须重写该方法或属性，否则会报冲突错误**

重写方法时调用的supper调用的其实是继承顺序中最后/最右一个特质中的方法，也可以在supper后面加中括号写上对应的特质名   supper[ xxx ].xxx()



##### 		举个例子

```scala
trait ScalatTrait{
    def hello (name:String) //没有具体实现的方法
    
    def smile(name:String): Unit = {
        print(s"不知$name 是谁")
    }
}

trait Fly{
    def fly(name:String) : Unit = {
        print(s"$name 会飞")
    }
}

object ScalaTraitImpl extends ScalaTrait with Fly {
    //如果方法未在特质中有具体实现，则再使用该方法时override可写可不写
    //如果方法在特质中有具体实现，且需重写该方法，则必须写override
    
    override def hello(name:String) : Unit = {
        print(s"hello $name")
    }
    
    
    override def smile(name:String) : Unit = {
        print(s"$name 在哪儿")
    }
    
    
    
   def main (arrgs: Array[String]) : Unit = {
       
       ScalaTraitImpl.hello("不知")
       ScalaTraitImpl.smile("困困")
       ScalaTraitImpl.fly("壮壮")
     
 //  可以在创建特定某一对象时单独实现特质，灵活使用，重写特种中的抽象方法，但不改变该类原本的状态
    val studentWithTalent = new Student12 with Young{
      override def play(): Unit = println("")
      override def dating(): Unit = println("")
    }
       
   }
    
}

```





#### 	抽象类

​	scala中，**abstract **所修饰的类被称为抽象类，在抽象类中可定义属性，未实现的方法和具体实现的方法

​	一个类只要存在抽象属性或抽象方法，则该类必须定义成抽象类，但抽象类中可以定义具体的方法/方法，甚至可以没有抽象类与抽象方法



##### 基本语法

​	1: 定义抽象类       abstract class xxx{}		// 通过abstract关键字标记抽象类

​	2: 定义抽象属性   val / var name: String 	// 一个属性没有初始化，也没有使用下划线赋予默认值，就是抽象属性

​	3: 定义抽象方法	def hello(): Unit			 // 只声明但没有具体实现的方法，就是抽象方法

​		

##### 继承与重写

​	1: 如果父类为抽象类，则子类需要将抽象的属性与方法实现，否则子类也需要声明为抽象类

​	

​	2: 重写非抽象方法需要使用 **override** 关键字修饰，但重写抽象方法则可以不加 override

​	

​	3: 子类中调用父类的方法使用 **super** 关键字

​	

​	4: 子类对抽象属性重写，父类抽象属性可以用var 修饰

​		子类对非抽象属性重写，父类**非抽象属性**只支持 val 类型，而不支持 var

​		**因为 var 修饰的为可变变量，子类继承之后可以直接修改使用，没有必要重写**





##### 匿名子类

​	在使用时直接实现抽象类的抽象方法/抽象属性，并不想单独定义实现类

```scala
// 定义抽象类
abstract class Person{
  va name: String
  def eat(): Unit
}


object xxx {
  def main(args: Array[String]): Unit = {
    
    // 创建抽象类的匿名子类
			val person: Person = new Person{
        override var name: String = "alice"
        override def eat(): Unit = println("person eat")
      }
    
    	prinln(person.name)
    	person.eat
    
  }
}
```



#### 	继承

​	继承是面向对象的概念，用于代码的可重用性，被拓展的类称为超类或父类，扩展的类称为派生类或子类。

scala可以使用extends关键字实现继续其他类或者是特质



​	子类继承了所有的属性，只是私有的属性不能直接访问，需要通过公共的方法去访问

​	

​	Scala中继承类的方式和Java一样，也是使用**extends**关键字，和Java一样，可在定义中给出子类需要而父类没有的字段和方法，或者重写父类的方法（**override**关键字）。



​	类有一个主构造器和任意数量的辅助构造器，而每个辅助构造器都必须以对先前定义的辅助构造器或主构造器的调用开始。



​	Scala在继承的时候构造器的执行顺序：首先执行父类的主构造器，其次执行子类自身的主构造器/辅助构造器。



​	子类的辅助构造器最终都会调用主构造器，而只有主构造器可以调用父类的构造器（可以是父类的主构造器，也可以是父类的辅助构造器，因为辅助构造器也需要间接或直接调用主构造器）



```scala
package chapter01

object demo08_Inherit {
  def main(args: Array[String]): Unit = {
    val student1 = new Student_05("alice",18)
    val student2 = new Student_05("bob",22,"a1111")
  }
}


// 定义一个父类
class Person_01{   // 主构造器为无需参构造器
  var name: String = _
  var age: Int = _

  println("1 父类的主构造器调用")

  // 辅助构造器
  def this(name: String,age: Int) = {
    // 显示调用父类构造器
    this()
    println("2 父类的辅助构造器调用")
    // 为属性成员赋值
    this.name = name
    this.age = age
  }

  def printInfo(): Unit = {
    println(s"Person: $name $age")
  }
}

/***
Scala在继承的时候构造器的执行顺序：首先执行父类的主构造器，其次执行子类自身的主构造器/辅助构造器
***/

// 定义一个子类
// class Student(name: String) extends Person (name) {
// class Student(name: String, age: Int) extends Person (name,age) {  调用父类的辅助构造器，也会间接调用父类的主构造器
class Student_05(name: String, age: Int) extends Person_01 {    // 此处需调用父类的主构造器，但当前没有，所以没有
  var stdNo: String = _
  println("3 子类的主构造器被调用")

  def this(name: String, age: Int, stdNo: String) {
      this(name,age)
      println("4 子类的辅助构造器被调用")
      this.stdNo = stdNo
  }

  // 重写父类中已存在的方法
  override def printInfo(): Unit = {
    println(s"Student_05: $name $age $stdNo")
  }
}


// 对应输出结果
//    1 父类的主构造器调用
//    3 子类的主构造器被调用
//    1 父类的主构造器调用
//    3 子类的主构造器被调用
//    4 子类的辅助构造器被调用
```



#### 多态

​		

​    在Java中，方法调用可以动态绑定，但属性是静态的，在初始化时就已经静态绑定

​	即使子类继承父类之后修改了继承的属性值（编译看左边（静态属性），执行看右边）		

```Java
			Person person = new Worker();					// Worker 继承 Person 并重写了父类的hello方法与name属性

			System.out.println ( person.name );		// 此处调用的仍然是父类的属性值

			person.hello( );			// 此处调用的是重写的子类方法
			
```

​		

​	在Scala中可以在属性前加 **override** 关键字明确 重写覆盖父类的属性

​	Scala的属性与方法都可以被重写，都属于动态绑定，而Java的方法属于动态绑定，属性属于静态绑定，重写之后也还是父类的值





​	



#### 	关键字



##### 		final关键字：

- ​	所修饰的类不能被继承	
- ​	所修饰的属性不能重写
- ​	所修饰的方法不能重写



##### 		type关键字

​	scala中的类型除了定义class，trait，object时会产生类型，还可以通过type关键字声明类型

​	type相当于声明一个类型别名

​	通常type用于声明某种复杂类型，或用于定义一个抽象类型



##### 		样例类/样例对象

​		样例类,使用 case 关键字 修饰的类, 其重要的特征就是支持模式匹配, 样例类默认是实现了序列化接口的.

​		样例对象(object), 其重要特征就是支持模式匹配







#### 枚举类与应用类



**枚举类**：需要继承 Enumeration （/ ɪˌnuːməˈreɪʃn / 枚举）

**应用类**：需要继承 app

```scala
// 定义枚举类对象
// Value类型需放置两个参数 Int：序号，String：对应值
object WorkDay extends Enumeration{

  val MONDAY = Value(1,"星期一")
  val TUEDAY = Value(2,"星期二")

}


// 定义类对象
object TestApp extends App{
  // 自带main方法，可直接运行
  println("app start")

}


// 测试main

main{
  
      // 测试枚举类
    println(WorkDay.MONDAY)
  
}
```



#### 扩展



**编译看左，执行看右**

​		对于多态中的静态变量、静态方法、成员变量来讲，编译看左，运行也看左。无论右边是当前类还是当前类的子类，编译和运行期间执行的都是当前类中的方法。
  对于多态中的非静态方法，编译看左，运行看右。也就是在编译期间看左边的类中有无该方法/属性，而实际在运行时执行的是右边类的方法/属性。如果编译期间没有在左边的类找到该非静态域，则会报编译错误。



##### 类型的检查与转换

obj.isInstanceOf [ T ] :	判断 obj 是不是 T 类型

obj.asInstanceOf [ T ] :	将 obj 强转成 T 类型

classOf 获取对象的类名



### 异常处理



Scala 提供 try 和 catch 块来处理异常。try 块用于包含可能出错的代码。catch 块用于处理 try 块中发生的异常。可以根据需要在程序中有任意数量的 try...catch 块



#### Java 异常处理回顾



1) java 语言按照 try—catch-catch...—finally 的方式来处理异常

2) 不管**有没有异常捕获，都会执行** **finally**, 因此通常可以在 finally 代码块**中释放资源**

3) 可以有多个 catch，分别捕获对应的异常，这时需要把范围小的异常类写在前面，把范围大的异

常类写在后面，否则编译错误。会提示 "Exception 'java.lang.xxxxxx' has already been caught"【案例演示】



```Java
package com.atguigu.chapter05.myexception;
public class JavaExceptionDemo01 {
 	public static void main(String[] args) {
		 try {
				 // 可疑代码
		 int i = 0;
		 int b = 10;
		 int c = b / i; // 执行代码时，会抛出 ArithmeticException 异常
		 } catch (ArithmeticException ex) {
		 	ex.printStackTrace();
		 } catch (Exception e) { //java 中不可以把返回大的异常写在前，否则报错!!
 			e.printStackTrace();
 	} finally {
 			// 最终要执行的代码
 		System.out.println("java finally");
 		}
	 System.out.println("ok~~~继续执行...");
	 }
}
```



#### Scala 异常处理举例

- 1) 我们将**可疑代码封装在** **try** **块中**。 在 try 块之后使用了一个 catch 处理程序来捕获异常。如果发生任何异常，catch 处理程序将处理它，**程序将不会异常终止**。

  

- 2) Scala 的异常的工作机制和 Java 一样，但是 **Scala** **没有“checked(编译期)”异常**，即 Scala 没有编译异常这个概念，异常都是在运行的时候捕获处理。

  

- 3) 用 **throw** **关键字，抛出一个异常对象**。所有异常都是 Throwable 的子类型。throw 表达式是有类型的，就是 Nothing，因为 Nothing 是所有类型的子类型，所以 throw 表达式可以用在需要类型的地方

  

- 4) 在 Scala 里，借用了模式匹配的思想来做异常的匹配，因此，在 catch 的代码里，是一系列 case子句来匹配异常。当匹配上后 => 有多条语句可以换行写，类似 java 的 switch case x: 代码块..

  

- 5) 异常捕捉的机制与其他语言中一样，如果有异常发生，catch 子句是按次序捕捉的。因此，在 catch子句中，**越具体的异常越要靠前，越普遍的异常越靠后**，如果把越普遍的异常写在前，把具体的异常写在后，**在** **scala** **中也不会报错，但这样是非常不好的编程风格**。

  

- 6) finally 子句用于执行不管是正常处理还是有异常发生时都需要执行的步骤，一般用于对象的清理工作，这点和 Java 一样。

  

- 7) Scala 提供了 throws 关键字来声明异常。可以使用方法定义声明异常。 它向调用者函数提供了此方法可能引发此异常的信息。 它有助于调用函数处理并将该代码包含在 try-catch 块中，以避免程序异常终止。在 scala 中，可以使用 throws 注释来声明异常

  

```Scala
try {
    val r = 10/0
}catch{
    case ex:ArithmeticException{
        println("捕获了除数为零的算数异常")
    }
    case ex:Exception => println("捕获了异常")
}finally{
    //最终执行的代码，常用于释放资源
}

println("搞定，继续执行~")
```

