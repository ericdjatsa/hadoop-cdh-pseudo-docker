# CDH 5 pseudo-distributed cluster Docker image ( Credits to chalimartines/cdh5-pseudo-distributed - https://github.com/chali/hadoop-cdh-pseudo-docker)

##### Main changes from "chalimartines/cdh5-pseudo-distributed" : 
- modified repository files to always use the latest version of CDH5
- use mysql as metastore DB for Hive
- use mysql as Oozie backend database
- installed WebHcat
- enabled and configured HUE Spark Notebook : provides an interactive interface for submitting tasks to Spark

Do you develop Hadoop mapreduce applications on top of Cloudera distribution? This docker image can help you. It contains basic CDH 5 setup with YARN. You can use it for developmeent and verification of your code in local environment without messing up your system with Hadoop instalation.

Docker image was prepared according to [Installing CDH 5 with YARN on a Single Linux Node in Pseudo-distributed mode](http://www.cloudera.com/content/cloudera-content/cloudera-docs/CDH5/latest/CDH5-Quick-Start/cdh5qs_yarn_pseudo.html) with a few adjustments for Docker environment. 
The CLoudera repository used for building this docker image is setup to always use the latest version of CDH5.

##### Installed services
* HDFS
* YARN
* Map-Reduce
* JobHistoryServer
* Oozie
* Hue
* Spark (instalation for execution on top of YARN)
* Hive + Hive-Server2
* WebHcat

### Execution
Get docker image

    docker pull ericdjatsa/cdh5-pseudo-distributed

Run image with specified port mapping

    docker run -it --name cdh -d -p 8020:8020 -p 50070:50070 -p 50010:50010 -p 50020:50020 -p 50075:50075 -p 8030:8030 -p 8031:8031 -p 8032:8032 -p 8033:8033 -p 8088:8088 -p 8040:8040 -p 8042:8042 -p 10020:10020 -p 19888:19888 -p 11000:11000 -p 8888:8888 -p 18080:18080 -p 50111:50111 -p 9999:9999 ericdjatsa/cdh5-pseudo-distributed

 Or you can just install [docker-compose](https://docs.docker.com/compose/install/) and run 

    docker-compose up 

which will start the docker container specified in configuration file docker-compose.yml
  
If you are Mac OS user with docker machine with virtualbox driver and you would like to get from your local system to a cdh container add these port forwardings. Name of virtual machine is equal to your docker machine name (here it is "dev").

	VBoxManage modifyvm "dev" --natpf1 "tcp-port8020,tcp,,8020,,8020"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port50070,tcp,,50070,,50070"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port50010,tcp,,50010,,50010"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port50020,tcp,,50020,,50020"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port50075,tcp,,50075,,50075"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8030,tcp,,8030,,8030"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8031,tcp,,8031,,8031"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8032,tcp,,8032,,8032"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8033,tcp,,8033,,8033"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8088,tcp,,8088,,8088"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8040,tcp,,8040,,8040"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8042,tcp,,8042,,8042"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port10020,tcp,,10020,,10020"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port19888,tcp,,19888,,19888"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port11000,tcp,,11000,,11000"
	VBoxManage modifyvm "dev" --natpf1 "tcp-port8888,tcp,,8888,,8888"
    VBoxManage modifyvm "dev" --natpf1 "tcp-port9999,tcp,,9999,,9999"
    VBoxManage modifyvm "dev" --natpf1 "tcp-port18080,tcp,,18080,,18080"

### UI entry points
Those urls consider port forwarding from localhost.
* name node - http://localhost:50070
* resource manager - http://localhost:8088
* job history server - http://localhost:19888
* oozie console - http://localhost:11000
* hue - http://localhost:8888
* spark history server - http://localhost:18080

### WebHcat
http://cdh5pseudo:50111/templeton/v1/ddl/database/default/table/?user.name=hdfs

#### Hue login
You will be asked to create account during the first login. You can pick your prefered username and password. It will create home folder on HDFS and it can be used as hadoop user.

#### Mac OS and Docker in Virtualbox resource setting.
This docker image doesn't completely follow  philosophy (one process = one image), but it prefers developer convenience to easily set up the whole hadoop dev stack. It comes with price and I recommend add resources to your virtualbox machine. I use 4 cores and 8GB RAM.

#### Custom port for your usecases
This image has exposed one port (9999). It is not used by any currently running service. It can be used by you for example when you need to attach debugger to running mapreduce job. So your mapreduce job can start debugging server on this port.
