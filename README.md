# Azure Synapse and SAP SFlight integration scenario 
 This article will describe an End to End(E2E) scenario to demonstrate the integration between Azure Synapse(SQL Data-Warehouse) and a SAP S/4HANA(ERP) on Azure system. For simplification and to make the scenario repeatable, the SAP SFlight sample application was selected. 

 The motivation for creating this E2E is to demonstrate how SAP data is potentially handled within the different Data-Warehouse(DWH) layers and services of Azure Synapse. 

 The implementation was done using Azure Synapse and it's dedicated DWH-tools and -services. 


## Scenario use-case and persona description 
For the creation and implementation of the scenario three types of personas were assumed.

### Bianca Basis 
* SAP Basis administrator
* Maintains RFC and data authorizations for the Azure Data Factory(ADF) SAP Table Connector 
* Monitors the RFC memory consumption and general performance 
* Contact for SAP ERP related troubleshooting  


### Sabrina Sap  
* DWH and BI developer 
* SAP domain and data model expert 
* SQL and data engineering knowledge 
* PowerBI expert 
* Contact person for LoB user 


### Daniel Data-Science
* Data-Scientist 
* Fluent in Scala and Python 
* Builds M/L models on SAP data 
* Builds on Sabrina's models 

![ Scenario persona description](https://github.com/ROBROICH/AZURE_SYNAPSE_AND_SAP_SFLIGHT_DEMO/blob/main/img/personas.png?raw=true)
