# ShoppingMart-Data-Warehouse-Microsoft-Fabric



This repository contains the full workflow for an end-to-end data warehouse project, "ShoppingMart," built entirely on the Microsoft Fabric platform. The project demonstrates a complete Medallion Architecture (Bronze, Silver, Gold) using Fabric's core components.

The pipeline is designed to be dynamic and metadata-driven. It ingests multiple files from a GitHub repository by calling the API, filters for a specific list of files, and then iterates over that list to load them into the Bronze Lakehouse. From there, a PySpark notebook transforms the data into the Silver and Gold layers, making it ready for BI analysis via the SQL Analytics Endpoint.

Key Components & Project Recall (Based on Images)
This project is built from two main parts: the dynamic ingestion pipeline and the warehouse transformation.

1. The Dynamic Ingestion Pipeline (The "How")

The pipeline ShoppingMart_DI_BL (shown in your pipeline canvas and run history) is responsible for the "Data Ingest" step:

Git_To_Fabric (Web Activity): This is the first step. It makes an API call to GitHub to get a JSON list of all the files in a specific repository.

Filter1 (Filter Activity): This activity takes the full file list from the step above and filters it down. Based on your previous projects, this is where you check for specific file names (e.g., customers.csv, products.csv, etc.).

FileArray (Set variable): This stores the output of the filter (the files you want) into an array variable.

ForEach1 (ForEach Loop): This loop runs in parallel and iterates over the FileArray.

Copy data1 (Copy Activity): Inside the loop, this is the main workhorse. It's a dynamic copy activity that uses parameters (like @item().source_url and @item().sink_file) to copy each file from GitHub directly into the Bronze Lakehouse.

2. The Medallion Architecture (The "What")

The workspace items show the full data flow:

shoppingmart_Bronze (Lakehouse): This is the Bronze Layer. It's the destination for the pipeline, storing the raw, unaltered data.

ShoppingMart_Silver (Lakehouse): This is the Silver Layer. Data from Bronze is cleaned, transformed, and joined here.

ShoppingMart_GoldLayer (Notebook): This is the Transform logic (the "T" in ELT). This PySpark notebook most likely contains the code that reads from the Bronze Lakehouse, applies business rules, and writes the clean data to the Silver and Gold layers.

Gold_Transformation (LakeCouse / SQL Endpoint): This is the final Gold Layer. It contains the aggregated, analytics-ready tables for BI. The SQL Analytics Endpoint for this Lakehouse is what you would connect to Power BI for your "Data visualize" step.
