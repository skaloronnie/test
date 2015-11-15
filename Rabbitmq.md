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

Problems

- rate of inserts slows down when rabbitmq is writing to died
- problem with executing *resync* after bringing one of the nodes down
  - got stuck with one thread at 100% cpu

https://softwaremill.com/mqperf/

http://technewsnow.com/news/vmware-s-rabbitmq-vs-linkedin-s-kafka

https://www.quora.com/topic/Message-Queuing

