https://wiki.apache.org/cassandra/ArchitectureInternals
http://www.slideshare.net/acunu/cassandra-internals-solving-problems

# Architecture overview
1. A **sequentially written commit log** on each node captures write activity
1. Data is then indexed and written to an in-memory structure, called a **memtable**, which resembles a write-back cache
1. Once the memory structure is full, the data is written to disk in an **SSTable data file**
1.  All writes are automatically partitioned and **replicated** throughout the cluster
1.  Using a process called **compaction** Cassandra periodically consolidates SSTables, discarding obsolete data and tombstones (an indicator that data was deleted)

Client request

1. When a client connects to a node with a request, that node serves as the coordinator for that particular client operation. 
1. The coordinator determines which nodes in the ring should get the request based on how the cluster is configured.


## Commands
    nodetool tpstats

    nodetool cfhistograms

[asfas](4%20Myths%20about%20In-Memory%20Databases.md)
