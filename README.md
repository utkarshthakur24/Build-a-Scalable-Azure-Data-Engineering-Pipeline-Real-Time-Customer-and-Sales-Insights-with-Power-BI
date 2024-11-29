# **"End-to-End Azure Data Engineering Project: Real-Time Sales Analytics and Scalable Insights with Power BI"**

This project is a data engineering pipeline solution to a fictional business problem, designed to help me learn and understand data pipelining.
For more detailed explanation, please refer to the [medium.com Article](https://medium.com/@utkarshthakur2404/project-headline-building-a-scalable-azure-data-engineering-pipeline-real-time-customer-and-sales-d5fbc266bc0a).

# Project Overview
## Business Request
In this project, my company has recognized a gap in understanding its customer demographics - specifically, the gender distribution within the customer base and how it might influence product purchases. With a significant amount of customer data stored in an on-premises SQL database, key stakeholders have requested a comprehensive KPI dashboard. This dashboard should provide insights into sales by gender and product category, showing total sold, total sates revenue. and a clear gender split among customers. Additionally, they need the ability to filter this data by product category and gender. with a user-friendly interface
for date-based queries.
## Solution Overview
To address this request, we'll build a robust data pipeline that extracts the on-premises data, loads it into Azure, and performs the necessary transformations to make the data more query-friendly. The transformed data will then feed into a custom-built report by Power BI dashboard that meets all the specified requirements. This pipeline will be scheduled to run automatically every day, ensuring that stakeholders always have access to up to-date and accurate data.



# Project Architecture

This step-by-step approach ensures a structured and efficient implementation of the data pipeline.
![1_MBQkjrrPyJNH_qUo2-YCIg](https://github.com/user-attachments/assets/edb26c4e-6c64-41ab-b637-f8312f95ce8a)


To meet these requirements, the solution is broken down into the following components:

## Part 1: Environment Setup
- **Azure Environment Setup**: Provision key resources (ADLS, ADF, Azure Databricks, Synapse Analytics, Key Vault) and configure secure networking with Private Link and Managed Identity.  
- **Source Database Setup**: Restore the AdventureWorksLT2022 database in SQL Server, create a user with `db_datareader` permissions, and verify access.  
- **Secure Credentials**: Store SQL Server credentials in Azure Key Vault and grant necessary access to Azure services.  

## Part 2: Data Ingestion
- **Source Data Extraction**: Dynamically extract all tables from the `SalesLT` schema in the on-premises SQL Server database.  
- **Data Load to Bronze Layer**: Use Azure Data Factory (ADF) to copy the extracted data into the Bronze layer of Azure Data Lake Storage (ADLS) in Parquet format, organizing data by schema and table.  
- **Dynamic and Scalable Pipeline**: Implement Lookup and ForEach activities in ADF to adapt to schema changes automatically, ensuring a scalable and efficient data ingestion process.  

## Part 3: Data Transformation

- **Data Cleansing**: Use Azure Databricks to clean and preprocess the raw data.
- **Data Organization**: Structure the data into the Bronze, Silver, and Gold layers:
    Bronze: Raw data as ingested from the source.
    Silver: Cleansed and enriched data.
    Gold: Aggregated data ready for analytics and reporting.

## Part 4: Data Loading

- **Synapse Integration**: Load the transformed data from the Gold layer into Azure Synapse Analytics for high-performance querying and reporting.

## Part 5: Data Reporting

- **Power BI Dashboard**: Create and connect a Power BI dashboard to Azure Synapse Analytics to visualize sales trends and demographic insights.
- **Stakeholder Exploration**: Enable interactive exploration of the data for business stakeholders.

## Part 6: Security, Automation & End-to-End Pipeline Testing

- Creating Security Group using Azure Active Directory (AAD)
- **Pipeline Scheduling**: Automate the entire pipeline using ADF triggers to ensure daily updates of data and reports.
- **Testing**: Conduct end-to-end testing of the pipeline to validate data quality, transformations, and reporting.
- **Monitoring**: Implement monitoring using Azure Monitor to track pipeline performance and detect failures.

## Technology Stack
- **Azure Data Factory (ADF)**: For orchestrating data movement and transformation.
- **Azure Data Lake Storage (ADLS)**: For storing raw and processed data.
- **Azure Databricks**: For data transformation and processing.
- **Azure Synapse Analytics**: For data warehousing and SQL-based analytics.
- **Power BI**: For data visualization and reporting.
- **Azure Key Vault**: For securely managing credentials and secrets.
- **SQL Server (On-Premises)**: Source of customer and sales data.


## Pipeline Development and Execution
Developing and running the data engineering pipeline to transform raw data into actionable insights.

## 1. Environment setup
Here are the steps we have followed:

### Part 1: Azure Environment Setup
### **1. Azure Resource Group Setup**
Log in to the Azure Portal and create a Resource Group to organize all related resources.

### **2. Create Required Azure Resources**
- **Azure Data Factory**: Deploy an Azure Data Factory instance within the Resource Group for orchestrating data workflows.
- **Azure Synapse Analytics**: Set up Synapse Analytics for data querying and reporting.
- **Azure Data Lake Storage (ADLS)**: Provision a Data Lake to store raw, cleansed, and transformed data.
- **Azure Databricks**: Create a Databricks workspace for data transformation and processing.
- **Azure Key Vault**: Configure a Key Vault for securely managing sensitive information such as database credentials.

![image](https://github.com/user-attachments/assets/04077117-db53-4db3-a818-b961802005a5)


## Part 2: Setting Up the Source Database

### 1.Download Dataset

Obtain the sample dataset from the [Microsoft Dataset Repository] You can use the .bak file to restore your sample database to your SQL Server instance. You first have to download the Database, you can download with below link

https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2022.bak and save it locally.

### 2. Restore Dataset in SQL Server
Use SQL Server Management Studio (SSMS) to restore the downloaded database backup:
Open SSMS and connect to your SQL Server instance.
Right-click on Databases → Restore Database → Select the downloaded backup file → Complete the restore process.


### 3. Create a Login and User in SQL Server  

Run the following script to create a new SQL Server login, user, and assign permissions:  

sql
-- Replace YourLogin and YourPassword with your desired login name and password  
CREATE LOGIN YourLogin WITH PASSWORD = 'YourPassword';  
GO  

-- Replace YourUser with your desired username  
CREATE USER YourUser FOR LOGIN YourLogin;  
GO  

-- Grant read permissions to the restored database  
USE [YourDatabaseName];  
ALTER ROLE db_datareader ADD MEMBER YourUser;  
GO  




### 4. Verify Permissions
- In SSMS, navigate to: 
 `Databases → YourDatabase → Security → Users → [YourUser] → Properties`
- Under **Membership**, ensure that `db_datareader` is selected.

![Screenshot (63)](https://github.com/user-attachments/assets/45e52879-f1fa-45ad-af12-0031be9d59a0)



## Part 3: Secure Credentials in Azure Key Vault
**1. Store Database Credentials**
- Navigate to the Azure Key Vault created earlier.
- Add Secrets for the SQL Server username and password:

Secret Name: `SQL-Username`
Secret Value: Your SQL Server username (e.g., `YourLogin`).
Secret Name: `SQL-Password`
Secret Value: Your SQL Server password (e.g., `YourPassword`).

**2. Configure Access Policies** 
In Key Vault, assign appropriate permissions to services like Azure Data Factory to access the secrets securely.
![image](https://github.com/user-attachments/assets/4e771f84-7a12-47ba-aae8-03a6755eb372)




## AdventureWorksLT2022 Database Overview:
AdventureWorksLT2022 is a lightweight version of the AdventureWorks sample database provided by Microsoft. It is designed for learning and demonstration purposes, focusing on simplified schemas suitable for understanding basic database operations and concepts. This database represents a fictional company’s sales and business data.
### Schema Overview

AdventureWorksLT2022 contains a logical organization of tables grouped into functional areas:

**1. Sales**:

- **SalesOrderHeader**: Contains details of individual sales orders, such as order dates, totals, and statuses.
- **SalesOrderDetail**: Includes line-item details for each sales order, linking to products.

**2. Customer**:

- **Customer**: Stores customer information, including name, contact details, and demographics.
- **CustomerAddress**: Tracks multiple addresses for each customer, including billing and shipping addresses.

**3. Products**:

- **Product**: Contains details about the products, such as names, prices, and categories.
- **ProductCategory** and **ProductModel**: Provide hierarchical information about product classifications.
- **ProductDescription**: Stores textual descriptions of products to enhance their understanding and marketing.
- **ProductModelProductDescription**: Acts as a bridge table linking ProductModel to ProductDescription, supporting multilingual or versioned descriptions.

**4. Address and Location**:

- **Address**: Stores address data used across customers and sales.

Entity Relation Diagram for this Database:
![image](https://github.com/user-attachments/assets/66ae2f06-c60a-40ac-8ca9-8e9b17868478)










## 2. Data Ingestion

Data ingestion is the process of collecting data from various sources and moving it to a central location for storage, processing, and analysis. In this phase, pipelines were set up in **Azure Data Factory (ADF)** to ingest data from an on-premises SQL Server database into **Azure Data Lake Storage (ADLS)**. Data in ADLS was organized into three folders based on processing layers: **Bronze**, **Silver**, and **Gold**.

### Step 1: Pipeline to Copy All Tables Dynamically

#### Task
Create a pipeline in ADF to copy all tables dynamically from the SQL Server database in a single execution.

#### 1.1 Lookup Activity  
1. **Query Configuration**  
   - Added a **Lookup** activity to the pipeline.  
   - Configured the Lookup source with the following SQL query to fetch table names:  

     ```sql
     SELECT 
         s.name AS SchemaName,
         t.name AS TableName
     FROM sys.tables t
     INNER JOIN sys.schemas s
         ON t.schema_id = s.schema_id
     WHERE s.name = 'SalesLT';
     ```

2. **Result Output**  
   - The query returned a JSON output containing all table names under the `SalesLT` schema.  

     **Example Output:**  
     ```json
     {
       "count": 10,
       "value": [
         { "SchemaName": "SalesLT", "TableName": "Address" },
         { "SchemaName": "SalesLT", "TableName": "Customer" },
         ...
       ]
     }
     ```

#### 1.2 ForEach Activity  
1. **Adding ForEach**  
   - Dragged a **ForEach** activity into the pipeline and connected it to the Lookup activity.  
   - Configured the **Items** field in ForEach to use the `value` array from the Lookup output:  

     ```json
     @activity('look for all the tables').output.value
     ```

2. **Dynamic Table Processing**  
   - Inside the ForEach activity, added a nested **Copy Data** activity.

#### 1.3 Copy Data Activity (Inside ForEach)  
1. **Source Configuration**  
   - Configured the SQL source with a dynamic query to fetch all rows from the current table being processed:  
     ```sql
     @{concat('SELECT * FROM ', item().SchemaName, '.', item().TableName)}
     ```

2. **Sink Configuration**  
   - **Parameters:** Created two parameters in the sink dataset:  
     - `SchemaName`: Assigned with `@item().SchemaName`.  
     - `TableName`: Assigned with `@item().TableName`.  
   - **File Path:** Used dynamic expressions to store the table's data as a **Parquet** file in the **Bronze** folder:  
     - **Directory:**  
       ```sql
       @concat(dataset().SchemaName, '/', dataset().TableName)
       ```  
     - **File Name:**  
       ```sql
       @concat(dataset().TableName, '.parquet')
       ```

---
![image](https://github.com/user-attachments/assets/7113581e-1c90-414a-9cbf-2ebb7d4e9c97)

### Step 2: Running the Pipeline

1. **Publish and Trigger**  
   - Published and triggered the pipeline for execution.  
   - The pipeline iterated over all tables in the source schema, dynamically copying their data into respective folders in the Bronze layer of ADLS.  

2. **Successful Execution**  
   - The pipeline executed successfully, creating a directory structure in ADLS that mirrored the source schema and storing each table's data as a **Parquet** file.

---

### Key Benefits of the Approach

- **Dynamic Scalability**: The pipeline adapts to schema changes automatically using Lookup and ForEach activities.  
- **Optimized Storage**: Data is stored in a structured, query-friendly **Parquet** format.  
- **Layered Architecture**: The Bronze-Silver-Gold structure ensures data is progressively refined for analytics and reporting.

This completes the **Data Ingestion** phase, establishing a strong foundation for downstream transformation and analysis.





## Part 3: Data Transformation

The **Data Transformation** phase focuses on refining raw ingested data (Bronze layer) into cleansed and structured data (Silver layer) and finally into aggregated and analytics-ready data (Gold layer). This phase uses **Azure Databricks** for scalable and efficient data processing, following the principles of the **Medallion Architecture**.

---

### Step 1: Setting up Azure Databricks for Transformation

1. **Cluster Creation**  
   - Created a Spark cluster in Azure Databricks with standard configurations for distributed data processing.

2. **Role Assignments**  
   - Assigned the **"Storage Blob Data Contributor"** role to:  
     - Azure Data Factory service.  
     - Azure Synapse Analytics service.  
     - Databricks user account.

3. **Storage Mounting**  
   - Created a Databricks notebook named `StorageMount` to mount **Azure Data Lake Storage (ADLS)** containers (`Bronze`, `Silver`, `Gold`) to the **Databricks File System (DBFS)**.  

     **Python Code for Mounting:**  
     ```python
     configs = {
         "fs.azure.account.auth.type": "CustomAccessToken",
         "fs.azure.account.custom.token.provider.class": spark.conf.get(
             "spark.databricks.passthrough.adls.gen2.tokenProviderClassName"
         )
     }

     dbutils.fs.mount(
         source="abfss://bronze@<storage-account-name>.dfs.core.windows.net/",
         mount_point="/mnt/bronze",
         extra_configs=configs
     )
     ```

   - Repeated the process for `Silver` and `Gold` containers.  
   - Verified successful mounting with:  
     ```python
     dbutils.fs.ls("/mnt/bronze")
     dbutils.fs.ls("/mnt/silver")
     dbutils.fs.ls("/mnt/gold")
     ```

---

### Step 2: Transforming Data (Bronze → Silver → Gold)

#### 2.1 Bronze to Silver Transformation
- **Objective:** Cleanse and standardize raw data in the Bronze layer.  

1. **Identifying Tables**  
   - Extracted all table names from the `Bronze/SalesLT` directory:  
     ```python
     table_names = [i.name.split('/')[0] for i in dbutils.fs.ls("/mnt/bronze/SalesLT/")]
     ```

2. **Date Field Standardization**  
   - Standardized "Date" columns in `yyyy-MM-dd` format using:  
     ```python
     from pyspark.sql.functions import from_utc_timestamp, date_format
     from pyspark.sql.types import TimestampType

     df = df.withColumn(
         col,
         date_format(from_utc_timestamp(df[col].cast(TimestampType()), "UTC"), "yyyy-MM-dd")
     )
     ```

3. **Saving to Silver Layer**  
   - Saved transformed tables in **Delta format** to the Silver layer:  
     ```python
     output_path = f"/mnt/silver/SalesLT/{table_name}/"
     df.write.format("delta").mode("overwrite").save(output_path)
     ```

#### 2.2 Silver to Gold Transformation
- **Objective:** Prepare data for analytics by optimizing column naming conventions.  

1. **Identifying Tables**  
   - Extracted table names from the Silver layer:  
     ```python
     table_names = [i.name.split('/')[0] for i in dbutils.fs.ls("/mnt/silver/SalesLT")]
     ```

2. **Renaming Columns**  
   - Converted `camelCase` column names to `snake_case`:  
     ```python
     new_col_name = "".join(
         ["_" + char if char.isupper() and not old_col_name[i-1].isupper() else char
         for i, char in enumerate(old_col_name)]
     ).lstrip("_")
     ```

3. **Saving to Gold Layer**  
   - Saved the finalized tables in **Delta format** to the Gold layer:  
     ```python
     output_path = f"/mnt/gold/SalesLT/{table_name}/"
     df.write.format("delta").mode("overwrite").save(output_path)
     ```

---

### Step 3: Automating Transformation with Azure Data Factory

1. **Token Storage**  
   - Generated an Azure Databricks token and securely stored it in **Azure Key Vault**.

2. **Creating Linked Services**  
   - Established a linked service between ADF and Databricks by providing the Databricks workspace URL and the authentication token.

3. **Integrating Notebooks into the Pipeline**  
   - Added Databricks Notebook activities in the ADF pipeline:  
     - `/Shared/bronze_to_silver` for Bronze to Silver transformation.  
     - `/Shared/silver_to_gold` for Silver to Gold transformation.  
![image](https://github.com/user-attachments/assets/721c8191-8d90-4ab0-b562-2a9bb8bbedb8)

4. **Triggering the Pipeline**  
   - Executed the pipeline to validate the entire workflow.

---

### Key Benefits of the Transformation Process

1. **Efficient Storage Access**: Seamless and secure access to ADLS containers via Databricks mounting.  
2. **Standardized Data Format**: Delta Lake ensures transactional consistency, scalability, and performance optimization.  
3. **Automated Workflows**: Integration with ADF enables smooth orchestration of the transformation process.  
4. **Layered Processing**: Medallion Architecture organizes data for progressive refinement and easy traceability.

This completes the **Data Transformation** phase, preparing the data for insights and analytics in the reporting stage.















![image](https://github.com/user-attachments/assets/721c8191-8d90-4ab0-b562-2a9bb8bbedb8)

--------









![image](https://github.com/user-attachments/assets/7113581e-1c90-414a-9cbf-2ebb7d4e9c97)



## Setup Instructions

### Prerequisites
- An Azure account with sufficient credits.
- Access to an on-premises SQL Server database.

### Step 1: Azure Environment Setup
- **Create Resource Group**: Set up a new resource group in Azure.
- **Provision Services**:
  - Create an Azure Data Factory instance.
  - Set up Azure Data Lake Storage with bronze, silver, and gold containers.
  - Set up an Azure Databricks workspace and Synapse Analytics workspace.
  - Configure Azure Key Vault for secret management.

### Step 2: Data Ingestion
- **Set up SQL Server**: Install SQL Server and SQL Server Management Studio (SSMS), restore the AdventureWorks database.
- **Ingest Data with ADF**: Create pipelines in ADF to copy data from SQL Server to the bronze layer in ADLS.

### Step 3: Data Transformation
- **Mount Data Lake in Databricks**: Configure Databricks to access ADLS.
- **Transform Data**: Use Databricks notebooks to clean and aggregate the data, transitioning it from bronze to silver to gold.

### Step 4: Data Loading and Reporting
- **Load Data into Synapse**: Set up a Synapse SQL pool and load the gold data for analysis.
- **Create Power BI Dashboard**: Connect Power BI to Synapse and create visualizations based on business requirements.

### Step 5: Automation and Monitoring
- **Schedule Pipelines**: Use ADF to schedule the data pipelines to run daily.
- **Monitor Pipeline Runs**: Use ADF and Synapse monitoring tools to ensure pipeline execution.

### Step 6: Security and Governance
- **Manage Access**: Set up role-based access control (RBAC) using Azure Entra ID (formerly Active Directory).

### Step 7: End-to-End Testing
- **Trigger and Test Pipelines**: Insert new records into the SQL database and ensure the pipeline runs successfully, updating the Power BI dashboard.

## Conclusion

This project provides a robust end-to-end solution for understanding customer demographics and their impact on sales. The automated data pipeline ensures stakeholders always have access to the most current and actionable insights.

---
For more detailed explanation, please refer to the [medium.com Article](https://medium.com/@utkarshthakur2404/project-headline-building-a-scalable-azure-data-engineering-pipeline-real-time-customer-and-sales-d5fbc266bc0a).
