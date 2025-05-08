---
title: 大数据期末复习
categories:
  - java
  
abbrlink: 2701
date: 2024.12.8
tags: 
   - java
   - Hadoop
   - BigData
---

# 基本概念

大数据的四个特点：数据量大，数据类型繁多，处理速度快和价值密度低。 

大数据对科学研究的影响：

第一种范式：实验科学， 第二种范式：理论科学，第三种范式：计算科学，第 四种范式：数据密集型科学。

云计算的关键技术包括虚拟化，分布式存储，分布式计算和多租户等

大数据和云计算物联网之间的关系：

大数据、云计算和物联网（IoT）紧密相关，互为支撑：

1. **物联网与大数据**：物联网设备生成海量数据，需要大数据技术进行存储、处理和分析，提取有价值的信息。
2. **物联网与云计算**：云计算提供弹性计算和存储资源，支持物联网设备的数据存储、实时分析和处理。
3. **大数据与云计算**：云计算为大数据提供基础设施支持，提供存储、计算能力和弹性扩展，使大数据处理更加高效。

# 简答题

## 1. **MapReduce工作流程的描述**

MapReduce工作流程主要分为两个阶段：Map阶段和Reduce阶段。

- **Map阶段**：在Map阶段，输入数据被分为多个片段，每个片段由一个Map任务处理。Map任务将输入数据（键值对）映射为中间结果（也为键值对）。每个Map任务输出的中间结果都会被按键进行分组，分组后的结果会被传递给Reduce阶段。
- **Shuffle阶段**：Map阶段完成后，Shuffle阶段会按照键对Map的输出进行排序和分组，确保同一键的值被发送到同一台Reducer机器上。此过程是MapReduce中的一个重要过程，称为Shuffle。
- **Reduce阶段**：在Reduce阶段，系统会接收到Map输出的分组结果，并对每个键的所有值进行处理，通常是进行聚合操作，如求和、求平均等，最后输出结果。

## 2. **Flink核心组件栈的层次及具体内容**

- **Flink Runtime**：执行任务调度和资源管理。

- **Flink API**：包括流处理的DataStream API和批处理的DataSet API，Table API/SQL支持声明性查询。

- **Connectors**：用于连接外部数据源和数据接收端（如Kafka、HDFS等）。

- **State & Time Layer**：用于处理有状态计算及时间处理（如事件时间和窗口操作）。

- **Fault Tolerance & Checkpointing**：确保作业容错性和状态一致性。

  - 内部容错机制包括：

    - at-least-once 至少执行一次
    - exactully-once 仅执行一次（跟storm一样）

    还有手动的checkpoint检查机制（spark streaming一样） 


```
+-------------------+     +-------------------+      +-------------------+
|    Client         |---->|    JobManager     |----->|   TaskManager     |
+-------------------+     +-------------------+      +-------------------+
       |                        |                        |
       v                        v                        v
+-------------------+    +-------------------+     +-------------------+
|   Data Source     |    |    Task Execution  |     |   State & Fault    |
|  (Kafka, HDFS,    |    |   (Map, Reduce)    |     |   Tolerance       |
|    DB, etc.)      |    |                    |     |                   |
+-------------------+    +-------------------+     +-------------------+

```



## 3. **YARN架构及MapReduce程序执行过程**

- **YARN架构**： YARN（Yet Another Resource Negotiator）是**Hadoop的资源管理层**，负责资源调度和管理。YARN的主要组件有：

  - **ResourceManager**：负责集群资源的管理和调度。
  - **NodeManager**：每个节点上的代理，负责向ResourceManager汇报节点的资源使用情况，并管理任务的执行。
  - **ApplicationMaster**：每个应用程序（例如MapReduce）会有一个独立的ApplicationMaster，负责与ResourceManager协作，获取资源并管理作业的生命周期。

  ```
  +-------------------+        +-------------------+       +-------------------+
  |    Client         |------->|  ResourceManager  |<----->|  NodeManager      |
  |  (Submit Job)     |        |   (Resource Mgmt) |       |   (Task Execution)|
  +-------------------+        +-------------------+       +-------------------+
                                     |
                                     v
                            +---------------------+
                            |   ApplicationMaster |
                            |    (App Life Cycle) |
                            +---------------------+
                                     |
                                     v
                           +------------------------+
                           |   Task Execution       |
                           |   (Map/Reduce Tasks)   |
                           +------------------------+
                                     |
                                     v
                            +---------------------+
                            |    Shuffle & Sort   |
                            +---------------------+
                                     |
                                     v
                             +---------------------+
                             |   HDFS Output       |
                             +---------------------+
  
  ```

  

- **MapReduce程序执行过程**：

  1. **提交作业**：用户提交MapReduce作业，JobClient与ResourceManager通信，申请资源。

  2. **资源分配**：ResourceManager分配资源，启动ApplicationMaster。

  3. **Map阶段**：ApplicationMaster向NodeManager请求启动Map任务，Map任务处理输入数据并生成中间结果。

  4. **Shuffle阶段**：Map任务完成后，进行数据的shuffle和排序，保证每个Reduce任务处理的数据是同一键的所有值。

  5. **Reduce阶段**：Reducer处理Map输出的中间结果，完成最终的数据计算。

  6. **作业完成**：所有任务完成后，ResourceManager会收集任务的状态，标记作业结束。

     

      总结：YARN架构包括ResourceManager、NodeManager、ApplicationMaster三个主要组件。

  ResourceManager负责资源调度，NodeManager负责管理节点资源，ApplicationMaster负责管理每个应用的生命周期。

  MapReduce作业执行时，ApplicationMaster请求资源并启动Map、Shuffle、Reduce任务，NodeManager负责执行具体任务。

## 4. **Hadoop系统要求、安装准备和配置文件介绍**

- **系统要求**： Hadoop运行在分布式环境下，通常需要多个节点。每个节点至少需要安装Java（推荐使用JDK 8或更高版本）。节点之间需要通过SSH进行通信。
- **安装准备**：
  - 下载并解压Hadoop发行包。
  - 配置Java环境变量。
  - 配置Hadoop配置文件，如`core-site.xml`、`hdfs-site.xml`、`mapred-site.xml`和`yarn-site.xml`。
- **配置文件介绍**：
  - `core-site.xml`：配置Hadoop核心服务，如HDFS文件系统的URI。
  - `hdfs-site.xml`：配置HDFS的具体设置，如数据存储路径、副本数等。
  - `mapred-site.xml`：配置MapReduce作业的相关设置。
  - `yarn-site.xml`：配置YARN的资源管理和任务调度。

# 大数据概念

### 三次信息浪潮及核心问题：

1. **第一次浪潮**：计算机引入，解决了自动化数据处理问题。
2. **第二次浪潮**：互联网普及，解决了信息传递和共享问题。
3. **第三次浪潮**：大数据、物联网、云计算兴起，解决了数据存储、处理、分析和价值提取问题。

### 大数据4V：

1. **Volume**：数据量大。
2. **Velocity**：数据生成速度快。
3. **Variety**：数据种类多。
4. **Veracity**：数据真实性。

## 大数据特点

1. **数据量大**：数据规模庞大，通常以TB、PB为单位。
2. **数据类型繁多**：包括结构化、半结构化和非结构化数据。
3. **处理速度快**：实时处理或近实时处理数据。
4. **价值密度低**：数据中的有用信息较少。

### 大数据技术层面：

1. **数据采集层**：获取原始数据。
2. **数据存储层**：存储数据，常用HDFS、NoSQL。
3. **数据处理层**：数据清洗与分析，使用MapReduce、Spark。
4. **数据分析层**：高级分析与建模，使用机器学习等。
5. **数据应用层**：业务应用，如推荐系统、预测分析。

### 典型计算模式：

1. **批处理模式**：Hadoop MapReduce。
2. **流处理模式**：Kafka、Flink。
3. **混合处理模式**：Spark。

### 三者关系：

- **物联网**生成数据。
- **大数据**处理、分析数据。
- **云计算**提供存储和计算资源。

## 云计算的关键：

包括**虚拟化**、**分布式存储**、**分布式计算**和**多租户**技术。

# Hadoop

### Hadoop项目架构图 （画图）

**这个感觉必考**

```
               +----------------------------+
               |        Client Application  |
               +----------------------------+
                          |
                          v
               +----------------------------+
               |      ResourceManager        |  
               +----------------------------+
                          |
                          v
               +----------------------------+       +-------------------------+
               |        NodeManager         | <--> |         HDFS            |
               +----------------------------+       +-------------------------+
                          |                                 |
                          v                                 v
               +----------------------------+       +-------------------------+
               |     JobHistoryServer       | <--> |       HDFS NameNode      |
               +----------------------------+       +-------------------------+
```

### Hadoop组件功能：(必考)

1. **HDFS**：分布式文件系统，存储大数据。
2. **MapReduce**：处理大规模数据集，基于分布式计算。
3. **YARN (Yet Another Resource Negotiator)**：资源管理框架，管理集群资源。
4. **JobHistoryServer**：存储和查看MapReduce任务的历史信息。
5. **ResourceManager**：负责资源调度和分配。
6. **NodeManager**：管理单个节点的资源，执行任务。
7. **Zookeeper**：协调分布式系统中的分布式应用。
8. **HBase**：一个分布式、可扩展的NoSQL数据库。
9. **Hive**：基于Hadoop的数据仓库，支持SQL查询。
10. **Pig**：为大数据处理提供更高层次的抽象，类似SQL。
11. **Mahout**：机器学习算法库。
12. **Sqoop**：用于在Hadoop与关系型数据库之间传输数据。
13. **Flume**：用于数据流的采集、聚合和传输。
14. **Ambari**：Hadoop集群的管理工具。
15. **MapReduce**：分布式数据处理框架。

### Hadoop特性：

1. **可扩展性**：支持PB级别的数据存储。
2. **容错性**：数据复制，节点失败时数据不丢失。
3. **高性能**：支持并行计算，大数据处理效率高。
4. **经济性**：开源，低成本。

### Hadoop集群节点类型：

1. **NameNode**：管理HDFS元数据，负责文件系统的命名和目录管理。

2. **DataNode**：存储实际数据，执行数据读取和写入操作。

3. **ResourceManager**：管理和调度集群资源。

4. **NodeManager**：管理每个节点上的资源和任务执行。

5. **JobHistoryServer**：保存MapReduce作业的历史信息。

   ## **Hadoop伪分布式安装过程**：

- **core-site.xml**：配置Hadoop的核心设置，如文件系统、默认的文件路径等。
- **hdfs-site.xml**：配置HDFS的设置，如块大小、复制因子、NameNode和DataNode的地址等。

### **SecondaryNameNode**：

- 解决EditLog过大问题，通过定期合并EditLog和FsImage，减少EditLog大小。

### SecondaryNameNode工作原理：

1. **合并过程**：SecondaryNameNode定期检查EditLog文件。如果EditLog有新的内容，它会将EditLog与当前的FsImage合并，并生成一个新的FsImage文件。
2. **更新FsImage**：合并后，新的FsImage会被传回给NameNode，更新HDFS的元数据。
3. **清空EditLog**：完成合并后，EditLog文件被清空，防止其过大。



# Hdfs

## hdfs结构

- **块**：大数据文件被切分为多个固定大小的块。
- **名称节点（NameNode）**：管理文件系统的元数据。
- **数据节点（DataNode）**：实际存储数据块的节点。
- **第二名称节点（SecondaryNameNode）**：帮助减轻NameNode的负担，通过定期合并编辑日志来创建文件系统的检查点。

## **HDFS冗余数据保存策略**：

默认复制因子为3，确保数据冗余存储，避免丢失。

- **数据复制策略**：第一个副本存储在写请求发起的DataNode上，第二个副本存储在**不同机架**的DataNode上，第三个副本存储在**另一个机架**的DataNode上。

## **HDFS的写数据流程**：

采用流水线方式进行数据写入，数据从客户端开始依次写入多个DataNode，直到所有副本都完成写入。

### HDFS读写过程：（画图）（感觉必考）

#### 写过程：

1. 客户端与NameNode通信，获取文件块的存储位置。
2. 客户端将数据分块，并发送到DataNode。
3. DataNode接收数据并存储，同时向客户端确认。

**写过程示意图**：

```
 Client ----> NameNode ----> DataNode1 ----> DataNode2 ----> DataNode3
        获取存储位置            存储数据块            存储数据块           存储数据块
```

#### 读过程：

1. 客户端向NameNode请求文件的块位置。
2. NameNode返回文件块的位置信息。
3. 客户端直接从相应的DataNode读取数据。

**读过程示意图**：

```
 Client ----> NameNode ----> DataNode1 ----> DataNode2
         获取块位置              读取数据                读取数据
```

### 判断文件是否存在（HDFS编程实现）：（编程）

**这个感觉也会考**

```
Configuration conf = new Configuration();
FileSystem fs = FileSystem.get(conf);
Path filePath = new Path("/user/hadoop/hdfsfle.txt");

if (fs.exists(filePath)) {
    System.out.println("File exists.");
} else {
    System.out.println("File does not exist.");
}
```

- **fs.exists(filePath)**：检查指定路径的文件是否存在。

# HBase

### HBase 三级寻址与Region个数计算：（计算）（必考）

1. 
1. 三级寻址
   - **RowKey** → **RegionServer** → **HRegion**
2. Region个数计算
   - 初始情况下，HBase有一个默认Region，随着数据增长，Region会自动分裂，Region数目与数据量和Region大小有关。
   - 计算公式：Region数量=数据总量/每个Region的最大大小

### HBase系统架构及组件功能：

1. **HMaster**：管理RegionServer的调度与分配，负责Region的分裂和负载均衡。
2. **RegionServer**：处理客户端请求，存储数据。
3. **Zookeeper**：协调HBase集群的节点，确保一致性和监控。
4. **HRegion**：存储HBase的数据表分区。
5. **HBase Client**：与HBase交互的客户端程序，执行读写操作。
5. **表**：数据存储的基本单元。
7. **行键**：唯一标识每一行的数据。
8. **列族**：列的集合。
9. **列限定符**：列族下的具体列名。
10. **单元格**：数据存储的位置。
11. **时间戳**：每个数据的版本标识。

## **HBase运行机制**：

- **RegionServe**：处理数据表的分区，负责存储与读写。
- **Store**：物理存储的数据结构。
- **HLog**：记录所有变更的日志。

### HBase Shell命令操作：

1. 创建表

   ：

   ```
   create 'my_table', 'cf1', 'cf2'
   ```

2. 添加数据

   ：

   ```
   put 'my_table', 'row1', 'cf1:col1', 'value1'
   ```

3. 浏览数据

   ：

   ```
   scan 'my_table'
   ```

4. 获得单元格数据

   ：

   ```
   get 'my_table', 'row1'
   ```

5. 删除表格

   ：

   ```
   disable 'my_table'
   drop 'my_table'
   ```

# Nosql

### CAP理论的含义：

1. **Consistency**（一致性）：所有节点的数据保持一致。
2. **Availability**（可用性）：每个请求都会收到响应，不管数据是否最新。
3. **Partition Tolerance**（分区容错性）：系统在网络分区的情况下仍然能够继续工作。

### 处理CAP问题的选择：

- **CP**（一致性 + 分区容错）：如HBase。
- **AP**（可用性 + 分区容错）：如Cassandra。
- **CA**（一致性 + 可用性）：适用于单机系统，无法容忍分区。



NoSQL数据库提供灵活的数据模型，适用于大数据场景，相较于传统关系数据库，NoSQL在横向扩展和高并发上有优势。

### NoSQL数据库的四大类型：

1. 键值存储（Key-Value Stores）

   ：存储数据以键值对的形式，如Redis。

   - **特征**：高效查询、可扩展性强，适合存储简单的数据。

2. 列族存储（Column-Family Stores）

   ：数据按列存储，如HBase、Cassandra。

   - **特征**：适合高吞吐量、高并发的场景，存储稀疏数据。

3. 文档存储（Document Stores）

   ：数据以JSON、XML等文档格式存储，如MongoDB。

   - **特征**：适合存储复杂的结构化数据，灵活性高。

4. 图数据库（Graph Databases）

   ：用于存储图结构数据，如Neo4j。

   - **特征**：适合复杂的关系数据，查询效率高。

## **BASE理论**：

- **基本可用**：部分系统可用。
- **软状态**：系统状态可能暂时不一致。
- **最终一致性**：系统最终会达到一致状态。

# MyReduce

### MapReduce体系架构图(画图)感觉必考哈

```
              +-------------------+
              |   Client          |
              +-------------------+
                       |
                       v
              +-------------------+
              |   JobTracker      |     (Resource Management)
              +-------------------+
                       |
                       v
       +--------------------------+  +--------------------------+
       |   TaskTracker (Map Task)  |  |   TaskTracker (Reduce Task)|
       +--------------------------+  +--------------------------+
                       |                       |
                       v                       v
               +-------------------+    +-------------------+
               |   HDFS Storage    |    |   HDFS Storage    |
               +-------------------+    +-------------------+

```

### MapReduce流程：

1. **Map端**：输入数据（如文件）被切分成多个块，分配给不同的Map任务处理。
2. **Shuffle过程**：Map任务的输出会根据键进行分组，并被传输到相应的Reduce任务。
3. **Reduce端**：根据分组的键，Reduce任务聚合数据，输出最终结果。

### MapReduce实现两个数据库表的自然连接：

1. **Map端**：将两个表的数据按主键进行映射。
2. **Reduce端**：对相同主键的数据进行连接。

### MapReduce实现WordCount：（编程）

1. **Map函数**：

   ```
   public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {
       public void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
           String[] words = value.toString().split("\\s+");
           for (String word : words) {
               context.write(new Text(word), new IntWritable(1));
           }
       }
   }
   ```

2. **Reduce函数**：

   ```
   public class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {
       public void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
           int sum = 0;
           for (IntWritable val : values) {
               sum += val.get();
           }
           context.write(key, new IntWritable(sum));
       }
   }
   ```

简短总结：

- **MapReduce架构**：分为Map任务、Shuffle过程、Reduce任务。
- **连接过程**：Map端根据主键映射，Reduce端进行连接。
- **WordCount**：Map函数分词计数，Reduce函数汇总计数。

# Storm

### Storm中的各个组件及功能：

1. **Topology**：整个Storm应用，包含所有的spouts和bolts。
2. **Spout**：数据源，负责向Storm系统发送原始数据。
3. **Bolt**：处理数据的单元，接收数据并进行处理后输出。
4. **Nimbus**：集群的主控节点，负责任务调度、分配。
5. **Supervisors**：工作节点，负责运行bolts和spouts。

### Storm的工作流程：

1. Spout从外部系统读取数据。
2. 数据经过Bolt处理。
3. 数据通过Storm集群进行分发和处理。

### Storm的特点：

- **实时性**：低延迟处理。
- **可扩展性**：能够处理大规模数据流。
- **容错性**：自动处理失败任务，保证数据处理完整。

### 编写Storm代码示例（Topology & Bolt）：

```
public class MyBolt extends BaseBasicBolt {
    @Override
    public void execute(Tuple input, BasicOutputCollector collector) {
        String word = input.getStringByField("word");
        collector.emit(new Values(word, 1));
    }
}

public class MyTopology {
    public static void main(String[] args) throws Exception {
        TopologyBuilder builder = new TopologyBuilder();
        builder.setSpout("spout", new MySpout(), 1);
        builder.setBolt("bolt", new MyBolt(), 1).shuffleGrouping("spout");

        Config conf = new Config();
        StormSubmitter.submitTopology("my-topology", conf, builder.createTopology());
    }
}

```

# Spark

### Spark Streaming和Storm的区别：感觉必考哈

- **实时性**：Storm是基于流的实时处理，适合低延迟需求；Spark Streaming是基于微批处理，延迟相对较高。
- **容错**：Storm具有内建的容错机制，Spark Streaming通过checkpoint机制。
- **架构**：Storm的架构较为简洁，直接处理数据流，Spark Streaming依赖于Spark的批处理架构，支持复杂操作。

### Spark中RDD的理解和运用：

- **RDD（Resilient Distributed Dataset）**：分布式数据集，Spark的核心抽象，支持并行计算、容错和分布式存储。

- RDD的操作

  ：

  - **转换操作**：如`map`、`filter`、`flatMap`，懒执行。
  - **行动操作**：如`collect`、`count`、`reduce`，触发实际计算。

  区别

  ：转换操作会返回新的RDD，行动操作会触发实际的计算和结果返回。

### 窄依赖和宽依赖的区别：

- **窄依赖**：每个父RDD的分区对应着子RDD的一个分区（如`map`、`filter`）。
- **宽依赖**：父RDD的一个分区可能会对应子RDD的多个分区（如`groupByKey`、`reduceByKey`）。

### 如何进行stage划分：

- 根据**宽依赖**进行Stage划分，不同Stage之间的数据需要shuffle操作（如`groupByKey`）。
- 每个Stage的操作会在同一节点内部进行，不需要跨节点的数据传输。

### park shell命令解读：

- `spark-shell`：启动Spark交互式shell。
- `sc.textFile("path")`：加载文本文件。
- `rdd.map(func)`：对每个元素应用`func`。
- `rdd.collect()`：获取RDD的所有数据。
- `rdd.reduce(func)`：对RDD数据进行聚合。

### Spark编程实现统计字符个数：

```
val textFile = sc.textFile("path/to/file.txt")
val charCount = textFile.flatMap(line => line.split("")).map(char => (char, 1)).reduceByKey(_ + _)
charCount.collect().foreach(println)

```

- **RDD**：Spark的核心数据结构，支持转换和行动操作。
- **窄/宽依赖**：窄依赖跨分区小，宽依赖跨分区大。
- **Stage划分**：宽依赖会引起Stage划分

# 概念

### 可视化的基本概念：

- **数据可视化**：通过图形、图表、地图等方式呈现数据，帮助理解、分析和展示数据。
- **目的**：将复杂的数据转化为易于理解的形式，揭示数据中的模式、趋势和关系。
- 类型：
  - **静态可视化**：如条形图、折线图、饼图等。
  - **动态可视化**：交互式图表，能够实时更新数据。
- **常用工具**：如Tableau、Power BI、D3.js等。

简短总结： 数据可视化通过图形化展示数据，帮助更直观地理解信息。

### 基于商品和基于用户的协同过滤算法：

1. **基于商品的协同过滤（Item-based CF）**：

   - **原理**：根据用户对商品的评分，找到相似商品并推荐给用户。

   - 步骤

     ：

     - 计算商品之间的相似度（如余弦相似度）。
     - 根据用户历史评分，推荐相似商品。

   - **优点**：相对稳定，推荐的商品相似度较高。

   - **缺点**：冷启动问题（新商品推荐困难）。

2. **基于用户的协同过滤（User-based CF）**：

   - **原理**：根据用户之间的相似度，推荐其他相似用户喜欢的商品。

   - 步骤

     ：

     - 计算用户之间的相似度。
     - 根据相似用户的评分，推荐商品。

   - **优点**：个性化较强，易于理解。

   - **缺点**：计算复杂度高，冷启动问题。

### Pregel求最大值/最小值计算过程：（画图）

1. **基本思想**：Pregel通过“顶点”的消息传递来处理大规模图数据，适用于图计算任务，如最大值/最小值计算。

2. **计算过程**：

   - **初始化**：每个顶点初始化为其自身的值（最大值或最小值）。
   - **迭代**：每个顶点将其当前值传递给相邻的顶点。每个接收到消息的顶点更新自己的值为当前值和收到的值中的最大值或最小值。
   - **终止条件**：当图中所有顶点的值不再发生变化时，算法结束。

   **图示说明**：

   ```
   A---B---C
    /     \
   D       E
   ```

   初始状态：顶点A、B、C、D、E的值分别为其初始值。通过迭代传递消息，直到最大值或最小值稳定。

### 迪杰斯特拉算法求解单源最短路径问题：

1. **基本思想**：从源顶点出发，逐步更新每个顶点的最短路径值，直到所有顶点的最短路径值确定。
2. **算法步骤**：
   - **初始化**：将源顶点的最短路径值设为0，其余顶点设为无穷大。
   - **选择最短路径**：选择尚未确定最短路径的顶点中最短的一个，更新其相邻顶点的最短路径。
   - **迭代**：重复选择顶点并更新路径，直到所有顶点的最短路径确定。

### 利用Pregel实现单源最短路径计算过程：

1. **初始化**：

   - 源顶点的最短路径为0，其他顶点初始化为无穷大。

2. **消息传递**：

   - 每个顶点将其当前最短路径值传递给相邻的顶点。
   - 接收到消息的顶点更新最短路径值，取当前值和从邻居收到的值的较小值。

3. **迭代**：

   - 继续进行消息传递，直到最短路径值不再变化。

4. **计算过程**（表格形式）：

   | 顶点 | 当前最短路径值 | 邻居 | 接收到的消息 | 更新后的最短路径值 |
   | ---- | -------------- | ---- | ------------ | ------------------ |
   | A    | 0              | B, D | ∞, ∞         | 0                  |
   | B    | ∞              | A, C | 0, ∞         | 1                  |
   | C    | ∞              | B, E | 1, ∞         | 2                  |
   | D    | ∞              | A, E | 0, ∞         | 1                  |
   | E    | ∞              | C, D | 2, 1         | 2                  |

5. **终止条件**：当所有顶点的最短路径值稳定，算法结束。

# Hive

### Hive中SQL语句转换成MapReduce的基本原理：

- **原理**：Hive使用HiveQL语言查询数据，查询语句会被转换为一系列的MapReduce任务。**Hive的查询处理器将SQL解析为一个逻辑执行计划，然后通过优化转换成物理执行计划（MapReduce作业）。**

- 过程

  ：

  1. **解析**：HiveQL语句被解析成抽象语法树（AST）。
  2. **优化**：对AST进行优化，去除冗余操作。
  3. **生成MapReduce作业**：根据查询逻辑生成MapReduce作业，并执行。

### Hive中SQL查询转换成MapReduce作业的过程：

1. SQL查询

   ：例如查询表中某字段的总和：

   ```
   
   SELECT SUM(amount) FROM transactions;
   ```

2. **解析阶段**：Hive将此SQL语句转换为一个抽象的查询计划。

3. **优化阶段**：Hive进行查询优化，合并某些操作。

4. **生成MapReduce作业**：Hive将最终的查询计划转化为MapReduce作业。Map任务执行数据扫描，Reduce任务聚合计算结果。

5. **执行**：Hive提交MapReduce作业到Hadoop集群执行。

### Hive中Shell环境下常用操作代码：

**就是Mysql的语法**

1. **创建数据库**：

   ```
   
   CREATE DATABASE mydb;
   ```

2. **创建表**：

   ```
   CREATE TABLE employees (
       id INT,
       name STRING,
       salary DOUBLE
   );
   ```

3. **修改表**：

   ```
   
   ALTER TABLE employees ADD COLUMNS (address STRING);
   ```

4. **查看表**：

   ```
   
   SHOW TABLES;
   ```

5. **描述表结构**：

   ```
   
   DESCRIBE employees;
   ```

6. **插入数据**：

   ```
   
   INSERT INTO TABLE employees VALUES (1, 'John', 5000.0);
   ```
