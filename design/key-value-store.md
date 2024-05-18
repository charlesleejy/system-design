Design a Key-Value Store

Key-Value Store Overview:
- Definition: Non-relational database storing unique keys with associated values.
- Keys: Plain text (e.g., "last_logged_in_at") or hashed (e.g., 253DDEC4).
- Values: Can be strings, lists, objects, etc.
- Operations: 
  - `put(key, value)` - Insert “value” associated with “key”.
  - `get(key)` - Retrieve “value” associated with “key”.

Design Characteristics:
- Small key-value pair size (<10 KB).
- Ability to store big data.
- High availability.
- High scalability.
- Automatic scaling based on traffic.
- Tunable consistency.
- Low latency.

Single Server Key-Value Store:
- Storage: Use a hash table in memory.
- Optimizations: 
  - Data compression.
  - Store frequently used data in memory, rest on disk.

Distributed Key-Value Store:
- Data Partition: Use consistent hashing to evenly distribute data and minimize movement.
- Data Replication: Replicate data across multiple servers (N servers).
- Consistency: Use quorum consensus for reads and writes.

CAP Theorem:
- Consistency: All clients see the same data at the same time.
- Availability: System responds to all requests, even if some nodes are down.
- Partition Tolerance: System continues to operate despite network partitions.
- Tradeoffs: Can only achieve 2 out of 3 properties (CP, AP, CA).

Consistency Models:
- Strong Consistency: Always returns the most recent write.
- Weak Consistency: May not return the most recent write.
- Eventual Consistency: Given enough time, all replicas will be consistent.

Inconsistency Resolution:
- Versioning: Treat each data modification as a new version.
- Vector Clocks: Track versions with [server, version] pairs to detect and resolve conflicts.

Handling Failures:
- Failure Detection: Use gossip protocol for decentralized failure detection.
- Temporary Failures: Use sloppy quorum and hinted handoff for availability.
- Permanent Failures: Use anti-entropy protocol and Merkle trees to synchronize replicas.

System Architecture:
- Coordinator: Acts as a proxy between clients and key-value store.
- Decentralization: Nodes are distributed on a ring using consistent hashing, and data is replicated across nodes.

Write Path:
1. Persist write request on a commit log.
2. Save data in memory cache.
3. Flush data to SSTable on disk when memory cache is full.

Read Path:
1. Check if data is in memory.
2. If not, use Bloom filter to find relevant SSTables on disk.
3. Retrieve data from SSTables and return to client.

Summary:
- Core Components and Techniques:
  - Data partition: Consistent hashing.
  - Data replication: Replicate across N servers.
  - Consistency: Quorum consensus, eventual consistency.
  - Inconsistency resolution: Vector clocks.
  - Failure handling: Gossip protocol, sloppy quorum, hinted handoff, anti-entropy protocol.
  - Read/Write path: Commit logs, memory cache, SSTables, Bloom filters.