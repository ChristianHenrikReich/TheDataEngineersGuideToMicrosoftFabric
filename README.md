# The Data Engineers Guide To Microsoft Fabric
This is the companion repository for the book *The Data Engineer's Guide to Microsoft Fabric*.

The purpose of this repository is to provide readers with access to code examples and supporting resources that are shown or referenced in the book, making it easier to explore the implementations.

The repository focuses on non-trivial examples rather than simple code snippets intended for illustrative purposes. However, I might be able to add more on requests.

The book can be acquired here:
- https://learning.oreilly.com/library/view/the-data-engineers/0642572249151
- https://www.amazon.com/Data-Engineers-Guide-Microsoft-Fabric/dp/B0HGCS816C

Else I do blog about related topics, however these are not found in the book:
- https://christianhenrikreich.medium.com

## Chapter 1

Examples:
- [Data builder](chapter_1/DataBuilder.ipynb)
  - Generate sample customers and orders tables with PySpark

Tools mentioned:
- **Visual Studio Code**: https://code.visualstudio.com/
  - Fabric Data Engineering VS Code extension
  - Microsoft Fabric
  - Microsoft Fabric MCP Server
  - SQL Server (mssql) extension
  - SQL SQL Database Projects extension
- **Azure Command-Line Interface (AZ CLI)**: https://learn.microsoft.com/en-us/cli/azure/
- **Fabric-cli**: https://microsoft.github.io/fabric-cli/
- **Fabric-cicd**: https://microsoft.github.io/fabric-cicd/latest/
- **Azure Storage Explorer** https://azure.microsoft.com/en-us/products/storage/storage-explorer
- **Git*** https://git-scm.com/ 

*Git can be installed with all kinds of editors and tools, so you might already have it install. Git-scm is Git's homepage, and it is the one I uses to do a clean git installation. 

## Chapter 4

Books mentioned:
- **Analytics Engineering with Microsoft Fabric and Power BI** by Shabnam Watson and Nikola Ilic: https://learning.oreilly.com/library/view/analytics-engineering-with/9798341645868/

## Chapter 5 

Examples:
- [Chapter 5 examples](chapter_5/examples_chapter_5.ipynb)
  - 5.12: Mounting lakehouses with Python
  - 5.15: Connect to a Delta Lake on OneLake
  - 5.16: Connect to SQL Endpoint with Token Credentials

## Chapter 6

Examples:
- [Chapter 6 examples](chapter_6/examples_chapter_6.ipynb)
  - 6.3: Delta Lake protocol versions check
  - 6.9: Delta Lake auto compaction Spark session level
  - 6.10: Delta Lake auto compaction table level

## Chapter 8

Books mentioned:

- **In-Memory Analytics with Apache Arrow** by Matt Topol: https://learning.oreilly.com/library/view/in-memory-analytics-with/9781835461228/

Examples:
- [Chapter 8 examples](chapter_8/examples_chapter_8.ipynb)
  - 8.4: Table maintinance
  - 8.14: Salting in Spark
  - 8.27: Read unstructured data (images) into dataframe
  - 8.36: Ingest from API
  - 8.38: Connect to SQL Endpoint with PyODBC using tokens
  - 8-39: Connect to SQL Endpoint with Spark


## Chapter 10

Books mentioned:

- **Real-Time Intelligence with Microsoft Fabric** by Johan Ludvig Brattås and Frank Geisler: https://learning.oreilly.com/library/view/real-time-intelligence-with/9781098178574/

Examples:
- [Chapter 10 examples](chapter_10/examples_chapter_10.ipynb)
  - 10.7: Creating a restore point before loading a table
  - 10.10: Creating an external source

## Chapter 12

Examples:
- [Chapter 12 examples](chapter_12/examples_chapter_12.ipynb)
  - 12.7: Incremental bronze-to-silver processing with streaming stopping after batch

## Chapter 13

Examples:
- [octo_merge_github.yaml](chapter_13/octo_merge/octo_merge_github.yaml)
- [octo_merge_azure_devops.yaml](chapter_13/octo_merge/octo_merge_azure_devops.yaml)

## Chapter 14

Examples:
- [Chapter 14 examples](chapter_14/examples_chapter_14.ipynb)
  - 14.2: Setting up a logger in Python
  - 14.3: How to read CSV log files with Python and PySpark
  - 14.6: Measuring the quality of a table

## Chapter 15

Examples:
- [Chapter 15 examples](chapter_15/examples_chapter_15.ipynb)
  - 15.1: Basic example of metadata in JSON
  - 15.2: The reader function
  - 15.3: The transform function for processing
  - 15.4: The main loop, which connects all parts of the engine
  - 15.5: The top notebook with a defined DAG calling runMultiple

## Chapter 16
