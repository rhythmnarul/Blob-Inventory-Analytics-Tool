## Blob-Inventory-Analytics-Tool

## 1. Creating Synapse Workspace 

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
![Review+Create](https://user-images.githubusercontent.com/106289147/177055875-e07052db-360f-45f2-ac6b-d8f5d2db10a6.png)  ![Create](https://user-images.githubusercontent.com/106289147/177055889-abac8fed-8a02-4b20-8b03-54ef628faae9.png)
Note: Wait for a few minutes until the deployment is complete. 

**Step 5:** On the overview, click on ‘Deployment details’ to find your created synapse workspace URL.
![CLick Here](https://user-images.githubusercontent.com/106289147/177056027-eeabfed2-b487-40be-8aa0-1fdb69775108.jpg)
**Step 6:** Click on the Synapse Workspace URL to be directed to ‘Synapse Dashboard’ and Open ‘Synapse Studio’. 
![Click Here](https://user-images.githubusercontent.com/106289147/177056047-eee11f16-8e60-4e21-9c93-d1fc20208963.png)

## 2. Procedures in Synapse Studio 
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
![Screenshot (5)](https://user-images.githubusercontent.com/106289147/177056871-875f1322-05e4-4ff5-ab42-7c4bf75bafa5.png)
![Screenshot (6)](https://user-images.githubusercontent.com/106289147/177056877-75a335f1-e71b-4b82-8cd6-d486a99a1458.png)

**2. Setting up Configuration File**

**Step 1:** Select Data from left panel, go to Linked and expand the storage account  
**Step 2:** Upload the configuration file to the reportdata container (or any other container you want to upload, make the corresponding changes in the first cell of script, steps are mentioned in the later section)  
![Upload Cocnfiguration json](https://user-images.githubusercontent.com/106289147/177056912-e7ec1e06-ffe0-4e6a-8ec4-c4145218ea09.png)

**3. Running the Script to Create Tables**

**Step 1:** Select Develop from left panel, click ‘+’ and import the ReportAnalysis.ipynb file  
**Step 2:** If you have created a new account and file name using the name mentioned above, just run the script by selecting run all else enter the corresponding details in the first cell of the script  
![Screenshot (3)](https://user-images.githubusercontent.com/106289147/177056945-6d5506f5-4d7e-497e-a99b-1c4da0d73053.png)

**3. Connecting to PowerBI**

**Step 1:** Open ReportAnalysis.pbit file in PowerBI 
**Step 2:** Connect to the created azure synapse workspace. 
**Visualization -  **
**Page 1: **
This visualization gives the overall analysis of all the inventory reports 
![Screenshot (1)](https://user-images.githubusercontent.com/106289147/177056985-a388874b-6e6c-4a76-a27b-179128d7c8a9.png)

**Page 2:** 

This visualization is partitioned into two parts.  

The left part represents the distribution of data present in hot tier and is accessed/not accessed in the last ‘x’ days where ‘x’ is determined by the slicer 

The right part represents the current analysis/distribution of data based on the last analyzed blob inventory report 
![Screenshot (9)](https://user-images.githubusercontent.com/106289147/177056998-fad0b37c-2ef5-4c6c-ab8c-30ac47460745.png)

**Page 3:** 

Slicer controls the number of reports to be analyzed from the selected date till the last analyzed report date 
![Screenshot (2)](https://user-images.githubusercontent.com/106289147/177057018-f6af5d59-4b59-4f4b-bff6-162bf93cccf0.png)


