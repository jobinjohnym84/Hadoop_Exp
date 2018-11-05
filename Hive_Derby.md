# Why HIVE-

Apache Hive is an open source data warehouse system built on top of Hadoop Haused for querying and analyzing large datasets stored in HDFS. Initially, you have to write complex Map-Reduce jobs, but now with the help of Hive, you just need to submit merely SQL queries. Hive is mainly targeted towards users who are comfortable with SQL. Hive use language called HiveQL (HQL), which is similar to SQL. HiveQL automatically translates SQL-like queries into MapReduce jobs.

Lets talk about the HIVE setup on top of existing Hadoop cluster-

# Create HDFS Directories first-

hadoop fs -mkdir /user/hive  
hadoop fs -mkdir /user/hive/warehouse  
hadoop fs -chmod g+w /user/hive/warehouse

# Download HIVE 

wget http://archive.apache.org/dist/hive/hive-2.3.3/apache-hive-2.3.3-bin.tar.gz  
tar xzf apache-hive-2.3.3-bin.tar.gz  
Rename the folder as Hive  

# Download Derby

Apache Derby is a relational database management system developed by the Apache Software Foundation that can be embedded in Java programs and used for online transaction processing. Derby is used to store the Schema of the Tables in HIVE.  

wget http://us.mirrors.quenda.co/apache//db/derby/db-derby-10.14.2.0/db-derby-10.14.2.0-bin.tar.gz  
tar zxvf db-derby-10.14.2.0-bin.tar.gz  
Rename the folder as Derby  

# Set below enviornment variables  (sudo nano .bashrc)

export HIVE_HOME=/home/superjjohny/hive  
export PATH=$HIVE_HOME/bin:$PATH  

export DERBY_HOME=/home/superjjohny/derby  
export PATH=$PATH:$DERBY_HOME/bin  
export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar  

# Create hive-env.sh and hive-site.xml files

cd $HIVE_HOME/conf  
cp hive-default.xml.template hive-site.xml  
cp hive-env.sh.template hive-env.sh  

# Edit the hive-env.sh file by appending the Hadoop path:
export HADOOP_HOME=/usr/local/hadoop  

# Make changes in the hive-site.xml file

 Three changes mainly-  
   1. Add below lines for the Derby connection  
        ```
        <property>
          <name>javax.jdo.option.ConnectionURL</name>  
          <value>jdbc:derby://localhost:1527/metastore_db;create=true </value>  
          <description>JDBC connect string for a JDBC metastore </description>  
        </property>
     
   2. Edit below lines for the Derby connection
        ```
         <property>
           <name>hive.exec.local.scratchdir</name>
           <value>/tmp/${user.name}</value>
           <description>Local scratch space for Hive jobs</description>
         </property>
         <property>
           <name>hive.downloaded.resources.dir</name>
           <value>/tmp/${user.name}_resources</value>
           <description>Temporary local directory for added resources in the remote file system.</description>
         </property>
   
   3. Change the hive.metastore.schema.verification property value to false

# Create a file named jpox.properties and add the following lines into it (sudo nano $HIVE_HOME/conf/jpox.properties)
``` 
javax.jdo.PersistenceManagerFactoryClass =  

org.jpox.PersistenceManagerFactoryImpl  
org.jpox.autoCreateSchema = false  
org.jpox.validateTables = false  
org.jpox.validateColumns = false  
org.jpox.validateConstraints = false  
org.jpox.storeManagerType = rdbms  
org.jpox.autoCreateSchema = true  
org.jpox.autoStartMechanismMode = checked  
org.jpox.transactionIsolation = read_committed  
javax.jdo.option.DetachAllOnCommit = true  
javax.jdo.option.NontransactionalRead = true  
javax.jdo.option.ConnectionDriverName = org.apache.derby.jdbc.ClientDriver  
javax.jdo.option.ConnectionURL = jdbc:derby://localhost:1527/metastore_db;create = true  
javax.jdo.option.ConnectionUserName = APP  
javax.jdo.option.ConnectionPassword = mine  

```

# Specify to use derby as schema

cd $HIVE_HOME  
$HIVE_HOME/bin/schematool -initSchema -dbType derby  

# Start HIVE

cd $HIVE_HOME  
bin/hive  

# Show the Tables
show tables;  

# Side Note ----------

I have attached the hive-env.sh , hive-site.xml and jpox.properties  

hdfs dfsadmin -safemode leave

# HIVE cannot perform- 

It's not designed for Online transaction processing (OLTP), it is only used for the Online Analytical Processing (OLAP).   
Hive supports overwriting or apprehending data, but not updates and deletes.  
Sub-queries are not supported, in Hive    

Just in cas of any permission issues related to the HIVE folder, please execute below command-

sudo chmod -R 777 /home/superjjohny/hive
