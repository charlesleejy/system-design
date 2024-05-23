## Optimisation on Denodo

- Data Source Optimization:
  - Connection Pooling:
    - Configure connection pooling settings to reuse database connections efficiently, reducing overhead.
  - Query Pushdown:
    - Delegate processing tasks to underlying data sources to minimize data transfer overhead.
  - Parallel Execution:
    - Enable parallel execution of queries across multiple threads or nodes for faster results.

- Cache Optimization:
  - Cache Configuration:
    - Adjust cache settings based on data access patterns, determining cache size, eviction policies, and expiration times.
  - Cache Warm-Up:
    - Preload frequently accessed data into the cache during off-peak hours to minimize query latency.
  - Cache Invalidation:
    - Implement mechanisms to ensure cached data remains up-to-date with changes in underlying sources using triggers, notifications, or scheduled tasks.

- Query Optimization:
  - Query Rewrite:
    - Leverage query rewrite capabilities to optimize SQL queries, transforming complex queries into more efficient forms.
  - Materialized Views:
    - Create materialized views or derived tables to precompute and store intermediate query results, reducing redundant processing.
  - Query Plan Analysis:
    - Analyze query execution plans using monitoring tools to identify bottlenecks and fine-tune resource allocation.

- Data Integration Optimization:
  - Incremental Data Loading:
    - Implement techniques such as change data capture (CDC) or delta detection to synchronize data between Denodo and external systems incrementally.
  - Batch Processing:
    - Group similar data integration tasks to minimize overhead, scheduling batch jobs during off-peak hours.

- Resource Management:
  - Resource Allocation:
    - Configure resource pools and limits to effectively allocate computing resources among users, applications, or workloads.
  - Workload Management:
    - Implement policies to prioritize and manage query execution based on business-criticality or SLAs, optimizing resource allocation and throughput.

- Performance Monitoring and Tuning:
  - Monitoring Metrics:
    - Monitor Denodo performance metrics such as query execution time, cache hit ratio, and system throughput using built-in tools.
  - Performance Tuning Iteration:
    - Continuously review and refine optimization strategies based on evolving data sources, workload patterns, and usage scenarios to maintain optimal performance and scalability.


## Data federation optimisation on Denodo

- Query Optimization:
  - Query Rewrite:
    - Utilize Denodo's query rewrite capabilities to transform complex queries into more efficient forms, enhancing performance.
  - Join Strategy Selection:
    - Choose appropriate join strategies such as nested loop join, hash join, or merge join based on data source characteristics and join conditions to minimize overhead and maximize performance.
  - Query Execution Plan Analysis:
    - Analyze query execution plans using Denodo's monitoring tools to identify bottlenecks and optimize query execution strategies for improved efficiency.

- Data Source Optimization:
  - Connection Pooling:
    - Configure connection pooling settings to efficiently reuse database connections, reducing connection overhead.
  - Query Pushdown:
    - Delegate processing tasks to underlying data sources whenever possible to minimize data transfer overhead and leverage native capabilities.
  - Data Source Indexing:
    - Ensure proper indexing of data sources to optimize query performance, especially for large volumes of data.

- Caching Strategies:
  - Result Set Caching:
    - Cache query results or intermediate data sets to avoid redundant computations and improve query response time for frequently accessed data.
  - Cache Invalidation:
    - Implement cache invalidation mechanisms to ensure cached data remains up-to-date with changes in underlying data sources, maintaining data consistency.

- Data Integration Optimization:
  - Incremental Data Retrieval:
    - Implement incremental data retrieval techniques such as change data capture (CDC) or delta detection to synchronize data efficiently between Denodo and external sources.
  - Parallel Data Retrieval:
    - Configure Denodo to retrieve data from multiple sources in parallel to exploit parallelism and maximize throughput, especially for federated queries across large datasets.

- Resource Management:
  - Resource Allocation:
    - Configure resource pools and limits to allocate computing resources effectively among different federated queries, users, or applications.
  - Workload Management:
    - Implement workload management policies to prioritize and manage federated queries based on business-criticality or performance requirements, optimizing resource utilization.

- Performance Monitoring and Tuning:
  - Monitoring Metrics:
    - Monitor performance metrics such as query execution time, data transfer rates, and resource utilization using Denodo's built-in monitoring tools.
  - Performance Tuning Iteration:
    - Continuously review and refine optimization strategies based on evolving data sources and workload patterns to maintain optimal performance and scalability of data federation in Denodo.


## Optimizing Join Operations in Virtual DataPort

A key aspect of query optimization in Virtual DataPort is selecting the most suitable join method. Virtual DataPort automatically selects a method based on internal cost data, but users can force a specific method if necessary. Join execution strategies involve the method used and the order in which input views are considered.

### Supported Join Methods:

1. Merge Join:
   - The merge method is very often the fastest way and with the lowest memory footprint of joining two views that are sorted by the fields of the join condition.
   - Reads a row from each input view, evaluates the join condition, and discards rows not meeting the condition.
   - Requires input views to be sorted by join attributes.
   - The execution engine repeats this process until all the rows of one of the tables have been processed.
   - Even if the other view still has rows, there is no need to process them because they will not match with any other row.
   - Merge join is selected when:
     - JDBC or ODBC base view meets specific conditions related to collation and i18n.
        - The Source Configuration property “Supports binary ORDER BY collation” of the views is “yes”.
        - The views have the same i18n.

2. Nested Join:
    - The Nested join method, first, queries the input view of the left side of the join, to obtain the tuples that verify the join condition. Then, it executes a query to the view of the right side, for each combination of values obtained for the field that takes part in the join.
    - If the view of the right side of the join retrieves the data from a database (from a JDBC or an ODBC data source), Virtual DataPort will retrieve the data from this view “in blocks”, instead of executing a query for each row obtained from the first view.
    - To retrieve the data “in blocks” it executes a query with the syntax selected in the property “Nested join optimization syntax” of the data sources’ Source configuration. Its values can be:



   - Queries the left-side input view, then executes a query to the right-side view for each combination of values.
   - Supports optimization syntax like OR clause, IN clause, WITH clause, or subquery, depending on the database.
   - Retrieves data "in blocks" for views sourced from databases.

3. Nested Parallel Join:
   - Similar to Nested join but issues subqueries to the right-side view in parallel.
   - Accepts a parameter specifying the maximum number of parallel subqueries.
   - Usually less efficient than Nested join for database-sourced data.

4. Hash Join:
   - Efficient for joining unordered and large datasets, minimizing sub-query calls.
   - Suitable for data sources with high query latency, such as web sources.
   - Minimizes data transfer and processing overhead.

### Optimization Considerations:

- Ensure input views are properly sorted for Merge join.
- Select appropriate optimization syntax for Nested join based on database compatibility.
- Consider trade-offs between Nested and Nested Parallel join methods.
- Evaluate query latency and dataset characteristics to determine the suitability of Hash join.

By optimizing join operations in Virtual DataPort, users can enhance query performance and efficiency, ensuring optimal execution based on specific data characteristics and source types.

## Data Movement

- Purpose: Data Movement optimization in Virtual DataPort aims to execute federated queries more efficiently, especially when one view involved in the query is significantly larger than the other.
  
- Operations Improved: The optimization enhances the performance of operations such as Join, Union, Minus, and Intersect.

- Example Scenario: Consider a scenario where a database contains a large table of sales data (1 billion rows) and a smaller table of product information (1 million rows), with a query to calculate sales of products in a specific category.

- Usual Strategy: The typical approach would involve a Nested Join, where Virtual DataPort groups product IDs obtained from the product view and queries sales data accordingly.

- Data Movement Optimization: Alternatively, Virtual DataPort can transfer the smaller view's data (product information) into the data source of the larger view (sales data) and execute the operation there.

- Improved Execution:
  1. Query the product view with the specified condition (e.g., category = 'electronics') to obtain a small result set.
  2. Insert the obtained rows into a temporary table in the database containing the sales data.
  3. Delegate the entire query to the database with the sales data, resulting in a single query execution.
  4. Summarize or process the retrieved data as required to obtain the final result.

- Target Data Source: The target of data movement can be a JDBC data source or a database used by the Cache Engine, provided it supports the necessary operations.

- Configuration and Privileges:
  - DB link creation requires appropriate read and write privileges over the target data source.
  - Users executing queries involving data movement must have the necessary privileges or be an administrator.

- Execution Plan Configuration: Data movement for a view is defined in the Execution Plan tab of the Options dialog of the view.

- CONTEXT Clause Options:
  - Parameters such as data_movement_bulk_load, data_movement_clean_resources, and data_movement_clean_resources_on_error control aspects like bulk loading, resource cleanup, and error handling during data movement operations.

- Benefits: 
  - Improved query performance, especially for federated queries involving large and small data sets.
  - Reduction in data transfer overhead and latency, leading to faster query execution times.

- Considerations:
  - Security: Access to remote data sources should be restricted to authorized users.
  - Resource Usage: Careful management of resources, especially disk space, is necessary when dealing with large data sets and frequent data movements.


## Space and time complexity of different join algorithms 

- Nested Loop Join:
  - Explanation: In a nested loop join, each tuple in one relation is compared with every tuple in the other relation based on a specified condition. For each tuple in the outer relation, the inner relation is scanned completely.
  - Time Complexity: If there are n tuples in the outer relation and m tuples in the inner relation, the time complexity is O(n * m). This is because, for each tuple in the outer relation, the inner relation needs to be scanned entirely.
  - Space Complexity: Typically O(1) because it doesn't require additional space proportional to the input size.

- Merge Join:
  - Explanation: Merge join involves sorting both input relations based on the join attribute and then merging them together to find matches. It works efficiently when both relations are sorted on the join attribute.
  - Time Complexity: Sorting requires O(n log n + m log m) time, where n and m are the number of tuples in the input relations. Merging the sorted lists afterward takes O(n + m) time. Hence, the overall time complexity is O(n log n + m log m) for sorting and O(n + m) for merging.
  - Space Complexity: O(n + m) if external sorting is used, where n and m are the number of tuples in the input relations. It requires additional space to store the sorted lists.

- Hash Join:
  - Explanation: Hash join involves hashing the tuples of both input relations based on the join attribute and then probing the hash tables to find matching tuples. It's particularly efficient for large relations and unsorted data.
  - Time Complexity: Building the hash tables takes O(n + m) time, and probing the hash tables also takes O(n + m) time in the worst case. Therefore, the overall time complexity is O(n + m).
  - Space Complexity: O(n + m) in the worst case, as it requires additional space to store the hash tables.

- Index Join:
  - Explanation: Index join utilizes indexes on the join attribute to efficiently locate matching tuples. It can be highly efficient when indexes are available and the join attribute is indexed.
  - Time Complexity: Depends on the efficiency of index lookup. With efficient index structures and lookup, it can achieve O(n + m) time complexity. However, if sorting is required for index lookup, the time complexity becomes O(n log n + m log m) due to sorting.
  - Space Complexity: Additional space is required to store index structures, typically O(n + m), where n and m are the number of tuples in the input relations.

## Different join algorithms 

- Nested Loop Join:
  - Usage: Nested loop joins are typically used when one of the relations is small, and the other is much larger. It's suitable for joining tables when no indexes are available and when one of the tables is significantly smaller than the other.
  - Example: Suppose you have a small table of employee information (e.g., employee_id, name) and a much larger table of sales transactions (e.g., employee_id, sale_amount). In this case, you can use a nested loop join to match each employee's sales transactions efficiently.

- Merge Join:
  - Usage: Merge join is used when both input relations are already sorted on the join attribute. It's efficient for joining large data sets with sorted attributes.
  - Example: Consider two sorted lists of employee IDs and their corresponding departments. You can perform a merge join on these lists to efficiently match each employee with their department.

- Hash Join:
  - Usage: Hash join is suitable for joining large unsorted relations. It works well when the join attribute has a high cardinality, and when memory is sufficient to hold the hash tables.
  - Example: Suppose you have two large tables of customer data and order data. By hashing the customer ID in both tables, you can efficiently match customers with their orders using a hash join.

- Index Join:
  - Usage: Index join is used when indexes are available on the join attribute of both input relations. It's efficient for joining tables with indexed attributes.
  - Example: Consider two tables, one containing employee information (e.g., employee_id, name) and another containing department information (e.g., department_id, department_name). If both tables have indexes on the employee_id and department_id columns respectively, you can perform an index join to efficiently match employees with their departments based on their IDs.

## Cost-Based Optimization

- Enabling the Cost-Based Optimization:
  - Describes the process of enabling the cost-based optimization in Virtual DataPort.

- Gathering the Statistics of Views:
  - Explains how to obtain the statistics needed for the cost-based optimization process, including the number of rows, number of NULL values, etc.

- Tuning the Cost-Based Optimization Process:
  - Details the information used for generating cost estimations, such as database indexes and view statistics.
  - Provides tips for tuning the cost-based optimization process effectively.

- Current Limitations of the Cost-Based Optimization Process:
  - Discusses the existing limitations of the cost-based optimization feature in Virtual DataPort.


## Tuning Cost-Based Optimization

- Estimating the Cost of Queries on Data Sources:
  - Virtual DataPort estimates the cost of executing a subquery on a data source by considering:
    1. The estimated cost of processing the query within the data source, mainly based on the estimated number of I/O operations performed by the data source.
    2. The estimated cost of transferring the obtained results through the network, influenced by the size of the result set.

  - To generate accurate cost estimations, Virtual DataPort requires:
    - View statistics, including the total number of rows and field statistics for all fields used in the query.
    - Available indexes for the query and their type, as indexes can significantly impact the number of I/O operations.
    - I/O parameters of the data source, such as block size and multi-block read count, which influence the number of I/O operations performed.

- Estimating Post-Processing Costs:
  - Virtual DataPort estimates the cost of post-processing operations, such as joins and aggregations, based on the input and output rows (and their size) of each operation.
  - Accurate estimations require statistics for all views pushed down to the data source, including leaf views, and optionally for derived views like flatten views and views with derived attributes.

- Specifying Indexes:
  - Virtual DataPort considers indexes over base views to estimate query costs. It distinguishes between different index types, including clustered, hash, and other types.
  - Accurate index information is crucial for cost estimation, and Virtual DataPort automatically introspects indexes from supported databases. However, manual modification may be necessary in specific cases.

- Data Source I/O Parameters:
  - The number of I/O operations performed by the data source is influenced by parameters such as block size and multi-block read count.
  - Virtual DataPort provides default values for common configurations but allows modification of these parameters in the data source configuration to match specific database configurations accurately.