Make HDFS Directories first-

hadoop fs -mkdir /user/hive
hadoop fs -mkdir /user/hive/warehouse
hadoop fs -chmod g+w /user/hive/warehouse

Download HIVE 

wget http://archive.apache.org/dist/hive/hive-2.3.3/apache-hive-2.3.3-bin.tar.gz
tar xzf apache-hive-2.3.3-bin.tar.gz
Make the folder name as Hive


Download Derby

wget http://us.mirrors.quenda.co/apache//db/derby/db-derby-10.14.2.0/db-derby-10.14.2.0-bin.tar.gz
tar zxvf db-derby-10.14.2.0-bin.tar.gz
Make the folder name as Derby


Set below enviornment variables


Create hive-env.sh and hive-site.xml files
cd $HIVE_HOME/conf
cp hive-default.xml.template hive-site.xml
cp hive-env.sh.template hive-env.sh

Edit the hive-env.sh file by appending the following line:
export HADOOP_HOME=/usr/local/hadoop


Create a file named jpox.properties and add the following lines into it:


cd $HIVE_HOME
$HIVE_HOME/bin/schematool -initSchema -dbType derby

cd $HIVE_HOME
bin/hive

show tables;

I have attached the hive-env.sh , hive-site.xml and jpox.properties

hdfs dfsadmin -safemode leave
