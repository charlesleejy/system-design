## DESIGN A WEB CRAWLER

### Step 1 - Understand the Problem and Establish Design Scope

1. Clarification Questions:
   - Main Purpose:
     - Candidate: What is the main purpose of the crawler? Is it used for search engine indexing, data mining, or something else?
     - Interviewer: Search engine indexing.
   - Number of Web Pages:
     - Candidate: How many web pages does the web crawler collect per month?
     - Interviewer: 1 billion pages.
   - Content Types:
     - Candidate: What content types are included? HTML only or other content types such as PDFs and images as well?
     - Interviewer: HTML only.
   - New or Edited Pages:
     - Candidate: Shall we consider newly added or edited web pages?
     - Interviewer: Yes, we should consider the newly added or edited web pages.
   - Storage Duration:
     - Candidate: Do we need to store HTML pages crawled from the web?
     - Interviewer: Yes, up to 5 years.
   - Duplicate Content:
     - Candidate: How do we handle web pages with duplicate content?
     - Interviewer: Pages with duplicate content should be ignored.

2. Characteristics of a Good Web Crawler:
   - Scalability:
     - Efficiently handle billions of web pages using parallelization.
   - Robustness:
     - Handle bad HTML, unresponsive servers, crashes, and malicious links.
   - Politeness:
     - Avoid making too many requests to a website within a short time interval.
   - Extensibility:
     - Support new content types with minimal changes.

3. Back of the Envelope Estimation:
   - Web pages per month: 1 billion.
   - QPS: \( \frac{1,000,000,000}{30 \times 24 \times 3600} \approx 400 \) pages/second.
   - Peak QPS: 800 pages/second.
   - Average web page size: 500k.
   - Storage per month: 500 TB.
   - Storage for 5 years: 30 PB.

### Step 2 - Propose High-Level Design and Get Buy-In

1. Design Components:
   - Seed URLs:
     - Starting point for the crawl process.
   - URL Frontier:
     - FIFO queue storing URLs to be downloaded.
   - HTML Downloader:
     - Downloads web pages.
   - DNS Resolver:
     - Translates URLs to IP addresses.
   - Content Parser:
     - Parses and validates web pages.
   - Content Seen?:
     - Eliminates duplicate content.
   - Content Storage:
     - Stores HTML content.
   - URL Extractor:
     - Extracts links from HTML pages.
   - URL Filter:
     - Excludes certain content types and blacklisted URLs.
   - URL Seen?:
     - Tracks visited URLs.
   - URL Storage:
     - Stores visited URLs.

2. Web Crawler Workflow:
   - Step 1: Add seed URLs to the URL Frontier.
   - Step 2: HTML Downloader fetches URLs from URL Frontier.
   - Step 3: HTML Downloader gets IP addresses from DNS resolver and starts downloading.
   - Step 4: Content Parser parses HTML pages.
   - Step 5: Content is checked by "Content Seen?".
     - If duplicate, discard.
     - If not, pass to URL Extractor.
   - Step 6: URL Extractor extracts links.
   - Step 7: Extracted links are filtered by URL Filter.
   - Step 8: Filtered links are checked by "URL Seen?".
     - If visited, ignore.
     - If not, add to URL Frontier.

### Step 3 - Design Deep Dive

1. DFS vs BFS:
   - BFS Preferred:
     - BFS is preferred for web crawling.
   - Problems with Standard BFS:
     - Many links from the same host.
     - No URL prioritization.

2. URL Frontier:
   - Politeness:
     - Avoid overwhelming web servers by sending too many requests.
     - Use FIFO queues per host.
     - Map hosts to queues.
   - Priority:
     - Prioritize URLs based on usefulness, PageRank, web traffic, update frequency.
     - Use prioritizer to compute URL priorities.
     - Select queues with higher priority.
   - Freshness:
     - Recrawl based on update history and prioritize important pages.
   - Storage:
     - Hybrid approach: memory for buffers, disk for majority storage.

3. HTML Downloader:
   - Robots.txt:
     - Follow rules specified by websites.
   - Performance Optimization:
     - Distributed Crawl:
       - Multiple servers and threads.
     - Cache DNS Resolver:
       - Maintain DNS cache.
     - Locality:
       - Geographically distribute crawl servers.
     - Short Timeout:
       - Max wait time for unresponsive servers.

4. Robustness:
   - Consistent Hashing:
     - Load distribution.
   - Save Crawl States and Data:
     - Failure recovery.
   - Graceful Exception Handling:
     - Prevent crashes.
   - Data Validation:
     - Prevent system errors.

5. Extensibility:
   - Add New Modules:
     - Support new content types (e.g., PNG Downloader, Web Monitor).

6. Detect and Avoid Problematic Content:
   - Redundant Content:
     - Use hashes to detect duplicates.
   - Spider Traps:
     - Set maximal length for URLs, identify traps manually.
   - Data Noise:
     - Exclude advertisements, code snippets, spam URLs.

### Step 4 - Wrap Up

1. Characteristics of a Good Crawler:
   - Scalability
   - Politeness
   - Extensibility
   - Robustness

2. Additional Topics:
   - Server-side Rendering:
     - Handle dynamically generated links.
   - Filter Out Unwanted Pages:
     - Use anti-spam components.
   - Database Replication and Sharding:
     - Improve data layer availability, scalability, and reliability.
   - Horizontal Scaling:
     - Use stateless servers.
   - Availability, Consistency, and Reliability:
     - Core concepts for system success.
   - Analytics:
     - Collect and analyze data for fine-tuning.

### Detailed Descriptions:

1. Seed URLs:
   - Purpose:
     - Starting point for crawling.
   - Selection Strategy:
     - Based on locality (different countries' popular sites).
     - Based on topics (shopping, sports, healthcare).

2. URL Frontier:
   - Split Crawl State:
     - To be downloaded.
     - Already downloaded.
   - Implementation:
     - First-in-First-out (FIFO) queue.

3. HTML Downloader:
   - Function:
     - Downloads web pages from provided URLs.
   - Optimizations:
     - Distributed crawl.
     - Cache DNS Resolver.
     - Locality.
     - Short timeout for slow/unresponsive servers.

4. DNS Resolver:
   - Function:
     - Translates URLs to IP addresses.
   - Example:
     - `www.wikipedia.org` to `198.35.26.96`.

5. Content Parser:
   - Function:
     - Parses and validates downloaded HTML pages.
   - Separation:
     - Separate component to avoid slowing down crawl process.

6. Content Seen?:
   - Function:
     - Eliminates duplicate content by checking hash values.
   - Efficiency:
     - Compare hash values instead of character-by-character comparison.

7. Content Storage:
   - Storage Types:
     - Disk for most content.
     - Memory for popular content to reduce latency.

8. URL Extractor:
   - Function:
     - Parses and extracts links from HTML pages.
   - Example:
     - Converts relative paths to absolute URLs.

9. URL Filter:
   - Function:
     - Excludes certain content types, file extensions, error links, and blacklisted URLs.

10. URL Seen?:
    - Function:
      - Keeps track of visited URLs.
    - Implementation:
      - Bloom filter or hash table.

11. URL Storage:
    - Function:
      - Stores already visited URLs.

### Workflow Details:

1. Step 1:
   - Add seed URLs to the URL Frontier.

2. Step 2:
   - HTML Downloader fetches URLs from URL Frontier.

3. Step 3:
   - HTML Downloader gets IP addresses from DNS resolver and starts downloading.

4. Step 4:
   - Content Parser parses HTML pages.

5. Step 5:
   - Content is checked by "Content Seen?".
   - If duplicate, discard.
   - If not, pass to URL Extractor.

6. Step 6:
   - URL Extractor extracts links.

7. Step 7:
   - Extracted links are filtered by URL Filter.

8. Step 8:
   - Filtered links are checked by "URL Seen?".
   - If visited, ignore.
   - If not, add to URL Frontier.

### Design Deep Dive:

1. DFS vs BFS

:
   - DFS:
     - Not suitable for web crawling due to deep depth.
   - BFS:
     - Preferred method, implemented using FIFO queue.
   - Problems with Standard BFS:
     - Links from the same host.
     - No URL prioritization.

2. URL Frontier:
   - Politeness:
     - Avoid overwhelming web servers.
     - Use FIFO queues per host.
   - Priority:
     - Prioritize URLs based on usefulness, PageRank, web traffic, update frequency.
   - Freshness:
     - Recrawl based on update history, prioritize important pages.
   - Storage:
     - Hybrid approach: memory for buffers, disk for majority storage.

3. HTML Downloader:
   - Robots.txt:
     - Follow rules specified by websites.
   - Performance Optimization:
     - Distributed crawl: Multiple servers and threads.
     - Cache DNS Resolver: Maintain DNS cache.
     - Locality: Geographically distribute crawl servers.
     - Short timeout: Max wait time for unresponsive servers.

4. Robustness:
   - Consistent Hashing:
     - Load distribution.
   - Save Crawl States and Data:
     - Failure recovery.
   - Graceful Exception Handling:
     - Prevent crashes.
   - Data Validation:
     - Prevent system errors.

5. Extensibility:
   - Add New Modules:
     - Support new content types (e.g., PNG Downloader, Web Monitor).

6. Detect and Avoid Problematic Content:
   - Redundant Content:
     - Use hashes to detect duplicates.
   - Spider Traps:
     - Set maximal length for URLs, identify traps manually.
   - Data Noise:
     - Exclude advertisements, code snippets, spam URLs.

### Wrap Up:

1. Characteristics of a Good Crawler:
   - Scalability
   - Politeness
   - Extensibility
   - Robustness

2. Additional Topics:
   - Server-side Rendering:
     - Handle dynamically generated links.
   - Filter Out Unwanted Pages:
     - Use anti-spam components.
   - Database Replication and Sharding:
     - Improve data layer availability, scalability, and reliability.
   - Horizontal Scaling:
     - Use stateless servers.
   - Availability, Consistency, and Reliability:
     - Core concepts for system success.
   - Analytics:
     - Collect and analyze data for fine-tuning.