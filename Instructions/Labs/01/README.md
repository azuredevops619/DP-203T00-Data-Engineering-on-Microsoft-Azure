# Module 1 - Explore compute and storage options for data engineering workloads

This module teaches ways to structure the data lake, and to optimize the files for exploration, streaming, and batch workloads. The student will learn how to organize the data lake into levels of data refinement as they transform files through batch and stream processing. Then they will learn how to create indexes on their datasets, such as CSV, JSON, and Parquet files, and use them for potential query and workload acceleration.

In this module, the student will be able to:

- Combine streaming and batch processing with a single pipeline
- Organize the data lake into levels of file transformation
- Index data lake storage for query and workload acceleration

## Lab details

- [Module 1 - Explore compute and storage options for data engineering workloads](#module-1---explore-compute-and-storage-options-for-data-engineering-workloads)
  - [Lab details](#lab-details)
  - [Lab 1 - Delta Lake architecture](#lab-1---delta-lake-architecture)
    - [Before the hands-on lab](#before-the-hands-on-lab)
      - [Task 1: Create and configure the Azure Databricks workspace](#task-1-create-and-configure-the-azure-databricks-workspace)
    - [Exercise 1: Complete the lab notebook](#exercise-1-complete-the-lab-notebook)
      - [Task 1: Clone the Databricks archive](#task-1-clone-the-databricks-archive)
      - [Task 2: Complete the following notebook](#task-2-complete-the-following-notebook)
  - [Lab 2 - Working with Apache Spark in Synapse Analytics](#lab-2---working-with-apache-spark-in-synapse-analytics)
    - [Before the hands-on lab](#before-the-hands-on-lab-1)
      - [Task 1: Create and configure the Azure Synapse Analytics workspace](#task-1-create-and-configure-the-azure-synapse-analytics-workspace)
      - [Task 2: Create and configure additional resources for this lab](#task-2-create-and-configure-additional-resources-for-this-lab)
    - [Exercise 1: Load and data with Spark](#exercise-1-load-and-data-with-spark)
      - [Task 1: Index the Data Lake storage with Hyperspace](#task-1-index-the-data-lake-storage-with-hyperspace)
      - [Task 2: Explore the Data Lake storage with the MSSparkUtil library](#task-2-explore-the-data-lake-storage-with-the-mssparkutil-library)
    - [Resources](#resources)

## Lab 1 - Delta Lake architecture

In this lab, you will use an Azure Databricks workspace and perform Structured Streaming with batch jobs by using Delta Lake. You need to complete the exercises within a Databricks Notebook. To begin, you need to have access to an Azure Databricks workspace. If you do not have a workspace available, follow the instructions below. Otherwise, you can skip to the bottom of the page to [Clone the Databricks archive](#clone-the-databricks-archive).

### Before the hands-on lab

> **Note:** Only complete the `Before the hands-on lab` steps if you are **not** using a hosted lab environment, and are instead using your own Azure subscription. Otherwise, skip ahead to Exercise 1.

Before stepping through the exercises in this lab, make sure you have access to an Azure Databricks workspace with an available cluster. Perform the tasks below to configure the workspace.

#### Task 1: Create and configure the Azure Databricks workspace

**If you are not using a hosted lab environment**, follow the [lab 01 setup instructions](https://github.com/azuredevops619/microsoft-data-engineering-ilt-deploy/blob/main/setup/01/lab-01-setup.md) to manually create and configure the workspace.

### Exercise 1: Complete the lab notebook

#### Task 1: Clone the Databricks archive

1. If you do not currently have your Azure Databricks workspace open: in the Azure portal, navigate to your deployed Azure Databricks workspace and select **Launch Workspace**.
1. In the left pane, select **Workspace** > **Users**, and select your username (the entry with the house icon).
1. In the pane that appears, select the arrow next to your name, and select **Import**.

  ![The menu option to import the archive](media/import-archive.png)

1. In the **Import Notebooks** dialog box, select the URL and paste in the following URL:

 ```
  https://github.com/solliancenet/microsoft-learning-paths-databricks-notebooks/blob/master/data-engineering/DBC/11-Delta-Lake-Architecture.dbc?raw=true
 ```

1. Select **Import**.
1. Select the **11-Delta-Lake-Architecture** folder that appears.

#### Task 2: Complete the following notebook

Open the **1-Delta-Architecture** notebook. Make sure you attach your cluster to the notebook before following the instructions and running the cells within.

Within the notebook, you will explore combining streaming and batch processing with a single pipeline.

