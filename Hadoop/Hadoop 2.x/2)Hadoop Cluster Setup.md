/**Jason,Jinfan Power 2017.08.07**/

# Hadoop Cluster Setup

## Prerequisites
    Install Java.
    Download a stable version of Hadoop from Apache mirrors.
	Ssh login without password.[Option]

## Demon Host
       u1             u2              u3
HDFS
    NameNode
    DataNode        DataNode        DataNode
                                    SecondaryNameNode

YARN
                    ResourceManager
    NodeManager     NodeManager     NodeManager

MapReduce
    JobHistoryServer

## Associated Files
    hdfs
	* hadoop-env.sh(JAVA_HOME)
	* core-site.xml(NameNode)
	* hdfs-site.xml(SecondaryNameNode)
	* slaves(DataNode)
    yarn
	* yarn-env.sh
	* yarn-site.xml(ResourceManager)
	* slaves(NodeManager)
    mapreduce
	* mapred-env.sh
	* mapred-site.xml(JobHistoryServer)

# Configuring Environment of Hadoop Daemons
    1)etc/hadoop/hadoop-env.sh
        JAVA_HOME for hdfs node.
    2)etc/hadoop/mapred-env.sh
        JAVA_HOME for mapreduce node.
    3)etc/hadoop/yarn-env.sh
        JAVA_HOME for yarn node.

# Configuring the Hadoop Daemons
    hdfs
        etc/hadoop/core-site.xml
        Parameter	        Value           Notes
        fs.defaultFS	    NameNode URI	hdfs://host:port/ default:file:///
        hadoop.tmp.dir      hadoop tmp dir  default:/tmp/hadoop-${user.name}
        fs.trash.interval   10080           Number of minutes default:0
        io.file.buffer.size	131072	        Size of read/write buffer used 
											in SequenceFiles.
        
        namenode
            etc/hadoop/hdfs-site.xml
                dfs.namenode.secondary.http-address host:port

        datanode
            slaves
                add hostname

        secondary namenode
            Parameter	                        Value      Notes
            dfs.namenode.secondary.http-address host:port  default:0.0.0.0:50090

    yarn
        resourcemanager
            etc/hadoop/yarn-site.xml
            Parameter	                        Value       Notes
            yarn.resourcemanager.hostname       hostname    default:0.0.0.0
                
        nodemanager
            etc/hadoop/yarn-site.xml
                Parameter                           Value            
                yarn.log-aggregation-enable         true
                yarn.log-aggregation.retain-seconds 86400
                yarn.nodemanager.aux-services       mapreduce_shuffle
                yarn.nod emanager.resource.memory-mb 1024
            slaves
                add hostname

    mapreduce
        etc/hadoop/mapred-site.xml
		Parameter	                        Value       Notes
		mapreduce.framework.name            yarn        Execution framework
														set to YARN.
		mapreduce.jobhistory.address	    host:port   default:0.0.0.0:10020
		mapreduce.jobhistory.webapp.address host:port   default:0.0.0.0:19888
	
# Distribute   
    copy files
        use scp

    time sync
        use ntp

# Startup
    hdfs
        bin/hdfs namenode -format
        sbin/hadoop-daemon.sh
        sbin/hadoop-daemon.sh start namenode
        sbin/hadoop-daemon.sh start datanode
        sbin/hadoop-daemon.sh start secondarynamenode

    yarn
        sbin/yarn-daemon.sh 
        sbin/yarn-daemon.sh start resourcemanager
        sbin/yarn-daemon.sh start nodemanager
        
    mapreduce
        no need startup

# Test
    url
        etc/hadoop/core-site.xml
        Parameter                               ValueFormat         defaultPort
        fs.defaultFS                            hdfs://host:port/   8020

        etc/hadoop/hdfs-site.xml
        Parameter                               ValueFormat         defaultPort
        dfs.namenode.secondary.http-address     host:port           50090

        etc/hadoop/mapred-site.xml
        Parameter                               ValueFormat         defaultPort
        mapreduce.jobhistory.address	        host:port           10020
        mapreduce.jobhistory.webapp.address     host:port           19888

    

