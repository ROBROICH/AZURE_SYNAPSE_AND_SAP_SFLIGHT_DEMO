# 🛫 Azure Synapse and SAP SFlight integration scenario
 This document will describe an End to End(E2E) scenario demonstrating the integration between Azure Synapse and a SAP S/4HANA on Azure system. 
 
To demonstrate the SAP integration with a well-known, less complex and repeatable data model, the SAP SFlight sample application was selected. Further information about the SAP SFlight sample application can be found [here](https://help.sap.com/doc/saphelp_nw70/7.0.31/en-US/cf/21f304446011d189700000e8322d00/content.htm?no_cache=true).

The motivation for creating this E2E scenario is to demonstrate, how SAP ERP data is practically processed inside the Data-Warehouse(DWH) management components and services of Azure Synapse. 
This documentation is work in progress and current plan is to continuously extend the scenarios and documentation.  
 The current backlog: 
 * Automated deployment of the scenarios 
 * Extended Data-Warehouse scenarios with Change Data Capture(CDC) handling
 * Azure Purview m integration ([Link](https://www.youtube.com/watch?v=Q9aIs9cnmps))
  
  ![scenario_exploration_and_discovery](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/scenario_exploration_and_discovery.png?raw=true)

## 👩‍💻 Scenario use-case and persona description 

For the creation and implementation of the scenario three types of personas were assumed:
![personas](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/personas.png?raw=true)

__Bianca Basis__
* SAP Basis administrator
* Maintains RFC and data authorizations for the Azure Data Factory(ADF) SAP Table Connector 
* Monitors the RFC memory consumption and general performance 
* Contact for SAP ERP related troubleshooting  

__Sabrina Sap__
* DWH and BI developer 
* SAP domain and data model expert 
* SQL and data engineering knowledge 
* PowerBI expert 
* Contact person for LoB user 

__Daniel Data-Science__
* Data-Scientist 
* Fluent in Scala and Python 
* Builds M/L models on SAP data 
* Builds on Sabrina's models 

Based on the personas and expertise described above, two scenarios or use-cases were defined and implemented.


### Scenario 1: SAP data model discovery and exploration 

This use-case is conceptually designed for rapid prototyping and piloting on top of SAP ERP data using Azure Synapse and Microsoft PowerBI(PBI). These prototypes and pilots would be the agile preparation for further productive DWH implementations. 

Before creating the final DWH data models and data ingestion pipelines, certain SAP data model and customer specific data discovery and exploration tasks have typically to be executed: 
* Identify custom columns (Z-fields)
* Filled and empty columns in SAP tables
* Identify timestamp columns for Change Data Capture  
* Identify partition criteria and filters for efficient data replication 

This scenario is fully integrated into Azure Synapse Workspaces using the following Azure data platform and Azure Synapse services and tools: 

* Azure Data Lake Gen2 
* Azure Synapse serverless SQL pools 
* Azure Synapse Pipelines
* Microsoft PowerBI (Integrated into Azure Synapse Workspace)

![Azure Synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/media/overview-what-is/synapse-architecture.png)

### SAP data visualization and M/L model creation 

In this scenario a Data Scientist utilizes Apache Spark and Spark Notebooks to implement machine learning models on SAP ERP data. 
Additional requirements are the option to use Python libraries for visualizations within the Apache Spark notebooks or to process large data volumes with Apache Spark. 

The scenario was designed based on the following Azure Synapse tools and services:
* Apache Spark Pools 
* Apache Spark Notebooks 
* Python libraries like Pandas, Matplotlib or Seaborn 

## 🗺️ High level architecture overview and scenario implementation 
The architecture and components were selected according to the capabilities provided by the Modern Data Warehousing reference architecture. 
Each architecture layer and corresponding high-level implementation steps are described along the numbers marked in the architecture diagram: 

![Scenario architecture](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/architecture.png?raw=true)

1. __SAP S/4HANA on Azure Virtual Machines (VMs)__

The scenario was implemented on an SAP S/4HANA demo system running on Azure. Further technical information about providing SAP S/4HANA with Virtual Machines on Azure as infrastructure can be found [here](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/sap/sap-s4hana).

Due to the integration via the Self hosted integration runtime, the scenario could as well be implemented for additional deployment options like SAP HANA Enterprise Cloud(HEC) or S/4HANA Private Cloud Edition (PCE) on Azure using networking options like VNet-peering.


2. __Self hosted Integration Runtime__

The Self Hosted Integration Runtime(SHIR) establishes the secure connection between the SAP system and Azure Synapse. Technically SAP RFC interface and SAP Connector for .NET are used to establish the communication between the SAP system and the SHIR. 
Detailed information for the required SAP Netweaver configuration can be found [here](https://docs.microsoft.com/en-us/azure/data-factory/connector-sap-table#prerequisites). 

[Create and configure a self-hosted integration runtime](https://docs.microsoft.com/en-us/azure/data-factory/create-self-hosted-integration-runtime). 

3. __Azure Synapse Pipeline built with Copy Data Tool__

For data ingestion from SAP S/4HANA to Azure Data Lake Gen2 an Azure Synapse Pipeline was implemented. 
The data extraction interface, from the SAP ERP to the Synapse Pipeline, is the Azure Data Factory(ADF) SAP table connector. The SAP table connector enables data engineers to configure SAP ABAP tables for data extraction by the Azure Synapse Pipeline. 
The Copy Data tool provides the capability to efficiently configure multiple SAP tables for extraction within a single Pipeline. Data engineers have the option to mass select and preview SAP tables in a single dialog as shown in the screenshot below. 

By providing a responsive design for browsing over a large number of ABAP-tables, the Copy Data Tool enables an efficient bulk configuration of the required SAP tables:

![Copy data tool mass selection](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/CopyDataTool1.png?raw=true)

Additional ADF SAP Table connector related topics like timestamp-based change data capture (CDC) or partitioning options and strategies will be covered in the near future as extension to this initial scenario. 


Further resources and additional reading about the ADF SAP Table Connector: 
* [Copy data from an SAP table by using Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/connector-sap-table)

* [Whitepaper SAP Data Integration using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/blob/main/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf)

* [Analytics and Integration for SAP Global Instance running on-premises with ADF](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/analytics-and-integration-for-sap-global-instance-running-on/ba-p/1431602)

* [SAP Data Extraction using Azure Data Factory](https://github.com/bdelangh/ADF_SAPDataExtraction)

* [SAP on Azure: SAP Data Pool](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/verovisgmbh.adva_app_sapdwh)

![Copy data tool configuration](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/CopyDataTool2.png?raw=true)

4. __SAP ABAP tables stored as parquet files on Azure Data Lake Storage (ADLS) Gen 2__

After executing the Copy Data Pipeline, the SFLIGHT model related tables are stored in the compressed parquet file format in an Azure Data Lake Storage Gen2(ADLS Gen2) folder and are ready for consumption by different consumers. 

Typical consumers would be Azure Synapse Pipelines, for further transformations, or for direct SQL-queries via Azure Synapse Serverless Pools would be used. 
 An additional option is the consumption with Spark Dataframes as demonstrated in this scenario. Not (yet) covered in the current scenarios and documentation is the potential integration with the [Microsoft Common Data Model](https://github.com/Microsoft/CDM) (CDM). 
CDM would allow to maintain the business terms and schema information, for typical SAP model specific tables and columns like BSEG or VBAP, as metadata in addition to the raw data (table and column names) exported into the parquet file. In addition, it would be possible to maintain the relationships between extracted SAP tables. 
This will allow tools like [Azure Data Factory ](https://docs.microsoft.com/en-us/azure/data-factory/format-common-data-model) or Microsoft PowerBI to read the CDM manifest file and consume the maintained metadata information as described [here](https://powerbi.microsoft.com/en-us/blog/whats-new-in-dataflows-april-2021/).
Further resources and additional reading about the first ideas and concepts about integrating SAP semantics with the Microsoft Common Data Model: 

* [Microsoft acquires ADRM Software, leader in large-scale, industry-specific data models](https://blogs.microsoft.com/blog/2020/06/18/microsoft-acquires-adrm-software-leader-in-large-scale-industry-specific-data-model)

* [Integration of SAP ERP Data into a Common Data Model](https://blogs.sap.com/2020/12/03/integration-of-sap-erp-data-into-a-common-data-model/)

* [From SAP S/4HANA Core Data Services (CDS) to the Common Data Model (CDM) on Azure Data Lake Gen2 (ADLS Gen2)](https://github.com/ROBROICH/SAP_AND_COMMON_DATA_MODEL_DEMO)

![Microsoft Common Data Model](https://powerbiblogscdn.azureedge.net/wp-content/uploads/2021/04/CDM-universe-graphic.png)

5. __Azure Synapse serverless SQL pool to create SQL views on parquet files__

For enabling the data exploration and discovery use cases on the SAP data stored in the data lake, the parquet files were queried using Synapse serverless SQL pools and T-SQL. Key aspect for efficient and fast data exploration is the option to create SQL views with auto detection (infer) of the schema from the underlying data lake files.

```sql 
-- Create SQL view from parquet file with schema auto detection. 
CREATE VIEW FS1_SFLIGHT_MODEL.SBOOK AS
SELECT * FROM
    OPENROWSET(
        BULK 'https://adlsv2.dfs.core.windows.net/sap/FS1-TABLE-SFLIGHT/SBOOK',
        FORMAT='PARQUET'
    ) AS [SBOOK];

```

6. __PowerBI workspace with Direct Query connection to SQL view__

For creating the first report pilots and prototypes based on the SAP ERP data, a semantical data model was created with Microsoft Power BI(PBI). This data model defines the relationship between the SFLIGHT tables and adds additional semantical information like DAX(Calculated) measures, calculated fields, aggregations and hierarchies.
In this scenario the Direct Query interface was used to create the connection between the PBI data model, Azure Synapse SQL views and the underlying parquet files stored on ADLS. 
As well important to mention for is the seamless integration of the PBI workspace into the Synapse Studio development environment. 

![PowerBI](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/PB1.png?raw=true)

6. __Create advanced visualizations leveraging Spark Dataframes and Apache Spark Notebooks__

In this example a Data Engineer or Data-Scientist uses Apache Spark Notebooks to create advanced visualizations using Spark Notebooks and Python Libraries like the Advanced Data Analysis Library (Pandas).  
Additional demonstrated features are queries on the parquet files using SparkSQL. 
For using these services an Apache Spark pool was deployed and made available within the Synapse Studio. 

![Spark Notebook](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/Spark_Notebook.png?raw=true)

## Summary 
Summarized this document provided a comprehensive overview about analytical integration scenarios and use-cases between Azure Synapse and data from an SAP ERP. 
The description covered the following implementation steps:

![Implementation steps](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/ImplementationSteps.png?raw=true)

Ideally the simplified SFLight model is starting point for Microsoft partners to create their own Azure Synapse & SAP demo-systems and scenarios. 
Microsoft partners are as well always invited to collaborate on this document with their own solutions or extensions. 
In case of comments, conceptual bugs or additional requirements please feel free to create issues on GitHub or reach out to the author via LinkedIn. 

🙏 Many thanks for your time and reading the document until here! 🙏












