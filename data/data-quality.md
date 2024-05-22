## Data quality checks

Data quality is a critical factor in ensuring the reliability and usefulness of data in decision-making and operational processes. Performing data quality checks is about ensuring that the data meets certain standards and criteria. Here's a comprehensive list of data quality checks ranging from simple to complex:

Basic Data Quality Checks

1. Accuracy Checks:
    - Ensure data accurately represents the real-world values they are expected to model.
    - Example: Check if the calculated totals match the sum of individual items.
    
2. Validity Checks:
    - Ensure data conforms to the specific syntax (format, type, range) defined for its domain.
    - Example: Dates are within a valid range and formatted correctly.
    
3. Completeness Checks:
    - Ensure all required data is present and there are no missing values where data is expected.
    - Example: No null values in mandatory fields like Primary Keys.
    
4. Consistency Checks:
    - Ensure data across different files, databases, or tables is consistent and does not contradict.
    - Example: The same customer ID should point to the same customer information in all related tables.
    
5. Uniqueness Checks:
    - Ensure that data records are unique and not duplicated unless allowed by the schema.
    - Example: No two users should have the same username.
    
6. Timeliness Checks:
    - Ensure data is up-to-date and available when needed.
    - Example: The last update timestamp should be within an expected range.


Intermediate Data Quality Checks

1. Referential Integrity Checks:
    - Ensure relationships between tables are consistent, and that foreign keys match primary keys in another table.
    - Example: All order records should correspond to a valid customer record.
    
2. Range and Boundary Checks:
    - Ensure data falls within the permissible range defined by business rules.
    - Example: The age field must be between 0 and 120.
    
3. Format Checks:
    - Ensure data is formatted correctly according to patterns.
    - Example: Phone numbers match a standard pattern (xxx) xxx-xxxx.
    
4. Enumeration Checks:
    - Ensure data fields that are restricted to a set of allowed values contain only those values.
    - Example: The status field should only contain 'New', 'In Progress', or 'Closed'.
    
Advanced Data Quality Checks

1. Cross-Field Validation:
    - Ensure the relationship between different fields of a record is logical and valid.
    - Example: A patient's date of discharge from the hospital should not be before the date of admission.
    
2. Checksums for Data Integrity:
    - Use checksums or hash values to ensure that a dataset has not been altered or corrupted.
    - Example: Verifying the checksum of a file to ensure it matches the expected value.
    
3. Dependency Checks:
    - Ensure certain conditions that must be met when data is in a particular state.
    - Example: If a 'discount' field is applied, a 'coupon code' field must not be empty.
    
4. Pattern Recognition:
    - Identify and flag data that does not match known patterns of legitimate data.
    - Example: Using regular expressions to detect unlikely or impossible combinations of characters in names or addresses.

Complex Data Quality Checks

1. Statistical Process Control:
    - Using statistical methods to monitor and control data quality.
    - Example: Identifying data points that fall outside of three standard deviations from the mean.
    
2. Data Profiling:
    - Analyzing datasets to collect statistics, identifying incorrect values.
    - Example: Identifying outliers or anomalies in data distributions.
    
3. Predictive Modelling:
    - Using machine learning models to predict the validity of data based on historical trends.
    - Example: A model that flags transactions as potentially fraudulent based on spending patterns.
    
4. Data Quality Scorecarding:
    - Creating a scorecard to track data quality metrics against predefined targets.
    - Example: A dashboard displaying the percentage of records passing all quality checks.
    
5. Linkage of Disparate Data:
    - Identifying and linking related records across disparate datasets.
    - Example: Matching customer records from different systems to a single entity.
    
6. Historical Data Validation:
    - Checking data against historical records for continuity and accuracy.
    - Example: Validating stock prices against a historical time series for abrupt changes.
    
7. Geospatial Data Validation:
    - Ensuring that geospatial data is accurate and consistent with real-world locations.
    - Example: Verifying if geo-coordinates fall within appropriate country boundaries.
    
8. Data Lineage Tracing:
    - Tracking the lineage of data to understand how data is transformed over time.
    - Example: Tracing back through ETL processes to find the source of an error.
    
9. Semantic Validation:
    - Ensuring that the data makes sense given its context and use.
    - Example: Product categories


## Data profiling for data quality checks

Data profiling is an essential step in assessing data quality, as it involves a thorough examination of the existing data in a database, data warehouse, or dataset. Here's an example of how data profiling can be applied to conduct data quality checks:

Context:
Imagine a company has a customer database that they want to analyze to ensure high data quality before using it for a marketing campaign.

Data Profiling Steps:

1. Column Statistics:
    - Calculate basic statistics for each column (e.g., count, max, min, mean, median).
    - For a customer database, you would look at statistics for columns like Age, Income, Last Purchase Date, etc.
    - Check for the presence of null or missing values in critical columns like Customer ID, Email Address, or Phone Number.
    
2. Data Type Check:
    - Verify that the data in each column matches the expected data type.
    - For example, ensure that the Phone Number column contains only numeric values or appropriately formatted strings and that Date of Birth is stored in a date format.
    
3. Value Distribution:
    - Analyze the distribution of values in a particular column.
    - For categorical data like Customer Status, check for the frequency and distribution of each status (e.g., active, inactive, prospective).
    
4. Pattern Check:
    - Identify common patterns within the data, particularly for textual data.
    - Use regular expressions to verify that Email Addresses adhere to the standard email format.
    
5. Range and Frequency Check:
    - Determine if numerical values fall within a logical range.
    - For instance, ensure that the Age column doesn't contain negative numbers or unlikely values (such as age > 120).
    
6. Uniqueness Check:
    - Assess if values expected to be unique, such as Customer ID or Email Address, are indeed unique.
    - Identify and flag any duplicate records.
    
7. Referential Integrity Check:
    - For databases with multiple related tables, ensure that foreign keys correctly reference primary keys in other tables.
    - Ensure that every Order ID in the Orders table has a corresponding Customer ID in the Customers table.
    
8. Cross-Column Validation:
    - Evaluate the logical relationship between multiple columns.
    - Confirm that Last Purchase Date is always after the Date Joined for each customer.
    
9. Data Consistency Check:
    - Compare data across different sources to ensure consistency.
    - If customer information is stored in multiple databases, check that it is the same in all locations.
    
10. Anomaly Detection:
    - Use statistical methods to identify outliers or anomalies in the data.
    - Unusually high transaction amounts or a sudden change in purchasing frequency could be signs of data entry errors or fraud.
    
Data Profiling Tools:

To automate and facilitate these checks, data engineers and analysts often use data profiling tools that can quickly analyze large datasets and produce reports detailing these aspects. Tools such as Informatica, Talend, and SQL-based queries can profile data and highlight areas that need attention.

Outcome:

The result of data profiling would be a detailed report outlining the state of the data across all these dimensions. Based on this report, the company can undertake data cleansing activities to correct the issues before deploying their marketing campaign, thus ensuring better results and compliance with data quality standards.
