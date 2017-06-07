---
layout: post
title: Kafka on Python
---

# Environments
* Redhat 6.6, CDH5.3 installed
* kafka 2.11-0.10.2.0; [Download](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.0/kafka_2.11-0.10.2.0.tgz)
  * [Quickstart](https://kafka.apache.org/quickstart)
* python 2.7.11/python3.5.1

# ref
* [A Practical Guide to Apache Kafka: Part 1](https://www.coshx.com/blog/2016/10/20/a-practical-guide-to-kafka1/)

# Library
* confluent-kafka
  * installation failed
  * ref
    * [Confluent's Python Client for Apache KafkaTM](https://github.com/confluentinc/confluent-kafka-python)
    * [confluent-kafka-python](http://docs.confluent.io/3.0.1/clients/confluent-kafka-python/)
    * [confluent-kafka](https://pypi.python.org/pypi/confluent-kafka)
* kafka-python
  * practice from [Python Kafka Code Example](http://gangmax.me/blog/2017/03/13/python-kafka-code-example/)
    * installation `pip install kafka-python` or `pip3 install kafka-python`
    * 1. start kafka server

      ```
      $ ./bin/kafka-server-start.sh config/server.properties &
      $ ./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic fast-messages
      $ ./bin/kafka-topics.sh --list --zookeeper localhost:2181
      fast-messages
      ```
    * 2. Consumer

      ```
      from kafka import KafkaConsumer
      consumer = KafkaConsumer('fast-messages', bootstrap_servers='localhost:9092')
      # consumer = KafkaConsumer('some topic', group_id='some group', bootstrap_servers=['some server:9092'])
      # consumer.subscribe('some topic')`
      for message in consumer:
        print(message)
      ```
    * 3. Producer

      ```
      from kafka import KafkaProducer
      producer = KafkaProducer(bootstrap_servers='localhost:9092')
      for i in range(1000):
        producer.send('fast-messages', key=str.encode('key_{}'.format(i)), value=b'some_message_bytes')
      ```
    * troubleshooting when `kafka topic already exists` happens, from [How to Delete a topic in apache kafka](https://stackoverflow.com/questions/33537950/how-to-delete-a-topic-in-apache-kafka)

      ```
      $ ./bin/kafka-topics.sh --list --zookeeper localhost:2181
      fast-messages

      $ sh /usr/lib/zookeeper/bin/zkCli.sh -server localhost:2181
      ...
      [zk: localhost:2181(CONNECTED) 0] ls /brokers/topics
      [fast-messages]
      [zk: localhost:2181(CONNECTED) 1] rmr /brokers/topics/fast-messages
      [zk: localhost:2181(CONNECTED) 2] ls /brokers/topics
      []

      $ ./bin/kafka-topics.sh --list --zookeeper localhost:2181
      ```
  * ref
    * [Kafka Producer sample code in Scala and Python](https://community.hortonworks.com/articles/74077/kafka-producer-sample-code-in-scala-and-python.html)
    * [Kafka-python(Programming) Questions & Answers](http://techqa.info/programming/tag/kafka-python)
    * [Part 2.3: Getting started with Apache Kafka and Python](https://www.cloudkarafka.com/blog/2016-12-13-part2-3-apache-kafka-for-beginners_example-and-sample-code-python.html)
    * [Python Kafka Consumer Record](http://codegists.com/code/python-kafka-consumer-record/)
* pykafka
  * installation

    ```
    # Redhat 6.6
    $ yum install snappy-devel
    $ pip install python-snappy
    $ pip install pykafka
    ```
  * ref
    * [PyKafka](https://github.com/Parsely/pykafka)
    * [PyKafka: Fast, Pythonic Kafka, at Last!](https://blog.parse.ly/post/3886/pykafka-now/)
    * [A Practical Guide to Apache Kafka: Part 2](https://www.coshx.com/blog/2016/10/20/a-practical-guide-to-kafka2/)
