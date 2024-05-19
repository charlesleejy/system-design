### Types of Indexing and When to Use Each

Definition:
- Indexing is a database optimization technique that improves the speed of data retrieval operations on a database table.

### 1. Primary Index

- Definition: Created on the primary key of a table.
- Characteristics: Automatically unique and clustered.
- Use Case: Ensures each record is uniquely identifiable. Use when you need to uniquely identify each record.

### 2. Secondary Index

- Definition: Created on non-primary key columns.
- Characteristics: Can be unique or non-unique, typically non-clustered.
- Use Case: Speeds up queries on columns other than the primary key. Use when queries frequently access non-primary key columns.

### 3. Clustered Index

- Definition: Data rows are stored physically in the order of the indexed columns.
- Characteristics: Only one per table, significant impact on physical storage order.
- Use Case: Optimizes retrieval of range queries. Use when range queries or sorting on the indexed columns are frequent.

### 4. Non-Clustered Index (Default)

- Definition: Creates a separate structure to store index data that points to the actual data rows.
- Characteristics: Multiple non-clustered indexes per table allowed.
- Use Case: Speeds up searches and queries without altering the physical order of data. Use when you need to improve query performance on specific columns.

### 5. Unique Index

- Definition: Ensures that all values in the indexed column(s) are unique.
- Characteristics: Automatically prevents duplicate values.
- Use Case: Enforces uniqueness on non-primary key columns. Use when you need to ensure column values are unique.

### 6. Composite Index

- Definition: Index created on two or more columns of a table.
- Characteristics: Useful for queries involving multiple columns.
- Use Case: Optimizes multi-column search conditions. Use when queries frequently filter or sort based on multiple columns.

### 7. Bitmap Index (Oracle)

- Definition: Uses bitmaps for indexing.
- Characteristics: Efficient for columns with a low cardinality.
- Use Case: Data warehousing and environments with large datasets. Use when columns have a low number of distinct values and you need to perform complex queries.

### 8. Function-Based Index

- Definition: Indexes based on expressions or functions.
- Characteristics: Allows indexing on computed columns.
- Use Case: Queries involving calculated fields. Use when queries involve expressions or functions on columns.

### 9. Full-Text Index

- Definition: Indexes large text data.
- Characteristics: Optimized for searching text fields.
- Use Case: Search engines, document retrieval systems. Use when you need to perform full-text searches on large text fields.

### 10. Spatial Index

- Definition: Used for indexing spatial data.
- Characteristics: Supports queries related to spatial data (e.g., GIS).
- Use Case: Geographic Information Systems, spatial data analysis. Use when you need to query spatial data efficiently.

### 11. Reverse Index

- Definition: Indexes the reverse of each value.
- Characteristics: Optimizes suffix-based queries.
- Use Case: Full-text search for suffixes and patterns. Use when you need to search for patterns or suffixes in text fields.

### 12. Partial Index

- Definition: Indexes a subset of rows in a table.
- Characteristics: Based on a condition or filter.
- Use Case: Optimizes performance for queries on frequently accessed data subsets. Use when only a portion of the table is frequently queried.

### Summary

- Primary Index: Use for unique identification of records.
- Secondary Index: Use for speeding up queries on non-primary key columns.
- Clustered Index: Use for optimizing range queries and sorting.
- Non-Clustered Index: Use for improving query performance on specific columns.
- Unique Index: Use for ensuring uniqueness on non-primary key columns.
- Composite Index: Use for multi-column search conditions.
- Bitmap Index: Use for low cardinality columns in data warehousing.
- Function-Based Index: Use for queries involving expressions or computed columns.
- Full-Text Index: Use for full-text searches on large text fields.
- Spatial Index: Use for querying spatial data.
- Reverse Index: Use for suffix-based and pattern searches.
- Partial Index: Use for frequently queried subsets of a table.