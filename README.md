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

##Part 6: Security, Automation & End-to-End Pipeline Testing

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












## Solution Overview

To meet these requirements, the solution is broken down into the following components:

### Data Ingestion
- Extract customer and sales data from an on-premises SQL database.
- Load data into Azure Data Lake Storage (ADLS) via Azure Data Factory (ADF).

### Data Transformation
- Use Azure Databricks to clean and transform the data.
- Organize the data into Bronze, Silver, and Gold layers for raw, cleansed, and aggregated data respectively.

### Data Loading and Reporting
- Load transformed data into Azure Synapse Analytics.
- Build a Power BI dashboard to visualize sales and demographic insights.

### Automation
- Schedule the pipeline to run daily, ensuring that data and reports remain up-to-date.

## Technology Stack

- **Azure Data Factory (ADF)**: Orchestrates data movement and transformation.
- **Azure Data Lake Storage (ADLS)**: Stores raw and processed data.
- **Azure Databricks**: Performs data transformation and processing.
- **Azure Synapse Analytics**: Data warehousing and SQL-based analytics.
- **Power BI**: For data visualization and reporting.
- **Azure Key Vault**: Securely manages credentials and secrets.
- **SQL Server (On-Premises)**: Source of customer and sales data.

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
