KAFKA TOPICS

Create a new Kafka topic:
    kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic the-topic
Listing all available topics:
    kafka-topics --list --zookeeper localhost:2181
Add more partitions:
    kafka-topics --zookeeper localhost:2181 --alter --topic the-topic --partitions 16
Delete a topic named `the-topic`:
    kafka-topics --zookeeper localhost:2181 --delete --topic the-topic
Find more details about a topic named `the-topic`:
    kafka-topics --describe --zookeeper localhost:2181 --topic the-topic
Check under-replicated partitions for all topics:
    kafka-topics --zookeeper localhost:2181/kafka-cluster --describe --under-replicated-partitions

KAFKA PRODUCERS

Produce messages from standard input (user manual messages production):
    kafka-console-producer --broker-list localhost:9092 --topic the-topic
Produce new messages from an existing file named messages-file.txt`:
    kafka-console-producer --broker-list localhost:9092 --topic test < messages.txt
Produce Avro messages:
    kafka-avro-console-producer --broker-list localhost:9092 --topic the-topic --property value.schema='{"type":"record","name":"myrecord","fields":[{"name":"f1","type":"string"}]}' --property schema.registry.url=http://localhost:8081

KAFKA CONSOLE CONSUMERS

Console consume from the beginning of the Topic
    kafka-console-consumer --bootstrap-server localhost:9092 --topic the-topic --from-beginning
Console consume a single message:
    kafka-console-consumer --bootstrap-server localhost:9092 --topic the-topic  --max-messages 1
Console consume a single message from `__consumer_offsets`:
    kafka-console-consumer --bootstrap-server localhost:9092 --topic __consumer_offsets --formatter 'kafka.coordinator.GroupMetadataManager$OffsetsMessageFormatter' --max-messages 1
Console consume with specified consumer group:
    kafka-console-consumer --topic the-topic --new-consumer --bootstrap-server localhost:9092 --consumer-property group.id=my-group

KAFKA AVRO MESSAGES CONSUMERS

Consume 10 Avro messages from a topic named `avro-topic`:
    kafka-avro-console-consumer --topic avro-topic --new-consumer --bootstrap-server localhost:9092 --from-beginning --property schema.registry.url=localhost:8081 --max-messages 10
Consume all existing Avro messages from a topic named `avro-topic`:
    kafka-avro-console-consumer --topic avro-topic --new-consumer --bootstrap-server localhost:9092 --from-beginning --property schema.registry.url=localhost:8081

KAFKA CONSUMER GROUPS COMMANDS

List all consumer groups:
    kafka-consumer-groups --new-consumer --list --bootstrap-server localhost:9092
Describe a Group named `firstgroup`:
    kafka-consumer-groups --bootstrap-server localhost:9092 --describe --group firstgroup

KAFKA CONFIGURATION

Set the retention for a topic:
    kafka-configs --zookeeper localhost:2181 --alter --entity-type topics --entity-name the-topic --add-config retention.ms=3600000 
Print all configuration overrides for a topic named `the-topic`:
    kafka-configs --zookeeper localhost:2181 --describe --entity-type topics --entity-name the-topic
Delete a configuration override for `retention.ms` for a topic named `the-topic`:
    kafka-configs --zookeeper localhost:2181 --alter --entity-type topics --entity-name the-topic --delete-config retention.ms 

PERFORMANCE

Test performance
    kafka-producer-perf-test --topic avro-topic --throughput 10000 --record-size 300 --num-records 20000 --producer-props bootstrap.servers="localhost:9092"

KAFKA ACLs

Add a new consumer ACL to an existing topic:
    kafka-acls --authorizer-properties zookeeper.connect=localhost:2181 --add --allow-principal User:Piotr --consumer --topic the-topic --group firstgroup
Add a new producer ACL to an existing topic:
    kafka-acls --authorizer-properties zookeeper.connect=localhost:2181 --add --allow-principal User:Piotr --producer --topic the-topic
List the ACLs of a topic named `the-topic`:
    kafka-acls --authorizer-properties zookeeper.connect=localhost:2181 --list --topic the-topic

ZOOKEEPER

Enter the zookeeper shell:
    zookeeper-shell localhost:2182 ls 
