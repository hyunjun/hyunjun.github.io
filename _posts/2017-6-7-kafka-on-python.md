---
layout: post
title: Kafka on Python
---

# Environments
* Redhat 6.6 (CDH5.3 installed)
  * python 2.7.11 / python3.5.1
* Redhat 7.3
  * python 2.7.5 / python3.6.1
* kafka 2.11-0.10.2.0; [Download](https://www.apache.org/dyn/closer.cgi?path=/kafka/0.10.2.0/kafka_2.11-0.10.2.0.tgz)
  * [Quickstart](https://kafka.apache.org/quickstart)
  * execution

    ```
    $ /path/to/kafka-console-consumer.sh \
      --bootstrap-server <server>:<port> \
      --topic <topic> \
      [--from-beginning|--partition <partition> --offset <offset>]
    ```
    * to use `offset`, must use `partition`, too

# ref
* [A Practical Guide to Apache Kafka: Part 1](https://www.coshx.com/blog/2016/10/20/a-practical-guide-to-kafka1/)
* [Getting Started with Apache Kafka for the Baffled, Part 2](http://www.shayne.me/blog/2015/2015-06-25-everything-about-kafka-part-2/)
* [kafka console tools](https://stackoverflow.com/documentation/apache-kafka/8990/kafka-console-tools)
* `auto_commit` or `auto_commit_enable`
  * [kafka.consumer.simple.SimpleConsumer](http://kafka-python.readthedocs.io/en/master/apidoc/kafka.consumer.html)
  * [KafkaConsumer commit offset did not work](https://github.com/dpkp/kafka-python/issues/458)
  * [SocketDisconnectedError on commit_offsets()](https://github.com/Parsely/pykafka/issues/224)
* `auto_offset_reset='smallest'`
  * 시작할 때 부터의 message를 받아오는 option
  * 아래 3가지 library 모두 공통 적용
  * [from-beginning equivalent when using the client](https://github.com/dpkp/kafka-python/issues/461)
* offset
  * [Kafka console consumer: How to get only the last N messages from a topic instead of everything from the beginning?](https://stackoverflow.com/questions/38983405/kafka-console-consumer-how-to-get-only-the-last-n-messages-from-a-topic-instead)
  * [Kafka - Rewind Consumer Offsets](https://jeqo.github.io/post/2017-01-31-kafka-rewind-consumers-offset/)
* `task_done`

# Library
* confluent-kafka
 * librdkafka 기반
  * installation

    ```
    $ sudo yum install librdkafka-devel python-devel
    $ pip[3] install confluent-kafka
    ```
  * ref
    * [Introduction to Apache Kafka for Python Programmers](https://www.confluent.io/blog/introduction-to-apache-kafka-for-python-programmers/)
    * [confluent.io/clients](https://www.confluent.io/clients/)
    * [Confluent's Python Client for Apache KafkaTM](https://github.com/confluentinc/confluent-kafka-python)
    * [confluent-kafka-python](http://docs.confluent.io/3.0.1/clients/confluent-kafka-python/)
    * [confluent-kafka](https://pypi.python.org/pypi/confluent-kafka)
* [kafka-docker](https://github.com/wurstmeister/kafka-docker)
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
    * [Installing Apache Kafka and using Python 3 to communicate with it](http://www.giantflyingsaucer.com/blog/?p=5541)
* pykafka
  * installation

    ```
    $ sudo yum install snappy-devel       # Redhat
    $ sudo apt-get install libsnappy-dev  # Ubuntu

    $ pip[3] install python-snappy
    $ pip[3] install pykafka
    ```
  * Consumer

    ```
    # python 3.6.1
    from pykafka import KafkaClient

    # for topic, bytearray doesn't work
    bootstrap_server, my_topic = '{}:{}'.format('x.y.z.w', '1234'), bytes('some_topic', 'utf8')

    client = KafkaClient(bootstrap_server)
    topic = client.topics[my_topic]
    consumer = topic.get_simple_consumer()
    for message in consumer:
      print(message.value)
    ```
    * use `consumer = topic.get_simple_consumer(auto_offset_reset=pykafka.common.OffsetType.LATEST)` to get message from the latest one
      * [pykafka.readthedocs.io/en/latest/api/common.html](http://pykafka.readthedocs.io/en/latest/api/common.html)
      * [kafka.apache.org/documentation.html](http://kafka.apache.org/documentation.html) -> find auto.offset.reset
  * failed to run on Docker; Everytime I run my test program, opened ports are various like below, so I don't know which ports to pen when running Docker

    ```
    $ python3 test_pykafka.py
    $ netstat -anp | grep -i python3
    (Not all processes could be identified, non-owned process info
     will not be shown, you would have to be root to see it all.)
    tcp        0      0 10.61.26.109:55584      10.195.23.74:3306       ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:42717      10.60.28.89:9092        ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:39935      10.60.5.205:9092        ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:38610      10.60.29.145:9092       ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:53859      10.60.29.83:9092        ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:55582      10.195.23.74:3306       ESTABLISHED 17084/python3
    tcp        0      0 10.61.26.109:43787      10.60.1.19:9092         ESTABLISHED 17084/python3

    # reexecute
    $ python3 test_pykafka.py
    $ netstat -anp | grep -i python3
    (Not all processes could be identified, non-owned process info
     will not be shown, you would have to be root to see it all.)
    tcp        0      0 10.61.26.109:43546      10.61.26.109:8202       ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:59354      10.60.29.145:9092       ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:40407      10.60.1.19:9092         ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:40749      10.60.28.89:9092        ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:53573      10.60.5.205:9092        ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:48598      10.60.29.83:9092        ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:60866      10.195.23.74:3306       ESTABLISHED 29088/python3
    tcp        0      0 10.61.26.109:60864      10.195.23.74:3306       ESTABLISHED 29088/python3
    ```
  * ref
    * [PyKafka](https://github.com/Parsely/pykafka)
    * [PyKafka: Fast, Pythonic Kafka, at Last!](https://blog.parse.ly/post/3886/pykafka-now/)
    * [A Practical Guide to Apache Kafka: Part 2](https://www.coshx.com/blog/2016/10/20/a-practical-guide-to-kafka2/)
