# Azure Synapse and SAP SFlight integration scenario 
 This article will describe an End to End(E2E) scenario to demonstrate the integration between Azure Synapse(SQL Data-Warehouse) and a SAP S/4HANA(ERP) on Azure system. For simplification and to make the scenario repeatable, the SAP SFlight sample application was selected. 

 The motivation for creating this E2E is to demonstrate how SAP data is potentially handled within the different Data-Warehouse(DWH) layers and services of Azure Synapse. 

 The implementation was done using Azure Synapse and it's dedicated DWH-tools and -services. 


## Scenario use-case and persona description 
For the creation and implementation of the scenario three types of personas were assumed.

Bianca Basis 
* SAP Basis administrator
* Maintains RFC and data authorizations for the Azure Data Factory(ADF) SAP Table Connector 
* Monitors the RFC memory consumption and general performance 
* Contact for SAP ERP related troubleshooting  


Sabrina Sap  
* DWH and BI developer 
* SAP domain and data model expert 
* SQL and data engineering knowledge 
* PowerBI expert 
* Contact person for LoB user 


 Daniel Data-Science
* Data-Scientist 
* Fluent in Scala and Python 
* Builds M/L models on SAP data 
* Builds on Sabrina's models 



Based on the personas and expertise described above, two scenario or use-cases were defined and implemented. 

### SAP data model discovery and exploration 
This use-case is conceptually designed for rapid prototyping and piloting of SAP ERP data using Azure Synapse and Microsoft PowerBI(PBI). These prototypes and pilots would be the preparation for productive DWH implementations. 

Before creating the final DWH data models and data ingestion pipelines, certain SAP data model and customer specific data discovery and exploration tasks have typically to be executed: 
* Identify custom columns (Z-fields)
* Filled and empty columns in SAP tables
* Identify timestamp columns for CDC 
* Identify partition criteria 

This scenario is fully integrated into Azure Synapse Workspaces using the following Azure data platform and Azure Synapse services and tools: 

* Azure Data Lake Gen2 
* Azure Synapse serverless SQL pools 
* Azure Synapse Pipelines
* Microsoft PowerBI (Integrated into Azure Synapse Workspace)

![ scenario_exploration_and_discovery](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/scenario_exploration_and_discovery.png?raw=true)



### SAP data visualization and M/L model creation 
In this scenario a Data Scientist utilizes Apache Spark and Spark Notebook to implement machine learning models on SAP ERP data. Additional requirements are the option to use Python libraries for visualizations within the Apache Spark notebooks or to process large data volumes with Apache Spark. 

The scenario was designed based on the following tools and services 

* Apache Spark Pools 
* Apache Spark Notebooks 
* Python libraries like Pandas, Matplotlib or Seaborn 


