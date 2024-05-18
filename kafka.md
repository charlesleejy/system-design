Apache Kafka Overview

Apache Kafka is a distributed streaming platform designed for real-time data pipelines and streaming applications. It was developed by LinkedIn and is managed by the Apache Software Foundation.

Core Concepts

Topics:
- Categories or feeds of messages.
- Producers write to and consumers read from topics.
- Can be partitioned for parallel processing and distributed storage.

Producers:
- Publish (write) data to Kafka topics.
- Distribute messages across topic partitions.

Consumers:
- Read data from topics.
- Subscribe to topics or partitions.
- Can form consumer groups for parallel and distributed processing.

Brokers:
Servers in a Kafka cluster.
Handle topic partitions, storage, and replication.
Ensure fault tolerance and high availability.
Partitions:

Divisions within topics.
Ordered logs of records.
Enable parallel processing by distributing across brokers.
Offsets:

Unique identifiers for messages within partitions.
Track consumer progress in a topic.


Key Features

Scalability:
Handles large-scale data streams.
Distributes data across multiple brokers.
Supports horizontal scalability.
Fault Tolerance:

Highly available and fault-tolerant.
Replicates data across multiple brokers.
Prevents data loss even if brokers fail.
Durability:

Persistent storage of messages.
Allows consumers to read past data.
Stream Processing:

Integrates with frameworks like Apache Flink and Apache Spark.
Enables real-time analytics, transformations, and computations.
Ecosystem Integration:

Rich ecosystem with connectors and integrations.
Easy integration with existing data infrastructure.
Summary
Kafka is ideal for real-time data processing due to its high throughput, fault tolerance, and scalability.
Utilizes a publish-subscribe model with producers, consumers, topics, and partitions.
Supports robust stream processing and integration with various data sources and tools.

Apache Kafka Overview

Apache Kafka is a distributed streaming platform for building real-time data pipelines and streaming apps, capable of handling trillions of events a day.

Core Components of Kafka

Producer:
- Publishes events to Kafka topics.
- Sends data to specific topics along with optional keys to determine partition placement.

Consumer:
- Reads data from topics.
- Subscribes to one or more topics and processes records.
- Operates in consumer groups, where each consumer reads exclusive partitions, providing scalability and fault tolerance.

Broker:
- Servers in a Kafka cluster.
- Each broker contains specific topic log partitions.
- Handles client requests (from producers and consumers) and persists data to disk.

ZooKeeper:
- Manages and coordinates Kafka brokers.
- Elects leaders among brokers and tracks partition and replica status.
Note: Starting with Kafka 2.8.0, Kafka can run without ZooKeeper using KRaft (Kafka Raft Metadata mode).

Topic:
- A category or feed name for published records.
- Multi-subscriber: can have multiple consumers.

Partition:
- Topics are split into partitions, ordered sequences of records (commit logs).
- Partitions are distributed across brokers.

How Kafka Works: Step-by-Step

Data Writing (Producing):
- Publishing Data: Producers publish records (key, value, timestamp) to topics.
- Partitioning: Records are sent to partitions, determined by round-robin or partition function (e.g., hash of key).

Data Storage:
- Log Storage: Partitions are log files on the broker’s file system.
- Segment Files: Partitions are segmented into files of similar size, containing subsets of records.
- Replication: Partitions are replicated across brokers for fault tolerance. Leaders handle reads/writes; followers replicate leaders.

Data Reading (Consuming):
- Consumption: Consumers pull data from subscribed topics.
- Offset Management: Offsets track consumed records and are committed to the __consumer_offsets topic for recovery.
- Balancing Load: Consumers in a group read from exclusive partitions. Failed consumer partitions are reassigned.

Retention and Cleanup:
- Retention Policies: Configurable based on time or size. Old data is purged.
- Compaction: Log compaction retains only the last known value for each record key within the partition for log-compacted topics.

Conclusion

Kafka’s architecture enables efficient real-time processing of data streams. Its distributed nature and robust replication mechanism ensure high availability and fault tolerance, making Kafka ideal for high-throughput, low-latency messaging systems.


Why is Kafka Fast?

Apache Kafka is known for its high throughput and scalability due to several architectural and design elements:

1. Disk-Based Storage

- Sequential Disk Access: Writes data to disk sequentially, optimizing modern disk technology.
- Persistent Data: Data remains on disk, leveraging the OS page cache.
- Zero Copy: Uses sendfile() system call to transfer bytes directly from the filesystem cache to the network socket, reducing CPU usage and context switches.

2. Partitioning

- Parallelism: Partitions hosted on different servers allow multiple consumers to read in parallel, increasing scalability and fault tolerance.
- Throughput: Distributes data across partitions and disks, sustaining higher total throughput.

3. Batching and Compression

- Batching: Allows producers to push many records together, reducing network calls and overhead.
- Compression: Supports compressing message batches, reducing data size for faster transfer and storage.

4. Horizontal Scalability

- Broker Scalability: Adding brokers increases throughput and storage capacity.
- Consumer Scalability: Consumer groups read from exclusive partitions, enabling load balancing.

5. Minimal CPU Usage

- Efficient Serialization/Deserialization: Simple message format for quick processing, keeping CPU usage low.
- Reduced I/O Operations: Sequential I/O and zero-copy optimization minimize CPU impact.

6. Simplified Message Retention

- Time and Space Bounded Storage: Configurable retention settings prevent space issues and ensure performance.
- Log Compaction: Retains only the last value for each key, preserving space and ensuring faster reads and writes.

Conclusion

Kafka's architecture is optimized for high throughput and efficiency, making it ideal for real-time data processing. It leverages hardware and OS capabilities, along with intelligent data storage, partitioning, and network transfer strategies, ensuring superior performance and scalability.
