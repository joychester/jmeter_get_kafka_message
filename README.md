# jmeter_get_kafka_message
To use Jmeter to get kafak messages by partitions, for preparing testing data purpose across all jmeter agents

## How to run the jmeter cmd line by input topic and partition value: 
> $ ./jmeter -n -t data_prepare_with_kafka.jmx -Jtopic=users -Jtopic_partition=0


## How to use default file-source connector to publish messages Round-Robin to kafka Topic partitions
using Confluent 5.5.0 to do this sample, but be noticed something has changed to the default producer partitioner method:  
In Confluent Platform versions **5.4.x and later**, the partition is assigned with awareness to **batching**. If a batch of records is not full and has not yet been sent to the broker, it will select the same partition as a prior record. Partitions for newly created batches are assigned randomly. For more information, see KIP-480: Sticky Partitioner and the related Confluent blog post.  
In Confluent Platform versions **prior to 5.4.x**, the partition is assigned in a **round robin method**, starting at a random partition.

#### 1. modify {Confluent_HOME}/etc/schema-registry/connect-avro-distributed.properties to enable the RR method for partitioner:
producer.partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner
connector.client.config.override.policy=All
producer.batch.size=1 

#### 2. modify: /Users/cchi/Data_Platform/confluent-5.5.0/etc/kafka-rest/kafka-rest.properties
uncomment line: zookeeper.connect=localhost:2181

#### 3. restart the confluent platform to reflect the changes

#### PS. Sample config towards file-source connector:

name=local-file-source  
connector.class=FileStreamSource  
tasks.max=1  
file=somewhere_in_your_local/etc/kafka/you_name_it.txt  
topic=users  
key.converter=org.apache.kafka.connect.storage.StringConverter  
value.converter=org.apache.kafka.connect.storage.StringConverter  
