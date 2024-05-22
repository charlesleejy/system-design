## DESIGN A URL SHORTENER

### Step 1 - Understand the Problem and Establish Design Scope

1. Clarification Questions:
   - Example of URL shortener:
     - Original URL: `https://www.systeminterview.com/q=chatsystem&c=loggedin&v=v3&l=long`
     - Shortened URL: `https://tinyurl.com/y7keocwj`
   - Traffic volume: 100 million URLs generated per day.
   - Shortened URL length: As short as possible.
   - Allowed characters: Numbers (0-9) and characters (a-z, A-Z).
   - Deletion/Update of URLs: Not allowed.

2. Basic Use Cases:
   - URL shortening: Given a long URL, return a shorter URL.
   - URL redirecting: Given a short URL, redirect to the original URL.
   - High availability, scalability, and fault tolerance.

3. Back of the Envelope Estimation:
   - Write operations: 100 million URLs/day.
   - Write operations per second: \( \frac{100,000,000}{24 \times 3600} = 1160 \) writes/second.
   - Read operations: Assuming a 10:1 read-to-write ratio, 11600 reads/second.
   - 10-year storage requirement: 365 billion records.
   - Average URL length: 100 bytes.
   - Storage requirement over 10 years: \( 365 \text{ billion} \times 100 \text{ bytes} = 365 \text{ TB} \).

### Step 2 - Propose High-Level Design and Get Buy-In

1. API Endpoints:
   - URL Shortening:
     - POST `/api/v1/data/shorten`
     - Request parameter: `{ longUrl: longURLString }`
     - Response: `shortURL`
   - URL Redirecting:
     - GET `/api/v1/shortUrl`
     - Response: `longURL` for HTTP redirection

2. URL Redirecting:
   - 301 Redirect: Permanent redirect, browser caches response, reduces server load.
   - 302 Redirect: Temporary redirect, tracks click rate and source, useful for analytics.
   - Implementation: Use hash tables to store `<shortURL, longURL>` pairs.
     - Get longURL: `longURL = hashTable.get(shortURL)`
     - Perform URL redirect.

3. URL Shortening:
   - Short URL format: `www.tinyurl.com/{hashValue}`
   - Hash function requirements:
     - Each `longURL` must map to one `hashValue`.
     - Each `hashValue` must map back to `longURL`.

### Step 3 - Design Deep Dive


1. Data Model:
   - Everything stored in hash table, but not feasible as memory resources are limited and expensive.
   - Use a relational database to store `<shortURL, longURL>` mappings.
   - Table columns: `id`, `shortURL`, `longURL`.

2. Hash Function:
   - Hash Value Length:
     - Characters: [0-9, a-z, A-Z], total 62 characters.
     - Length `n`: Find the smallest `n` such that \( 62^n \geq 365 \text{ billion} \).
     - \( n = 7 \) (62^7 ≈ 3.5 trillion).
   - Hash Functions:
     - Hash + Collision Resolution: Use CRC32, MD5, SHA-1; resolve collisions with predefined strings.
     - Base 62 Conversion: Convert numbers to base 62.

3. URL Shortening Flow:
   - Input: `longURL`.
   - Check if `longURL` exists in the database.
   - If exists: Fetch and return `shortURL`.
   - If not: Generate unique ID, convert to `shortURL` using base 62, save in database.

4. Distributed Unique ID Generator:
   - Generate globally unique IDs for short URLs.

5. URL Redirecting Flow:
   - User clicks short URL.
   - Load balancer forwards request to web servers.
   - Check cache for `shortURL`.
   - If not in cache: Fetch `longURL` from database.
   - Return `longURL` to user.

### Step 4 - Wrap Up

1. Additional Talking Points:
   - Rate Limiter: Prevent malicious users from overwhelming the system.
   - Web Server Scaling: Stateless web tier, easy to scale by adding/removing servers.
   - Database Scaling: Use replication and sharding.
   - Analytics: Track user interactions, click rates, and more.
   - Availability, Consistency, and Reliability: Core concepts for system success.