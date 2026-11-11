# The Data Engineer's Guide to Microsoft Fabric
This is the companion repository for the book *The Data Engineer's Guide to Microsoft Fabric*.

It contains code examples and supporting resources shown or referenced in the book, making it easier to explore the implementations.

The repository focuses on non-trivial examples rather than short, illustrative snippets. Additional examples may be added on request.

Find the book here:
- https://learning.oreilly.com/library/view/the-data-engineers/0642572249151
- https://www.amazon.com/Data-Engineers-Guide-Microsoft-Fabric/dp/B0HGCS816C

For additional articles on related topics beyond the book:
- https://christianhenrikreich.medium.com

## Getting started

1. Download or clone this repository and choose an example from the chapter index below.
2. For Fabric notebook examples, import the `.ipynb` file into a workspace on an active Fabric capacity. Attach your own default lakehouse where required; replace any saved lakehouse binding with your own.
3. Use a Fabric Spark runtime compatible with the example. Warehouse writes require a runtime that supports the [Fabric Warehouse Spark connector's write API](https://learn.microsoft.com/en-us/fabric/data-engineering/spark-data-warehouse-connector). Review imports and install any additional libraries in the notebook's Fabric Environment or your local Python environment, as appropriate.
4. Review the book and notebook setup notes before running cells. Update workspace IDs, paths, endpoints, and other example configuration, and ensure you have permission to access the required items. Some cells illustrate local Python or SQL workflows rather than code to run unchanged in a Fabric Spark session.
5. To generate sample data, run the [Chapter 1 data builder](chapter_1/DataBuilder.ipynb) in cell order. It creates 1 million customers and 5 million orders in the default lakehouse. The final cell also loads an existing Warehouse named `gold` in the same workspace as the notebook and its default lakehouse.

**Warning:** Some examples overwrite tables or change existing resources. The data builder overwrites `customers` and `orders` in the default lakehouse and `gold.dbo.customers` and `gold.dbo.orders` in the Warehouse. Use a development workspace and review write operations before running them.

## Chapter 1: Data Engineering in Microsoft Fabric

Examples:
- [Data builder](chapter_1/DataBuilder.ipynb)
  - Generate sample customers and orders tables with PySpark
  - Load the saved tables into an existing gold Warehouse in the same workspace, overwriting dbo.customers and dbo.orders

Tools mentioned:
- **Visual Studio Code**: https://code.visualstudio.com/
  - Fabric Data Engineering VS Code extension
  - Microsoft Fabric
  - Microsoft Fabric MCP Server
  - SQL Server (mssql) extension
  - SQL Database Projects extension
- **Azure Command-Line Interface (AZ CLI)**: https://learn.microsoft.com/en-us/cli/azure/
- **Fabric-cli**: https://microsoft.github.io/fabric-cli/
- **Fabric-cicd**: https://microsoft.github.io/fabric-cicd/latest/
- **Azure Storage Explorer**: https://azure.microsoft.com/en-us/products/storage/storage-explorer
- **Git**: https://git-scm.com/

Git may already be installed with your editor or other development tools. For a standalone installation, use the installer from Git's official website above.

## Chapter 4: Workspaces

Books mentioned:
- **Analytics Engineering with Microsoft Fabric and Power BI** by Shabnam Watson and Nikola Ilic: https://learning.oreilly.com/library/view/analytics-engineering-with/9798341645868/

## Chapter 5: Fabric Lakehouses

Examples:
- [Chapter 5 examples](chapter_5/examples_chapter_5.ipynb)
  - 5.12: Mounting lakehouses with Python
  - 5.15: Connect to a Delta Lake on OneLake
  - 5.16: Connect to SQL Endpoint with Token Credentials

## Chapter 6: Delta Lake and Tables

Examples:
- [Chapter 6 examples](chapter_6/examples_chapter_6.ipynb)
  - 6.3: Delta Lake protocol versions check
  - 6.9: Delta Lake auto compaction Spark session level
  - 6.10: Delta Lake auto compaction table level

## Chapter 8: Data Engineering with Lakehouses

Books mentioned:

- **In-Memory Analytics with Apache Arrow** by Matt Topol: https://learning.oreilly.com/library/view/in-memory-analytics-with/9781835461228/

Examples:
- [Chapter 8 examples](chapter_8/examples_chapter_8.ipynb)
  - 8.4: Table maintenance
  - 8.14: Salting in Spark
  - 8.27: Read unstructured data (images) into dataframe
  - 8.36: Ingest from API
  - 8.38: Connect to SQL Endpoint with PyODBC using tokens
  - 8.39: Connect to SQL Endpoint with Spark

## Chapter 10: Data Engineering with Warehouses

Books mentioned:

- **Real-Time Intelligence with Microsoft Fabric** by Johan Ludvig Brattås and Frank Geisler: https://learning.oreilly.com/library/view/real-time-intelligence-with/9781098178574/

Examples:
- [Chapter 10 examples](chapter_10/examples_chapter_10.ipynb)
  - 10.7: Creating a restore point before loading a table
  - 10.10: Creating an external source

## Chapter 12: Real-Time Data Engineering

Examples:
- [Chapter 12 examples](chapter_12/examples_chapter_12.ipynb)
  - 12.7: Incremental bronze-to-silver processing with streaming stopping after batch

## Chapter 13: Lifecycle Management

Examples:
- [Chapter 13 examples](chapter_13/examples_chapter_13.ipynb)
  - 13.11: A function to read functions and classes from a notebook stored as a .py file in a Git repository
  - 13.12: A Python test script unit testing a function from a notebook item

Pipeline templates:
- [GitHub Actions octopus-merge template](chapter_13/octo_merge/octo_merge_github.yaml)
- [Azure DevOps octopus-merge template](chapter_13/octo_merge/octo_merge_azure_devops.yaml)

These are illustrative templates, not standalone deployment pipelines. They reference supporting deployment scripts and configuration that are not included in this repository; the Azure DevOps template also requires a Python preparation template. Supply these dependencies and configure authentication, target workspaces, environments, and PR triggers before use.

## Chapter 14: Platform Observability

Examples:
- [Chapter 14 examples](chapter_14/examples_chapter_14.ipynb)
  - 14.2: Setting up a logger in Python
  - 14.3: How to read CSV log files with Python and PySpark
  - 14.6: Measuring the quality of a table

## Chapter 15: Metadata-Driven Lakehouse Solutions

Examples:
- [Chapter 15 examples](chapter_15/examples_chapter_15.ipynb)
  - 15.1: Basic example of metadata in JSON
  - 15.2: The reader function
  - 15.3: The transform function for processing
  - 15.4: The main loop, which connects all parts of the engine
  - 15.5: The top notebook with a defined DAG calling runMultiple
