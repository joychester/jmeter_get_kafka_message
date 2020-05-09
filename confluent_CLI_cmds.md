### start Confluent local server
confluent local start

### Check Confluent status
confluent local status

### Create topics
./kafka-topics --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 3 --topic users

### List topics
./kafka-topics --list --bootstrap-server localhost:9092

### install the confulent connectors
confluent-hub install mmolimar/kafka-connect-fs:1.0.0

### config the file-source connector
vi ./etc/kafka/connect-file-source.properties

### start the connector
confluent local load file-source

### check running connectors
confluent local status connectors

### load a predefined connector
./confluent load file-source

### unload a connector
./confluent unload file-source

### check status of file-source connector
confluent local status file-source

### stop connector
confluent local stop connect

### start connector
confluent local start connect

### starting to consume messages from partition 1
./kafka-console-consumer --bootstrap-server localhost:9092 --topic users --partition 1

### check connector logs
confluent local log connect
