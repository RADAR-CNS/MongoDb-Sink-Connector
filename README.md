# RADAR-MongoDbConnector

The MongoDB sink connector is a tool for scalably and reliably streaming data between Apache Kafka and MongoDB. It exports data form RADAR-CNS Kafka topics into the RADAR-CNS Hot Storage.
 
Currently it supports only two types of data:
 - [singleton double aggregator](https://github.com/RADAR-CNS/RADAR-Backend/blob/master/src/main/resources/avro/aggregator/double_aggregator.avsc)
 - [array double aggregator](https://github.com/RADAR-CNS/RADAR-Backend/blob/master/src/main/resources/avro/aggregator/double_array_aggregator.avsc)

The current version proofs how to extract data coming from an Empatica E4 device using the [RADAR-CNS Android application](https://github.com/RADAR-CNS/RADAR-AndroidApplication) and analysed by the [RADAR-CNS Kafka Backend](https://github.com/RADAR-CNS/RADAR-Backend) 

## Dependence

The following assumes you have Kafka and the Confluent Schema Registry running.

## Quickstart for Empatica

1. Build the project. Go inside the project folder and run
```shell
./gradlew clean build
```
2. Modify `sink.properties` file according your cluster
```ini
# MongoDb server
mongo.host=
mongo.port=

# MongoDb db configuration
mongo.username=
mongo.password=
mongo.database=
```
3. (optional) Modify `standalone.properties` and `standalone.properties` file according your cluster instances. You may need to update the bootstraps and Schema Registry locations.
```ini
bootstrap.servers=
key.converter.schema.registry.url=
```
4. Copy your jar file inside your Kafka Server
5. Copy all configuration files inside your Kafka Server
  - sink.properties 
  - standalone.properties (optional)
  - cluster.properties (optional)
6. Run your connector
```shell
export CLASSPATH=mongoconnector-1.0.jar
```
  - standalone mode

  ```shell
  /bin/connect-standalone standalone.properties sink.properties
  ```
  - distributed mode

  ```shell
  /bin/connect-distributed cluster.properties sink.properties
  ```
7. stop your connector using `CTRL-C`
 
### Tuning
The only available setting is the number of records returned in a single call to `poll()` (i.e. `consumer.max.poll.records` param inside `standalone.properties`)

### Note
Connectors can be run inside any machine where Kafka has been installed. Therefore, you can fire them also inside a machine that does not host a kafka broker.

## Reset
To reset a connector running in `standalone mode` you have to stop it and then modify `name` and `offset.storage.file.filename` respectively inside `sink.properties` and `standalone.properties`
