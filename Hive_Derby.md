# Create HDFS Directories first-

hadoop fs -mkdir /user/hive  
hadoop fs -mkdir /user/hive/warehouse  
hadoop fs -chmod g+w /user/hive/warehouse

# Download HIVE 

wget http://archive.apache.org/dist/hive/hive-2.3.3/apache-hive-2.3.3-bin.tar.gz  
tar xzf apache-hive-2.3.3-bin.tar.gz  
Rename the folder as Hive  

# Download Derby

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
        
        <property>
          <name>javax.jdo.option.ConnectionURL</name>  
          <value>jdbc:derby://localhost:1527/metastore_db;create=true </value>  
          <description>JDBC connect string for a JDBC metastore </description>  
        </property>
    
   2. Add below lines for the Derby connection 
  
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
