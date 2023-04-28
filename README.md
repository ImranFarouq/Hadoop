# Installations


## Hadoop setup


Oracle Virtual Box - https://www.virtualbox.org/wiki/Downloads  <br>
Ubuntu Desktop LTS - https://ubuntu.com/download/desktop <br>
Setup parameters - 
<br>

### Ip address
ifconfig<br>
sudo apt update <br>

#### Java
sudo apt install openjdk-8-jdk<br>
ls /usr/lib/jvm/java-8-openjdk-amd64<br>
nano ~/.bashrc<br>
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64<br>
export PATH=$PATH:$JAVA_HOME/bin<br>
source .bashrc<br>
echo $JAVA_HOME<br>
java -version #1.8<br>

#### Passwordless ssh
ssh localhost<br>
sudo apt-get install openssh-server openssh-client<br>
ssh localhost<br>
ssh-keygen -t rsa -P ""<br>
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys<br>
ssh localhost<br>
exit<br>

#### Hadoop<br>
mkdir -p course/softwares<br>
cd course/softwares<br>
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz <br>
mv hadoop-3.3.4.tar.gz course/softwares<br>
tar -xzvf hadoop-3.3.4.tar.gz<br>

'''
Local (or Standalone) mode: There are no daemons and everything runs on a single JVM.<br>
Pseudo-Distributed mode: Each daemon(Namenode, Datanode etc) runs on its own JVM on a single host.<br>
Distributed mode: Each Daemon run on its own JVM across a cluster of hosts<br>
'''

#### Stand alone mode
ls<br>
nano ~/.bashrc<br>
export HADOOP_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4<br>
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin<br>
source .bashrc<br>
hadoop version<br>

#### Word count problem in standalone mode<br>
ls $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar<br>

mkdir wordcountex <br>
####### add text files inside that folder or cp $HADOOP_HOME/*.txt wordcountex<br>
jar tf $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar | grep wordcount -i<br>
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar  wordcount wordcountex texts_output <br>
cat texts_output/part-r-00000 | sort -k 2 -nr | head n -5<br>

#### Pseudo Distributed mode<br>
nano ~/.bashrc<br>

export HADOOP_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4<br>
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin<br>
export HADOOP_HDFS_HOME=$HADOOP_HOME<br>
export HADOOP_MAPRED_HOME=$HADOOP_HOME<br>
export HADOOP_COMMON_HOME=$HADOOP_HOME<br>
export HADOOP_YARN_HOME=$HADOOP_HOME<br>
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native<br>
export HADOOP_INSTALL=$HADOOP_HOME<br>
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"<br>
export HADOOP_OPTS="-Djava.library.path=$HADOOP_COMMON_LIB_NATIVE_DIR"<br>
export HADOOP_SECURITY_CONF_DIR<br>

source ~/.bashrc<br>

hadoop version<br>
cd $HADOOP_HOME/etc/hadoop<br>

nano hadoop-env.sh <br>
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64<br>

nano core-site.xml<br>
<configuration><br>

   <property> <br>
      <name>fs.default.name</name> <br>
      <value>hdfs://localhost:9000</value> <br>
   </property><br>
   
</configuration><br>

nano hdfs-site.xml<br>
<configuration><br>

   <property> <br>
      <name>dfs.replication</name> <br>
      <value>1</value> <br>
   </property> <br>
   <property> <br>
      <name>dfs.name.dir</name> <br>
      <value>file:///home/srividya/hadoopinfra/hdfs/namenode </value> <br>
   </property> <br>
   <property> <br>
      <name>dfs.data.dir</name><br>
      <value>file:///home/srividya/hadoopinfra/hdfs/datanode </value > <br>
   </property><br>
   
</configuration><br>

hadoop classpath<br>
/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/etc/hadoop:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/common/lib/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/common/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/hdfs:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/hdfs/lib/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/hdfs/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/mapreduce/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/yarn:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/yarn/lib/*:/home/srividya/Desktop/trialrun/softwares/hadoop-3.3.4/share/hadoop/yarn/*<br>

nano yarn-site.xml<br>
<configuration><br>

   <property> <br>
      <name>yarn.nodemanager.aux-services</name> <br>
      <value>mapreduce_shuffle</value> <br>
   </property><br>
   <property><br>
    <name>yarn.application.classpath</name><br>
    <value>output from hadoop classpath</value><br>
 </property><br>
   
</configuration><br>

nano mapred-site.xml<br>
<configuration><br>

   <property> <br>
      <name>mapreduce.framework.name</name> <br>
      <value>yarn</value> <br>
   </property><br>
<property><br>
  <name>mapreduce.reduce.env</name><br>
  <value>HADOOP_MAPRED_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4</value><br>
</property><br>

<property><br>
  <name>yarn.app.mapreduce.am.env</name><br>
  <value>HADOOP_MAPRED_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4</value><br>
</property><br>

<property><br>
  <name>mapreduce.map.env</name><br>
  <value>HADOOP_MAPRED_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4</value><br>
</property><br>


</configuration><br>

cd ~<br>
hdfs namenode -format<br>
start-dfs.sh<br>
start-yarn.sh<br>

# Port to access Hadoop
http://localhost:9870/<br>
http://localhost:8088/<br>

stop-yarn.sh<br>
stop-dfs.sh<br>


#### Word count problem in psuedo distributed mode<br>
ls $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar<br>
mkdir wordcountex <br>
####### add text files inside that folder or cp $HADOOP_HOME/*.txt wordcountex<br>
jar tf $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar | grep wordcount -i<br>
####### add files to hdfs <br>
hdfs dfs -mkdir -p /user/input/wcexample<br>
hdfs dfs -put wordcountex/*.txt /user/input/wcexample<br>

#### run wc from hdfs files and save into hdfs<br>
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.4.jar  wordcount /user/input/wcexample /user/output/ex2 <br>
hdfs dfs -cat /user/output/ex2/part-r-00000 | sort -k 2 -nr | head -n5<br>
<br>


# HIVE
cd course/softwares<br>
wget https://apache.osuosl.org/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz<br>
tar -xzf apache-hive-3.1.2-bin.tar.gz<br>
mv apache-hive-3.1.2-bin hive<br>



nano ~/.bashrc<br>
export HIVE_HOME=$HOME/Desktop/trialrun/softwares/hive<br>
export PATH=$PATH:$HIVE_HOME/sbin:$HIVE_HOME/bin<br>
export CLASSPATH=$CLASSPATH:$HADOOP_HOME/lib/*:$HIVE_HOME/lib/*<br>
source ~/.bashrc<br>

cd $HIVE_HOME/conf<br>
nano hive-env.sh<br>
export HADOOP_HOME=$HOME/Desktop/trialrun/softwares/hadoop-3.3.4<br>

#### Metastore - Apache Derby configuration<br>
cd course/softwares<br>
wget http://archive.apache.org/dist/db/derby/db-derby-10.4.2.0/db-derby-10.4.2.0-bin.tar.gz<br>
tar zxvf db-derby-10.4.2.0-bin.tar.gz<br>
mv db-derby-10.4.2.0-bin derby<br>

nano ~/.bashrc<br>
export DERBY_HOME=$HOME/Desktop/trialrun/softwares/derby<br>
export PATH=$PATH:$DERBY_HOME/bin<br>
export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar<br>
source ~/.bashrc<br>

mkdir $DERBY_HOME/data<br>

#### Configure Derby for Hive<br>
cd $HIVE_HOME/conf<br>
nano hive-site.xml<br>
<configuration>

   <property>
   	<name>javax.jdo.option.ConnectionURL</name>
   	<value>jdbc:derby:;databaseName=metastore_db;create=true</value>
    <description>JDBC connect string for a JDBC metastore </description>
   </property>
   
</configuration>

nano jpox.properties<br>

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
javax.jdo.option.ConnectionURL = jdbc:derby://hadoop1:1527/metastore_db;create = true
javax.jdo.option.ConnectionUserName = APP
javax.jdo.option.ConnectionPassword = mine

#### Setup Hive in HDFS<br>
$HADOOP_HOME/bin/hadoop fs -mkdir /tmp <br>
$HADOOP_HOME/bin/hadoop fs -mkdir /user/hive/warehouse<br>
$HADOOP_HOME/bin/hadoop fs -chmod g+w /tmp <br>
$HADOOP_HOME/bin/hadoop fs -chmod g+w /user/hive/warehouse<br>

cd $HIVE_HOME<br>
bin/schematool -initSchema -dbType derby<br>
bin/hive<br>


