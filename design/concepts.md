System Design Concepts

Sections and Concepts

1. Networks & Protocols (IP, DNS, HTTP, TCP, etc.)
- IP (Internet Protocol)
  - Function: Fundamental protocol for data communication across networks.
  - Structure: Data is sent in packets containing a header (metadata like source and destination IP addresses) and the actual data.
  - IP Address: A unique identifier for each device on a network.
- TCP (Transmission Control Protocol)
  - Purpose: Ensures reliable and ordered delivery of data packets over IP.
  - Mechanism: Establishes a connection using a handshake process before transmitting data.
  - Components: Adds a TCP header to each packet for managing data order and retransmission of lost packets.
- HTTP (Hyper Text Transfer Protocol)
  - Role: A higher-level protocol for web communication, built on TCP/IP.
  - Request-Response Pattern: Clients (e.g., web browsers) send requests, and servers respond with data.
  - HTTP Methods: Common methods include GET (retrieve data), POST (submit data), PUT (update data), DELETE (remove data).

2. Storage, Latency & Throughput
- Storage
  - Memory Storage: Temporary, fast, and used for data that doesn’t need to persist after power loss (e.g., RAM).
  - Disk Storage: Persistent, slower than memory, used for long-term data storage (e.g., SSDs, HDDs).
- Latency
  - Definition: The time taken for data to travel from source to destination.
  - Example: The time for a user’s request to reach a server and for the server to send back a response.
  - Optimization: Reduce latency by using faster storage, closer servers, and efficient data structures.
- Throughput
  - Definition: The amount of data processed in a given time frame (e.g., requests per second).
  - Example: An internet connection of 100 Mbps means it can handle 100 megabits of data per second.
  - Improvement: Increase throughput by scaling horizontally (adding more servers) or vertically (enhancing server capacity).

3. Availability
- High Availability (HA)
  - Concept: Systems designed to be operational continuously with minimal downtime.
  - Techniques: Redundancy (multiple components performing the same function), load balancing, failover mechanisms.
- Quantifying Availability
  - Uptime Percentage: Commonly expressed as a percentage of time a system is operational (e.g., 99.999% uptime).
  - SLAs (Service Level Agreements): Contracts guaranteeing a certain level of service availability, often with penalties for not meeting targets.

4. Caching
- Purpose: Stores frequently accessed data in a fast storage medium to reduce access time.
- Levels: 
  - Client-Side: Browser caching.
  - Server-Side: In-memory caching (e.g., Redis).
  - Between Client and Server: Content Delivery Networks (CDNs).
- Handling Stale Data
  - Challenges: Ensuring cached data is up-to-date with the main data store.
  - Strategies: Cache invalidation policies, TTL (Time To Live) settings, asynchronous updates.

5. Proxies
- Forward Proxy
  - Function: Acts on behalf of the client, masking the client's identity from the server.
  - Use Case: Accessing restricted content, improving security.
- Reverse Proxy
  - Function: Acts on behalf of the server, handling client requests.
  - Benefits: Load balancing, SSL termination, security enhancement.

6. Load Balancing
- Purpose: Distributes incoming traffic across multiple servers to ensure no single server is overwhelmed.
- Strategies:
  - Round Robin: Distributes requests sequentially among servers.
  - Weighted Round Robin: Assigns weights to servers based on their capacity.
  - Least Connections: Directs traffic to the server with the fewest active connections.
  - IP Hashing: Uses the client’s IP address to determine the server.
- Use Case: Ensures high availability and reliability for web applications.

7. Consistent Hashing
- Problem with Simple Hashing: Adding or removing servers significantly changes the hash distribution, causing many cache misses.
- Solution: Consistent hashing reduces the impact of server changes.
  - Mechanism: Maps both servers and requests to a circular hash space.
  - Benefit: Only a small portion of keys need to be remapped when a server is added or removed.

8. Databases
- Relational Databases
  - Structure: Data is organized in tables with rows and columns, enforcing relationships (e.g., PostgreSQL).
  - SQL: Structured Query Language used for managing relational databases.
  - ACID Properties: Ensures reliable transactions (Atomicity, Consistency, Isolation, Durability).
- Non-Relational Databases (NoSQL)
  - Types: Key-value stores, document databases, wide-column stores, graph databases.
  - Flexibility: Allows for varied data structures (e.g., MongoDB).
  - BASE Properties: Basically Available, Soft State, Eventual Consistency.
- Indexing
  - Purpose: Speeds up data retrieval by creating data structures (indexes) for quick lookups.
- Replication and Sharding
  - Replication: Duplicates data across multiple servers for redundancy.
  - Sharding: Divides a large database into smaller, manageable pieces (shards).

9. Leader Election
- Purpose: Designates a single server to act as the leader for specific tasks, ensuring coordinated actions.
- Mechanism: Uses consensus algorithms (e.g., Paxos, Raft) to elect a leader among servers.
- Example: etcd uses leader election to maintain consistency and high availability.

10. Polling, Streaming, Sockets
- Polling
  - Definition: Clients repeatedly request data from a server at regular intervals.
  - Drawbacks: Can lead to high network traffic and server load.
- Streaming
  - Definition: Continuous data transfer between client and server.
  - Mechanism: Uses web-sockets to maintain a long-lived connection for real-time communication.
- Use Case: Real-time applications like collaborative tools, multiplayer games.

11. Endpoint Protection
- Rate Limiting
  - Purpose: Controls the number of requests a client can make in a given time period.
  - Use Case: Prevents abuse, mitigates DoS attacks.
- Implementation: Uses algorithms like token bucket or leaky bucket to enforce limits.

12. Messaging & Pub-Sub
- Pub-Sub Model
  - Components: Publishers send messages, subscribers receive messages, topics categorize messages.
  - Benefits: Decouples producers and consumers, ensures reliable message delivery.
- Use Case: Event-driven architectures, asynchronous task processing.
- Example Services: Apache Kafka, RabbitMQ, Google Cloud Pub/Sub, AWS SNS/SQS.

13. Smaller Essentials
- Logging
  - Purpose: Collects data on system performance, errors, and user actions.
  - Use: Debugging, monitoring, analytics.
- Monitoring
  - Purpose: Analyzes log data to ensure system health and performance.
  - Tools: Dashboards, alerting systems.
- Alerting
  - Purpose: Notifies of significant events or metrics exceeding thresholds.
  - Use Case: Early detection of issues, proactive system management.