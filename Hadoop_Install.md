# Introduction

The intention of this repository is to have a running log of the Tests and Steps which I have taken care to make a Hadoop Cluster up and running.

# Tools used

a) PUTTY  b) WINSCP c) Jupyter NoteBook

# Steps

1. Download Hadoop (version n-1)  
curl -O http://apache.claz.org/hadoop/common/hadoop-2.9.0/hadoop-2.9.0.tar.gz

2. Unzip the file  

tar xfz hadoop-2.8.3.tar.gz  

3. Update the BASHRC file (sudo nano .bashrc)  

``` 
export HADOOP_PREFIX=/home/jobin/hadoop-2.9.0
export HADOOP_HOME=/home/jobin/hadoop-2.9.0
export HADOOP_MAPRED_HOME=${HADOOP_HOME}
export HADOOP_COMMON_HOME=${HADOOP_HOME}
export HADOOP_HDFS_HOME=${HADOOP_HOME}
export YARN_HOME=${HADOOP_HOME}
export HADOOP_CONFDIR=${HADOOP_HOME}/etc/hadoop
# Native path
export HADOOP_COMMON_LIB_NATIVE_DIR=${HADOOP_PREFIX}/lib/native
export HADOOP_OPTS="-Djava.library.path=${HADOOP_PREFIX}/lib/native"
export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH
# Add Hadoop bin/ directory to PATH
export PATH=$PATH:$HADOOP_HOME/
export PATH=$PATH:$HADOOP_HOME/bin

``` 
4. Update the BASH_PROFILE (sudo nano bash_profile)  

``` 
## JAVA env variables
export JAVA_HOME=/usr/lib/jvm/jre-1.7.0-openjdk-1.7.0.141-2.6.10.5.el7.x86_64
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
## HADOOP env variables
export HADOOP_HOME=/home/jobin/hadoop-2.9.0
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_YARN_HOME=$HADOOP_HOME
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin

``` 

5. Edit the core-site.xml  

``` 
<configuration>

<property>
  <name>hadoop.tmp.dir</name>
  <value>/home/jobin/tmp</value>
  <description>A base for other temporary directories.</description>
</property>

<property>
<name>fs.defaultFS</name>
<value>hdfs://servername1:9000/</value>
</property>

</configuration>

``` 
6. Create namenode and datanode folders   

mkdir -p namenode  
mkdir -p datanode  


7. Edit the HDFS Site.xml  nano hadoop-2.9.0/etc/hadoop/hdfs-site.xml  
``` 
<configuration>

<property>
<name>dfs.data.dir</name>
<value>file:///home/jobin/datanode</value>
</property>
<property>
<name>dfs.webhdfs.enabled</name>
<value>true</value>
</property>
<property>
<name>dfs.permissions.enabled</name>
<value>false</value>
</property>
</configuration>

``` 
8. Edit  nano hadoop-2.9.0/etc/hadoop/mapred-site.xml  
``` 
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
</configuration>

``` 
9. Edit nano hadoop-2.9.0/etc/hadoop/yarn-site.xml  

``` 
<configuration>

<!-- Site specific YARN configuration properties -->
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>

<property>
    <name>yarn.resourcemanager.scheduler.address</name>
    <value>http://ServerName1:8030</value>
</property>
<property>
    <name>yarn.resourcemanager.address</name>
    <value>http://ServerName1:8032</value>
</property>
<property>
    <name>yarn.resourcemanager.webapp.address</name>
    <value>http://ServerName1:8088</value>
</property>
<property>
    <name>yarn.resourcemanager.resource-tracker.address</name>
    <value>http://ServerName1:8031</value>
</property>
<property>
    <name>yarn.resourcemanager.admin.address</name>
    <value>http://ServerName1:8033</value>
</property>


</configuration>

``` 
10. 






# Troubleshooting


nano hadoop-2.9.0/etc/hadoop/hadoop-env.sh

hadoop-2.9.0/sbin/start-dfs.sh
hadoop-2.9.0/sbin/start-yarn.sh




