## Apache Spark
- 是一种分布式集群计算架构（Distributed Cluster Computing Framework）
- 内存内运算技术，能在数据尚未写入硬盘时即在内存内分析运算，比Hadoop的MapReduce快100倍
- 本身用Scala写的
- 对JAVA, PYTHON, R, SCALA, SQL提供支持
- RDD：Resilient Distributed Datasets，Spark的基本计算单元
- Resillient：Able to withstand failures

## PySpark
- 给python用的集成API
- PySpark Shell：基于python的命令行工具，让数据科学家可以使用Spark数据结构以及连接到一个集群
- 读入数据（创建RDD）

```Python
sc: SparkContext PySpark的默认Entry Point
rdd：弹性分布式数据集，代表一个不可变、可分区、里面的元素可并行计算的集合。

rdd1 = sc.parallelize([1,2,3,4,5])

rdd2 = sc.textFile("test.txt")
```

## Transformation


```Python
lambda: anonymous function
eg. lambda x: x + 2

1. map(function, list):将function作用于list里的所有值
eg. RDD.map(lambda x: x * x)

2. filter(function, list):返回list里function等于True的值
eg. RDD.filter(lambda x: x > 2)

3. flatmap(): 对于每个RDD中的元素，返回多个值。比如可以在Split时使用
eg. RDD_flatmap = RDD.flatMap(lambda x: x.split(" "))

4. UNION():合并两个RDD
eg.
inputRDD = sc.textFile("logs.txt")
errorRDD = inputRDD.filter(lambda x: "error" in x.split())
warningsRDD = inputRDD.filter(lambda x: "warnings" in x.split())
combinedRDD = errorRDD.union(warningsRDD)
```

## Action

```Python
1. collect(): 将dataset里的所有值以array的形式返回

2. take(N): 返回dataset里的前N个值

3. first(): 返回RDD中第一个值

4. count(): 返回RDD中元素的个数

5. reduct(): 累积一个RDD中的所有元素
eg.
x = [1,3,4,6]
RDD = sc.parallelize(x)
RDD.reduce(lambda x, y : x + y)

result：14

6. saveAsTextFile()：将RDD储存成一个directory里的多个file

7. coalesce(): 将RDD储存成一个directory里的一个file
```

## Pair RDD
有Key / Value组合的RDD

创建PairRDD的方法
```Python
# From a list of key-value tuple
my_tuple = [('Sam', 23), ('Mary', 34), ('Peter', 25)]
pairRDD_tuple = sc.parallelize(my_tuple)

# From a regular RDD
my_list = ['Sam 23', 'Mary 34', 'Peter 25']
regularRDD = sc.parallelize(my_list)
pairRDD_RDD = regularRDD.map(lambda s: (s.split(' ')[0], s.split(' ')[1]))
```

## 可以作用于PairRDD的Transformation


```PYTHON
1. reduceByKey(): 将相同key的value组合到一起

eg.
regularRDD = sc.parallelize([("Messi", 23), ("Ronaldo", 34),
("Neymar", 22), ("Messi", 24)])

pairRDD_reducebykey = regularRDD.reduceByKey(lambda x,y : x + y)

pairRDD_reducebykey.collect()
[('Neymar', 22), ('Ronaldo', 34), ('Messi', 47)]
```
```Python
2. sortByKey(): 根据key排序

pairRDD_reducebykey_rev = pairRDD_reducebykey.map(lambda x: (x[1], x[0]))
pairRDD_reducebykey_rev.sortByKey(ascending=False).collect()

[(47, 'Messi'), (34, 'Ronaldo'), (22, 'Neymar')]
```
```Python
3. groupByKey(): 根据key分组

airports = [("US", "JFK"),("UK", "LHR"),("FR", "CDG"),("US", "SFO")]
regularRDD = sc.parallelize(airports)
pairRDD_group = regularRDD.groupByKey().collect()

for cont, air in pairRDD_group:
print(cont, list(air))

FR ['CDG']
US ['JFK', 'SFO']
UK ['LHR']
```
```Python
4. join(): 根据key值合并两个RDD

RDD1 = sc.parallelize([("Messi", 34),("Ronaldo", 32),("Neymar", 24)])
RDD2 = sc.parallelize([("Ronaldo", 80),("Neymar", 120),("Messi", 100)])

RDD1.join(RDD2).collect()

[('Neymar', (24, 120)), ('Ronaldo', (32, 80)), ('Messi', (34, 100))]
```

## 可以作用于PairRDD的Action
```Python
1. countByKey(): 数每个key有几个元素

rdd = sc.parallelize([("a", 1), ("b", 1), ("a", 1)])
for kee, val in rdd.countByKey().items():
print(kee, val)

('a', 2)
('b', 1)
```
```Python
2. collectAsMap(): 把key，value对以字典dictionary的形式返回

sc.parallelize([(1, 2), (3, 4)]).collectAsMap()

{1: 2, 3: 4}
```
