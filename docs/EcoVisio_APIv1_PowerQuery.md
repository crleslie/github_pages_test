---
layout: default
title: PowerQuery Integration
parent: Guides
nav_order: 1
---


# Power BI - EcoCounter API Template

## Overview

This document is a companion to the `EcoCounter_API_Template.pbix`, a Power BI report and data model designed for ingesting count data using the Eco-Visio API. It serves as a starting point for agencies developing a visualization platform for Eco-Visio data.

> **Prerequisites:** This guide assumes a basic understanding of Power BI (specifically the Power Query data model) and how web APIs work.

-----

## 1\. Power Query Fundamentals

The foundation of Power BI is the **Power Query** data model, which allows developers to ingest data from various sources (CSV, SQL, Web, etc.) and combine them into a single model.

  * **Processing Steps:** When building a data model, you are creating a series of processing steps using the **Power Query M Formula Language**. The output of one step becomes the input for the next.
  * **Sources:** Sources can include documents, other queries, or a JSON dataset retrieved from the Eco-Visio server.
  * **Data Cleaning:** After ingestion, typical steps include defining data types, filtering, removing columns, or expanding nested data (arrays).

### Handling JSON Arrays

In Power Query, nested JSON arrays appear as a column with hyperlinked values (e.g., `Record`, `List`). These indicate complex structures that must be expanded into primitive values.

-----

## 2\. The Eco-Visio API

The Eco-Visio API allows you to send a request to an endpoint and receive a JSON object.

  * **Authentication:** Requires a token (API Key) passed in the header to authenticate the domain.
  * **Parameters:** Specific data requests (date ranges, site IDs, time steps) are defined in the Request URL.
  * **Documentation:** [Eco-Visio API Documentation](http://eco-test2.com/apidoc/wso2/apidoc.html#authentication-prog).

### Structure of Data

Data often returns in multiple levels. For example, a "Site" record contains a "Channel" array, which contains individual data channels (unique combinations of user type and direction).

### Example Request (cURL)

To make a request, we use a `GET` command with specific headers. Below is an example using the test API key.

```bash
curl -X GET "https://apieco.eco-counter-tools.com/api/1.0/counter" \
-H "Accept: application/json" \
-H "Authorization: Bearer cbd0f13a845b2364839b59919e02b6c"
```

**Breakdown of the Command:**

1.  **The URL:** `https://apieco.eco-counter-tools.com/api/1.0` (Base URL) + `/counter` (Attribute to list counters).
2.  **Accept Header:** `-H "Accept: application/json"` tells the server to return JSON.
3.  **Authorization Header:** `-H "Authorization: Bearer [KEY]"` passes the API key for access.

-----

## 3\. Step-by-Step: Defining the Source (Basic Method)

This section covers connecting via the graphical interface.

1.  In Power Query Editor, select **New Source** → **Web**.
2.  Switch to the **Advanced** view.
3.  **URL Parts:** Split the URL into the base and the specific attribute.
      * *Part 1:* `https://apieco.eco-counter-tools.com/api/1.0`
      * *Part 2:* `/counter`
4.  **HTTP Request Header Parameters:**
      * Select **Accept** and enter `application/json`.
      * Type **Authorization** and enter `Bearer [Your_API_Key]`.

> *[Insert Screenshot: Web Configuration Dialog]* 

5.  Click **OK**. If prompted for authentication, select **Anonymous** and ensure the level is set to the base URL.

### Expanding the Data

Once connected, Power BI creates a query named `counter` containing a list of Records.

1.  **Convert to Table:**
      * Delete the "Navigation" step if clicking a record generated one.
      * Click the **To Table** button in the ribbon.
      * Keep default settings (Delimiter: None, Extra columns: Error) and click OK.
2.  **Expand Columns:**
      * Click the **Expand icon** (double arrows) in the column header.
      * Select the attributes you wish to keep (e.g., `serial`, `gsm`, `articleCode`).
      * Uncheck "Use original column name as prefix" for cleaner headers.

> *[Insert Screenshot: Expanded Table View]* 

-----

## 4\. Customizing with Advanced Editor (Parameters)

Setting up connections manually is tedious and error-prone. Using **Parameters** and **Power Query M** allows for programmatic control.

To view the underlying code, right-click the `counter` query and select **Advanced Editor**.

### The M Code Logic

The connection relies on two main functions:

1.  `Web.Contents()`: Downloads data from the URL.
2.  `Json.Document()`: Parses the binary data into JSON.

**Example Code:**

```powerquery
let
    Source = Json.Document(Web.Contents("https://apieco.eco-counter-tools.com/api/1.0" & "/counter", 
    [Headers=[Accept="application/json", Authorization="Bearer cbd0f13a845b2364839b59919e02b6c"]])),
    
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", 
        {"serial", "gsm", "iccid", "articleCode", "softVersion", "hardVersion", "expiditionDate"}, 
        {"serial", "gsm", "iccid", "articleCode", "softVersion", "hardVersion", "expiditionDate"})
in
    #"Expanded Column1"
```

### Key Concepts for Customization

  * **Concatenation:** You can join strings (like Base URL + Endpoint) using the `&` operator.
  * **Web.Contents Options:** Because this is an API request, we use optional parameters within `Web.Contents`:
      * `Headers`: Specified as a record to supply HTTP headers.
      * `RelativePath`: Appends text to the base URL (useful for clean parameter handling).

-----

## 5\. Security and Privacy Levels

*(Note: Ensure privacy levels are configured correctly to prevent data leakage between sources)*.

> *[Insert Screenshot: Privacy Levels Dialog]*