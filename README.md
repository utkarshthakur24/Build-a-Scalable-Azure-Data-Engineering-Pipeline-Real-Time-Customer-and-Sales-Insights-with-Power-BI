# Azure End-to-End Data Engineering Real-Time Project

This project is a data engineering pipeline solution to a fictional business problem, designed to help me learn and understand data pipelining.

## Project Overview

This project builds a comprehensive data pipeline on Azure to address a critical business need. The objective is to extract customer and sales data from an on-premises SQL database, transform it in the cloud, and generate actionable insights via a Power BI dashboard. The dashboard highlights key performance indicators (KPIs) such as gender distribution and product category sales. Stakeholders can filter and analyze data by date, product category, and gender.

## Business Requirements

The business identified a gap in understanding customer demographics, particularly gender distribution, and its influence on product purchases. The key requirements are:

- **Sales by Gender and Product Category**: A dashboard displaying total products sold, total sales revenue, and gender breakdown.
- **Data Filtering**: Ability to filter data by product category, gender, and date.
- **User-Friendly Interface**: Easy access for stakeholders to query data.

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
For more details, please refer to the [documentation](./docs).
