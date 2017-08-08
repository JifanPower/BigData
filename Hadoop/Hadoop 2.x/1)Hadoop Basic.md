/**Jason,Jinfan Power 2017.08.07**/

# Hadoop Basic

## Hadoop
The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer, so delivering a highly-available service on top of a cluster of computers, each of which may be prone to failures.

## Prerequisites
    Linux Basic
    Java[Option] 
    Sql[Option] 

## Hadoop Origin
    Google
        GFS          ->     HDFS
        MapReduce    ->     MapReduce
        BigTable     ->     HBase
    Hadoop was created by Doug Cutting, the creator of Apache Lucene, the widely used text search library. 
    Hadoop has its origins in Apache Nutch, an open source web search engine, itself a part of the Lucene project.
    
## Hadoop Modules
    Hadoop Common: The common utilities that support the other Hadoop modules.
    Hadoop Distributed File System (HDFS™): A distributed file system that provides high-throughput access to application data.
    Hadoop YARN(Yet Another Resource Negotiator): A framework for job scheduling and cluster resource management.
    Hadoop MapReduce: A YARN-based system for parallel processing of large data sets.

## Hadoop-Related Projects 
    HBase™: A scalable, distributed database that supports structured data storage for large tables.
    Hive™: A data warehouse infrastructure that provides data summarization and ad hoc querying.
    Spark™: A fast and general compute engine for Hadoop data. Spark provides a simple and expressive programming model that supports a wide range of applications, including ETL, machine learning, stream processing, and graph computation.
    ZooKeeper™: A high-performance coordination service for distributed applications.

## Hadoop Directory
    /etc    config files
    /bin    executable files
    /sbin   sh files
    ...

## Hadoop Modules Detail
    HDFS
        namenode
        datanode
        secondary namenode

    YARN
        resource manager
        node manager

    MapReduce
        map
        reduce

    HDFS(Hadoop Distributed File System)
        HDFS is a distributed filesystem designed for storing very large files with streaming data access patterns, running on clusters of commodity hardware.
        
        namenode
        It maintains the filesystem tree and the metadata for all the files and directories in the tree.

        datanode
        Datanodes are the workhorses of the filesystem. 
        They store and retrieve blocks when they are told to (by clients or the namenode), and they report back to the namenode periodically with lists of blocks that they are storing.

        secondary namenode
        Its main role is to periodically merge the namespace image with the edit log to prevent the edit log from becoming too large.

    YARN(Yet Another Resource Negotiator)
        It is Hadoop’s cluster resource management system.
        
        resource manager
        Resource manager manage the use of resources across the cluster.

        node manager
        Node managers running on all the nodes in the cluster to launch and monitor containers.
    
    MapReduce
        MapReduce is a programming model for data processing.
        MapReduce works by breaking the processing into two phases,Each phase has key-value pairs as input and output.
        (input) <k1, v1> -> map -> <k2, v2> -> combine -> <k2, v2> -> reduce -> <k3, v3> (output)

## Hadoop Modules Startup
    HDFS
        bin/hdfs namenode -format (format the DFS filesystem)
        sbin/hadoop-daemon.sh start namenode
        sbin/hadoop-daemon.sh start datanode
        sbin/hadoop-daemon.sh start secondarynamenode

        etc/hadoop/core-site.xml
            <property>
                <name>fs.defaultFS</name>
                <value>hdfs://localhost:9000</value>
            </property>

    YARN
        sbin/yarn-daemon.sh  start  resourcemanager
        sbin/yarn-daemon.sh  start  nodemanager

    MapReduce
        sbin/mr-jobhistory-daemon.sh start historyserver
    
    Tool
        jps
        http://localhost:50070 (hdfs)
        http://localhost:8088(yarn)

    Config
        etc/hadoop/hadoop-env.sh
            JAVA_HOME

        etc/hadoop/hdfs-site.xml:
             <property>
                 <name>dfs.replication</name>
                 <value>1</value>
             </property>

        

## Hdfs File Operator
    Command
         bin/hdfs dfs -mkdir -p
         bin/hdfs dfs -ls /
         bin/hdfs dfs -cat
         bin/hdfs dfs -put
         bin/hdfs dfs -get

## MapReduce Demo
### WordCount
    bin/hdfs dfs -mkdir -p  /user/jason/mapreduce/wordcount/input
    bin/hdfs dfs -put wordcountinput/w.input /user/jason/mapreduce/wordcount/input
    1.Local
       bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.1.jar wordcount /user/jason/mapreduce/wordcount/input/ /user/jason/mapreduce/wordcount/output/

    2.YARN
        etc/hadoop/yarn-site.xml
            yarn.nodemanager.aux-services       mapreduce_shuffle
            <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
            </property>

        etc/hadoop/mapred-site.xml
            mapred-site.xml.template to mapred-site.xml
            Default is local,Change to yarn
            mapreduce.framework.name            yarn
            <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
            </property>

        bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.8.1.jar wordcount /user/jason/mapreduce/wordcount/input/ /user/jason/mapreduce/wordcount/output/