# 📊 Oracle Retail Analytics (O.R.A.) – Data Pipeline Project

## 🚀 Project Overview

This project demonstrates the design and implementation of a **data pipeline using Informatica Intelligent Cloud Services (IICS)**.

The pipeline extracts retail data from flat files, applies transformations, and loads it into an Oracle Data Warehouse using a **Full Refresh (Truncate & Load)** strategy.

---

## 📂 Data Description

### 1. Store Master (`src_stores.csv`)

Contains store-related details:

* `STORE_ID` – Unique store identifier
* `CITY` – Store location
* `REGION` – Geographic region

---

### 2. Product Master (`src_products.csv`)

Contains product details:

* `PROD_ID` – Unique product identifier
* `PROD_NAME` – Name of product
* `CATEGORY` – Product category
* `BASE_PRICE` – Base price of product

---

### 3. Transaction Data (`src_sales_txn.csv`)

Contains transactional sales data:

* `TXN_ID` – Transaction ID
* `DATE` – Transaction date
* `STORE_ID` – Store reference
* `PROD_ID` – Product reference
* `UNITS` – Quantity sold
* `PRICE` – Selling price

---

## 🎯 Objective

The goal is to:

* Build **dimension tables** for Store and Product
* Build a **fact table (FACT_SALES)** for transactional data
* Perform data transformations and enrichment
* Calculate **TOTAL_REVENUE**
* Implement **data validation and orchestration**

---

## 🏗️ Data Model

### 🔹 Dimension Tables

* `DIM_STORE`
* `DIM_PRODUCT`

### 🔹 Fact Table

* `FACT_SALES`

The fact table contains:

* Foreign keys → `STORE_ID`, `PROD_ID`
* Measures → `UNITS`, `PRICE`, `TOTAL_REVENUE`

---

## ⚙️ Transformations Performed

### ✅ Store Mapping (`m_Store_Load`)

* Direct load from source
* Added `LOAD_DATE` using Expression

---

### ✅ Product Mapping (`m_Product_Load`)

* Applied **Filter Transformation**
* Removed products where:

  ```
  BASE_PRICE <= 500
  ```

---

### ✅ Sales Fact Mapping (`m_Sales_Fact_Load`)

* Used **Lookup Transformations**:

  * Store lookup → fetch CITY, REGION
  * Product lookup → fetch all fiend of product

* Applied **Expression Transformation**:

  ```
  TOTAL_REVENUE = UNITS * PRICE
  ```

* Standardized data:

  * Converted CITY and CATEGORY to uppercase

---

## 🔁 Taskflow Design

Execution flow:

```
m_Store_Load → m_Product_Load → m_Sales_Fact_Load
```

5. Run Taskflow
