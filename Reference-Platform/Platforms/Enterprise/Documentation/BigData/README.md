# Big Data

This file provides all the instructions required to Build, Install, Configure and Test following Big Data components
	Hadoop - YARN, Hadoop - HDFS, Hadoop - MapReduce, Hive, Spark, HBase and ELK Stack (ElasticSearch, LogStash and Kibana)

The following Big data components are built as part of Linaro's Reference Architecture - ERP-17.08.
These components were built using Apache BigTop v1.2.

* alluxio		v1.0.1
* Apache ambari		v2.5.0
* Apache apex		v3.5.0
* groovy		v2.4.10
* Apache commons - jsvc	v1.0.15
* Apache tomcat		v6.0.45
* bigtop_utils		v1.2.0
* Apache crunch		v0.14.0
* Pig UDF datafu	v1.3.0
* Apache flink		v1.1.3
* Apache flume		v1.7.0
* Apache giraph		v1.1.0
* Greenplum gpdb	v5.0.0-alpha.0
* Apache hadoop		v2.7.3
* Apache hama		v0.7.0
* Apache hbase		v1.1.3
* Apache hive		v1.2.1
* Apache hue		v3.11.0
* Apache ignite		v1.9.0
* Apache kafka		v0.10.1.1
* kite			v1.1.0
* Apache mahout		v0.12.2
* Apache oozie		v4.3.0
* Apache phoenix	v4.9.0-HBase-1.1
* Apache pig		v0.15.0
* Quantcast qfs		v1.1.4
* Apache solr		v4.10.4
* Apache spark 1.1 	v1.6.2
* Apache spark 2.0	v2.1.1
* Apache sqoop v1	v1.4.6
* Apache sqoop v2	v1.99.4
* Apache tajo		v0.11.1
* Apache tez		v0.6.2
* ycsb			v0.4.0
* Apache zeppelin	v0.7.0
* Apache zookeeper	v3.4.6
* ELK 			v5.4.1

The following components were tested (smoke tests) as part of the ERP release as well.

* Hadoop - HDFS
* Hadoop - YARN
* Hadoop - MapReduce
* Hive
* Spark

### Note
It has been decided to skip HBase smoke tests for ERP17.08. For the reason that it needs clean up on the Bigtop upstream

# About BigTop
Bigtop is a set of tools and framework for comprehensive packaging, testing, and configuration of the open source big data components including, but not limited to, Hadoop, HBase and Spark. Bigtop support many Operating Systems, including Debian, Ubuntu, CentOS, Fedora, openSUSE and many others. Check out the website http://bigtop.apache.org/

# Sources
* Upstream: https://git.linaro.org/leg/bigdata/bigtop-trunk.git/ (The branch is erp17.08)
* Root upstream: https://github.com/apache/bigtop (Release-1.2.0 is used in this Wiki, with specific patches for aarch64)

# Repo
Docker: http://repo.linaro.org/debian/erp-17.08-stable/pool/main/d/docker.io/

# Setup Environment
* Debian 17.08 ERP release

>	$ echo "deb http://repo.linaro.org/debian/erp-17.08-stable/ jessie main" | sudo tee /etc/apt/sources.list.d/linaro-overlay-repo.list
>	
>	$ apt-get update

# Prerequisites

* JDK-8
OpenJDK-8 is provided in jessie-backports repo. Issue following cmd to install jessie-backports repo.

	$ echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/jessie-backports.list

	$ apt-get update

Then install openjdk-8

	$ apt-get install -y -t jessie-backports openjdk-8-jdk

* Python pip
	
	$ apt-get install -y python-pip
    
* Ruby

	$ apt-get install -y ruby

* Docker
Bigtop uses docker based images as build environment for its components.

	$ apt-get install -y docker.io

* Docker-compose
Bigtop uses docker-compose to create clusters, deploy packages and run smoke tests.

	$ pip install docker-compose

Check docker-compose installation

	$ docker-compose -v

	#docker-compose version 1.15.0, build e12f3b9

# Dependencies

# Build Steps:
## Clone source 

	$ git clone --depth 1 https://git.linaro.org/leg/bigdata/bigtop-trunk.git/ -b erp17.08
	$ cd bigtop-trunk
	
## Prepare docker images

Currently, Bigtop community has docker image only for ubuntu-aarch in docker hub. Hence need to create a new image for debian.
To build and test bigtop packages, two docker images should be ready: bigtop/puppet:erp17.08-debian-8-aarch64 and bigtop/slaves:erp17.08-deb-8-aarch64

* bigtop/puppet:erp17.08-debian-8-aarch64

	$ cd docker/bigtop-puppet/debian-8-aarch64
	$ ./build.sh

* bigtop/slaves:erp17.08-debian-8-aarch64
	
	$ cd <BIGTOP_SRC_TOP>
	$ docker build -t bigtop/slaves:erp17.08-deb-8-aarch64 -f docker/bigtop-slaves/debian-8-aarch64/Dockerfile .
	
Verify if the docker image is created

	$ docker images

That should list erp17.08-deb-8-aarch64 image

## Build packages
### Basic packages

Basic packages are required for all big data components. They are: bigtop-groovy, bigtop-utils, bigtop-jsvc and bigtop-tomcat
To build these basic packages, issue:

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew bigtop-groovy-deb bigtop-utils-deb bigtop-jsvc-deb bigtop-tomcat-deb'

### Target packages

To build specific target packages (hadoop, hbase, spark and hive), issue:

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew hadoop-deb hbase-deb spark-deb hive-deb'
	
To build all components part of Bigtop, issue:

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew deb'

### Smoke test dependent packages

To execute smoke tests for target packages, extra big data components are required to install. So these packages need to be built if you want to run bigtop smoke test.
These packages are: flume, mahout, pig, sqoop, zookeeper. If all components were already built in earlier step, skip this step.

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew flume-deb mahout-deb pig-deb sqoop-deb zookeeper-deb'
	
## Smoke Tests
### Setup repo

Bigtop release repo doesn't have arm64 support. To execute smoke test it is required to setup local repo with packages we build out.

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew apt'
	
### Create test configuration
Bigtop smoke test uses yaml to configure test repo/environment/components. A typical configuration yaml can be found at <BIGTOP_SRC_TOP>/provisioner/docker/erp17.08-deb-8-aarch64

Generally following properties should be customized for certain smoke test:
* docker image
* repo url
* distro type
* components to install
* components to test

Following is configuration yaml for Hive:

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
        	memory_limit: "4g"
        	image:  "bigtop/puppet:debian-8-aarch64"
 
	repo: "http://bigtop-repos.s3.amazonaws.com/releases/1.2.0/debian/8-aarch64/aarch64"
	distro: debian
	components: [hdfs, yarn, mapreduce, hive]
	enable_local_repo: true
	smoke_test_components: [hive]
	jdk: "openjdk-8-jdk"

### Execute test
It is quite straight forward to execute smoke in bigtop

	$ cd provisioner/docker
	$ ./docker-hadoop.sh -C <smoke_test_cfg_yaml> -c <node_count> -s -d

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

# ERP 17.08 Building ELK (ElasticSearch, LogStash and Kibana) on Aarch64

## Build

### Sources
* Upstream: https://git.linaro.org/leg/bigdata/elasticsearch.git/ 
                  https://git.linaro.org/leg/bigdata/logstash.git 
                  https://git.linaro.org/leg/bigdata/kibana.git
* The ELK stack version for erp17.08 is v5.4.1

### Environment
Docker: Debian 8

### Pre-Requisites
Docker, git

### Dependencies
#### Install Prerequisites

	# install docker engine
	$ apt-get install -y docker.io git

### Build Steps

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
	
