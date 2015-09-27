https://wiki.apache.org/cassandra/ArchitectureInternals
http://www.slideshare.net/acunu/cassandra-internals-solving-problems

# Architecture overview
* A **sequentially written commit log** on each node captures write activity
* Data is then indexed and written to an in-memory structure, called a **memtable**, which resembles a write-back cache
* Once the memory structure is full, the data is written to disk in an **SSTable data file**
*  All writes are automatically partitioned and **replicated** throughout the cluster
*  Using a process called **compaction** Cassandra periodically consolidates SSTables, discarding obsolete data and tombstones (an indicator that data was deleted)

## Commands
    nodetool tpstats

    nodetool cfhistograms

[asfas](4%20Myths%20about%20In-Memory%20Databases.md)
