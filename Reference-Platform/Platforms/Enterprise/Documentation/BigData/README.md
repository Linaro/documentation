# Big Data

This file provides all the instructions required to Build, Install, Configure and Test following Big Data components. 
The following components were tested (smoke tests) as part of the ERP release as well.

* Hadoop - HDFS
* Hadoop - YARN
* Hadoop - MapReduce
* Zookeeper
* Spark
* Hive
* HBase
* Ambari
* ElasticStack - ElasticSearch
* ElasticStack - Logstash
* ElasticStack - Kibana

We have used upstream of Apache Bigtop and upgraded the versions of the following components:

Component	Version in Bigtop	Version in upstream	Version in ERP18.06
Hadoop		2.8.1			2.8.4/3.0.2		2.8.4
Spark		2.2.1			2.3.0			2.2.1
HBase		1.3.1			1.3.2/1.4.4/2.0.0	1.3.2
Hive		2.3.2			2.3.3			2.3.3
Ambari		2.5.2			2.6.2			2.6.1
Zookeeper	3.4.6			3.4.12/3.5.4		3.4.6
Elasticsearch	X			5.6.9/6.2.3		5.6.9
Logstash	X			5.6.9/6.2.4		5.6.9
Kibana		X			5.6.9/6.2.5		5.6.9

# About BigTop
Bigtop is a set of tools and framework for comprehensive packaging, testing, and configuration of the open source big data components including, but not limited to, Hadoop, HBase and Spark. Check out the website http://bigtop.apache.org/

# Sources
* Upstream: https://git.linaro.org/leg/bigdata/bigtop-trunk.git/ (The branch is erp18.06)
* Root upstream: https://github.com/apache/bigtop 

# Repo
[TODO: update this] Docker: http://repo.linaro.org/debian/erp-18.06-stable/pool/main/d/docker.io/

# How to Use the repo
[TODO: Document how to use the repo steps here....]

# Build steps
## Setup Environment
* Debian 18.06 ERP release

[TODO: update this]>	$ echo "deb http://repo.linaro.org/debian/erp-18.06-stable/ jessie main" | sudo tee /etc/apt/sources.list.d/linaro-overlay-repo.list
>	
>	$ apt-get update

## Prerequisites

* Install distro packages
	$ sudo apt-get install unzip openjdk-8-jdk git autoconf curl iputils-ping net-tools build-essential ruby

* Set JAVA_HOME
	$ export JAVA_HOME=`readlink -f /usr/bin/java | sed "s:jre/bin/java::"`

* Install docker CE
	$ sudo apt-get install apt-transport-https ca-certificates gnupg2 software-properties-common
## download docker's gpg key
	$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
 
## verify key
-- the output should be:
-- pub   rsa4096 2017-02-22 [SCEA]
--      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
-- uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
-- sub   rsa4096 2017-02-22 [S]
	$ sudo apt-key fingerprint 0EBFCD88
 
## import docker's repo
	$ echo "deb [arch=arm64] https://download.docker.com/linux/debian \
  		$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
 
## now update sources
	$ sudo apt update
 
## now install docker-ce and docker-compose
	$ sudo apt install -y docker-ce docker-compose

## Clone source 

	$ git clone https://git.linaro.org/leg/bigdata/bigtop-trunk.git -b erp18.06
	
## Build source
### Build Bigtop dev images
#### Bigtop-puppet
	$ cd <BIGTOP_SRC_TOP>
	$ ./gradlew -POS=debian-9 -Pprefix=erp18.06 bigtop-puppet
#### Bigtop-slaves
	$ cd <BIGTOP_SRC_TOP>
	$ ./gradlew -POS=debian-9 -Pprefix=erp18.06 bigtop-slaves

### Build Components
-- components to be built: ambari bigtop-groovy bigtop-jsvc bigtop-tomcat bigtop-utils hadoop hbase hive spark zookeeper
-- docker run -v `pwd`:/ws bigtop/slaves:erp18.06-debian-9-aarch64 bash -l -c 'cd /ws ; ./gradlew <comp>-deb'
	$ docker run -v `pwd`:/ws bigtop/slaves:erp18.06-debian-9-aarch64 bash -l -c 'cd /ws ; ./gradlew hadoop-deb zookeeper-deb spark-deb hive-deb hbase-deb ambari-deb'
	
### Smoke test dependent packages

To execute smoke tests for target packages, extra big data components are required to install. So these packages need to be built if you want to run bigtop smoke test.
These packages are: flume, mahout, pig, sqoop, zookeeper. If all components were already built in earlier step, skip this step.

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew flume-deb mahout-deb pig-deb sqoop-deb zookeeper-deb'
	
## Smoke Tests
### Generate local repo
	$ ./gradlew apt

### Create test configuration yaml file
There are several sample smoke test yaml files in <BIGTOP_ROOT>/provisioner/docker/, you may use one as the start point. 

Following is a example configuration yaml for Hive:

	# Licensed to the Apache Software Foundation (ASF) under one or more
	# contributor license agreements.  See the NOTICE file distributed with
	# this work for additional information regarding copyright ownership.
	# The ASF licenses this file to You under the Apache License, Version 2.0
	# (the "License"); you may not use this file except in compliance with
	# the License.  You may obtain a copy of the License at
	#
	#     http://www.apache.org/licenses/LICENSE-2.0
	#
	# Unless required by applicable law or agreed to in writing, software
	# distributed under the License is distributed on an "AS IS" BASIS,
	# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
	# See the License for the specific language governing permissions and
	# limitations under the License.
	
	docker:
        	memory_limit: "8g"
        	image: "bigtop/puppet:erp18.06-debian-9-aarch64"
 
	repo: "file:///bigtop-home/output/apt"
	distro: debian
	components: [hdfs, yarn, mapreduce, hive, zookeeper]
	enable_local_repo: false
	smoke_test_components: [hive]
	jdk: "openjdk-8-jdk"

### Execute test
It is quite straight forward to execute smoke in bigtop

	$ cd provisioner/docker
	# $ ./docker-hadoop.sh -C <smoke_test_cfg_yaml> -c <node_count> -s -d
	# For detail usage of docker-hadoop.sh, you may just issue "./docker-hadoop.sh", it will show help messages

	$ ./docker-hadoop.sh -C smoke.yaml -c 3 -s -d

Before running smoke test, it is suggested to run environment check first

	$ ./docker-hadoop.sh -E
	
For detailed usage, just issue: ./docker-hadoop.sh

	usage: docker-hadoop.sh [-C file ] args
      		-C file                                   Use alternate file for config.yaml
  	commands:
      		-c NUM_INSTANCES, --create NUM_INSTANCES  Create a Docker based Bigtop Hadoop cluster
      		-d, --destroy                             Destroy the cluster
      		-e, --exec INSTANCE_NO|INSTANCE_NAME      Execute command on a specific instance. Instance can be specified by name or number.
		                                           For example: docker-hadoop.sh --exec 1 bash
                                                              docker-hadoop.sh --exec docker_bigtop_1 bash
      		-E, --env-check                           Check whether required tools has been installed
      		-l, --list                                List out container status for the cluster
      		-p, --provision                           Deploy configuration changes
      		-s, --smoke-tests                         Run Bigtop smoke tests
      		-h, --help

#### Spark smoke test

To run the Spark smoke tests you need to apply 2 patches https://issues.apache.org/jira/browse/BIGTOP-2860  and https://issues.apache.org/jira/browse/BIGTOP-2866

	$ vim provisioner/docker/config_debian-8-aarch64.yaml
 	
Do the below changes
 	
	components: [hdfs, yarn, mapreduce, hive, spark]
	enable_local_repo: true
	smoke_test_components: [spark]
	jdk: "openjdk-8-jdk"
 	
save and quit.
 	
	$ vim provisioner/utils/smoke-tests.sh
 	
Add the below lines
 	
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
	 
	export SPARK_HOME=/usr/lib/spark
	export SPARK_MASTER_IP=<IP Address of your host>
	export SPARK_MASTER_PORT=7077
	 
save and quit.
 	
Run the command smoke test commands
 	
	$ ./docker-hadoop.sh -C config_debian-8-aarch64.yaml -c 3 -s -d
 	
If the smoke test is still failing.  It might be because of you have't set the "SPARK_MASTER_IP" properly.  Another way to do is that remove the $masterMode and add local[*] as below
 
	$ vim bigtop-tests/smoke-tests/spark/TestSpark.groovy
 
make the below change
 
	final String SPARK_SHELL = SPARK_HOME + "/bin/spark-shell --master local[*]"
 	
save and quit. Now run the spark smoke-tests again.


Generally following properties should be customized for certain smoke test:
* docker image
* repo url
* distro type
* components to install
* components to test


### List of Smoke Tests Verified:

- Hadoop-HDFS		
- TestBlockRecovery	
- TestChgrp		
- TestCmdTest		
- TestCmdText		
- TestCount		
- TestCp		
- TestDFSAdmin		
- TestDFSCLI		
- TestDistCpIntra	
- TestDu		
- TestFileAppend	
- TestFsck		
- TestGet		
- TestHDFSBalancer	
- TestHDFSCLI		
- TestHDFSQuota		
- TestLs		
- TestMkdir		
- TestMv		
- TestPut		
- TestStat		
- TestTextSnappy	

### Hadoop-Yarn
- TestNode		
- TestRmAdmin		
- TestRmAdminRefreshCommands

### Hadoop-MapReduce
- TestHadoopExamples	
- TestMRExample		

### Spark
- TestSparkExample
- TestSparkPythonExample
- ShellTest
- HDFSTest 
- JobTest 

### Hive
- TesttHiveSmoke

# ERP 18.06 ElasticStack / ELK (ElasticSearch, LogStash and Kibana) on Aarch64

## Sources
* Upstream: https://github.com/elastic/elasticsearch.git/ 
                  https://github.com/elastic/logstash.git 
                  https://github.com/elastic/kibana.git
* We are using upstream version (v5.6.9) of ElasticStack for ERP18.06 

### Install Pre-Requisites
	$ apt-get install -y openjdk-8-jdk curl unzip wget apt-transport-https ca-certificates wget software-properties-common

### Add repo
	$ sudo echo 'deb [arch=amd64] https://artifacts.elastic.co/packages/5.x/apt stable main' >> /etc/apt/sources.list.d/elk.list
	$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	$ apt-get update

### Install the components
	$ sudo apt install elasticsearch logstash kibana

### Start the services
	$ sudo service elasticsearch start
	$ sudo service logstash start
	$ sudo service kibana start
	
### Check service status
	$ sudo service elasticsearch status

## Build Steps

Following scripts are expected to run in a debian-8 container.

#### Building ElasticSearch

##### install prerequisites
	
	$ echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/backports.list
	$ sudo apt-get update -y
	$ sudo apt-get install -y -t jessie-backports openjdk-8-jdk git build-essential automake autoconf libtool curl unzip rpm texinfo locales-all tar wget python-requests
 	
	$ wget https://services.gradle.org/distributions/gradle-3.5.1-bin.zip -O /tmp/gradle-3.5.1-bin.zip
	$ cd ${HOME} && unzip /tmp/gradle-3.5.1-bin.zip && rm /tmp/gradle-3.5.1-bin.zip
	$ ln -s gradle-3.5.1 gradle
 	
##### setup environments

	$ export LANG="en_US.UTF-8"
	$ export PATH=${HOME}/gradle/bin:$PATH
	$ export JAVA_TOOL_OPTIONS="-Dfile.encoding=UTF8"
 
##### setup JAVA_HOME

	$ cd /usr/lib/jvm/java-8-openjdk-*
	$ export JAVA_HOME=${PWD}
 
##### clone the ElasticSearch v5.4.1

	$ git clone --depth 1 https://git.linaro.org/leg/bigdata/elasticsearch.git -b v5.4.1 ${WORKSPACE}/elasticsearch
	$ cd ${WORKSPACE}/elasticsearch
 
okay everything is in place

	$ gradle assemble -Dbuild.snapshot=false


#### Building Logstash

##### install prerequisites

	$ echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/backports.list
	$ sudo apt-get update -y
	$ sudo apt-get install -y -t jessie-backports openjdk-8-jdk git build-essential rubygems texinfo locales-all automake autoconf libtool wget unzip curl maven ant tar python-requests
 
	$ sudo gem install rake
	$ sudo gem install bundler
 	
##### setup environments

	$ export RELEASE=1
	$ export LANG="en_US.UTF-8"
	$ export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64
 	
##### clone the Logstash definitions

	$ git clone --depth 1 https://git.linaro.org/leg/bigdata/logstash.git -b v5.4.1 ${WORKSPACE}/logstash
	$ cd ${WORKSPACE}/logstash
 
##### okay everything is in place

	$ rake bootstrap
	$ rake plugin:install-default
 	
	$ rake artifact:deb
	
#### Building Kibana

##### setup environments

	$ export LANG="en_US.UTF-8"
 
##### install prerequisites

	$ sudo apt-get update -y
	$ sudo apt-get install -y git build-essential automake autoconf libtool libffi-dev ruby-dev rubygems python2.7 curl zip rpm python-requests
 	
	$ sudo gem install fpm -v 1.5.0
	$ sudo gem install pleaserun -v 0.0.24
	$ sudo ln -s /usr/bin/python2.7 /usr/bin/python
 	
	$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
	$ source ${HOME}/.bashrc
 	
##### clone the Kibana definitions

	$ git clone --depth 1 https://git.linaro.org/leg/bigdata/kibana.git -b v5.4.1 ${WORKSPACE}/kibana
 	
	$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
	$ source ${HOME}/.profile
          	   
Install the version of node.js listed in the .node-version file (this can be easily automated with tools such as nvm and avn)

	$ nvm install $(cat ${WORKSPACE}/kibana/.node-version)

	$ cd ${WORKSPACE}/kibana
             
Install npm dependencies

	$ npm install
	$ npm rebuild node-sass
          	   
	$ npm run build -- --deb --release
 	
 	$ mkdir -p out
	$ cp -a ${WORKSPACE}/kibana/target/kibana-*-arm64.deb* out/
	
