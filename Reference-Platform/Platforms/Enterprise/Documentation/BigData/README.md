# Big Data

This file provides all the instructions required to install Big Data components - Hadoop, Spark and Hive

# Big Data packages

The following Big data components are built as part of Linaro's Reference Architecture

* Hadoop 2.7.2
* Spark 2.0
* Hive 2.0.1

These components were built using Apache BigTop 1.1 and uses ODPi's code base.


# About ODPi

Check out the website https://www.odpi.org/  

# Prerequisites

Java 8 (e.g. openjdk-8-jre) installed  

# Linaro Repo

The package is available at the following repo:

Debian Jessie - http://repo.linaro.org/debian/erp-16.12-stable/  
CentOS 7 - http://repo.linaro.org/rpm/linaro-overlay/centos-7/repo 

# Installation

## For Ubuntu

Add to repo source list (not required if you are using the installer from the Reference Platform): 
	
	$ echo "deb http://repo.linaro.org/debian/erp-16.12-stable/ jessie main" | sudo tee /etc/apt/sources.list.d/linaro-overlay-repo.list


Update the source list and install the dependencies: 

	$ sudo apt-get update
	$ sudo apt-get build-dep build-essential
 
 
Check Java version 
 
	java -version
 
This should print out OpenJDK8.
 
Install Hadoop, Spark and Hive
 
	$ sudo apt-get install -ft jessie bigtop-tomcat bigtop-utils hadoop* spark-core zookeeper ^hive-* hbase oozie
 
## For Centos:

Add to repo source list (not required if you are using the installer from the Reference Platform): 

	$ sudo wget http://repo.linaro.org/rpm/linaro-staging/centos-7/linaro-staging.repo -O /etc/yum.repos.d/linaro-overlay.repo
 
Update the source list and install the dependencies
 
	$ sudo yum update
	$ sudo yum -y install openssh-server openssh-clients java-1.8.0-openjdk*
 
 
Install Hadoop, Spark and Hive
 
	$ sudo yum install -y hadoop* spark* hive*
 
# Verifying Hadoop Installation

Packages would get installed in /usr/lib  

Type hadoop to check if hadoop is installed. 

	$ hadoop
 
And you should see the following:  
	
		"linaro@debian:~$ hadoop Usage: hadoop [--config confdir] COMMAND where COMMAND is one of:
		
		fs run a generic filesystem user client  version print the version jar <jar> run a jar file  checknative [-a|-h] check native hadoop and compression libraries availability  distcp <srcurl> <desturl> copy file or directories recursively  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive  classpath prints the class path needed to get the  credential interact with credential providers
		Hadoop jar and the required libraries
		daemonlog get/set the log level for each daemon  trace view and modify Hadoop tracing settings
		or CLASSNAME run the class named CLASSNAME 
		   
		Most commands print help when invoked w/o parameters. "

# Setup, Configuration and Running Hadoop

## Add Hadoop User

We need to create a dedicated user (hduser) for running Hadoop. This user needs to be added to hadoop user group: 
 
	$ sudo adduser hduser -G hadoop
 
give a password for hduser 
 
	$ sudo passwd hduser
 
Add hduser to sudoers list:  

On Debian: 
 
	$ sudo adduser hduser sudo
 
On CentOS: 
	
	$ sudo usermod -G wheel hduser

Switch to hduser 
 
	$ su - hduser
 
Generate ssh key for hduser

	$ ssh-keygen -t rsa -P ""
  
Press <enter> to leave to default file name.   
Enable ssh access to local machine 
 
	$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
	$ chmod 600 $HOME/.ssh/authorized_keys
	$ chmod 700 $HOME/.ssh 
 
Test ssh setup

	$ ssh localhost
	$ exit

## Disabling IPv6

	$ sudo vi /etc/sysctl.conf

add the below lines and save  

	net.ipv6.conf.all.disable_ipv6 = 1
	net.ipv6.conf.default.disable_ipv6 = 1
	net.ipv6.conf.lo.disable_ipv6 = 1
  
Prefer IPv4 on Hadoop:
 
	$ sudo vi /etc/hadoop/conf/hadoop-env.sh
  
uncomment line   
	
	export HADOOP_OPTS=-Djava.net.preferIPV4stack=true
 
Run sysctl to apply the changes: 
 
	$ sudo sysctl -p
 
## Configuring the app environment

Back to the system, we need to configure the app environment by following steps:   
 
	$ sudo mkdir -p /app/hadoop/tmp
	$ sudo chown hduser:hadoop /app/hadoop/tmp
	$ sudo chmod 750 /app/hadoop/tmp
	$ sudo chown hduser:hadoop /usr/lib/hadoop
	$ sudo chmod 750 /usr/lib/hadoop
 
## Setting up Environment Variables

Follow the below steps to setup Environment Variables in bash file :  
 
	$ vi .bashrc
 
Add the following to the end and save:  

	export HADOOP_HOME=/usr/lib/hadoop
	export HADOOP_PREFIX=$HADOOP_HOME
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib/native"
	export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec
	export HADOOP_CONF_DIR=/etc/hadoop/conf
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	export HADOOP_COMMON_HOME=$HADOOP_HOME
	export HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce
	export HADOOP_HDFS_HOME=/usr/lib/hadoop-hdfs
	export YARN_HOME=/usr/lib/hadoop-yarn
	export HADOOP_YARN_HOME=/usr/lib/hadoop-yarn/
	export HADOOP_USER_NAME=hdfs
	export CLASSPATH=$CLASSPATH:.
	export CLASSPATH=$CLASSPATH:$HADOOP_HOME/hadoop-common-2.7.2.jar:$HADOOP_HOME/client/hadoop-hdfs-2.7.2.jar:$HADOOP_HOME/hadoop-auth-2.7.2.jar:/usr/lib/hadoop-mapreduce/*:/usr/lib/hive/lib/*:/usr/lib/hadoop/lib/*:
	export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
	export PATH=/usr/lib/hadoop/libexec:/etc/hadoop/conf:$HADOOP_HOME/bin/:$PATH

	$ source .bashrc
 
## Modifying config files

### core-site.xml
 
	$ sudo vi /etc/hadoop/conf/core-site.xml
  
And add/modify the following settings: Look for property with fs.defaultFS and modify as below: 
 
	<property>
	  <name>fs.default.name</name>
	  <value>hdfs://localhost:54310</value>
	  <description>The name of the default file system. A URI whose
	  scheme and authority determine the FileSystem implementation. The
	  uri's scheme determines the config property (fs.SCHEME.impl) naming
	  the FileSystem implementation class. The uri's authority is used to
	  determine the host, port, etc. for a filesystem.
	  </description>
	</property>
 
Add this to the bottom before tag:  "</configuration>"  
 
	<property>
	  <name>hadoop.tmp.dir</name>
	  <value>/app/hadoop/tmp</value>
	  <description>A base for other temporary directories.</description>
	</property>
 
### mapred-site.xml

	$ sudo vi /etc/hadoop/conf/mapred-site.xml
 
Modify existing properties as follows: Look for property tag with as mapred.job.tracker and modify as below: 
 
	<property>
	  <name>mapred.job.tracker</name>
	  <value>localhost:54311</value>
	  <description>The host and port that the MapReduce job tracker runs
	  at. If "local", then jobs are run in-process as a single map
	  and reduce task.
	  </description>
	</property>
 
### hdfs-site.xml
 
	$ sudo vi /etc/hadoop/conf/hdfs-site.xml
 
Modify existing property as below: 
 
	<property>
	  <name>dfs.replication</name>
	  <value>1</value>
	  <description>Default block replication. The actual number of replications can be specified when the file is created. The default is used if replication is not specified in create time.
	  </description>
	</property>
 
  
Make sure the following properties are set correctly as below in hdfs-site.xml  
 
	<property>
	  <name>hadoop.tmp.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}</value>
	</property>
	 
	 
	<property>
	  <name>dfs.namenode.name.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}/dfs/name</value>
	</property>
	  
	<property>
	  <name>dfs.namenode.checkpoint.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}/dfs/namesecondary</value>
	</property>
	  
	<property>
	  <name>dfs.datanode.data.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}/dfs/data</value>
	</property>
 
  
Make sure the following properties are also present:
 
	<property>
	  <name>dfs.name.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}/dfs/nn</value>
	</property>
	  
	<property>
	  <name>dfs.data.dir</name>
	  <value>/var/lib/hadoop-hdfs/cache/${user.name}/dfs/dn</value>
	</property>
	 
	 
	<property>
	  <name>dfs.permissions.supergroup</name>
	  <value>hadoop</value>
	</property>
 
## Format Namenode

This step is needed for the first time. Doing it every time will result in loss of content on HDFS. 
 
	$ sudo /etc/init.d/hadoop-hdfs-namenode init
 
## Start the YARN daemons

	$ for i in hadoop-hdfs-namenode hadoop-hdfs-datanode ; do sudo service $i start ; done
  
	$ sudo /etc/init.d/hadoop-yarn-resourcemanager start
	$ sudo /etc/init.d/hadoop-yarn-nodemanager start
 
## Validating Hadoop

Check if hadoop is running. jps command should list namenode, datanode, yarn resource manager. or use ps aux  
 
	$ sudo jps  
 
or  
 
	$ ps aux | grep java
 
  
Alternatively, check if yarn managers are running: 
 
	$ sudo /etc/init.d/hadoop-yarn-resourcemanager status
	$ sudo /etc/init.d/hadoop-yarn-nodemanager status
 
You would see like below:  

	" ● hadoop-yarn-nodemanager.service - LSB: Hadoop nodemanager
	Loaded: loaded (/etc/init.d/hadoop-yarn-nodemanager)  Active: active (running) since Tue 2015-12-22 18:25:03 UTC; 1h 24min ago  CGroup: /system.slice/hadoop-yarn-nodemanager.service └─10366 /usr/lib/jvm/java-1.8.0-openjdk-arm64/bin/java -Dproc_node... 
	 
	 
	 
	Dec 22 18:24:57 debian su[10348]: Successful su for yarn by root  Dec 22 18:24:57 debian su[10348]: + ??? root:yarn  Dec 22 18:24:57 debian su[10348]: pam_unix(su:session): session opened for ...0)  Dec 22 18:24:57 debian hadoop-yarn-nodemanager[10305]: starting nodemanager, ...  Dec 22 18:24:58 debian su[10348]: pam_unix(su:session): session closed for ...rn  Dec 22 18:25:03 debian hadoop-yarn-nodemanager[10305]: Started Hadoop nodeman... "
 
## Run teragen, terasort and teravalidate

	$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar teragen 1000000 terainput
	$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar terasort terainput teraoutput
	$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar teravalidate -D mapred.reduce.tasks=8 teraoutput teravalidate

## Run a demo application to verify installation

	$ mkdir in
	$ cat > in/file << EOFThis is one line
	This is another one
	EOF

Add this directory to HDFS:  
 
	$ hadoop dfs -copyFromLocal in /in  
 
## Run wordcount example provided
 
	$ hadoop jar /usr/lib/hadoop-mapreduce/hadoop-mapreduce-examples.jar wordcount /in /out  
 
Check the output
	
	$ hadoop dfs -cat /out/*

## Web Interface 

* http://master:50070/dfshealth.jsp  
* http://master:8088/cluster  
* http://master:19888/jobhistory (for Job History Server)  

## Stop the Hadoop Services

	$ sudo /etc/init.d/hadoop-yarn-nodemanager stop
	$ sudo /etc/init.d/hadoop-yarn-resourcemanager stop
	$ for i in hadoop-hdfs-namenode hadoop-hdfs-datanode ; do sudo service $i stop; done
 
# SPARK

'NOTE:' Make sure you have followed above steps to set up Hadoop.  
Change to hduser  
 
	$ su - hduser
 
## Configuring Spark

	export HADOOP_HOME=/usr/lib/hadoop
	export HADOOP_PREFIX=$HADOOP_HOME
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib/native"
	export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec
	export HADOOP_CONF_DIR=/etc/hadoop/conf
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	export HADOOP_COMMON_HOME=$HADOOP_HOME
	export HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce
	export HADOOP_HDFS_HOME=/usr/lib/hadoop-hdfs
	export YARN_HOME=/usr/lib/hadoop-yarn
	export HADOOP_YARN_HOME=/usr/lib/hadoop-yarn/
	export HADOOP_USER_NAME=hdfs
	export CLASSPATH=$CLASSPATH:.
	export CLASSPATH=$CLASSPATH:$HADOOP_HOME/hadoop-common-2.7.2.jar:$HADOOP_HOME/client/hadoop-hdfs-2.7.2.jar:$HADOOP_HOME/hadoop-auth-2.7.2.jar:/usr/lib/hadoop-mapreduce/*:/usr/lib/hive/lib/*:/usr/lib/hadoop/lib/*:
	export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
	export PATH=/usr/lib/hadoop/libexec:/etc/hadoop/conf:$HADOOP_HOME/bin/:$PATH
	export SPARK_HOME=/usr/lib/spark
	export PATH=$HADOOP_HOME\bin:$PATH
	export SPARK_DIST_CLASSPATH=$HADOOP_HOME\bin\hadoop:$CLASSPATH:/usr/lib/hadoop/lib/*:/usr/lib/hadoop/lib/*:/usr/lib/hadoop-mapreduce/*:.
	export CLASSPATH=$CLASSPATH:/usr/lib/hadoop/lib/*:.

 
	$ source .bashrc
 
## Verifying Spark Installation
 
	$ $SPARK_HOME/bin/spark-shell --master local[*]
 
## Running SparkPi Example  

Once Spark is built successfully, try running the following pi example to calculate value of pi. The number at the end of the command is the number of splits. If needed, you can increase this number to stress out the CPU.  
 
		$ $SPARK_HOME/bin/run-example SparkPi 100
 
# HIVE

## Setting up environment for Hive 

You can set up the Hive environment by appending the following lines to ~/.bashrc file:  

	export HADOOP_HOME=/usr/lib/hadoop
	export HADOOP_PREFIX=$HADOOP_HOME
	export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib/native"
	export HADOOP_LIBEXEC_DIR=/usr/lib/hadoop/libexec
	export HADOOP_CONF_DIR=/etc/hadoop/conf
	export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
	export HADOOP_COMMON_HOME=$HADOOP_HOME
	export HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce
	export HADOOP_HDFS_HOME=/usr/lib/hadoop-hdfs
	export YARN_HOME=/usr/lib/hadoop-yarn
	export HADOOP_YARN_HOME=/usr/lib/hadoop-yarn/
	export HADOOP_USER_NAME=hdfs
	export CLASSPATH=$CLASSPATH:.
	export CLASSPATH=$CLASSPATH:$HADOOP_HOME/hadoop-common-2.7.2.jar:$HADOOP_HOME/client/hadoop-hdfs-2.7.2.jar:$HADOOP_HOME/hadoop-auth-2.7.2.jar:/usr/lib/hive/lib/*:/usr/lib/hadoop/lib/*:.
	export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
	export PATH=/usr/lib/hadoop/libexec:/etc/hadoop/conf:$HADOOP_HOME/bin/:$PATH
	export PATH=$HADOOP_HOME\bin:$PATH
	export HIVE_HOME=/usr/lib/hive
	export PATH=$PATH:$HIVE_HOME/bin
 
The following command is used to execute ~/.bashrc file.  
 
	$ source ~/.bashrc
 
## Configuring hive

To configure Hive with Hadoop, you need to edit the hive-env.sh file, which is placed in the $HIVE_HOME/conf directory. The following commands redirect to Hive config folder and copy the template file:  
 
	$ cd $HIVE_HOME/conf
	$ sudo cp hive-env.sh.template hive-env.sh   
 
Hive installation is completed successfully. Now you require an external database server to configure Metastore. We use Apache Derby database.  

## Downloading and Installing Apache Derby

Follow the steps given below to download and install Apache Derby:  

## Downloading Apache Derby

The following command is used to download Apache Derby. It takes some time to download.  
 
	$ cd ~
	$ wget http://archive.apache.org/dist/db/derby/db-derby-10.4.2.0/db-derby-10.4.2.0-bin.tar.gz
 
 
The following command is used to verify the download:  
 
	$ ls
  
On successful download, you get to see the following response:  

	db-derby-10.4.2.0-bin.tar.gz

## Extracting and verifying Derby archive

The following commands are used for extracting and verifying the Derby archive:  
 
	$ tar zxvf db-derby-10.4.2.0-bin.tar.gz
	$ ls
 
  
On successful download, you get to see the following response:  

	db-derby-10.4.2.0-bin
	db-derby-10.4.2.0-bin.tar.gz
  
Copy the files from the extracted directory to the /usr/local/derby directory:  
 
	$ sudo mv db-derby-10.4.2.0-bin /usr/local/derby
 
## Setting up Environment for Derby

Set up the Derby environment by appending the following lines to ~/.bashrc file:  
 
	$ vi .bashrc

	export DERBY_HOME=/usr/local/derby
	export PATH=$PATH:$DERBY_HOME/bin
	export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar

The following command is used to execute ~/.bashrc file:  
 
	export DERBY_HOME=/usr/local/derby
	export PATH=$PATH:$DERBY_HOME/bin
	export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar
	
	$ source ~/.bashrc
 
### Create a directory to store Metastore

Create a directory named data in $DERBY_HOME directory to store Metastore data.  
 
	$ sudo mkdir $DERBY_HOME/data
 
Derby installation and environmental setup is now complete.  

## Configuring Metastore of Hive  

Configuring Metastore means specifying to Hive where the database is stored. You can do this by editing the hive-site.xml file, which is in the $HIVE_HOME/conf directory. First of all, copy the template file using the following command:  
 
	$ cd $HIVE_HOME/conf
	$ sudo cp hive-default.xml.template hive-site.xml
 
Edit hive-site.xml and find entry 'javax.jdo.option.ConnectionURL' and modifiy the value as below: 
 
	<name>hive.exec.scratchdir</name>
	<value>/tmp/hive-${user.name}</value>
	  
	<name>hive.exec.local.scratchdir</name>
	<value>/tmp/${user.name}</value>
	  
	<name>hive.downloaded.resources.dir</name>
	<value>/tmp/${user.name}_resources</value>
	  
	<name>hive.scratch.dir.permission</name>
	<value>733</value>
 
and change the values for the below properties like below:  

	<property>
	   <name>javax.jdo.option.ConnectionURL</name>
	   <value>jdbc:derby:;databaseName=/usr/lib/hive/tmp/metastore_db;create=true </value>
	   <description>JDBC connect string for a JDBC metastore </description>
	</property>

Create a file named jpox.properties and add the following lines into it:  
 
	$ sudo vi jpox.properties


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

## Verifying Hive Installation

Before running Hive, you need to create the /tmp folder and a separate Hive folder in HDFS. Here, we use the /user/hive/warehouse folder. You need to set write permission for these newly created folders as shown below:   

Make sure you are using hduser account. If not switch to hduser.  

	$ su - hduser

Now set them in HDFS before verifying Hive. Use the following commands:  

	$ $HADOOP_HOME/bin/hadoop fs -mkdir /tmp
	$ $HADOOP_HOME/bin/hadoop fs -mkdir -p /user/hive/warehouse
	$ $HADOOP_HOME/bin/hadoop fs -chmod g+w /tmp
	$ $HADOOP_HOME/bin/hadoop fs -chmod g+w /user/hive/warehouse

'NOTE:' Before invoking hive make sure you have followed above instructions in installing and setting up Hadoop. Make sure hadoop services are running. 
 
Run Hive metastore service  
 
	$ sudo service hive-metastore start
	$ sudo $HIVE_HOME/bin/metatool -listFSRoot
 
Create tmp directory to run Hive under.  
 
	$ cd $HIVE_HOME
	$ sudo mkdir tmp
	$ sudo chown hduser tmp
	$ cd tmp
 
The following commands are used to verify Hive installation:  
 
	$ $HIVE_HOME/bin/schematool -dbType derby -initSchema
	$ hive -hiveconf hive.root.logger=DEBUG,console
 
On successful installation of Hive, you get to see the following response:  

	Logging initialized using configuration in jar:file:/home/hadoop/hive-0.9.0/lib/hive-common-0.9.0.jar!/hive-log4j.properties
	Hive history file=/tmp/hadoop/hive_job_log_hadoop_201312121621_1494929084.txt
	………………….
	hive>

The following sample command is executed to display all the tables:  

	hive> show tables;
	OK
	Time taken: 2.798 seconds
	hive>    


# Errors / Issues and Resolutions

* If after creating hduser, trying to switch to hduser ( || $ su - hduser || ) gave the following error:  

/* ‘No directory, logging in with HOME=/ #  

Then do the following:  
Exit to root user  delete the hduser and recreate it. 
 
	$ exit
	$ sudo userdel hduser
	$ sudo useradd -d /home/hduser -G hadoop -m hduser
 
* If Teragen, TeraSort and TeraValidate error out with 'permission denied' exception. The following steps can be done: 
 
	$ sudo groupadd supergroup
	$ sudo usermod -g supergroup hduser
 
* If for some weird reason, if you notice the config files (core-site.xml, hdfs-site.xml, etc) are empty.  

You may have delete all the packages and re-run the steps of installation from scratch.  

	/* Error while formatting namenode With the following command:  
 
	$ sudo /etc/init.d/hadoop-hdfs-namenode init
   
* If you see the following error: WARN net.DNS: Unable to determine local hostname -falling back to "localhost" java.net.UnknownHostException: centos: centos at java.net.InetAddress.getLocalHost(InetAddress.java:1496) at org.apache.hadoop.net.DNS.resolveLocalHostname(DNS.java:264) at org.apache.hadoop.net.DNS.(DNS.java:57)  

Something is wrong in the network setup. Please check /etc/hosts file.  
 
	$ sudo vi /etc/hosts
 
The hosts file should like below:  
 
	127.0.0.1 <hostname> localhost localhost.localdomain #hostname should have the output of $ hostname
	 
	::1 localhost
 
Also try the following steps:  
 
	$ sudo rm -Rf /app/hadoop/tmp
	$ hadoop namenode -format
   
* If you see the below error with Hive while doing 'schematool -initSchema -dbType derby':  

	'Error:' FUNCTION 'NUCLEUS_ASCII' already exists. (state=X0Y68,code=30000)  org.apache.hadoop.hive.metastore.HiveMetaException: Schema initialization FAILED! Metastore state would be inconsistent !!  Underlying cause: java.io.IOException : Schema script failed, errorcode 2  Use --verbose for detailed stacktrace.  *** schemaTool failed ***  

Following actions need to be taken to resolve:  
 
	$ cd $HIVE_HOME/tmp
	mv metastore_db metastore_db.tmp
	../bin/schematool -initSchema -dbType derby
 
   
* If you get the following error with Hive:  

	Error:  Cannot get a connection, pool error Could not create a validated object, cause: A read-only user or a user in a read-only database is not permitted to disable read-only mode on a connection.  org.datanucleus.exceptions.NucleusDataStoreException: Cannot get a connection, pool error Could not create a validated object, cause: A read-only user or a user in a read-only database is not permitted to disable read-only mode on a connection.
 
Resolution is: delete all .lck files in $HIVE_HOME/tmp directory
