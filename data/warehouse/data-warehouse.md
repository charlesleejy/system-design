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