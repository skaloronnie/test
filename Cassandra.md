https://wiki.apache.org/cassandra/ArchitectureInternals
http://www.slideshare.net/acunu/cassandra-internals-solving-problems

# Architecture overview
* A **sequentially written commit log** on each node captures write activity
* Data is then indexed and written to an in-memory structure, called a **memtable**, which resembles a write-back cache
* Once the memory structure is full, the data is written to disk in an **SSTable data file**

## Commands
    nodetool tpstats

    nodetool cfhistograms

[asfas](4%20Myths%20about%20In-Memory%20Databases.md)
