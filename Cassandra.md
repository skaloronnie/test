https://wiki.apache.org/cassandra/ArchitectureInternals
http://www.slideshare.net/acunu/cassandra-internals-solving-problems

# Architecture overview
1. A **sequentially written commit log** on each node captures write activity
1. Data is then indexed and written to an in-memory structure, called a **memtable**, which resembles a write-back cache
1. Once the memory structure is full, the data is written to disk in an **SSTable data file**
1.  All writes are automatically partitioned and **replicated** throughout the cluster
1.  Using a process called **compaction** Cassandra periodically consolidates SSTables, discarding obsolete data and tombstones (an indicator that data was deleted)

## Client request

1. When a client connects to a node with a request, that node serves as the coordinator for that particular client operation. 
1. The coordinator determines which nodes in the ring should get the request based on how the cluster is configured.

## SSTable

* A sorted string table (SSTable)
* **immutable** data file to which Cassandra writes memtables periodically. 
* append only
* stored on disk **sequentially** 
* maintained for each Cassandra table.

## Key components for configuring Cassandra

* Gossip
 * persisted locally by each node to use immediately when a node restarts.
* Partitioner
 * Each row of data is uniquely identified by a partition key and distributed across the cluster by the value of the token
 * You must set the partitioner and assign the node a num_tokens value for each node. The number of tokens you assign depends on the hardware capabilities of the system. If not using virtual nodes (vnodes), use the initial_token setting instead.
* Replication factor
 * You define the replication factor for each data center
* Replica placement strategy
 * When creating a **keyspace**, you must define the replica placement strategy and the number of replicas you want.
* Snitch
 * You must configure a snitch when you create a cluster. 
 * All snitches use a dynamic snitch layer, which monitors performance and chooses the best replica for reading. It is enabled by default and recommended for use in most deployments. 
 * Configure dynamic snitch thresholds for each node in the cassandra.yaml configuration file.
 * The **GossipingPropertyFileSnitch** is recommended for production. It defines a node's data center and rack and uses gossip for propagating this information to other nodes.

# Database internals

* storage structure similar to a Log-Structured Merge Tree
* avoids reading before writing
 * can produce stalls in read performance and other problems
 * corrupts caches and increases IO requirements
* groups inserts/updates to be made
* and sequentially writes only the updated parts of a row in append mode
* never re-writes or re-reads existing data, and never overwrites the rows in place.
* avoids overwrites and uses sequential IO to update data

## The write path to compaction
http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_write_path_c.html

## The write path of an update
http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_write_update_c.html

## Hinted handoff writes
During a write operation, when hinted handoff is enabled and **consistency can be met**, the coordinator stores a hint about dead replicas in the local system.hints table under either of these conditions:
* A replica node for the row is known to be down ahead of time.
* A replica node does not respond to the write request.

A hint indicates that a write needs to be replayed to one or more unavailable nodes.

If the node recovers after the save time has elapsed, run a repair to **re-replicate the data** written during the down time.

Example:

* cluster of two nodes, A and B, replication factor (RF) of 1
* node A is down while we write row K to it with consistency level of one
* write fails because reads always reflect the most recent write when `W + R > replication factor`
 * W is the number of nodes to block for writes
 * R is the number of nodes to block for reads
* does not write a hint to B and call the write good
* cannot read the data at any consistency level until A comes back up and B forwards the data to A
* with 3 nodes and RF=2, hint is written

![dml_hinted_handoff](https://cloud.githubusercontent.com/assets/14850484/10122154/3db10690-6506-11e5-96dd-e736c372f39c.png)

# Commands
    nodetool tpstats

    nodetool cfhistograms

[asfas](4%20Myths%20about%20In-Memory%20Databases.md)
