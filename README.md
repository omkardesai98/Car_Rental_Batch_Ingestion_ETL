
# Car-Rental-Batch-Ingestion-ETL

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Usage](#usage)
- [Data Flow](#data-flow)
- [Data Validation](#data-validation)
- [SCD2 Implementation](#scd2-implementation)
- [Challenges and Solutions](#challenges-and-solutions)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Project Overview
This project demonstrates an ETL pipeline built using Snowflake, PySpark, and Airflow. The pipeline processes two daily files stored in a Google Cloud Storage (GCS) bucket: one containing transaction data and another with customer-related data. The transaction data is validated and transformed before being loaded into a Snowflake table. Additionally, a Slowly Changing Dimension Type 2 (SCD2) merge operation is performed on the customer data to maintain historical accuracy.

## Features
- Automated ETL pipeline orchestrated with Airflow
- Data validation and transformation using PySpark
- SCD2 implementation for customer dimension table in Snowflake
- Integration with Google Cloud Storage for daily data ingestion

## Architecture
![Architecture Diagram](./screenshots/Architecture_diagram.png)

The architecture consists of:
- **GCS Bucket**: Stores daily transaction and customer data files.
- **Airflow**: Orchestrates the ETL process, triggering the pipeline daily.
- **PySpark**: Handles data transformation, validation, and load.
- **Snowflake**: Data warehouse where transformed data is stored and perform SCD2 Merge.

## Tech Stack
- **Languages**: Python, SQL
- **Data Processing**: PySpark
- **Orchestration**: Apache Airflow
- **Cloud Storage**: Google Cloud Storage
- **Data Warehouse**: Snowflake
- **Version Control**: Git

## Usage
In this project, we automate the processing of two daily files: one JSON file containing transaction data and another CSV file with customer data changes. The process begins by creating all necessary dimension tables in Snowflake, except for the customer dimension and fact tables. Data is then loaded into these dimension tables.

Next, we establish a storage integration in Snowflake to directly load data from the Google Cloud Storage (GCS) bucket into Snowflake tables. A PySpark job is then executed to validate and transform the incoming transaction data from the JSON file in GCS. This job also prepares the fact table with the required columns by leveraging the Snowflake dimension tables.

To orchestrate the workflow and manage dependencies, an Airflow DAG is used. The DAG is designed to support both daily processing and backfilling of data. It accepts an execution date parameter, allowing users to specify the date for which the files should be processed. If no date is provided, the DAG processes the current day's files. The workflow is structured to first update the customer data, followed by inserting the transformed transaction data into the Snowflake table. This is achieved using the SnowflakeOperator in Airflow, ensuring smooth interaction with Snowflake and maintaining the correct order of operations.

## Data Flow
1. The Airflow DAG is triggered by the user for a specified date.
2. It first updates the customer dimension table in Snowflake if it already exists.
3. New customer data from the GCS bucket is then loaded and inserted into the customer dimension table using Snowflake.
4. The DAG proceeds to load the new transaction data by utilizing PySpark, which reads the data from the Google Cloud Storage bucket and processes it accordingly into the Snowflake.








