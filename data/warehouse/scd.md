### Slowly Changing Dimension (SCD)

In data warehousing and business intelligence, a **Slowly Changing Dimension (SCD)** is a dimension that changes slowly over time, rather than changing on a regular schedule, time-base. Handling these changes is crucial for maintaining accurate historical data and enabling effective analysis and reporting. There are several methods for managing SCDs, each with its own use cases and trade-offs.

### Types of Slowly Changing Dimensions

1. **Type 0: Retain Original**
2. **Type 1: Overwrite**
3. **Type 2: Add New Row**
4. **Type 3: Add New Attribute**
5. **Type 4: Add Historical Table**
6. **Type 6: Hybrid (Type 1 + Type 2 + Type 3)**

### Examples and Explanations

#### Type 0: Retain Original

Type 0 SCDs do not change over time. Once the data is inserted into the dimension table, it remains unchanged.

**Example:**

- Original Data: `Customer ID: 1, Name: John Doe, City: New York`
- Updated Data: (No updates allowed)

The customer's city remains "New York" even if they move to a different city.

#### Type 1: Overwrite

Type 1 SCDs simply overwrite the old data with the new data. No historical data is preserved.

**Example:**

- Original Data: `Customer ID: 1, Name: John Doe, City: New York`
- Updated Data: `Customer ID: 1, Name: John Doe, City: Los Angeles`

**Table Before Update:**

| Customer ID | Name     | City      |
|-------------|----------|-----------|
| 1           | John Doe | New York  |

**Table After Update:**

| Customer ID | Name     | City         |
|-------------|----------|--------------|
| 1           | John Doe | Los Angeles  |

#### Type 2: Add New Row

Type 2 SCDs add a new row for each change, preserving the historical data. This type includes effective dates or version numbers to distinguish between different versions of the data.

**Example:**

- Original Data: `Customer ID: 1, Name: John Doe, City: New York`
- Updated Data: `Customer ID: 1, Name: John Doe, City: Los Angeles`

**Table Before Update:**

| Customer ID | Name     | City      | Start Date | End Date   |
|-------------|----------|-----------|------------|------------|
| 1           | John Doe | New York  | 2020-01-01 | 9999-12-31 |

**Table After Update:**

| Customer ID | Name     | City         | Start Date | End Date   |
|-------------|----------|--------------|------------|------------|
| 1           | John Doe | New York     | 2020-01-01 | 2021-01-01 |
| 1           | John Doe | Los Angeles  | 2021-01-01 | 9999-12-31 |

#### Type 3: Add New Attribute

Type 3 SCDs add a new attribute to store the previous value of a changing attribute. This method is useful when changes are infrequent, and only the previous value needs to be tracked.

**Example:**

- Original Data: `Customer ID: 1, Name: John Doe, City: New York`
- Updated Data: `Customer ID: 1, Name: John Doe, City: Los Angeles`

**Table Before Update:**

| Customer ID | Name     | City      | Previous City |
|-------------|----------|-----------|---------------|
| 1           | John Doe | New York  | NULL          |

**Table After Update:**

| Customer ID | Name     | City         | Previous City |
|-------------|----------|--------------|---------------|
| 1           | John Doe | Los Angeles  | New York      |

#### Type 4: Add Historical Table

Type 4 SCDs use a separate historical table to track changes. The main dimension table stores the current value, while the historical table maintains all the changes.

**Example:**

**Main Table Before Update:**

| Customer ID | Name     | City      |
|-------------|----------|-----------|
| 1           | John Doe | New York  |

**Historical Table Before Update:**

| Customer ID | Name     | City      | Start Date | End Date   |
|-------------|----------|-----------|------------|------------|

**Main Table After Update:**

| Customer ID | Name     | City         |
|-------------|----------|--------------|
| 1           | John Doe | Los Angeles  |

**Historical Table After Update:**

| Customer ID | Name     | City      | Start Date | End Date   |
|-------------|----------|-----------|------------|------------|
| 1           | John Doe | New York  | 2020-01-01 | 2021-01-01 |

#### Type 6: Hybrid (Type 1 + Type 2 + Type 3)

Type 6 SCDs combine features of Type 1, Type 2, and Type 3 SCDs. This method adds a new row for each change (Type 2), stores the current value in the main dimension table (Type 1), and adds a new attribute to store the previous value (Type 3).

**Example:**

**Main Table Before Update:**

| Customer ID | Name     | City      | Current City | Previous City | Start Date | End Date   |
|-------------|----------|-----------|--------------|---------------|------------|------------|
| 1           | John Doe | New York  | New York     | NULL          | 2020-01-01 | 9999-12-31 |

**Main Table After Update:**

| Customer ID | Name     | City         | Current City | Previous City | Start Date | End Date   |
|-------------|----------|--------------|--------------|---------------|------------|------------|
| 1           | John Doe | New York     | Los Angeles  | New York      | 2020-01-01 | 2021-01-01 |
| 1           | John Doe | Los Angeles  | Los Angeles  | New York      | 2021-01-01 | 9999-12-31 |

### Conclusion

Slowly Changing Dimensions (SCDs) are essential for maintaining accurate historical data in data warehousing. The choice of SCD type depends on the specific requirements of the business and the frequency and nature of the changes. Understanding the different types of SCDs helps in designing efficient data warehouses that can handle changes in dimension data effectively.


## Implementing Slowly Changing Dimension Type 2 (SCD Type 2) in Snowflake 

Implementing Slowly Changing Dimension Type 2 (SCD Type 2) in Snowflake involves tracking historical data changes by adding new rows for each change, rather than updating existing rows. This method preserves the historical data and provides a complete audit trail of changes over time.

### Steps to Implement SCD Type 2 in Snowflake

1. **Create a Dimension Table**
2. **Load Initial Data**
3. **Detect Changes**
4. **Insert New Rows for Changes**
5. **Update End Dates of Previous Rows**

### Example Scenario

Assume we have a customer dimension table that tracks changes in customer data over time. We want to implement SCD Type 2 to handle changes in customer information.

### Step-by-Step Implementation

#### 1. Create a Dimension Table

First, create a dimension table with necessary columns to store historical data. Include columns for the start date and end date to track the validity period of each record.

```sql
CREATE OR REPLACE TABLE customer_dimension (
    customer_id INT,
    customer_name STRING,
    address STRING,
    start_date DATE,
    end_date DATE,
    active BOOLEAN,
    PRIMARY KEY (customer_id, start_date)
);
```

#### 2. Load Initial Data

Load the initial set of data into the dimension table.

```sql
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
VALUES
    (1, 'John Doe', '123 Main St', '2023-01-01', '9999-12-31', TRUE),
    (2, 'Jane Smith', '456 Elm St', '2023-01-01', '9999-12-31', TRUE);
```

#### 3. Detect Changes

Assume you have a staging table (`customer_staging`) that contains the latest data, including any updates.

```sql
CREATE OR REPLACE TABLE customer_staging (
    customer_id INT,
    customer_name STRING,
    address STRING,
    load_date DATE
);
```

Load new data into the staging table.

```sql
INSERT INTO customer_staging (customer_id, customer_name, address, load_date)
VALUES
    (1, 'John Doe', '789 Oak St', '2023-06-01'),  -- Address changed
    (2, 'Jane Smith', '456 Elm St', '2023-06-01'), -- No change
    (3, 'Alice Johnson', '101 Pine St', '2023-06-01'); -- New customer
```

#### 4. Insert New Rows for Changes

Identify records in the staging table that have changes or are new and insert new rows into the dimension table.

```sql
-- Insert new rows for changes
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
SELECT 
    s.customer_id, 
    s.customer_name, 
    s.address, 
    s.load_date, 
    '9999-12-31', 
    TRUE
FROM 
    customer_staging s
LEFT JOIN 
    customer_dimension d 
ON 
    s.customer_id = d.customer_id
WHERE 
    d.active = TRUE
    AND (
        d.customer_name != s.customer_name 
        OR d.address != s.address
    );

-- Insert new rows for new customers
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
SELECT 
    s.customer_id, 
    s.customer_name, 
    s.address, 
    s.load_date, 
    '9999-12-31', 
    TRUE
FROM 
    customer_staging s
LEFT JOIN 
    customer_dimension d 
ON 
    s.customer_id = d.customer_id
WHERE 
    d.customer_id IS NULL;
```

#### 5. Update End Dates of Previous Rows

Update the `end_date` and `active` status of the previous rows to reflect that they are no longer current.

```sql
UPDATE customer_dimension
SET 
    end_date = (SELECT load_date FROM customer_staging WHERE customer_staging.customer_id = customer_dimension.customer_id),
    active = FALSE
WHERE 
    customer_id IN (SELECT customer_id FROM customer_staging)
    AND active = TRUE
    AND end_date = '9999-12-31';
```

### Putting It All Together

Here is the complete SQL script for implementing SCD Type 2 in Snowflake:

```sql
-- Create dimension table
CREATE OR REPLACE TABLE customer_dimension (
    customer_id INT,
    customer_name STRING,
    address STRING,
    start_date DATE,
    end_date DATE,
    active BOOLEAN,
    PRIMARY KEY (customer_id, start_date)
);

-- Load initial data
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
VALUES
    (1, 'John Doe', '123 Main St', '2023-01-01', '9999-12-31', TRUE),
    (2, 'Jane Smith', '456 Elm St', '2023-01-01', '9999-12-31', TRUE);

-- Create staging table
CREATE OR REPLACE TABLE customer_staging (
    customer_id INT,
    customer_name STRING,
    address STRING,
    load_date DATE
);

-- Load new data into staging table
INSERT INTO customer_staging (customer_id, customer_name, address, load_date)
VALUES
    (1, 'John Doe', '789 Oak St', '2023-06-01'),  -- Address changed
    (2, 'Jane Smith', '456 Elm St', '2023-06-01'), -- No change
    (3, 'Alice Johnson', '101 Pine St', '2023-06-01'); -- New customer

-- Insert new rows for changes
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
SELECT 
    s.customer_id, 
    s.customer_name, 
    s.address, 
    s.load_date, 
    '9999-12-31', 
    TRUE
FROM 
    customer_staging s
LEFT JOIN 
    customer_dimension d 
ON 
    s.customer_id = d.customer_id
WHERE 
    d.active = TRUE
    AND (
        d.customer_name != s.customer_name 
        OR d.address != s.address
    );

-- Insert new rows for new customers
INSERT INTO customer_dimension (customer_id, customer_name, address, start_date, end_date, active)
SELECT 
    s.customer_id, 
    s.customer_name, 
    s.address, 
    s.load_date, 
    '9999-12-31', 
    TRUE
FROM 
    customer_staging s
LEFT JOIN 
    customer_dimension d 
ON 
    s.customer_id = d.customer_id
WHERE 
    d.customer_id IS NULL;

-- Update end dates of previous rows
UPDATE customer_dimension
SET 
    end_date = (SELECT load_date FROM customer_staging WHERE customer_staging.customer_id = customer_dimension.customer_id),
    active = FALSE
WHERE 
    customer_id IN (SELECT customer_id FROM customer_staging)
    AND active = TRUE
    AND end_date = '9999-12-31';
```

### Conclusion

Implementing SCD Type 2 in Snowflake involves creating a dimension table, loading initial data, detecting changes, inserting new rows for changes, and updating the end dates of previous rows. This method ensures that historical data is preserved, providing a complete audit trail of changes over time. By following these steps, you can effectively manage slowly changing dimensions in your data warehouse.



## Slowly Changing Dimension

Slowly Changing Dimensions (SCD) are a concept in data warehousing used to manage and track changes in the dimensions of a dataset over time. Dimensions are attributes or descriptive characteristics of the data, often used to describe facts in a fact table. When these attributes change infrequently, they are referred to as slowly changing dimensions. There are several methods to handle these changes, each with its own trade-offs. The most common types of SCDs are Types 0, 1, 2, 3, 4, and 6.

### SCD Types Explained

#### 1. Type 0: Retain Original
- Description: No changes are allowed. The original value is retained even if the source data changes.
- Use Case: Useful when historical accuracy is paramount and changes should never be recorded.

#### 2. Type 1: Overwrite
- Description: The new data overwrites the old data. No history of previous values is kept.
- Use Case: Useful when corrections or non-critical updates are made, and only the latest value is needed.
- Example:
  ```sql
  UPDATE customer_dim
  SET address = 'New Address'
  WHERE customer_id = 123;
  ```

#### 3. Type 2: Add New Row
- Description: A new record is added with a new version key when a change occurs. The old record is marked as inactive or given an end date.
- Use Case: Useful when it is important to keep a complete history of changes.
- Implementation:
  - Add a `version` or `effective_date` and `end_date` column to the dimension table.
  - When an attribute changes, mark the old record as inactive and insert a new record with the updated attribute.
- Example:
  ```sql
  -- Mark old record as inactive
  UPDATE customer_dim
  SET end_date = '2024-05-01'
  WHERE customer_id = 123 AND end_date IS NULL;
  
  -- Insert new record
  INSERT INTO customer_dim (customer_id, name, address, start_date, end_date)
  VALUES (123, 'John Doe', 'New Address', '2024-05-01', NULL);
  ```

#### 4. Type 3: Add New Column
- Description: A new column is added to store the previous value of an attribute.
- Use Case: Useful when only the previous value needs to be kept for comparison purposes.
- Example:
  ```sql
  ALTER TABLE customer_dim ADD COLUMN previous_address VARCHAR(255);
  
  UPDATE customer_dim
  SET previous_address = address,
      address = 'New Address'
  WHERE customer_id = 123;
  ```

#### 5. Type 4: History Table
- Description: Historical data is stored in a separate history table.
- Use Case: Useful when keeping a history of changes without cluttering the main dimension table.
- Implementation:
  - Create a history table with the same structure as the dimension table plus additional columns for timestamps.
  - Insert old records into the history table before updating the dimension table.
- Example:
  ```sql
  -- Create history table
  CREATE TABLE customer_dim_history AS SELECT * FROM customer_dim WHERE 1=0;
  ALTER TABLE customer_dim_history ADD COLUMN change_date DATE;

  -- Insert old record into history table
  INSERT INTO customer_dim_history SELECT *, CURRENT_DATE FROM customer_dim WHERE customer_id = 123;

  -- Update dimension table
  UPDATE customer_dim
  SET address = 'New Address'
  WHERE customer_id = 123;
  ```

#### 6. Type 6: Hybrid (1+2+3)
- Description: Combines the features of Types 1, 2, and 3. A new row is added with a new version key, and previous values are stored in additional columns.
- Use Case: Useful when a complete history is needed, along with the ability to track previous values in the same record.
- Implementation:
  - Add columns for `current_version`, `previous_version`, `start_date`, `end_date`, and other relevant attributes.
  - Update records as in Type 2, but also maintain previous values in the same record.
- Example:
  ```sql
  -- Update old record as inactive and move previous values to new columns
  UPDATE customer_dim
  SET end_date = '2024-05-01',
      previous_address = address
  WHERE customer_id = 123 AND end_date IS NULL;

  -- Insert new record with updated address
  INSERT INTO customer_dim (customer_id, name, address, start_date, end_date, previous_address)
  VALUES (123, 'John Doe', 'New Address', '2024-05-01', NULL, 'Old Address');
  ```

### Choosing the Right SCD Type

The choice of SCD type depends on business requirements:
- Type 0: When historical accuracy and no changes are paramount.
- Type 1: When only the latest data is needed and historical changes are not important.
- Type 2: When a complete history of changes is needed.
- Type 3: When only the previous state needs to be tracked.
- Type 4: When historical data should be kept separately to avoid clutter in the main table.
- Type 6: When a combination of a full history and previous state tracking is required.

Understanding and implementing the appropriate SCD type ensures that your data warehouse accurately reflects historical data changes and meets business requirements.