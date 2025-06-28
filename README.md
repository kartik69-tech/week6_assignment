# week6_assignment
#  Azure Data Factory: End-to-End Data Ingestion & Automation

## Project Overview

This project demonstrates an end-to-end data ingestion and automation pipeline using Azure Data Factory (ADF). The objective is to extract data from multiple sources — including an on-premises SQL Server and FTP/SFTP servers — and load it into an Azure SQL Database. 

It features both incremental loading and automated scheduling, including:
- Daily runs
- A custom monthly trigger for the last Saturday of each month


## Key Components Implemented

### 1. Self-hosted Integration Runtime (SHIR)
- Installed and configured SHIR on a local server
- Connected to on-premises SQL Server for secure data extraction

### 2. On-Premises to Azure SQL Pipeline
- Built an ADF pipeline using **Copy Activity**
- Source: `Customers` table from on-prem SQL Server
- Sink: Azure SQL Database

### 3. FTP/SFTP to Azure SQL Pipeline
- Connected to an external FTP/SFTP server
- Extracted `.csv` files and ingested them into the same Azure SQL target

### 4. Incremental Load Pipeline
- Implemented watermark logic using `LastModifiedDate`
- Used a **Lookup** activity to fetch the last load timestamp
- Used **dynamic SQL** to filter for new/updated rows
- Called a **Stored Procedure** to update the watermark after each successful run

### 5. Daily Trigger
- Configured a **daily trigger** to run the incremental pipeline at midnight

### 6. Monthly Trigger (Last Saturday)
- Scheduled the pipeline to run **every Saturday**
- Used a **Lookup + If Condition** to determine if it's the **last Saturday** of the month
- Ensured execution happens only once per month accordingly


## Testing & Validation

- Verified data transfer from both source types (on-prem & SFTP)
- Validated `Customers` data was loaded without primary key conflicts
- Ensured `WatermarkTracking` table updates correctly
- Confirmed that triggers work as expected

## Structure
```

├── pipelines/
│   └── OnPremToAzurePipeline.json
│   └── FtpToAzurePipeline.json
├── datasets/
│   └── DS_OnPrem_Customers.json
│   └── DS_Azure_Customers.json
│   └── DS_FTP_Customers.json
├── linkedServices/
│   └── OnPrem_SQL_LinkedService.json
│   └── Azure_SQL_LinkedService.json
│   └── FTP_LinkedService.json
├── triggers/
│   └── DailyTrigger.json
│   └── SaturdayTrigger.json
├── sql/
│   └── CreateWatermarkTracking.sql
│   └── UpdateWatermarkProcedure.sql
├── README.md
