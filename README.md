# jmeter_get_kafka_message
To use Jmeter to get kafak messages by partitions, for preparing testing data purpose across all jmeter agents

## How to use default file-source connector to publish messages Round-Robin to kafka Topic partitions


#### modify {Confluent_HOME}/etc/schema-registry/connect-avro-distributed.properties to enable the RR method for partitioner:
producer.partitioner.class=org.apache.kafka.clients.producer.RoundRobinPartitioner
connector.client.config.override.policy=All
producer.batch.size=1 

#### modify: /Users/cchi/Data_Platform/confluent-5.5.0/etc/kafka-rest/kafka-rest.properties
uncomment line: zookeeper.connect=localhost:2181

#### restart the confluent platform to reflect the changes

#### PS. Sample config towards file-source connector:

name=local-file-source
connector.class=FileStreamSource
tasks.max=1
file=somewhere_in_your_local/etc/kafka/you_name_it.txt
topic=users
key.converter=org.apache.kafka.connect.storage.StringConverter
value.converter=org.apache.kafka.connect.storage.StringConverter
