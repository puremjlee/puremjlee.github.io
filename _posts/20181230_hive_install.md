---
layout: post
title: Hive Install Guide
---

## Step 0. Prerequisite
```
$ ssh-keygen
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
Shutdown firewall
```
$ service firewalld stop
$ service iptables stop
```

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
```
$ mkdir -p /home/hadoop/hadoopinfra
```

**yarn-site.xml**
```
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
## Step 4. Hive Install
Hive version 2.3.4
```
$ cd /usr/local
$ wget http://apache.tt.co.kr/hive/hive-2.3.4/apache-hive-2.3.4-bin.tar.gz
$ tar xvfz apache-hive-2.3.4-bin.tar.gz
$  mv apache-hive-2.3.4-bin /usr/local/hive
```
environment variable setting
```
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=$CLASSPATH:/usr/local/hadoop/lib/*:.
export CLASSPATH=$CLASSPATH:/usr/local/hadoop/share/hadoop/common/
export CLASSPATH=$CLASSPATH:/usr/local/hive/lib/*:.
export HIVE_CONF_DIR=$HIVE_HOME/conf
```
```
$ source ~/.bashrc
$ cd $HIVE_HOME/conf
```
**hive-env.sh**
```
export HADOOP_HOME=/usr/local/hadoop
```
**hive-site.xml**
It's embedded derby setting as metastore.
```
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:derby:metastore_db;create=true </value>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>org.apache.derby.jdbc.EmbeddedDriver</value>
  </property>
```
## Step 5. Derby install
Derby version 10.4.2.0
```
$ cd /usr/local
$ wget http://archive.apache.org/dist/db/derby/db-derby-10.4.2.0/db-derby-10.4.2.0-bin.tar.gz
$ tar zxfv db-derby-10.4.2.0-bin.tar.gz
$ mv db-derby-10.4.2.0-bin /usr/local/derby
```
environment variable setting
```
export DERBY_INSTALL=/usr/local/derby
export DERBY_HOME=/usr/local/derby
export PATH=$PATH:$DERBY_HOME/bin
export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar
```
```
$ source ~/.bashrc
$ mkdir $DERBY_HOME/data
```
## Step 6. Verifying Hive Install
```
$ hadoop fs -mkdir /tmp
$ hadoop fs -mkdir /user
$ hadoop fs -mkdir /user/hive
$ hadoop fs -mkdir /user/hive/warehouse
$ hadoop fs -chmod g+w /tmp
$ hadoop fs -chmod g+w /user/hive
$ hadoop fs -chmod g+w /user/hive/warehouse
($ rm -rf $HIVE_HOME/metastore_db)
$ schematool -initSchema -dbType derby
$ unset HADOOP_CLIENT_OPTS
$ export HADOOP_CLIENT_OPTS="$HADOOP_CLIENT_OPTS -XX:MaxPermSize=512m"
$ hive
```
