## Data validation in data pipeline

Data validation is a crucial step in any data pipeline to ensure the accuracy, quality, and reliability of the data being processed. Implementing robust data validation checks helps identify and handle data issues early in the pipeline, reducing downstream errors and improving overall data integrity. Here are detailed explanations of various data validation checks that can be incorporated into a data pipeline:

### 1. **Schema Validation**

**Purpose**:
Ensure that the data conforms to the expected schema, including the structure, data types, and constraints.

**Checks**:
- **Data Type Checks**: Verify that each field contains the correct data type (e.g., integers, strings, dates).
- **Mandatory Fields**: Ensure that required fields are not missing or null.
- **Field Length**: Check that the length of data in each field is within the expected range.
- **Enum Validation**: Ensure that fields containing categorical data only have allowed values.

**Example**:
```python
def validate_schema(data, schema):
    for field, expected_type in schema.items():
        if field not in data:
            raise ValueError(f"Missing field: {field}")
        if not isinstance(data[field], expected_type):
            raise TypeError(f"Field {field} should be of type {expected_type}")
```

### 2. **Uniqueness Checks**

**Purpose**:
Ensure that certain fields or combinations of fields contain unique values to avoid duplicates.

**Checks**:
- **Primary Key Uniqueness**: Verify that the primary key or unique identifier is unique across the dataset.
- **Composite Key Uniqueness**: Ensure that a combination of fields (composite key) is unique.

**Example**:
```python
def check_uniqueness(data, unique_fields):
    seen = set()
    for record in data:
        key = tuple(record[field] for field in unique_fields)
        if key in seen:
            raise ValueError(f"Duplicate record found: {record}")
        seen.add(key)
```

### 3. **Range and Boundary Checks**

**Purpose**:
Ensure that numerical values fall within specified ranges and that dates are within acceptable boundaries.

**Checks**:
- **Numerical Range**: Verify that numerical fields are within specified minimum and maximum values.
- **Date Range**: Ensure that date fields fall within a reasonable range (e.g., not in the future or too far in the past).

**Example**:
```python
def check_range(data, field, min_value, max_value):
    for record in data:
        if not (min_value <= record[field] <= max_value):
            raise ValueError(f"Field {field} out of range in record: {record}")
```

### 4. **Null and Missing Value Checks**

**Purpose**:
Identify and handle missing or null values in critical fields.

**Checks**:
- **Not Null**: Ensure that fields marked as non-nullable do not contain null values.
- **Missing Values**: Check for missing values in critical fields and handle them appropriately (e.g., imputation, removal).

**Example**:
```python
def check_not_null(data, fields):
    for record in data:
        for field in fields:
            if record[field] is None:
                raise ValueError(f"Null value found in field {field} for record: {record}")
```

### 5. **Referential Integrity Checks**

**Purpose**:
Ensure that foreign key relationships between tables are maintained and valid.

**Checks**:
- **Foreign Key Existence**: Verify that foreign key values exist in the referenced table.
- **Cascading Rules**: Ensure that deletions or updates in parent tables are properly cascaded to child tables.

**Example**:
```python
def check_referential_integrity(data, foreign_key, reference_table):
    reference_keys = {record['id'] for record in reference_table}
    for record in data:
        if record[foreign_key] not in reference_keys:
            raise ValueError(f"Invalid foreign key {foreign_key} in record: {record}")
```

### 6. **Consistency Checks**

**Purpose**:
Ensure that related fields are consistent with each other across records.

**Checks**:
- **Cross-Field Validation**: Verify that related fields are consistent (e.g., start date should be before end date).
- **Conditional Checks**: Ensure that certain conditions hold true (e.g., if field A is true, then field B should not be null).

**Example**:
```python
def check_consistency(data):
    for record in data:
        if record['start_date'] > record['end_date']:
            raise ValueError(f"Inconsistent dates in record: {record}")
```

### 7. **Duplicate Checks**

**Purpose**:
Identify and handle duplicate records within the dataset.

**Checks**:
- **Record-Level Duplicates**: Detect and handle duplicate records based on all or a subset of fields.

**Example**:
```python
def check_duplicates(data, key_fields):
    seen = set()
    duplicates = []
    for record in data:
        key = tuple(record[field] for field in key_fields)
        if key in seen:
            duplicates.append(record)
        seen.add(key)
    if duplicates:
        raise ValueError(f"Duplicate records found: {duplicates}")
```

### 8. **Custom Business Rules Validation**

**Purpose**:
Ensure that data adheres to specific business rules or domain-specific constraints.

**Checks**:
- **Custom Constraints**: Implement custom validation logic based on business requirements.

**Example**:
```python
def check_business_rules(data):
    for record in data:
        if record['age'] < 18 and record['status'] == 'employed':
            raise ValueError(f"Invalid business rule in record: {record}")
```

### 9. **Statistical Validation**

**Purpose**:
Use statistical methods to detect anomalies or outliers in the data.

**Checks**:
- **Outlier Detection**: Identify and handle outliers using statistical methods (e.g., z-scores, IQR).
- **Distribution Checks**: Ensure that data follows expected statistical distributions.

**Example**:
```python
import numpy as np

def check_outliers(data, field):
    values = [record[field] for record in data]
    mean = np.mean(values)
    std_dev = np.std(values)
    for record in data:
        z_score = (record[field] - mean) / std_dev
        if abs(z_score) > 3:
            raise ValueError(f"Outlier detected in field {field}: {record}")
```

### Conclusion

Incorporating these data validation checks into your data pipeline helps ensure that the data being processed is accurate, consistent, and reliable. By catching errors early in the pipeline, you can prevent bad data from propagating through your system, leading to better data quality and more reliable insights.