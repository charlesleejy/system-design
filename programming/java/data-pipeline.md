## Creating a data pipeline in Java

Creating a data pipeline in Java involves a series of steps that include data ingestion, transformation, and storage. In this example, we'll use Apache Spark to build a data pipeline in Java. Apache Spark is a powerful open-source processing engine that supports data processing at scale.

### Example Scenario

Suppose we have a data source containing customer transaction data stored in a CSV file, and we want to perform the following steps in our pipeline:
1. Ingest data from the CSV file.
2. Clean and transform the data.
3. Perform some aggregation (e.g., total transactions per customer).
4. Store the processed data into an output directory (e.g., HDFS or local file system).

### Prerequisites

1. **Java Development Kit (JDK)**: Ensure you have JDK 8 or later installed.
2. **Apache Spark**: Download and set up Apache Spark.
3. **Maven**: Build and manage your Java project dependencies.

### Step-by-Step Guide

#### 1. Set Up Maven Project

Create a Maven project with the following directory structure:

```
data-pipeline/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── DataPipeline.java
│   │   └── resources/
│   │       └── log4j.properties
├── pom.xml
```

#### 2. Define Maven `pom.xml`

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>data-pipeline</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- Spark Core Dependency -->
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.12</artifactId>
            <version>3.1.2</version>
        </dependency>
        <!-- Spark SQL Dependency -->
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.12</artifactId>
            <version>3.1.2</version>
        </dependency>
        <!-- Logging Dependency -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.30</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.1.2</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.example.DataPipeline</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

#### 3. Create the Java Class for Data Pipeline

Create a `DataPipeline.java` file under `src/main/java/com/example/`.

```java
package com.example;

import org.apache.spark.sql.Dataset;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SparkSession;
import org.apache.spark.sql.functions;
import static org.apache.spark.sql.functions.*;

public class DataPipeline {

    public static void main(String[] args) {
        // Create Spark Session
        SparkSession spark = SparkSession.builder()
                .appName("Java Spark SQL Example")
                .config("spark.master", "local")
                .getOrCreate();

        // Ingest data from CSV file
        Dataset<Row> df = spark.read().format("csv")
                .option("header", "true")
                .option("inferSchema", "true")
                .load("data/transactions.csv");

        // Data Transformation: Clean data by removing rows with null values
        Dataset<Row> cleanedDF = df.na().drop();

        // Data Transformation: Add a new column 'amount_in_usd'
        Dataset<Row> transformedDF = cleanedDF.withColumn("amount_in_usd", col("amount").multiply(1.1));

        // Aggregation: Total transactions per customer
        Dataset<Row> aggregatedDF = transformedDF.groupBy("customer_id")
                .agg(sum("amount_in_usd").alias("total_amount_usd"));

        // Store the processed data to an output directory
        aggregatedDF.write().format("csv").save("output/processed_transactions");

        // Stop the Spark session
        spark.stop();
    }
}
```

#### 4. Prepare the Data and Configuration

1. **Place Data File**: Ensure you have a `transactions.csv` file in the `data/` directory.
2. **Log4j Configuration**: Create a `log4j.properties` file in `src/main/resources/` for logging.

```properties
log4j.rootCategory=INFO, console

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.target=System.err
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %p %c{1}: %m%n
```

#### 5. Build and Run the Project

1. **Build the Project**:
   ```bash
   mvn clean package
   ```

2. **Run the Data Pipeline**:
   ```bash
   java -cp target/data-pipeline-1.0-SNAPSHOT.jar com.example.DataPipeline
   ```

### Explanation of the Pipeline

1. **Ingest Data**:
   - The Spark session reads the transaction data from a CSV file located at `data/transactions.csv`.
   ```java
   Dataset<Row> df = spark.read().format("csv")
           .option("header", "true")
           .option("inferSchema", "true")
           .load("data/transactions.csv");
   ```

2. **Clean Data**:
   - The pipeline removes rows with null values to clean the data.
   ```java
   Dataset<Row> cleanedDF = df.na().drop();
   ```

3. **Transform Data**:
   - A new column `amount_in_usd` is added, converting the original `amount` column by multiplying it by 1.1.
   ```java
   Dataset<Row> transformedDF = cleanedDF.withColumn("amount_in_usd", col("amount").multiply(1.1));
   ```

4. **Aggregate Data**:
   - The pipeline calculates the total transaction amount per customer.
   ```java
   Dataset<Row> aggregatedDF = transformedDF.groupBy("customer_id")
           .agg(sum("amount_in_usd").alias("total_amount_usd"));
   ```

5. **Store Processed Data**:
   - The processed data is stored in the `output/processed_transactions` directory in CSV format.
   ```java
   aggregatedDF.write().format("csv").save("output/processed_transactions");
   ```

### Conclusion

This example demonstrates how to set up a basic data pipeline in Java using Apache Spark. The pipeline involves data ingestion, transformation, aggregation, and storage. By following the steps outlined above, you can create a scalable and efficient data processing pipeline that can handle large datasets. You can further extend this pipeline to include more complex transformations, error handling, and integration with other data sources or destinations.
