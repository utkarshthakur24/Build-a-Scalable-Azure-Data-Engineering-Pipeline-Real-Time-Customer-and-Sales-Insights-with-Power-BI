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

```sql
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
```



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
Link for Data Bricks NoteBooks:
- [StorageMount.ipynb](https://github.com/utkarshthakur24/End-to-End-Azure-Data-Engineering-Project--Real-Time-Sales-Analytics-and-Insights-with-Power-BI/blob/main/Azure%20DataBricks%20NoteBooks/StorageMount.ipynb)
- [bronze to silver.ipynb](https://github.com/utkarshthakur24/End-to-End-Azure-Data-Engineering-Project--Real-Time-Sales-Analytics-and-Insights-with-Power-BI/blob/main/Azure%20DataBricks%20NoteBooks/bronze%20to%20silver.ipynb)
- [silver to gold.ipynb](https://github.com/utkarshthakur24/End-to-End-Azure-Data-Engineering-Project--Real-Time-Sales-Analytics-and-Insights-with-Power-BI/blob/main/Azure%20DataBricks%20NoteBooks/silver%20to%20gold.ipynb)





# Part 4: Data Loading

In this phase of the project, we use **Azure Synapse Analytics** to load and manage data from the **Gold container** in the data lake. The goal is to make data accessible for querying and reporting through a **serverless SQL database**. This approach allows for efficient and cost-effective data handling while leveraging the integration and transformation capabilities of Synapse Analytics.

## Using Azure Synapse Analytics for Data Loading

Azure Synapse Analytics serves as a comprehensive platform combining **data orchestration** (similar to Azure Data Factory) and **data transformation** (similar to Azure Databricks). It simplifies workflows by enabling the creation of both **pipelines** and **Spark-based notebooks** in a unified environment.

For this project, the **serverless SQL database** option was chosen due to the following benefits:

### Benefits:

- **Separation of Compute and Storage:**
  - Data remains in the data lake, and only compute resources are utilized when querying the data.
  - Ideal for smaller datasets or scenarios where the data structure is already optimized in the data lake.

- **Cost Efficiency:**
  - Serverless SQL databases charge only for query execution, unlike dedicated SQL pools, which require both storage and compute resources.

## Steps to Set Up the Serverless SQL Database

### 1. Database Creation

A serverless SQL database named **gold_db** was created in **Synapse Analytics**. This database is directly connected to the data lake, so no additional linked service was required.

### 2. Creating Views Dynamically

To simplify querying, **views** were created for each table in the Gold container. A stored procedure was developed to automate this process.

#### Stored Procedure: `CreateSQLServerlessview_gold1`

This stored procedure dynamically creates or updates views for tables in the Gold container:

```sql
USE gold_db
GO

CREATE OR ALTER PROC CreateSQLServerlessview_gold1 @ViewName NVARCHAR(100)
AS
BEGIN
    DECLARE @statement VARCHAR(MAX)
    SET @statement = N'CREATE OR ALTER VIEW ' + @ViewName + ' AS
    SELECT *
    FROM
    OPENROWSET(
        BULK ''https://intechsg24.dfs.core.windows.net/gold/SalesLT/' + @ViewName + '/'',
        FORMAT = ''DELTA''
    ) AS [result]'
    EXEC(@statement)
END
GO
```

This stored procedure generates views dynamically for Delta-formatted data in the Gold container.


## 3. Pipeline Creation in Synapse Analytics

To automate the execution of the stored procedure for all tables in the **Gold container**, a **Synapse pipeline** was created.

### Pipeline Configuration:

#### Step 1: Get Table Names

- **Activity:** Get Metadata
- **Settings:**
  - **Linked Service:** Data Lake Gen 2
  - **Field List:** Child items
  - **Output:** Retrieves the folder names (table names) in the Gold container.

**Sample Output:**

```json
{
    "childItems": [
        {"name": "Address", "type": "Folder"},
        {"name": "Customer", "type": "Folder"},
        ...
    ]
}
```

#### Step 2: ForEach Loop

- Iterates over the table names retrieved from the **Get Metadata** activity.

#### Step 3: Execute Stored Procedure

- **Activity:** Stored Procedure
- **Linked Service:** Connection to the serverless SQL database.
- **Parameters:**
  - `ViewName`: The name of each table (retrieved dynamically).
  - **Dynamic Expression:** `@item().name`.
![image](https://github.com/user-attachments/assets/ae6f74b3-0d57-408c-8327-ee1748fad9b1)


After publishing and running the pipeline, views were successfully created for all tables in the Gold container.

## Advantages of the Data Loading Setup

- **Dynamic View Creation:** Automating view creation ensures scalability and minimizes manual effort when new tables are added to the Gold container.
- **Seamless Integration:** The built-in connection between **Synapse Analytics** and **Azure Data Lake** simplifies data access without additional configurations.
- **Optimized Query Performance:** Using **serverless SQL pools** enables querying **Delta-formatted data** directly from the data lake without data duplication.

## Next Steps: Data Reporting

With views now available in the **serverless SQL database**, the next step involves integrating **Power BI** to connect to this database. Power BI will be used to fetch and visualize the data, enabling actionable insights and reporting. This final phase will complete the end-to-end data pipeline, delivering a robust analytics solution.




# Part 5: Data Reporting

The final phase of the project focuses on using **Power BI** to visualize the transformed data from the serverless SQL database. By connecting Power BI to the Synapse Analytics endpoint, we leverage the pre-built views from the **Gold container** to create meaningful dashboards and reports for decision-making.

---

## Steps for Data Reporting

### 1. Connecting Power BI to Azure Synapse Analytics

1. Open **Power BI Desktop**.
2. Go to **Get Data** > Select **Azure Synapse Analytics** as the data source.
3. Fill in the required details:
   - **Server:** Retrieve the Serverless SQL endpoint from the Synapse Analytics properties page.
   - **Database Name:** Enter the name of the serverless SQL database (e.g., `gold_db`).
4. Choose **Import** mode for optimal performance.

### 2. Authentication

- Login using your Microsoft account credentials (same as your Azure portal credentials).

### 3. Loading Data

1. After successfully connecting, all views from the serverless SQL database are displayed.
2. Select the desired tables (tick all or specific ones as required).
3. Click **Load** to bring the data into Power BI for modeling and reporting.

---

## Data Modeling and Dashboard Creation

### Data Modeling

After importing the data into Power BI, we established the necessary relationships between tables to ensure seamless integration for analysis and reporting. This step was crucial to building an accurate and reliable data model.

---

### Preparing Data for Insights

To enhance insights, we derived additional data points. For example, we added a **Gender** column to the `Customer` table, capturing customer gender based on the `Title` field. Below is the DAX formula used:

```DAX
Gender = 
IF(Customer[Title] = "Mr.", "Male", 
    IF(Customer[Title] = "Ms.", "Female", "Other"))
```


This allowed segmentation of data by gender, enriching the analysis and visualizations.


# Designing the Dashboard

Using the pre-built views from the **Gold layer**, we developed a comprehensive dashboard to meet business and management requirements.

---

## Key Features of the Dashboard

### Key Performance Indicators (KPIs):
- **Total Sales Revenue** (Card Visualization)
- **Total Sales Quantity** (Card Visualization)
- **Total Number of Products** (Card Visualization)
- Filters for **Gender** and **Product Category** to allow drill-down insights.

---

## Charts and Visualizations

### 1. Pie Chart:
- **Legend:** Gender (from the `Customer` table)
- **Values:** Line Total (from the `SalesOrderDetail` table)

### 2. Clustered Bar Chart:
- **Y-Axis:** Product Name (from the `Product` table)
- **X-Axis:** Line Total (from the `SalesOrderDetail` table)

---

## Final Output

The final dashboard offers intuitive visualizations and actionable insights, empowering stakeholders to make data-driven decisions.

### Key Insights:

💸 **Total Revenue by Product Category**:  
- **Touring Bikes** is the top category, generating **32%** of revenue, followed by **Road Bikes** (26%) and **Mountain Bikes** (24%).

🚻 **Revenue by Gender**:  
- **81.47%** of revenue is generated by **Male** customers.  
- **18.53%** of revenue is generated by **Female** customers.  

**Observation:** The higher revenue from male customers may reflect their greater interest in outdoor activities and bike-related purchases compared to female customers.

---

## Screenshot of the Completed Dashboard

![image](https://github.com/user-attachments/assets/24e7b463-d6f9-48c6-b592-6094f9367ded)

---

## Conclusion

The dashboard showcases key metrics and trends derived from the Gold layer, enabling stakeholders to make data-driven decisions effectively. Its intuitive design and insightful visualizations provide actionable insights for business growth.









![image](https://github.com/user-attachments/assets/24e7b463-d6f9-48c6-b592-6094f9367ded)




# Part 6: Security, Automation, and End-to-End Pipeline Testing

In this phase, we focus on implementing security measures, enabling pipeline automation, and conducting end-to-end testing to ensure a reliable and scalable data engineering workflow.

## 1. Security Using Azure Active Directory (AAD)

For real-time projects involving multiple data engineers, managing access to resources is critical. Assigning access individually to team members is inefficient and hard to maintain. Instead, **Azure Active Directory (AAD)** allows you to simplify access control using **security groups**.

### Steps to Implement Security with AAD

#### 1.1 Create a Security Group

1. Navigate to **Azure Active Directory** > **Manage** > **Groups**.
2. Click on **New Group**.
3. Fill in the details:
   - **Group Type:** Select **Security**.
   - **Group Name:** Provide a descriptive name (e.g., Data Engineering Team).
   - **Owner:** Assign the group owner.
   - **Members:** Add all team members who require access.
4. Click **Create**.

#### 1.2 Assign Access to the Resource Group

1. Go to the **Resource Group** associated with the project.
2. Navigate to **Access Control (IAM)** > **Add Role Assignment**.
3. Choose a **Role** (e.g., **Contributor** to allow full access to the resources within the group).
4. In **Members**, select the newly created security group.
5. Click **Review and Assign**.

### Benefits:

- Adding new team members is as simple as adding them to the security group.
- **Centralized access management** ensures security and scalability.
- Access is revoked automatically if a team member is removed from the security group.

## 2. Automation and Testing

Automation ensures pipelines run reliably without manual intervention, while testing validates the end-to-end data flow and system behavior.

### Automation with Triggers

**Azure Data Factory (ADF)** supports triggers to schedule or automate pipeline executions based on specific events or time intervals.

#### 2.1 Configure a Schedule Trigger

1. Open **Azure Data Factory** > **Author** > **Pipelines**.
2. Select the pipeline to automate.
3. Click **Add Trigger** > **New/Edit** > **New Trigger**.
4. Fill in the trigger details:
   - **Name:** Provide a meaningful name (e.g., **Daily Trigger**).
   - **Type:** Select **Schedule**.
   - **Time Zone:** Choose your desired time zone (e.g., **Indian Standard Time**).
   - **Recurrence:** Define the frequency (e.g., **1 Day**) and execution time.
5. Save the settings and **publish** the pipeline.

### Testing the Pipeline

1. Add test data to the source system (e.g., insert new rows into source tables).
2. Wait for the configured trigger to execute the pipeline.
3. Validate the data flow:
   - Verify that the pipeline processed the new records and updated the **Gold container** in the data lake.
   - Open **Power BI**, click **Refresh**, and confirm the updated data reflects on the dashboard.

#### Scenario Example:

- The trigger is set to run daily at **00:00**.
- Test data is added to the source table.
- At midnight, the pipeline runs automatically, processes the new data, and updates the Gold container.
- Upon refreshing Power BI, the dashboard reflects the new data.

By incorporating **security** with Azure Active Directory, **automation** through triggers, and **rigorous testing**, we ensure:

- **Secure Resource Access:** Simplified and scalable access management for the data engineering team.
- **Seamless Automation:** Pipelines execute without manual intervention, improving reliability.
- **Validated Processes:** End-to-end testing guarantees the system handles data changes and updates the reporting layer accurately.

This step completes the setup of a **secure, automated**, and **fully functional data engineering pipeline**, ready for production use.





## Conclusion

This project provides a robust end-to-end solution for understanding customer demographics and their impact on sales. The automated data pipeline ensures stakeholders always have access to the most current and actionable insights.

---
For more detailed explanation, please refer to the [medium.com Article](https://medium.com/@utkarshthakur2404/project-headline-building-a-scalable-azure-data-engineering-pipeline-real-time-customer-and-sales-d5fbc266bc0a).
