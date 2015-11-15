RabbitMQ

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

https://softwaremill.com/mqperf/

http://technewsnow.com/news/vmware-s-rabbitmq-vs-linkedin-s-kafka

https://www.quora.com/topic/Message-Queuing

