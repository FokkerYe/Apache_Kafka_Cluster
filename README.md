# Apache_Kafka_Cluster_3-Nodes_Ubuntu 22.04_Server

java 8 or higher
RAM size Minimum 1 GB

Node1. ip 192.168.201.62 
Node2. ip 192.168.201.79
Node3. ip 192.168.201.80


```
# sudo apt update  
# sudo apt install default-jdk 
# java --version 
# wget https://archive.apache.org/dist/kafka/3.2.0/kafka_2.13-3.2.0.tgz
# tar xzf kafka_2.13-3.2.0.tgz 
# sudo mv kafka_2.13-3.2.0 /usr/local/kafka

```


=====================================
 ```
# sudo nano /etc/systemd/system/zookeeper.service 

[Unit]
Description=Apache Zookeeper server
Documentation=http://zookeeper.apache.org
Requires=network.target remote-fs.target
After=network.target remote-fs.target

[Service]
Type=simple
ExecStart=/usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties
ExecStop=/usr/local/kafka/bin/zookeeper-server-stop.sh
Restart=on-abnormal

[Install]
WantedBy=multi-user.target
==============================
# sudo nano /etc/systemd/system/kafka.service 

[Unit]
Description=Apache Kafka Server
Documentation=http://kafka.apache.org/documentation.html
Requires=zookeeper.service

[Service]
Type=simple
Environment="JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64"
ExecStart=/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties
ExecStop=/usr/local/kafka/bin/kafka-server-stop.sh

[Install]
WantedBy=multi-user.target
======================================================
Service  Checking Command
# sudo systemctl daemon-reload 
# sudo systemctl start zookeeper 
# sudo systemctl start kafka 
# sudo systemctl status zookeeper 
# sudo systemctl status kafka
============================================================
Node_2 ip 192.168.201.79
broder-2***Producer

# /usr/local/kafka/config
# cat server.properties

broker.id=2
listeners=PLAINTEXT://192.168.201.79:9093
log.dirs=/tmp/kafka-logs-2
zookeeper.connect=192.168.201.62:2181,192.168.201.79:2181,192.168.201.80:2181

********
# cat producer.properties
bootstrap.servers=192.168.201.62:9092
******************
# cat consumer.properties
bootstrap.servers=192.168.201.62:9092
```


===========================================================
```
Node_3 ip 192.168.201.80

# /usr/local/kafka/config
# cat server.properties
broker.id=3
listeners=PLAINTEXT://192.168.201.80:9092
log.dirs=/tmp/kafka-logs-3
zookeeper.connect=192.168.201.62:2181,192.168.201.79:2181,192.168.201.80:2181

**********

# cat producer.properties
bootstrap.servers=192.168.201.62:9092
# cat consumer.properties
bootstrap.servers=192.168.201.62:9092

===============================================================
```
Node_1 ip 192.168.201.62
```
# /usr/local/kafka/config
# cat server.properties
broker.id=1
listeners=PLAINTEXT://192.168.201.62:9092
log.dirs=/tmp/kafka-logs-1
zookeeper.connect=192.168.201.62:2181,192.168.201.79:2181,192.168.201.80:2181

===========================================================
```
Testing
Node_1 ip 192.168.201.62
```
# bin/kafka-topics.sh --create --topic my-topic --bootstrap-server 192.168.201.62:9092 --replication-factor 1 --partitions 3

************
Node_2 ip 192.168.201.79---===Producer

# bin/kafka-console-producer.sh --broker-list 192.168.201.62:9092 --topic my-topic

*******************
Node_3 ip 192.168.201.8----===Comsumer

# bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.62:9092 --topic my-topic --from-beginning 
===========================================================================================
```
Reverse message
```
Node_2
# bin/kafka-console-producer.sh --broker-list 192.168.201.62:9092 --topic my-topic | bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.62:9092 --topic my-topic --from-beginning
Node_3
# bin/kafka-console-producer.sh --broker-list 192.168.201.62:9092 --topic my-topic | bin/kafka-console-consumer.sh --bootstrap-server 192.168.201.62:9092 --topic my-topic --from-beginning
===========================================================================================
 ```



