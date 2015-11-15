# RabbitMQ

- AMQP
- supports persistence replication
- publishers confirms are better than transactions
  - guarantee both persistence and replication to the cluster
- performance: poor
  - single thread: 1000 msgs/s
  - multiple threads/nodes: 3600 msgs/ss
- web based console
- if you want to have high persistence guarantees, RabbitMQ ensures replication across the cluster, and on disk, on message send
- a very popular choice and used in many projects

## Problems

- rate of inserts slows down when rabbitmq is writing to disk
- problem with executing *resync* after bringing one of the nodes down
  - got stuck with one thread at 100% cpu

## Introduction

http://levvel.io/blog-post/rabbitmq-an-introduction/

- exchanges and queues
- queues can be replicated across nodes in a cluster
  - master queueand slave queue (used when node holding master queue crashes)

![](http://levvel.wpengine.com/wp-content/uploads/2015/08/1432916675391.png)

http://levvel.io/blog-post/extending-a-rabbitmq-cluster-across-a-wan/


# Kafka

- very high throughput: 30k msgs/s
- file based, consumers based on reading from offset
- not flexible
- no front end ui
- no routing rules


https://softwaremill.com/mqperf/

http://technewsnow.com/news/vmware-s-rabbitmq-vs-linkedin-s-kafka

https://www.quora.com/topic/Message-Queuing

