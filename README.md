# docker-kafka-cluster
A very simple setup for running a kafka cluster using docker images. It requires a zookeeper cluster and I have provided the same in another repo.

# The Need
There are lots of kafka docker images that help you to run a standalone version (with an inbuilt zookeeper or not) on a single node. These images help you to have a single node of kafka and doesn't provide you with high availability. This particular repo helps you in achieving a cluster (high availability) using simple scripts and docker.

# Prerequisites
**You need a zookeeper cluster.** [See here](https://github.com/gten/docker-zookeeper-cluster). Create a working zookeeper cluster and then try to implement the kafka cluster as follows.

# Docker image
```
docker pull jeygeethan/kafka-cluster
```

# Setting up a kafka cluster
For each of the nodes, try running the following command to run a broker. The following environment variables are needed.
* **KAFKA_HOST** - give the external FQDN or IP address from which the clients (and the zookeeper) can access the kafka broker
* **KAFKA_PORT** - Port where the broker will listen
* **ZOOKEEPER_CONNECT** - list of zookeeper servers along with their ports in a comma separated fashion so that kafka can register themselves
* **BROKER_ID** - An integer stating the broker id (starting from 0,1,2 till 255)

## Sample script to run a three node/broker cluster
Run these scripts on individual hosts to create a cluster
### Node1
```
docker run -p 9092:9092 -e KAFKA_HOST=host_kafka1 -e KAFKA_PORT=9092 -e ZOOKEEPER_CONNECT=10.0.0.1:2181,10.0.0.2:2181,10.0.0.3:2181 -e BROKER_ID=0 --name kafka1 jeygeethan/kafka-cluster
```
### Node2
```
docker run -p 9092:9092 -e KAFKA_HOST=host_kafka2 -e KAFKA_PORT=9092 -e ZOOKEEPER_CONNECT=10.0.0.1:2181,10.0.0.2:2181,10.0.0.3:2181 -e BROKER_ID=1 --name kafka1 jeygeethan/kafka-cluster
```

### Node3
```
docker run -p 9092:9092 -e KAFKA_HOST=host_kafka3 -e KAFKA_PORT=9092 -e ZOOKEEPER_CONNECT=10.0.0.1:2181,10.0.0.2:2181,10.0.0.3:2181 -e BROKER_ID=2 --name kafka1 jeygeethan/kafka-cluster
```
