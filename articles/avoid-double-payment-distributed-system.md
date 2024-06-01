## Avoiding Double Payments in a Distributed Payments System


### Avoiding Double Payments in a Distributed Payments System: Key Points

#### Background
- Migration to SOA: Airbnb transitioned to a Service Oriented Architecture (SOA) to enable developer specialization and faster iteration.
- Challenges in SOA: Maintaining data integrity in distributed systems, especially for billing and payments, is complex due to state changes and side effects across multiple services.

#### Common Techniques for Eventual Consistency
- Read Repair: Fixing inconsistencies during read operations.
- Write Repair: Fixing inconsistencies during write operations.
- Asynchronous Repair: Running periodic consistency checks and notifications to ensure data consistency.

#### Solution Overview
- Write Repair: Implemented to allow clients to achieve eventual consistency on-demand with idempotency.
- Idempotency: Ensures that multiple identical requests have the same effect as a single request, preventing double payments.

#### Problem Statement
- Requirements: Needed a generic, configurable idempotency solution for Airbnb's payments services without compromising data consistency or incurring high latency.

#### Implementation: Orpheus Idempotency Framework
- Unique Request Identification: Each request is identified uniquely with an idempotency key.
- Sharded Master Database: Idempotency information stored in a sharded master database to ensure consistency.
- Database Transactions: Combined with Java lambdas to ensure atomicity in different codebase parts.
- Error Handling: Classified errors as "retryable" or "non-retryable."

#### Key Practices
- Minimize Database Commits: Ensures consistency with clear success or failure outcomes.
- Three Phases:
  - Pre-RPC: Record payment request details in the database.
  - RPC: Make the request to the external service and receive a response.
  - Post-RPC: Record response details in the database.
- Avoid Mixing Database and Network Calls: Prevents issues like connection pool exhaustion and performance degradation.

#### Java Lambdas
- Combining Transactions: Use Java lambda expressions to combine multiple operations into a single database transaction seamlessly.

#### Exception Handling
- Retry Logic: Mark exceptions correctly as "retryable" or "non-retryable" to ensure proper retry behavior.
- Consistency in Request Payloads: Ensure payloads remain the same across retries.

#### Client Responsibilities
- Unique Idempotency Keys: Pass and persist unique idempotency keys for each request and reuse them for retries.
- No Payload Mutation: Ensure request payloads do not change between retries.
- Retry Strategies: Implement intelligent auto-retry strategies with mechanisms like exponential backoff.

#### Idempotency Key Selection
- Request-Level Idempotency: Use unique keys (e.g., UUID) for each request.
- Entity-Level Idempotency: Use deterministic keys based on the entity (e.g., "payment-1234-refund").

#### Lease Mechanism
- Database Row-Level Locks: Acquire locks on idempotency keys with expiration to prevent race conditions and double payments.

#### Recording Responses
- Persist Responses: Store responses in the database to handle retries efficiently, while managing table growth carefully.

#### Master vs. Replica Databases
- Use Master for Consistency: Read and write idempotency information only from the master database to avoid issues from replica lag.

#### Sharding for Scalability
- Shard by Idempotency Key: Distribute idempotency data across shards to handle scaling.

#### Final Thoughts
- Orpheus Effectiveness: Achieved high consistency with minimal latency, while doubling annual payment volume.
- Ongoing Improvements: Continuously enhance the framework for better support and performance. 


Reference:
https://medium.com/airbnb-engineering/avoiding-double-payments-in-a-distributed-payments-system-2981f6b070bb