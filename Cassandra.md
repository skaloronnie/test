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

SSTable

* A sorted string table (SSTable)
* **immutable** data file to which Cassandra writes memtables periodically. 
* append only
* stored on disk **sequentially** 
* maintained for each Cassandra table.

Key components

Key components for configuring Cassandra¶

* Gossip
 * persisted locally by each node to use immediately when a node restarts.
* Partitioner
 * Each row of data is uniquely identified by a partition key and distributed across the cluster by the value of the token

    You must set the partitioner and assign the node a num_tokens value for each node. The number of tokens you assign depends on the hardware capabilities of the system. If not using virtual nodes (vnodes), use the initial_token setting instead.
    Replication factor

    The total number of replicas across the cluster. A replication factor of 1 means that there is only one copy of each row on one node. A replication factor of 2 means two copies of each row, where each copy is on a different node. All replicas are equally important; there is no primary or master replica. You define the replication factor for each data center. Generally you should set the replication strategy greater than one, but no more than the number of nodes in the cluster.
    Replica placement strategy

    Cassandra stores copies (replicas) of data on multiple nodes to ensure reliability and fault tolerance. A replication strategy determines which nodes to place replicas on. The first replica of data is simply the first copy; it is not unique in any sense. The NetworkTopologyStrategy is highly recommended for most deployments because it is much easier to expand to multiple data centers when required by future expansion.

    When creating a keyspace, you must define the replica placement strategy and the number of replicas you want.
    Snitch

    A snitch defines groups of machines into data centers and racks (the topology) that the replication strategy uses to place replicas.

    You must configure a snitch when you create a cluster. All snitches use a dynamic snitch layer, which monitors performance and chooses the best replica for reading. It is enabled by default and recommended for use in most deployments. Configure dynamic snitch thresholds for each node in the cassandra.yaml configuration file.

    The default SimpleSnitch does not recognize data center or rack information. Use it for single-data center deployments or single-zone in public clouds. The GossipingPropertyFileSnitch is recommended for production. It defines a node's data center and rack and uses gossip for propagating this information to other nodes.
    The cassandra.yaml configuration file

    The main configuration file for setting the initialization properties for a cluster, caching parameters for tables, properties for tuning and resource utilization, timeout settings, client connections, backups, and security.
    By default, a node is configured to store the data it manages in a directory set in the cassandra.yaml file.
        Packaged installs: /var/lib/cassandra
        Tarball installs: install_location/data/data

    In a production cluster deployment, you can change the commitlog-directory to a different disk drive from the data_file_directories.
    System keyspace table properties

    You set storage configuration attributes on a per-keyspace or per-table basis programmatically or using a client application, such as CQL.



## Commands
    nodetool tpstats

    nodetool cfhistograms

[asfas](4%20Myths%20about%20In-Memory%20Databases.md)
