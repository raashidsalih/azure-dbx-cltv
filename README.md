# Data Pipeline for CLTV and Customer Segmentation Analysis Using Azure Services
An end to end data pipeline that models and visualizes CLTV (Customer Lifetime Value) and customer segmentation. The project seeks to employ Azure Databricks and other Azure services as its architectural basis. It is currently a work in progress, but the proposed architecure is detailed below.

## Proposed Architecture
1. Raw training data ingested into databricks following medallion architecture, the basis is Azure Data Lake Storage. Curated data present in Delta Lake.
    - Raw data is stored in bronze tier.
    - Move to Silver tier after transformations and feature engineering. Data now present in delta lake.
    - Keep in mind medallion is not an architectural reconceptualization, just a new way to articulate the traditional pipeline design from raw data to staging layer to presentation layer. It tries to convey the process of data enrichment as said data flows through the pipeline from bronze to silver to gold.
2. Two Machine Learning models trained using databricks services:
    - CLTV estimation - regression problem, estimate the total revenue that a customer will generate for the company over their entire relationship. The model can help the company to optimize its marketing and sales efforts by segmenting and targeting customers based on their CLTV.
    - Customer segmentation - clustering problem, The model can help the company to understand its customer base better and tailor its products and services according to each cluster’s needs and preferences.
3. MLFlow manages MLOps aspect of operation. 
    - Databricks stores information about models in the MLflow Model Registry.
    - The registry makes models available through batch, streaming, and REST APIs.
4. New customer data flows through from Azure Data Factory.
    - Synthetic data generated from SDV model hosted on another VM
    - Obtained via REST API
5. Get predictions and results from aforementioned models, feed it into delta lake.
    - Transformations move it from bronze to silver tier
    - Aggregations based on final dashboard and metric requirements move it into gold tier
6. Use PowerBI for visualization and dashboarding
    - Leverage Azure Databricks connector to access data
7. A possible alternative is to use Azure Analysis Services or Azure Synapse Analytics as a semantic layer between Azure Databricks and Power BI, and use the optimized Synapse connector to export gold data sets out of the data lake.
8. Additional Azure services provides support in terms of governance and security:
    - Azure key vault for storing secrets
    - Azure devops for CI/CD and version control. Could potentially use Databricks Repos for the same.
    - Azure Monitor for resource based telemetry.
