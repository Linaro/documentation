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

-----------------------------------------------------------------------------------------------------------------
| Component 	|	Version in Bigtop 	|	Version in upstream 	|	Version in ERP18.06 	|
|---------------|-------------------------------|-------------------------------|-------------------------------|
| Hadoop    	|	2.8.1 			|	2.8.4/3.0.2		|	2.8.4			|
| Spark     	|	2.2.1 			|	2.3.0			|	2.2.1			|
| HBase     	|	1.3.1 			|	1.3.2/1.4.4/2.0.0	|	1.3.2			|
| Hive      	|	2.3.2 			|	2.3.3			|	2.3.3			|
| Ambari    	|	2.5.2 			|	2.6.2			|	2.6.1			|
| Zookeeper 	|	3.4.6			|	3.4.12/3.5.4		|	3.4.6			|
| Elasticsearch |	X			|	5.6.9/6.2.3		|	5.6.9			|
| Logstash 	|	X			|	5.6.9/6.2.4		|	5.6.9			|
| Kibana 	|	X			|	5.6.9/6.2.5		|	5.6.9			|
-----------------------------------------------------------------------------------------------------------------

# About BigTop
Bigtop is a set of tools and framework for comprehensive packaging, testing, and configuration of the open source big data components including, but not limited to, Hadoop, HBase and Spark. Check out the website https://bigtop.apache.org/

# Sources
* Upstream: https://git.linaro.org/leg/bigdata/bigtop-trunk.git/ (The branch is erp18.06)
* Root upstream: https://github.com/apache/bigtop 

# Repo
[TODO: update this] Docker: https://repo.linaro.org/debian/erp-18.06-stable/pool/main/d/docker.io/

# How to Use the repo
[TODO: Document how to use the repo steps here....]

# Build steps
## Setup Environment
* Debian 18.06 ERP release

[TODO: update this]>	$ echo "deb https://repo.linaro.org/debian/erp-18.06-stable/ jessie main" | sudo tee /etc/apt/sources.list.d/linaro-overlay-repo.list
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
These packages are: bigtop-groovy, bigtop-jsvc, bigtop-tomcat, bigtop-utils. If all components were already built in earlier step, skip this step.

	$ docker run -v `pwd`:/ws bigtop/slaves:erp17.08-deb-8-aarch64 bash -l -c 'cd /ws ; ./gradlew bigtop-groovy-deb bigtop-jsvc-deb bigtop-tomcat-deb bigtop-utils-deb'
	
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
	#     https://www.apache.org/licenses/LICENSE-2.0
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


### Setup environment

You need to export several environment variables.   For example,

export HADOOP_CONF_DIR=/etc/hadoop/conf/
export HADOOP_MAPRED_HOME=/usr/lib/hadoop-mapreduce/
export HIVE_HOME=/usr/lib/hive/
export PIG_HOME=/usr/lib/pig/
export FLUME_HOME=/usr/lib/flume/
export HIVE_CONF_DIR=/etc/hive/conf/
export JAVA_HOME="/usr/lib/jvm/java-openjdk/"
export MAHOUT_HOME="/usr/lib/mahout"
export SPARK_HOME="/usr/lib/spark"
export TAJO_HOME="/usr/lib/tajo"
export OOZIE_TAR_HOME="/usr/lib/oozie/doc"

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


Generally following properties should be customized for certain smoke test:
* docker image
* repo url
* distro type
* components to install
* components to test


### List of Smoke Tests Verified:

#### Hadoop-HDFS		
- TestBlockRecovery
- TestDistCpIntra
- TestFileAppend
- TestFsck
- TestHDFSQuota
- TestHDFSCLI
- TestTextSnappy
- TestDFSAdmin
- TestHDFSBalancer

#### Hadoop-MapReduce
- TestHadoopExamples		

#### Spark
- testSparkSQL

#### Hive
- TestHiveSmoke
- TestHiveSmokeBulk

#### HBase
- TestHBaseBalancer
- TestHBaseCompression
- TestHBaseImportExport
- TestHBasePigSmoke
- TestHbck
- TestImportTsv
- IncrementalPELoad
- TestCopyTable
- TestHBaseSmoke
- TestHFileOutputFormat
- TestLoadIncrementalHFiles

#### Zookeeper
- testZkServerStatus

#### Ambari
- TestAmbariSmoke
- testStackNameVersion
- testBlueprints
- testHosts

# ERP 18.06 Installing ElasticStack / ELK (ElasticSearch, LogStash and Kibana) on Aarch64
We are using ElasticStack upstream repo.

## Sources
* Upstream: https://github.com/elastic/elasticsearch.git/ 
                  https://github.com/elastic/logstash.git 
                  https://github.com/elastic/kibana.git
* We are using upstream version (v5.6.9) of ElasticStack for ERP18.06 

## Add repo

	$ deb [arch=amd64] https://artifacts.elastic.co/packages/5.x/apt stable main (add those two lines into /etc/apt/sources.list.d/elk.list)
	$ sudo apt-get update -y

This gives ELK 5.x with elasticsearch and logstash from upstream and kibana from ERP.
"[arch=amd64]" is important - this tells APT to fetch list of packages for amd64 architecture and then it allows us to install 'arch:all' packages from there.

### Install the components
	$ sudo apt install elasticsearch logstash kibana

### Start the services
	$ sudo service elasticsearch start
	$ sudo service logstash start
	$ sudo service kibana start
	
### Check service status
	$ sudo service elasticsearch status

