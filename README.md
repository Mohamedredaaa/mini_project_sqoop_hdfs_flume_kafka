# Mini Project: SQL to HDFS with Sqoop, Flume, and Kafka

## Overview
This project demonstrates a pipeline for data transfer and processing using Sqoop, HDFS, Flume, and Kafka. The goal is to efficiently migrate data from an SQL database to HDFS, subsequently use Flume to transfer the data from HDFS to a Kafka topic, and create a Kafka consumer and producer to process the data.

---

## Components and Workflow
1. **Data Source**: SQL database (e.g., MySQL, MariaDB).
2. **Data Ingestion to HDFS**: Sqoop is used to transfer data from the SQL database to HDFS.
3. **Data Transfer to Kafka**: Apache Flume is configured to read the data from HDFS and publish it to a Kafka topic.
4. **Kafka Processing**: A Kafka producer sends data to the topic, and a Kafka consumer retrieves and processes the data.

---

## Steps
### 1. **Data Transfer from SQL to HDFS with Sqoop**
   - Install and configure Sqoop.
   - Use the Sqoop import command to transfer data from the SQL database to an HDFS directory.
   - Example command:
     ```bash
     sqoop import \
     --connect jdbc:mysql://<hostname>:<port>/<database> \
     --username <username> --password <password> \
     --table <table_name> \
     --target-dir /user/<hdfs_user>/data_dir \
     --as-textfile \
     --m 1
     ```

### 2. **Configure Flume for HDFS to Kafka Data Transfer**
   - Install and configure Apache Flume.
   - Create a Flume configuration file to define the source, channel, and sink.
     - **Source**: HDFS SpoolDir source to monitor the HDFS directory.
     - **Sink**: Kafka sink to publish data to the Kafka topic.
     - Example configuration:
       ```properties
       agent.sources = source1
       agent.channels = channel1
       agent.sinks = sink1

       agent.sources.source1.type = spooldir
       agent.sources.source1.spoolDir = /path/to/hdfs/dir

       agent.sinks.sink1.type = org.apache.flume.sink.kafka.KafkaSink
       agent.sinks.sink1.kafka.bootstrap.servers = <kafka_broker>
       agent.sinks.sink1.topic = <kafka_topic>

       agent.channels.channel1.type = memory
       agent.sources.source1.channels = channel1
       agent.sinks.sink1.channel = channel1
       ```

### 3. **Set Up Kafka Producer and Consumer**
   - Start Kafka and create a topic.
     ```bash
     kafka-topics.sh --create --topic <kafka_topic> --bootstrap-server <kafka_broker>
     ```
   - Implement a Kafka producer to publish data to the topic.
   - Implement a Kafka consumer to retrieve and process the data.

---

## Results
- Data successfully transferred from SQL database to HDFS using Sqoop.
- Flume monitored the HDFS directory and streamed the data to a Kafka topic.
- Kafka producer and consumer processed the data seamlessly.

---

## Requirements
- **Software**: Apache Sqoop, Hadoop, Apache Flume, Apache Kafka.
- **Programming**: Basic knowledge of shell scripting, Kafka APIs (e.g., Python, Java).

---

## Challenges
- Ensuring compatibility between Flume, Kafka, and Hadoop versions.
- Tuning Flume's memory channel configuration to handle large data volumes.

---

## Future Improvements
- Automate the pipeline with scripts.
- Add monitoring and logging to track data flow.
- Enhance the Kafka consumer to perform advanced data analytics.

---

## Conclusion
This project successfully showcases the integration of Sqoop, Flume, and Kafka to create a robust data pipeline. It provides a scalable approach for ingesting, transferring, and processing large datasets in real time.

