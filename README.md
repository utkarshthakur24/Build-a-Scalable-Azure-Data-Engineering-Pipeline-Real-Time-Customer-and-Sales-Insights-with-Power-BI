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
- Provision Azure Resources: Set up necessary resources including Azure Data Lake Storage (ADLS), Azure Data Factory (ADF), Azure Databricks, and Azure Synapse Analytics.
- Configure Networking and Access: Ensure secure communication between services using Azure Private Link and Managed Identity.

## Part 2: Data Ingestion
- Source Data: Extract customer and sales data from the on-premises SQL database.
- Data Load: Use Azure Data Factory (ADF) to load the extracted data into the raw layer (Bronze) of ADLS.

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
### **1.Azure Resource Group Setup**
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
-- Replace `YourLogin` and `YourPassword` with your desired login name and password
 CREATE LOGIN YourLogin WITH PASSWORD = 'YourPassword';
 GO
-- Replace `YourUser` with your desired username
 CREATE USER YourUser FOR LOGIN YourLogin;
 GO
-- Grant read permissions to the restored database
 USE [YourDatabaseName];
 ALTER ROLE db_datareader ADD MEMBER YourUser;
 GO

 Example: Replace `YourLogin` with `xyx`, `YourPassword` with `admin`, and `YourDatabaseName` with the name of the restored database.


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

1. Sales:

    SalesOrderHeader: Contains details of individual sales orders, such as order dates, totals, and statuses.
    SalesOrderDetail: Includes line-item details for each sales order, linking to products.

2. Customer:

    Customer: Stores customer information, including name, contact details, and demographics.
    CustomerAddress: Tracks multiple addresses for each customer, including billing and shipping addresses.

3. Products:

    Product: Contains details about the products, such as names, prices, and categories.
    ProductCategory and ProductModel: Provide hierarchical information about product classifications.
    ProductDescription: Stores textual descriptions of products to enhance their understanding and marketing.
    ProductModelProductDescription: Acts as a bridge table linking ProductModel to ProductDescription, supporting multilingual or versioned descriptions.

4. Address and Location:

    Address: Stores address data used across customers and sales.

Entity Relation Diagram for this Database:
![image](https://github.com/user-attachments/assets/66ae2f06-c60a-40ac-8ca9-8e9b17868478)





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
