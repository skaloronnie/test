RabbitMQ

- AMQP
- supports persistence replication
- publishers confirms better than transactions
  - guarantee both persistence and replication to the cluster
- performance: poor
  - single thread: 1000 msgs/s
  - multiple threads/nodes: 3600 msgs/s

https://softwaremill.com/mqperf/

http://technewsnow.com/news/vmware-s-rabbitmq-vs-linkedin-s-kafka

https://www.quora.com/topic/Message-Queuing
