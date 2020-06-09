# Lecture 9 Big Data Analytics
## Examples of Data Analytics:
* Full-text searching <font color=pink>(similar to the Google search engine)</font>
* Aggregation of data <font color=pink>(summing up the number of hits by web-page, day, month, etc)</font>
* Clustering <font color=pink>(grouping customer into classes based on their spending habits)</font>
* Sentiment analysis <font color=pink>(deciding whether a tweet oon a given topic express a positive or negative sentiment)</font>
* Recommendations <font color=pink> (suggesting additional products to a client during checkout based on the choices made by other clients who bought a similar product)</font>
## Challenges of Big Data Analytics
### A framework for analyzing big data has to distribute both data and processing over many nodes, which implies:
* Reading and writing distributed datasets
* Preserving data in the presence of failing data nodes
* Supporting the execution of MapReduce tasks
* Being fault-tolerant
* Coordinating the execution of tasks across a cluster
## Tools for Analytics
* There are many tools that can perform data analytics:
    * Statistical packages <font color=pink>(R, Stata, SAS, SPSS, etc)</font>
    * Business Intelligence tools <font color=pink>(Tableau, Business Object, Pentaho, etc)</font>
    * Information retrieval tools <font color=pink> ElasticSearch, IBM, TextExtender, etc)</font>
* When it comes to big data, the majority of applications are built on top of an open-source framework: *<font color=red>Apache Hadoop</font>*
# *Apache Hadoop*
## The Hadoop Ecosystem
Apache Hadoop started as a way to distribute files over a cluster and execute MapReduce tasks, but many tools have now been built on foundation to add further functionality.
* DBMSs <font color=pink> (Hive, HBase, Accumulo)</font>
* Business Intelligence tools <font color=pink> (Apache Pig)</font>
* Distributed configuration tools <font color=pink> (Apache Zookeeper) </font>
* Distributed resource management tools <font color=pink> (YARN, Mesos) </font>
* Machine learning libraries <font color=pink> (Mahout)</font>
* Object storage <font color='pink'> (Ozone)</font>
## <font color=red>Hadoop Distributed File System</font>
The core of Hadoop is fault tolerant file system that has been explicitly designed to span many nodes.
*HDFS* blocks are much larger than blocks used by an ordinary file system, the reasons for this unusual size are:
* Reduced need for memory to store information about where the blocks are.<font color=pink> (metadata)</font>
* More efficient use of the network. With a large block, a reduced number network connection needs to be kept open.
* Reduced need for seek operation on big files.
* Efficient when most data of a block have to be processed.
## HDFS Architecture
A HDFS file is a collection of blocks stored in <font color=red>datanodes </font>, with metadata, such as the position of those blocks, that is stored in <font color=red>namenodes</font>
> Example of HDFS Architecture
>><img src='HDFS_Architecture.png' width='50%'>
## <font color=red>The Hadoop Resource Manager (YARN)</font>
* The other main component of Hadoop is the MapReduce task manager, <font color=red>YARN</font><font color=pink> (Yet Another Resource Negotiator)</font>
* YARN deals with executing MapReduce jobs on a cluster. It is composed of a central *Resource Manager* <font color=pink>(on the master) </font>and many *Node Managers* that reside on slave machines
* Every time a MapReduce job is scheduled for execution on a Hadoop cluster, YARN starts an *Application Master* that negotiates resources with Resource Manager and starts *Containers* on the slave nodes. <font color=pink>(Note: Containers are the processes were the actual processing is done, not to be confused with Docker containers)</font>
## HDFS Shell
* Managing the files on a HDFS cluster cannot be done on the operating system shell, hence a custom HDFS shell must be used.
* The HDFS file system shell replicates many of the usual commands <font color=pink>(ls, rm, etc)</font>, with some other commands dedicated to loading files from the operating system to the cluster:
```
$HADOOP_HOME/bin/hadoop fs -copyFromLocal <localsrc> <dst>
```
* In addition, the status of the HDFS cluster and its contents can be seen on a web application <font color=pink>(Normally found on port 50070 of the master node)</font>
## Programming on Hadoop
* The main programming language to write MapReduce jobs on Hadoop is Java, but many other languages can be used via different APIs.
* Indeed any language that can read from standard input and write to standard output can be used.
* Practically, the `hadoop` command is used to load the program, with the `-file` option, and send it to the cluster, and the mapper and reducer are specified with the `-mapper` and `-reducer` options. <font color=pink>(`aggregate` uses the Hadoop internal aggregator library)</font>:
```
$HADOOP_HOME/bin/hadoop jar \
 $HADOOP_HOME/hadoop-streaming.jar \
    -D mapreduce.job.reduces=12 \
    -input myInputDir \
    -output myOutputDir \
    -mapper myAggregateorForKeyCount.py \
    -reducer aggregate \
    -file myAggregatorForKeyCount.py
```
# *Apache Spark*
## <font color=red>Spark</font>
* While Hadoop MapReduce works well, it is geared towards performing relatively simple jobs on large datasets.
* However, when complex jobs are performed, there is a strong incentive for caching data in memory and in having finer-grained control on the execution of jobs.
* Apache Spark was designed to reduce the latency inherent in the Hadoop approach for the execution of MapReduce jobs.
* Spark can operate within the Hadoop architecture, using YARN and Zookeeper to manage computing resources, and storing data on HDFS.
## Spark Architecture
* One of the strong points of Spark is the tightly-coupled nature of its main components:\
<img src='Spark_Architecture.png' width='50%'>
* Spark ships with a cluster manager of its own, but it can work with other managers, such as YARN or MESOS.
## Programming on Spark
* Spark is mostly written in Scala, and uses this language by default in its interactive shell. However, the APIs of Spark can be accessed by different languages: R, Python, and Java.
* Scala is a multi-paradigm language <font color=pink> (both functional and object-oriented)</font> that runs on the Java Virtual Machine and can use Java libraries and Java objects
* The most popular languages used to develop Spark application are Java and Python
* Other than Scala, there is a Python Shell
## Getting Data In and Out of Spark
* Spark can read and write data in many formats, from text file to database tables, and it can used different file systems and DBMSs
* The simplest way to get data into Spark is reading from a CSV file from the local file system:
```
csv = sc.textFile("file.csv")
```
* With a small change, Spark can be made to read from HDFS file systems or Amazon S3:
```
csv = sc.textFile("hdfs://file.csv")
csv = sc.textFile("s3://myBucket/myFile.csv")
```
* The text lines in the file would have to be parsed and put into objects before they can be sued by Spark
* Another popular format is JSON, which can be parsed and streamed back into a file using Java libraries such as Jackson or Gson
* An efficient data format that is unique to Hadoop is the *sequence* file. This is a flat file composed of key/value pairs.
* Another option is to load/save data is the use of serialized Java objects. <font color=pink>(The Kryo library, rather than the native Java serialization is commonly used)</font>
* While this option is simple to implement, it is neither fast nor robust. 
* HDFS or distributed DBMSs can be used in conjunction with Spark
* SQL queries can also be used to extract data:
```
df = sqlContext.sql("SELECT * FROM table")
```
* Relational DBMSs can be a source of data. <font color=pink>via JDBC</font>
* CouchDB, MongoDB, and Elastic Search connectors are also available.
## The Spark Shell
* The Spark Shell allows to send commands to the cluster interactively in either Scala or Python
* A simple program in Python to count the occurrences of the word "Spark" in the README of the framework: 
```python
./bin/pyspark
>>> textFile = sc.textFile("README.md")
>>> textFile.filter(lambda line: "Spark" in line).count()
15
```
* While the shell can be extremely useful, it prevents Spark from deploying all of its optimizations, leading to poor performances.
## Non-interactive Jobs in Spark
The usual word count, but in Java 7
``` java
JavaRDD<String> input = sc.textFile(inputFile);
JavaRDD<String> words = input.flatMap(
    new FlatMapFunction<String, String>(){
        public Iterable<String> call(String x) {
            return Arrays.asList(x.split(" "));
        }
    }
);
JavaPairRDD<String, Integer> counts = 
words.mapToPair(new PairFunction<String, String, Integer>(){
    public Tuple2<String, Integer> call(String, x){
        return new Tuple2(x, 1);
    }
}).reduceByKey(new Function2<Integer, Integer, Integer>(){
    public Integer call(Integer x, Integer y) {return x + y;}
});
counts.saveAsTextFile(outputFile);
```
## Spark Runtime Architecture
* Applications in Spark are composed of different components including"
    * <font color=red>Job</font>: the data processing that has to be performed on a dataset
    * <font color=red>Task</font>: a single operation on a dataset
    * <font color=red>Executors</font>: the processes in which tasks are executed
    * <font color=red>Cluster Manager</font>: the process assigning tasks to executors
    * <font color=red>Driver program</font>: the main logic of the application
    * <font color=red>Spark application</font>: Driver program + Executors
    * <font color=red>Spark Context</font>: the general configuration of the job
* These different components can be arranged in three different deployment modes across the cluster
### Spark Runtime Architecture: Local Mode
In <font color=red>local mode</font>, every Spark component runs within the same JVM. However, the Spark Application can still run in parallel as there may be more than one executor active. <font color=pink>Local mode is good when developing/debugging</font>
>Local mode Spark Runtime Architecture:\
<img src='Local_mode.png' width='50%'>
### Spark Runtime Architecture: Cluster Mode
In <font color=red>cluster mode</font>, every component, including the driver program, is executed on the cluster; hence, upon launching, the job can run autonomously. <font color=pink>This is the common way of running non-interactive Spark jobs.</font>
>Cluster mode Spark Runtime Architecture:\
<img src='Cluster_mode.png' width='50%'>
### Spark Runtime Architecture: Client Mode
In <font color=red>client mode</font>, the driver program talks directly to the executors on the worker nodes. Therefore, the machines hosting the driver program has to be connected to the cluster until job completion. Client mode must be used when the applications are interactive, as happens in the Python or Scala Spark Shells.
>Client mode Spark Runtime Architecture:\
<img src='Client_mode.png' width='50%'>
## Spark Context
* The deployment mode is set in the *<font color=red>Spark Context</font>*, which is also used to set the configuration of a Spark application, including the cluster it connects to in cluster mode.
* E.g. This hard-coded Spark Context directs the execution to run locally, using 2 threads:
``` 
sc = new Spark Context(new SparkConf().setMaster("local[2]"));
```
* E.g. This other hard-coded line directs the execution to a remote cluster:
```
sc = new SparkContext(new SparkConf().setMaster("spark://192.168.1.12:6066"));
```
* Spark Contexts can also be used to tune the execution by setting the memory, or the number of executors to use. 
## How to Submit Java Jobs to Spark
* For an application to be executed on Spark, either a shell or a submit script has to be used. The submit script is to be given all the information it needs:
```
./bin/spark-submit \
  --class <main-class> \
  --master <master-url> \
  --deploy-mode <deploy-mode> \
  --conf <key>=<value> \
  <application-jar (an uber-JAR)> \
  [application-arguments]
```
* The application JAR must be accessible from all the nodes in cluster deploy mode, hence it is usually put on HDFS.
* The submit script can be used to launch Python programs as well. Uber-JARs can be assembled by Maven with Shade plugin.
## <font color=red>Resilient Distributed Dataset</font>
* <font color=red>Resilient Distributed Dataset(RDDs)</font> are the way data are stored in Spark during computation, and understanding them is crucial to writing programs in Spark:
    * <font color=red>R</font>esilient: Data are stored redundantly, hence a failing node would not affect their integrity.
    * <font color=red>D</font>istributed: Data are split into chunks, and these chunks are sent to different nodes.
    * <font color=red>D</font>ataset: A dataset is just a collection of objects, hence very generic.
## Properties of RDDs
* RDDs are <font color=red> immutable</font>, once defined, they cannot be changed. This greatly simplifies parallel computations on them, and is consistent with the functional programming paradigm
* RDDs are <font color=red> transient</font>, they are meant to be used only once, then discarded. But they can cached if it improves performance
* RDDs are <font color=red> lazy-evaluated</font>, the evaluation process happens only when data cannot be kept in an RDD, as when the number of objects in an RDD has to be computed, or an RDD has to be written to a file, but not when an RDD are transformed into another RDD. <font color=pink>(These are called transformations)</font>
## How to build RDD
* RDDs are usually created out of data stored elsewhere, as in:
```java
JavaRDD<String> lines = sc.textFile("data.txt");

DataSet<Row> teenagers = sparkSession.sql(
    "SELECT name FROM table WHERE age >= 12 AND age <= 19");
JavaRDD<Row> rddTeenagers = teenagers.javaRDD();
```
* RDDs can be created out of collections too, using the parallelize function:
```java
List<Integer> distData = Arrays.asList(1, 2, 3, 4, 5);
JavaRDD<Integer> distData = sc.parallelize(data);
```
## Order of Execution of MapReduce Tasks
* While the execution order of Hadoop MapReduce is fixed, the lazy evaluation of Spark allows the developer to stop worrying about it, and have the Spark optimizer take care of it. 
* In addition, the driver program can be divided into steps that easier to understand without sacrificing performance as long as those steps are composed of transformations. 
## Example of Transformation of RDDs
* `rdd.filter(lambda)` selects elements from an RDD
* `rdd.distinct()` returns an RDD without duplicated elements
* `rdd.union(otherRdd)` merges two RDDs
* `rdd.intersection(otherRdd)` returns elements common to both
* `rdd.subtract(otherRdd)` reomves elements of otherRdd
* `rdd.cartesian(otherRdd)` returns the Cartesian product of both RDDs
## Examples of Actions
* `rdd.collect()` returns all elements in an RDD
* `rdd.count()` returns the number of elements in an RDD
* `rdd.reduce(lambda)` applies the function to all elements repeatedly, resulting in one result.
* `rdd.foreach(lambda)` applies lambda to all elements of an RDD
## Examples of Key/Value Pairs Transformations
* `rdd.map(lambda)` creates a key/value pair RDD by applying function lambda and returning one pair for element.
* `rdd.flatMap(lambda)` applies a function to RDD elements and returns zero, one, or more pairs for element.
* `rdd.reduceByKey(lambda)` processes all the pairs with the same key into one pair
* `rdd.join(otherRdd)` merges two key/value pair RDDs based on a key.
## Caching Intermediate Results
* `rdd.persist(storageLevel)` can be used to save an RDD either in memory and/or disk. This `storageLevel` can be tuned to a different mix of use of RAM or disk to store the RDD
* Since RDDs are immutable, the result of the final transformation is cached, not the input RDD. 
    * E.g. `rddB = rddA.persist(DISK_ONLY)` only rddB has been written to disk.
## Tuning the Degree of Parallelism
* Some transformations allow for a second parameter containing the desired of partitions.
>`sc.textFile("bigfile.csv", 10)`
* An RDD can also be re-partitioned explicitly:
>`rdd.repartition(partitionNum)`
* Another way to partition an RDD is to provide an *partitioner*, a strategy to guide the partitioning.
> `rdd.partitionBy(new HashPartitioner(100))`\
> This splits RDD elements into 100 partitions according to their keys.
## Spark Jobs, Tasks, and Stages
* A <font color=red>job</font> is the overall processing that Spark is directed to perform by a driver program.
* A <font color=red>task</font> is a single transformation operating on a single partition of data on a single node.
* A <font color=red>stage</font> is a set of tasks operating on a single partition.
* A job is composed of more than one stage when data are to be transferred across nodes. <font color=red>(shuffling)</font>
* The fewer the number of stages, the faster the computation.<font color=pink>(shuffling data across the cluster is slow)</font>
