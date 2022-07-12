## Blob-Inventory-Analytics-Tool

**Blob Inventory Analytics – User Guide** 

Overview

 - Requirements 
 - Step Guide 

   1. Creating Synapse Workspace 
   2. Procedures in Synapse Studio 
   3. Connecting to PowerBI 
 - Overview of Visualizations 
 - Exploring Custom Queries 
 - Creating Pipeline 
  
## 1. Requirements
1.  PowerBI Desktop Application (Power BI Desktop—Interactive Reports | Microsoft Power BI) 
2.  Clone the GitHub repository in your system (rhythmnarul/Blob-Inventory-Analytics-Tool (github.com) 
  
    Enter the details of the storage account you want to analyze in the  		BlobInventoryStorageAccountConfiguration.json file by providing 

    StorageAccountName 
    
    accessKey (present in ‘Security+networking’ section of storage account) 
    
    destinationContainer (container where blob inventory reports are stored) 	
    
    blobInventoryRuleName (inventory rule which you want to analyze) 

## 1. Step Guide 

### 1.1 Creating Synapse Workspace 
**Step 1:** Click to create [Synapse Workspace](https://ms.portal.azure.com/#create/Microsoft.Synapse)    

**Step 2:** Click on ‘Subscription’ to select your service. Next, click on ‘Resource Group’ to select an existing resource group or create a new one

**Step 3:** Enter your Workspace Details like Workspace Name, Account Name, File System Name and select the region (Example - ‘East US 2’)

Note: Do not enable ‘Use Spark on Cosmos’ while creating your account. 
Example of Workspace Name -: {your name}-analysis.  
There will be two options -:  
1. If you have Data Lake Storage Gen2 accounts with Hierarchical Name Space (HNS) enabled  
    - Select ‘Storage Account Name’ and corresponding ‘File Name’ 

2. In case the user does not have a ‘Data Lake Storage Gen 2’ account - 
 	 - Select ‘Create New’ for your Account Name and File Name. 
   	 Account Name -: reportanalysis  
     File Name -: reportdata  

**Step 4:** Click ‘Review + Create’ for validation and then Click ‘Create’ to begin the deployment
<p align="middle">
  <img src="https://user-images.githubusercontent.com/106289147/177055875-e07052db-360f-45f2-ac6b-d8f5d2db10a6.png" width="100" />
  <img src="https://user-images.githubusercontent.com/106289147/177132144-4bce42fb-2c4b-4382-a8df-1f7ea414eb20.png" width="100" /> 
</p>

[Grant permissions to managed identity in Synapse workspace - Azure Synapse Analytics | Microsoft 	Docs ](https://docs.microsoft.com/en-us/azure/synapse-analytics/security/how-to-grant-workspace-managed-identity-permissions)

Note: Wait for a few minutes until the deployment is complete. 

**Step 5:** On the overview, click on ‘Deployment details’ to find your created synapse workspace URL.

![CLick Here](https://user-images.githubusercontent.com/106289147/177056027-eeabfed2-b487-40be-8aa0-1fdb69775108.jpg)

**Step 6:** Click on the Synapse Workspace URL to be directed to ‘Synapse Dashboard’ and Open ‘Synapse Studio’. 

![Click Here](https://user-images.githubusercontent.com/106289147/177056047-eee11f16-8e60-4e21-9c93-d1fc20208963.png)

### 2.2 Procedures in Synapse Studio 
Expand the left panel for better navigation  
![Synapse Studio Homepage](https://user-images.githubusercontent.com/106289147/177056542-28756645-01f5-4ea8-9208-e442ffc64289.png)

  **1. Creating a new Apache Spark Pool**
**Step 1:** Select ‘Manage’ from the left panel, then click ‘Apache Spark Pools’ under Analytic Pools and click ‘New’.  
Manage > Apache Spark Pools > New 

![Click Here (1)](https://user-images.githubusercontent.com/106289147/177056648-01ec1ff7-c5bb-4bd0-94a5-f2a53d08efc5.png)

**Step 2:** Enter your Apache Spark pool name in the box. Example –: pool1 

**Step 3:** Select choices like Node Size Family, Node Size. 	 
Node Size Family – Preferably Hardware Accelerated (Memory Optimized also works)  
Node Size – Depends on the Inventory Report Size

Note: Continue the process with default settings for autoscale, number of nodes.  

**Step 4:** Select ‘Enabled’ for Dynamically allocate executors 

**Step 5:** Click ‘Review + Create’ for validation and then Click ‘Create’ to begin the deployment 

<p float="left">
  <img src="https://user-images.githubusercontent.com/106289147/177056871-875f1322-05e4-4ff5-ab42-7c4bf75bafa5.png" width="100" />
  <img src="https://user-images.githubusercontent.com/106289147/177056877-75a335f1-e71b-4b82-8cd6-d486a99a1458.png" width="100" /> 
</p>



**2. Setting up Configuration File**

**Step 1:** Select Data from left panel, go to Linked and expand the storage account  
**Step 2:** Upload the configuration file to the reportdata container (or any other container you want to upload, make the corresponding changes in the first cell of script, steps are mentioned in the later section)  
![Upload Cocnfiguration json](https://user-images.githubusercontent.com/106289147/177056912-e7ec1e06-ffe0-4e6a-8ec4-c4145218ea09.png)

**3. Running the Script to Create Tables**

**Step 1:** Select Develop from left panel, click ‘+’ and import the ReportAnalysis.ipynb file  
**Step 2:** If you have created a new account and file name using the name mentioned above, just run the script by selecting run all else enter the corresponding details in the first cell of the script  
![Screenshot (3)](https://user-images.githubusercontent.com/106289147/177134041-0a7f9d71-737b-4b7d-b736-7b160ac4af07.png)

### 3.3 Connecting to PowerBI

**Step 1:** Right Click on ReportAnalysis.pbit file (available in GitHub repository), go to ‘Open with’ and select ‘Microsoft Power BI Desktop’ and click on ‘Ok’ 

Note: Make sure that you have Microsoft Power BI Desktop Application installed as mentioned in the Requirements section  

**Step 2:** Prompt will open in Power BI requesting Synapse Workspace Name and Database Name. If you are following the naming convention mentioned above, enter 

synapse_workspace_name = {your name}-analysis 

database_name = reportdata 

Else, enter the respective details 

(Synapse workspace name is mentioned at the top bar of the synapse studio, next to Synapse Analytics)  

**Step 3:** Click on Load, it will start loading the data from the database created in the previous step 

Note: It might ask you to Sign into your azure synapse workspace, please click ‘Sign in’ and enter your synapse credentials 

This will ensure that your PowerBI is connected to your Synapse Workspace 

Note: If an error occurs while loading the data 

**Step 1:** Click on Close 

If a panel like this appears, click on ‘Apply changes’ and then ‘Refresh now’ 

If you still face issues, continue with the steps below 

**Step 2:** Click ‘Transform data’ then again Select ‘Transform Data’ from dropdown

This will open a separate tab named ‘Power Query Editor’ 

Inside Power Query Editor 

**Step 3:** Click ‘Refresh Preview’ button and then Click ‘Refresh all’ from the dropdown 

This will refresh the data for all the tables 

**Step 4:** Click ‘Close and Apply’ button on the top-left, this will make the corresponding 	changes and will close the Power Query Editor 

Note: If you face any issues while visualizing data on ‘Detailed Analysis’ Page (second page), expand the ‘Filters’ section, then expand ‘ReportGenerationDate’ filter and scroll down to select the last date (or any specific date for which you want to visualize) 

**Step 4:** Go to ‘File’ and then ‘Save’ to save the report 

**Step 5:** Click ‘Publish’ to publish your report 

**Step 6:** Select the destination where you want to ‘Publish’ your report, for this step you can also continue with ‘My Workspace’ then click on ‘Select’ 

Note: If the prompt shows an option to replace the current report, click on ‘Replace’ 

**Step 7:** Click on the link, it will open the report in the browser 

## Overview of Visualizations  

Visualizations are spread across three pages in PowerBI report.  

**Visualization -**

**Overview Page**

This page gives the overall analysis of all the processed blob inventory reports.  

Top-left represents the growth of data in the storage account/ amount of data present in the storage account with date 

Top-right represents the number of files that are modified corresponding to the respective date of modification 

Bottom-left represents the amount of data created corresponding to the respective date of creation 

Bottom-right represents the amount of space occupied by snapshots corresponding to the respective date on which blob inventory report is generated 
![Screenshot (1)](https://user-images.githubusercontent.com/106289147/177056985-a388874b-6e6c-4a76-a27b-179128d7c8a9.png)

**Detailed Analysis Page** 

This page is partitioned into two parts.  

The upper section of the left part represents the distribution of data present in hot tier and is accessed/not accessed in the last ‘x’ days where ‘x’ is determined by the slicer, correspondingly the lower section represents the data present in cool tier and is accessed/not accessed in the last ‘x’ days where ‘x’ is determined by the slicer 

Note: Only ‘Block Blobs’ are considered while calculating data size in hot and cool tier as only ‘Block Blob’ have the feature to be shifted to different access tier 

The right part represents the current analysis/distribution of data based on the last analyzed blob inventory report  

![Screenshot (9)](https://user-images.githubusercontent.com/106289147/177134586-09388ae3-436b-44df-beee-63bcbceda347.png)

**Breakdown of Reports page** 

This page represents the overall comparison of all the processed blob inventory reports. Slicer on the top left side controls the range of dates from which you want to start the comparison till the last processed blob inventory report. Correspondingly all the visualizations will change based on value selected using the slicer.  

Top middle: represents the distribution of data in different types of blobs I.e., Space occupied by Block Blob, Page Blob and Append Blob 

Top right: represents the distribution of data in different access tiers I.e., Space occupied by Data present in Hot, Cool and Archive Tier 

Bottom Left: represents the Data Size of Containers 

Bottom Right: represents the Data occupied by respective File Type (example – space occupied by pdf files or Json files) 

All these visualizations are with respect to the date of the blob inventory report being analyzed  

![Screenshot (2)](https://user-images.githubusercontent.com/106289147/177057018-f6af5d59-4b59-4f4b-bff6-162bf93cccf0.png)



## Exploring Custom Queries 

**Step 1:** Open Synapse Studio (mentioned in ‘Creating Synapse Workspace’ section) 

**Step 2:** Click ‘Data’ from left panel and select ‘Workspace’ 

**Step 3:** Right click over the created database (example – reportdata) and select ‘New SQL Script’ and click on ‘New Script’ 

reportdata>New SQL Script>New Script 

**Step 4:** Collapse the properties section, publish the report (again click on ‘publish’), enter the SQL query and click ‘Run’ to execute the query 

Sample SQL Query - SELECT * FROM [reportdata].[dbo].[accesstierinfo] 

This will return all the data stored in the accesstierinfo table 

## Creating Pipeline 

**Step 1:** Open ReportAnalysis.ipynb (provided in GitHub repo) in the Synapse Studio and Click on the ‘Pipeline icon’ mentioned in the image below 

**Step 2:** Click on the ‘Existing Pipeline’ if you have already created one else continue with ‘New Pipeline'. This will open a new Tab 

**Step 3:** Collapse the side bar by clicking on ‘Properties’ Section. Add a trigger to the pipeline by clicking on ‘Add Trigger’ and then ‘New/Edit’ 

**Step 4:** Click on ‘Choose trigger’ and then select ‘New’ 

**Step 5:** Enter the details for the trigger and click on ‘Ok’ and again select ‘Ok’ 

Note: Type will be ‘Schedule’, Select Recurrence to be 24 hours if blob inventory reports are generated on daily basis 

It's completely fine for Trigger to have no parameters 

Example of Trigger 

**Step 6 (Optional Step):** Click on ‘Trigger’ and select ‘Trigger now’ to trigger the pipeline at that instant 

**Step 7:** Go to ‘Monitor’ from left panel, then Select ‘Pipeline runs’ to monitor the created pipeline 

