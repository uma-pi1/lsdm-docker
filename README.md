# Docker Setup for the Course Large-Scale Data Management 
This is a docker environment for the exercises of the course "Large-Scale Data Management (LSDM)".
This environment contains:
- MySQL database
- phpMyAdmin
- MongoDB
- Hadoop
- Jupyterlab with PySpark

This environment is based on [this repository]().

## Table of contents

1. [Install Docker](#1-install-docker)
2. [Setup](#2-setup)
3. [phpMyAdmin](#3-phpmyadmin)
4. [MySQL](#4-mysql)
5. [MongoDB](#5-mongodb)
6. [Hadoop](#6-hadoop)
7. [PySpark Notebook](#7-pyspark-notebook)

## 1. Install Docker
Before starting the local development environment, you need to install Docker.

### Docker Installation - Windows
To use Docker on Windows install the Docker Desktop.
We encourage you to use the WSL2 (Windows Subsystem for Linux) as backend.
You can find the download link and corresponding installation instructions [here](https://docs.docker.com/desktop/install/windows-install/).

[https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)


#### Troubleshooting WSL
Docker in the WSL can use up too many resources. We therefore limit the RAM usage with the following commands.

Create the file

```
C:\Users\<username>\.wslconfig
```

with the following content

```
[wsl2]
memory=3GB
```

You can adapt the memory usage to your system. 
Furthermore, you can limit the amount of processors used by `processors=1`.


#### Starting the Docker Engine
On Windows you always need to start Docker first manually.
Open Docker Desktop and click the little Docker icon in the bottom left corner to start the engine.



### Docker Installation - Linux
#### Installation using Snap
You can install docker using a single command on Ubuntu using Snap:

```
sudo snap install docker
```

#### Installation using apt-get
You can also install docker using apt-get. Please follow the official instuctions given [here](https://docs.docker.com/engine/install/ubuntu/).

[https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/)


## 2. Setup
Clone this repository and go into the root directory of the repository.

```
git clone https://github.com/AdrianKs/lsdm-docker
cd lsdm-docker
```


With an installed Docker environment and a started engine you can now run the Docker containers.
Run the command.

**Note: The first time you are running this command it will take some time depending on your notebook and internet connection.**
**So feel free to grab some coffee.**

**It will only take that long the first time you run this command. All following start-ups should be quick.**

```
docker-compose up -d
```


## 3. phpMyAdmin
With a successful setup you should be able to access phpMyAdmin here:

[localhost:8080](localhost:8080)

## 4. MySQL
You can connect to the database via localhost:3308

username: root

password: root

## 5. MongoDB
You can connect to the database on `mongodb://root:root@localhost:27017/

## 6. Hadoop
You can copy files to the namenode via
```
docker cp <filename> namenode:<path on namenode>
```

You can access the namenode via
```
docker exec -it namenode /bin/bash
```

On the namenode you can create a new directory on the hdfs via
```
hadoop fs -mkdir <folder name>
```

You can put a local file onto the hdfs via
```
hadoop fs -put <file name> <path on hdfs>
```

You can run a jar file via 

```
hadoop jar <jar file>
```


### Hadoop UI
You can access the Hadoop UI on

[localhost:9870](localhost:9870)


## 7. PySpark Notebook
You can access JupyterLab on

[localhost:8888](localhost:8888)

Here you can run any Python code you want.
But most importantly you can use PySpark and connect to the HDFS.


```python
import pyspark
from pyspark.sql import SparkSession

spark = SparkSession.builder.master("local").appName("hdfs_test").getOrCreate()

hello_world_rdd = spark.sparkContext.textFile("hdfs://namenode:9000/helloWorld/hello.txt")

hello_world_rdd.collect()
```
