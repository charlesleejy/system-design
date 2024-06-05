### HDFS (Hadoop Distributed File System)

![alt text](../images/hdfs-architecture.png)



## Hadoop Distributed File System (HDFS): A Detailed Overview

Hadoop Distributed File System (HDFS) is a scalable, fault-tolerant file system designed to run on commodity hardware. It is a core component of the Apache Hadoop ecosystem and is tailored for storing large datasets and enabling high-throughput data access. HDFS is widely used for big data analytics, processing, and storage.

### Key Features of HDFS

1. **Scalability**:
   - HDFS can scale horizontally by adding more commodity hardware to the cluster. It efficiently stores and manages petabytes of data.

2. **Fault Tolerance**:
   - HDFS is designed to handle hardware failures gracefully. Data is automatically replicated across multiple nodes to ensure reliability and availability.

3. **High Throughput**:
   - HDFS is optimized for high-throughput data access, making it ideal for large-scale data processing applications such as batch processing and analytics.

4. **Replication**:
   - Data blocks are replicated across multiple DataNodes to ensure data durability and availability. The default replication factor is three, but it can be configured based on requirements.

5. **Write Once, Read Many**:
   - HDFS follows a write-once, read-many access model, which simplifies data coherency issues and enables efficient data access.

### HDFS Architecture

HDFS follows a master-slave architecture consisting of the following key components:

1. **NameNode**:
   - The NameNode is the master server that manages the metadata and namespace of HDFS. It keeps track of file-to-block mappings, the locations of blocks, and the overall structure of the file system.
   - **Responsibilities**:
     - Managing the file system namespace.
     - Regulating access to files by clients.
     - Managing metadata and file system operations such as file creation, deletion, and replication.

2. **DataNode**:
   - DataNodes are the slave servers that store actual data blocks. They are responsible for serving read and write requests from clients and performing block creation, deletion, and replication upon instruction from the NameNode.
   - **Responsibilities**:
     - Storing and retrieving blocks as directed by the NameNode.
     - Reporting block information to the NameNode periodically (block reports).
     - Performing block replication, deletion, and recovery as instructed by the NameNode.

3. **Secondary NameNode**:
   - The Secondary NameNode is not a backup for the NameNode but assists in managing the metadata. It periodically merges the NameNode’s namespace image with the edit logs to create a new namespace image.
   - **Responsibilities**:
     - Reducing the NameNode's workload by handling namespace image and edit log merging.
     - Keeping a backup of the merged namespace image to help in the recovery process if the NameNode fails.

### Data Storage in HDFS

1. **Blocks**:
   - HDFS stores data in fixed-size blocks (default 128 MB or 256 MB). Each file is divided into blocks, and these blocks are distributed across the DataNodes in the cluster.

2. **Replication**:
   - Each block is replicated to multiple DataNodes to ensure fault tolerance. The default replication factor is three, but this can be adjusted based on data reliability and performance requirements.

3. **File System Namespace**:
   - The NameNode maintains the file system namespace and handles file system operations like opening, closing, and renaming files and directories.

### Data Access in HDFS

1. **Read Operation**:
   - A client requests the NameNode for the location of blocks for a file.
   - The NameNode responds with the block locations.
   - The client reads the blocks directly from the DataNodes.

2. **Write Operation**:
   - A client requests the NameNode to create a new file.
   - The NameNode checks for permissions and creates a new file entry.
   - The client writes data in blocks to the DataNodes.
   - DataNodes replicate the blocks to other DataNodes based on the replication factor.

### HDFS Commands

1. **File System Operations**:
   - `hdfs dfs -ls /`: List files and directories in the root directory.
   - `hdfs dfs -mkdir /user/data`: Create a new directory.
   - `hdfs dfs -put localfile /user/data`: Upload a file from the local file system to HDFS.
   - `hdfs dfs -get /user/data/hdfsfile localfile`: Download a file from HDFS to the local file system.
   - `hdfs dfs -rm /user/data/hdfsfile`: Remove a file from HDFS.

2. **Administrative Commands**:
   - `hdfs dfsadmin -report`: Report the status of the HDFS cluster.
   - `hdfs dfsadmin -safemode enter`: Enter safemode for the NameNode.
   - `hdfs dfsadmin -safemode leave`: Leave safemode for the NameNode.
   - `hdfs dfsadmin -refreshNodes`: Refresh the list of DataNodes.

### Setting Up HDFS

#### Prerequisites

1. **Java Development Kit (JDK)**:
   - Ensure JDK 8 or later is installed on all nodes.

2. **SSH Configuration**:
   - Set up passwordless SSH access between all nodes in the HDFS cluster.

#### Step-by-Step Setup

1. **Download and Extract Hadoop**:
   ```bash
   wget https://downloads.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz
   tar -xzvf hadoop-3.3.1.tar.gz
   mv hadoop-3.3.1 /usr/local/hadoop
   ```

2. **Configure Environment Variables**:
   - Add the following to `~/.bashrc` or `~/.bash_profile`:
     ```bash
     export HADOOP_HOME=/usr/local/hadoop
     export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
     export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
     ```

3. **Configure Hadoop**:
   - Edit `core-site.xml`:
     ```xml
     <configuration>
       <property>
         <name>fs.defaultFS</name>
         <value>hdfs://<namenode-hostname>:9000</value>
       </property>
     </configuration>
     ```
   - Edit `hdfs-site.xml`:
     ```xml
     <configuration>
       <property>
         <name>dfs.replication</name>
         <value>3</value>
       </property>
       <property>
         <name>dfs.namenode.name.dir</name>
         <value>file:///usr/local/hadoop/hadoop_data/hdfs/namenode</value>
       </property>
       <property>
         <name>dfs.datanode.data.dir</name>
         <value>file:///usr/local/hadoop/hadoop_data/hdfs/datanode</value>
       </property>
     </configuration>
     ```

4. **Format the NameNode**:
   ```bash
   hdfs namenode -format
   ```

5. **Start HDFS**:
   ```bash
   start-dfs.sh
   ```

6. **Verify the Setup**:
   - Access the NameNode web UI at `http://<namenode-hostname>:9870`.

### Monitoring and Management

1. **NameNode Web UI**:
   - Provides a dashboard for monitoring the health and status of the HDFS cluster.
   - URL: `http://<namenode-hostname>:9870`.

2. **ResourceManager Web UI**:
   - Monitors the resource usage and running applications in the YARN cluster.
   - URL: `http://<resourcemanager-hostname>:8088`.

3. **Cloudera Manager**:
   - A comprehensive management tool for monitoring and managing Hadoop clusters, providing detailed metrics, alerts, and configurations.

### Conclusion

HDFS is a powerful and scalable distributed file system designed for big data storage and processing. Its fault tolerance, high throughput, and ability to handle large datasets make it a crucial component of the Hadoop ecosystem. By understanding its architecture, operations, and setup process, you can effectively utilize HDFS to store and manage large volumes of data in a distributed environment.


## Setting Up Cloudera HDFS for a Data Pipeline Running on PySpark

Setting up a Cloudera HDFS for a data pipeline involves several steps, from installing Cloudera Distribution of Hadoop (CDH) and configuring HDFS to setting up a PySpark environment and integrating PySpark with HDFS. Below is a detailed guide to help you through the process.

### Prerequisites

1. **Linux Server**: Ensure you have access to a Linux server (e.g., Ubuntu or CentOS).
2. **Java**: Install Java Development Kit (JDK) 8 or later.
3. **SSH Access**: Ensure SSH access to your server.

### Step-by-Step Guide

#### 1. Install Cloudera Distribution of Hadoop (CDH)

##### a. Download and Install Cloudera Manager

1. **Download Cloudera Manager Installer**:
   ```bash
   wget https://archive.cloudera.com/cm7/7.2.1/cloudera-manager-installer.bin
   chmod +x cloudera-manager-installer.bin
   ```

2. **Run the Installer**:
   ```bash
   sudo ./cloudera-manager-installer.bin
   ```

3. **Follow the On-Screen Instructions**: The installer will guide you through the installation process.

##### b. Set Up Cloudera Manager

1. **Access Cloudera Manager Web UI**:
   - Open a web browser and navigate to `http://<your-server-ip>:7180`.
   - Log in with the default credentials (`admin`/`admin`).

2. **Install CDH**:
   - Use the Cloudera Manager wizard to install CDH on your cluster.
   - Select the services you want to install, including HDFS, YARN, and Spark.

#### 2. Configure HDFS

Once CDH is installed, configure HDFS through the Cloudera Manager.

1. **Open Cloudera Manager**:
   - Navigate to `http://<your-server-ip>:7180` and log in.

2. **Navigate to HDFS Configuration**:
   - Go to `Clusters` > `HDFS` > `Configuration`.

3. **Set HDFS Properties**:
   - Configure the properties according to your needs, such as replication factor, block size, etc.

4. **Deploy and Restart**:
   - Save the changes and deploy the configuration.
   - Restart the HDFS service to apply the changes.

#### 3. Set Up PySpark Environment

##### a. Install PySpark

1. **Install Python and Pip**:
   ```bash
   sudo apt-get update
   sudo apt-get install python3 python3-pip
   ```

2. **Install PySpark**:
   ```bash
   pip3 install pyspark
   ```

##### b. Configure PySpark to Use HDFS

1. **Set Environment Variables**:
   - Add the following environment variables to your `~/.bashrc` or `~/.bash_profile` file:
     ```bash
     export HADOOP_CONF_DIR=/etc/hadoop/conf
     export SPARK_HOME=/path/to/spark
     export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-<version>-src.zip:$PYTHONPATH
     export PATH=$SPARK_HOME/bin:$PATH
     ```

2. **Source the File**:
   ```bash
   source ~/.bashrc
   ```

#### 4. Integrate PySpark with HDFS

##### a. Write a PySpark Script to Read and Write Data to HDFS

1. **Example PySpark Script**: `pyspark_hdfs.py`
   ```python
   from pyspark.sql import SparkSession

   # Initialize SparkSession
   spark = SparkSession.builder \
       .appName("PySparkHDFSExample") \
       .getOrCreate()

   # Read data from HDFS
   df = spark.read.csv("hdfs:///user/hdfs/input.csv")

   # Perform some transformations
   df = df.filter(df['_c0'] > 100)

   # Write data back to HDFS
   df.write.csv("hdfs:///user/hdfs/output.csv")

   # Stop the SparkSession
   spark.stop()
   ```

2. **Submit the PySpark Job**:
   ```bash
   spark-submit pyspark_hdfs.py
   ```

### Step-by-Step Execution

1. **Set Up Cloudera Manager and Install CDH**:
   - Follow the installer and wizard to set up Cloudera Manager and install CDH.
   - Ensure HDFS, YARN, and Spark services are installed and running.

2. **Configure HDFS via Cloudera Manager**:
   - Access Cloudera Manager web UI and configure HDFS properties as needed.
   - Save and deploy the configuration, then restart the HDFS service.

3. **Set Up and Configure PySpark**:
   - Install Python, Pip, and PySpark on your server.
   - Configure environment variables to point to Hadoop and Spark directories.

4. **Create and Run PySpark Script**:
   - Write a PySpark script that reads data from HDFS, processes it, and writes it back to HDFS.
   - Use `spark-submit` to run the PySpark script.

### Monitoring and Managing

1. **Cloudera Manager**:
   - Use Cloudera Manager to monitor the health and performance of your HDFS and Spark services.
   - Check logs and metrics for any issues.

2. **YARN ResourceManager**:
   - Access the YARN ResourceManager web UI to monitor running applications and resource usage.

### Conclusion

Setting up Cloudera HDFS for a data pipeline running on PySpark involves installing CDH, configuring HDFS, and integrating PySpark with HDFS. By following the detailed steps outlined above, you can efficiently set up a robust data pipeline capable of processing large datasets in a scalable and distributed environment. Using Cloudera Manager, you can easily manage and monitor your Hadoop and Spark services, ensuring optimal performance and reliability for your data processing tasks.


