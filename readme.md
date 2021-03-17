# Create an "auto-loader" to ingest from on-prem to Azure Storage

When starting any data solution in the cloud, one of the first places to start is loading the data into Azure Storage. A common task is to create a process to allow users to upload data into Azure for further downstream analysis automatically. This could be anything – Databricks, SQL, even just Power BI directly. Imagine a scenario where I have a Power BI report that refreshes from on-prem, and I want to speed up the daily refresh by simply moving the files to Azure. Today we’re going to create a generic file loader into Azure that takes whatever is copied into a file share on-prem and load it to an Azure Storage blob. The process flow looks like the below:

[](./images/ArchitecturalDiagram.jpg?raw=true)

Pre-requisites:
•	A Windows server (either on-prem or in Azure) that has a file share created with a list of files that you wish to upload and a self-hosted Integration Runtime installed
•	An Azure Data Factory instance created

The pipeline consists of only a few activities: A “get Metadata” activity and a for each loop with a “Copy Data” activity.

[](./images/Pipeline.jpg?raw=true)

# The Get Metadata activity
The get Metadata activity monitors the on-prem folder and retrieves a list of the child items inside the folder, as illustrated in the below screenshot:
[](https://github.com/christophermschmidt/adfbinaryloader/images/getMetadataActivity.jpg)

# The For Each Loop

## Copy Data Activity