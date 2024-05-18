Scale from Zero to Millions of Users

Single Server Setup:

- Everything runs on a single server (web app, database, cache).
- Users access the website via domain names (e.g., api.mysite.com) using DNS.
- IP address (e.g., 15.125.23.214) returned to the browser or mobile app.
- HTTP requests are sent to the web server, which returns HTML or JSON responses.

Traffic Sources:

- Web application: 
    - Server-side languages (Java, Python) to handle business logic 
    - Client-side languages (HTML, JavaScript) for presentation
- Mobile application: Communicates with the web server using HTTP and typically returns JSON formatted data.

Database:

- Split servers for web/mobile traffic (web tier) and database (data tier) for independent scaling.
- Relational Databases (RDBMS): SQL-based, e.g., MySQL, Oracle, PostgreSQL.
    - represent and store data in tables and rows. 
    - can perform join operations using SQL across different database tables.
- Non-Relational Databases (NoSQL): NoSQL-based, e.g., CouchDB, Cassandra, HBase, DynamoDB.
    - Join operations are generally not supported in non-relational databases.

Scaling Types:

- Vertical Scaling (Scale Up): Add more power (CPU, RAM) to existing servers.
- Horizontal Scaling (Scale Out): Add more servers to the pool.

Load Balancer:

- Distributes incoming traffic among multiple web servers.
- Improves availability and redundancy:
    - Routes traffic to healthy servers.
    - Scales by adding more servers as needed.

Database Replication:

Master-Slave Model:
- Master handles write operations.
- Slaves handle read operations.
- Improves performance, reliability, and high availability.

Cache:

- Stores frequently accessed data in memory for faster response.
- Considerations:
    - Use cache for frequently read but infrequently modified data.
    - Implement an expiration policy to manage stale data.
    - Ensure data consistency between cache and data store.
    - Avoid single points of failure by using multiple cache servers.
    - Choose an appropriate eviction policy (e.g., LRU).

Content Delivery Network (CDN):

- Delivers static content from geographically dispersed servers.
- Improves load times by serving content from the nearest server.
- Considerations:
    - Manage costs by caching frequently accessed assets.
    - Set appropriate cache expiry times.
    - Implement fallback strategies for CDN failures.
    - Invalidate outdated content using APIs or object versioning.

Stateless Web Tier:

- Move state (e.g., session data) out of the web tier to persistent storage (database, NoSQL, Memcached).
- Enables easier horizontal scaling and auto-scaling based on traffic.

Multiple Data Centers:

- Use geoDNS to route traffic to the nearest data center.
- Replicate data across data centers for availability and resilience.
- Address challenges in traffic redirection, data synchronization, and deployment consistency.

Message Queue:

- Supports asynchronous communication between services.
- Decouples system components, allowing independent scaling of producers and consumers.

Logging, Metrics, Automation:

- Logging: Monitor error logs for troubleshooting.
- Metrics: Track system performance (CPU, memory, I/O) and business metrics (active users, revenue).
- Automation: Use continuous integration and automated deployment tools to improve productivity.

Database Scaling:

- Vertical Scaling: Add more power to the database server.
- Horizontal Scaling (Sharding): Distribute data across multiple servers.
    - Choose an appropriate sharding key to evenly distribute data.
    - Address challenges in resharding, hotspot keys, and join operations.

Summary:

- Keep the web tier stateless.
- Build redundancy at every tier.
- Cache data extensively.
- Support multiple data centers.
- Host static assets in a CDN.
- Scale the data tier by sharding.
- Split tiers into individual services.
- Monitor the system and use automation tools.