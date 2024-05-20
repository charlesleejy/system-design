### HDFS (Hadoop Distributed File System)

![alt text](../images/hdfs-architecture.png)

#### Overview
- Purpose: Designed to store and manage large datasets across a distributed computing environment.
- Part of Hadoop: Core component of the Hadoop ecosystem.

#### Architecture
- Master-Slave Architecture: Composed of a single NameNode (master) and multiple DataNodes (slaves).
  - NameNode: Manages metadata and directory structure of the file system, including file locations, permissions, and hierarchy.
  - DataNodes: Store actual data and handle read/write requests from clients.

#### Key Concepts
- Blocks: Files are split into large blocks (default 128MB or 256MB) and distributed across DataNodes.
- Replication: Each block is replicated across multiple DataNodes (default replication factor is 3) to ensure fault tolerance.
- High Throughput: Designed for high data throughput, optimizing for large file reads and writes.

#### Data Storage
- Write Once, Read Many (WORM): Files are typically written once and read multiple times, minimizing the need for random writes.
- Streaming Data Access: Optimized for streaming reads and writes, enabling fast data access.

#### Fault Tolerance
- Replication: Ensures data reliability by replicating blocks across multiple nodes.
- Heartbeat and Block Reports: DataNodes send heartbeats and block reports to the NameNode to confirm their status and block health.
- Automatic Failover: In case of DataNode failure, NameNode re-replicates the lost blocks to maintain the desired replication factor.

#### Scalability
- Horizontal Scaling: Can easily scale out by adding more DataNodes to the cluster.
- Capacity: Capable of storing petabytes to exabytes of data across thousands of nodes.

#### Performance
- Data Locality: Moves computation closer to where the data is stored, reducing network congestion and increasing performance.
- Rack Awareness: Aware of the network topology (rack locations) to optimize data placement and replication for fault tolerance and network efficiency.

#### Data Integrity
- Checksums: Uses checksums to detect and recover from data corruption during storage and transmission.
- Rebalancing: Periodically rebalances data across the cluster to ensure even distribution of data and optimal utilization of resources.

#### Security
- Authentication and Authorization: Supports Kerberos-based authentication and fine-grained authorization controls.
- Encryption: Provides data encryption both at rest and in transit.

#### Interfaces and Tools
- HDFS Shell: Command-line interface for interacting with HDFS (e.g., `hdfs dfs -ls /`).
- Web UI: Browser-based interface to monitor the health and status of the HDFS cluster.
- APIs: Provides APIs for accessing HDFS from applications written in Java, Python, and other languages.

#### Integration
- Hadoop Ecosystem: Integrates seamlessly with other Hadoop components like MapReduce, YARN, Hive, and HBase.
- Big Data Tools: Compatible with various big data processing tools and frameworks (e.g., Apache Spark, Apache Flink).

#### Advantages
- Fault Tolerance: High availability and reliability through replication and automatic failover.
- Scalability: Easily scales to accommodate growing data volumes.
- Cost-Effective: Utilizes commodity hardware to reduce storage costs.
- Performance: High throughput and efficient data access for large-scale data processing.

#### Limitations
- Small Files: Not optimized for storing a large number of small files due to overhead in metadata management.
- Latency: Higher latency compared to traditional file systems, making it less suitable for low-latency applications.

Understanding these key points about HDFS helps in effectively managing and leveraging it for big data storage and processing needs.