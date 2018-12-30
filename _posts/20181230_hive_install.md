## Step 1. Java Install
java version "1.7.0_55"
```
$ cd /usr/local/
$ tar zxf jdk-7u55-linux-x64.tar.gz
$ mv jdk1.7.0_55 /usr/local
```
environment variable setting
```
export JAVA_HOME=/usr/local/jdk1.7.0_71
export PATH=$JAVA_HOME/bin:$PATH
```
```
source ~/.bashrc
```
java -version

## Step 2. Hadoop Install
hadoop version "2.5.2"
```
$ cd /usr/local
$ wget https://archive.apache.org/dist/hadoop/core/hadoop-2.5.2/hadoop-2.5.2.tar.gz
$ tar xvzf hadoop-2.5.2.tar.gz
$ mv hadoop-2.5.2 hadoop
```
environment variable setting for Pseudo Distributed Mode
```
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
```
```
source ~/.bashrc
```
```
$ cd $HADOOP_HOME/etc/hadoop
```
**hadoop-evn.sh**
```
export JAVA_HOME=/usr/local/jdk1.7.0_71
```
**core-site.xml**
```
   <property>
      <name>fs.default.name</name>
      <value>hdfs://10.211.55.15:9000</value>
   </property>
```
**hdfs-site.xml**
```
  <property>
      <name>dfs.replication</name>
      <value>1</value>
   </property>
   <property>
      <name>dfs.name.dir</name>
      <value>file:///home/hadoop/hadoopinfra/hdfs/namenode</value>
   </property>
   <property>
      <name>dfs.data.dir</name>
      <value>file:///home/hadoop/hadoopinfra/hdfs/datanode</value>
   </property>
```
**yarn-site.xml**
   <property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
   </property>
 ```
 **mapred-site.xml**
 ```
   <property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
   </property>
```
## Step 3. Hadoop Start

```
$ hadoop namenode -format
$ start-dfs.sh
$ start-yarn.sh
$ jps
```
**Accessing hadoop on Browser**
```
http://localhost:50070/
http://localhost:8088/
```
