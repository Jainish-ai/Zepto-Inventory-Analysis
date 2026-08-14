# 🛒 Zepto E-commerce SQL Data Analyst Portfolio Project

## 📌 Project Overview

This project is a real-world **E-commerce Data Analysis project using PostgreSQL and SQL**, based on a Zepto product inventory dataset originally sourced from Kaggle and scraped from Zepto's product listings.

The project simulates a typical **Data Analyst workflow**, starting from raw and inconsistent inventory data and progressing through **database creation, data exploration, data cleaning, and business-focused SQL analysis**.

The objective is to use SQL to uncover actionable insights related to:

* Product pricing
* Discounts
* Inventory
* Stock availability
* Product categories
* Revenue potential
* Product value
* Inventory weight

---

## 🎯 Business Objectives

The project focuses on answering business questions such as:

* Which products offer the highest discounts?
* Which expensive products are currently out of stock?
* Which categories have the highest potential revenue?
* Which categories provide the highest average discounts?
* Which products offer the best price per gram?
* How much inventory weight is available across categories?
* Which products have pricing inconsistencies?
* How is inventory distributed across product categories?

---

## 📊 Dataset Overview

The dataset represents an **e-commerce product inventory system**, where each row represents a unique **SKU (Stock Keeping Unit)**.

Duplicate product names can exist because the same product may be listed under different:

* Package sizes
* Weights
* Discounts
* Categories
* SKU combinations

This makes the dataset representative of the type of messy product-catalog data encountered in real-world e-commerce analytics.

### Dataset Columns

| Column                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| `sku_id`                 | Unique identifier for each product/SKU        |
| `name`                   | Product name                                  |
| `category`               | Product category                              |
| `mrp`                    | Maximum Retail Price                          |
| `discountPercent`        | Discount percentage applied to MRP            |
| `discountedSellingPrice` | Final selling price after discount            |
| `availableQuantity`      | Available inventory quantity                  |
| `weightInGms`            | Product weight in grams                       |
| `outOfStock`             | Indicates whether the product is out of stock |
| `quantity`               | Number of units per package                   |

---

## 🛠️ Tools & Technologies

* **PostgreSQL**
* **SQL**
* **pgAdmin**
* **CSV**
* **Kaggle Dataset**

### SQL Concepts Used

* `SELECT`
* `WHERE`
* `DISTINCT`
* `GROUP BY`
* `HAVING`
* `ORDER BY`
* `LIMIT`
* Aggregate Functions

  * `COUNT()`
  * `SUM()`
  * `AVG()`
  * `MIN()`
  * `MAX()`
* Conditional Filtering
* Data Cleaning
* Data Transformation
* Business KPI Analysis
* Ranking and Aggregation

---

# 🔧 Project Workflow

## 1. Database & Table Creation

The project begins by creating a PostgreSQL table with appropriate data types.

```sql
CREATE TABLE zepto (
    sku_id SERIAL PRIMARY KEY,
    category VARCHAR(120),
    name VARCHAR(150) NOT NULL,
    mrp NUMERIC(8,2),
    discountPercent NUMERIC(5,2),
    availableQuantity INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms INTEGER,
    outOfStock BOOLEAN,
    quantity INTEGER
);
```

---

## 2. Data Import

The dataset was imported into PostgreSQL using **pgAdmin's Import/Export functionality**.

Alternatively, the CSV can be imported using:

```sql
\copy zepto(
    category,
    name,
    mrp,
    discountPercent,
    availableQuantity,
    discountedSellingPrice,
    weightInGms,
    outOfStock,
    quantity
)
FROM 'data/zepto_v2.csv'
WITH (
    FORMAT csv,
    HEADER true,
    DELIMITER ',',
    QUOTE '"',
    ENCODING 'UTF8'
);
```

### Encoding Issue

During data import, a UTF-8 encoding issue was encountered.

The issue was resolved by saving the dataset using **CSV UTF-8 format** before importing it into PostgreSQL.

---

# 🔍 3. Exploratory Data Analysis

The initial exploration focused on understanding the structure and quality of the dataset.

### Analysis Performed

* Counted total number of records
* Viewed sample records
* Examined table structure
* Checked for NULL values
* Identified distinct product categories
* Compared in-stock and out-of-stock products
* Identified duplicate product names
* Examined SKU-level product variations

### Key EDA Questions

```text
How many products/SKUs are present?

How many unique categories exist?

Are there missing values?

How many products are in stock?

How many products are out of stock?

Which products appear multiple times?

Are duplicate products actually different SKUs?
```

---

# 🧹 4. Data Cleaning

Before performing business analysis, the dataset was cleaned to improve data quality and consistency.

### Cleaning Steps

### Removed Invalid Pricing Records

Products with:

```text
MRP = 0
```

or

```text
Discounted Selling Price = 0
```

were identified and removed from the analysis.

### Currency Transformation

Pricing values originally stored in **paise** were converted into **Indian Rupees (₹)**.

For example:

```text
Price in Rupees = Price in Paise / 100
```

This made the pricing data easier to interpret and analyze.

---

# 📊 5. Business Analysis

After cleaning the data, SQL was used to answer business-focused questions.

## 🏷️ Product Discount Analysis

### Analysis

Identified the **Top 10 products with the highest discount percentages**.

### Business Use

Helps identify products that provide the strongest customer discounts and can support promotional strategy analysis.

---

## 💰 High-Value Out-of-Stock Products

Identified products with:

* High MRP
* Out-of-stock status

### Business Use

Helps identify potentially valuable products where stock availability may be affecting sales opportunities.

---

## 💵 Estimated Revenue Potential

Estimated potential revenue at the product/category level using available inventory and selling price.

### Business Use

Helps estimate the revenue opportunity associated with current inventory.

> **Note:** This represents estimated potential revenue based on available inventory, not actual realized sales revenue.

---

## 🏷️ Expensive Products With Low Discounts

Filtered products with:

```text
MRP > ₹500
```

and relatively low discount percentages.

### Business Use

Helps identify expensive products where pricing or promotional strategies may need further review.

---

## 📈 Category Discount Analysis

Ranked the **Top 5 product categories by average discount percentage**.

### Business Use

Helps identify categories where customers receive the strongest average discounts.

---

## ⚖️ Price Per Gram Analysis

Calculated:

```text
Price Per Gram =
Discounted Selling Price / Weight in Grams
```

### Business Use

Helps identify products that provide better value relative to their weight.

This can support **price comparison and value-for-money analysis**.

---

## 📦 Product Weight Segmentation

Products were grouped into:

* **Low Weight**
* **Medium Weight**
* **Bulk**

based on their weight in grams.

### Business Use

Helps understand product packaging and inventory distribution across different weight segments.

---

## ⚖️ Category-Level Inventory Weight

Calculated the total inventory weight available within each product category.

### Business Use

Helps identify categories holding the largest physical inventory volume.

This can support:

* Inventory planning
* Warehouse management
* Category-level stock monitoring

---

# 💡 Key Business Insights

The analysis provides insights into several important e-commerce business areas:

### Pricing

Identifies products with high MRP, high discounts, and low-discount expensive products.

### Inventory

Highlights stock availability, out-of-stock products, and category-level inventory.

### Revenue Potential

Estimates potential revenue based on current inventory and discounted selling prices.

### Product Value

Uses price-per-gram analysis to compare product value across different package sizes.

### Category Performance

Ranks categories based on discount levels and inventory weight.

### Product Catalog Quality

Identifies duplicate products/SKUs and invalid pricing records during data exploration and cleaning.

---

# 📈 Business Questions Answered

1. What is the total number of products/SKUs?
2. How many unique product categories are available?
3. Are there any NULL values in the dataset?
4. How many products are in stock?
5. How many products are out of stock?
6. Which products appear multiple times?
7. Which products have the highest discount percentage?
8. Which high-MRP products are out of stock?
9. What is the estimated potential revenue by product/category?
10. Which products have MRP above ₹500 with minimal discounts?
11. Which categories have the highest average discounts?
12. Which products have the lowest price per gram?
13. How can products be segmented based on weight?
14. Which categories contain the highest total inventory weight?

---

# 📁 Project Structure

```text
Zepto-SQL-Data-Analysis/
│
├── data/
│   └── zepto_v2.csv
│
├── sql/
│   ├── 01_create_table.sql
│   ├── 02_data_exploration.sql
│   ├── 03_data_cleaning.sql
│   └── 04_business_analysis.sql
│
└── README.md
```

---

# 🎓 Skills Demonstrated

This project demonstrates practical experience in:

* SQL Data Analysis
* PostgreSQL
* Exploratory Data Analysis
* Data Cleaning
* Data Validation
* Data Transformation
* E-commerce Analytics
* Inventory Analysis
* Pricing Analysis
* Discount Analysis
* Revenue Estimation
* Business KPI Analysis
* Business Problem Solving
* Data-driven Decision Making

---

# 👨‍💻 About

**Jainish Parikh**
Data Analyst | SQL | Advanced Excel | Data Science

I enjoy using data and analytics to solve business problems and turn raw datasets into actionable insights.

---

## ⭐ Project Highlights

**Domain:** E-commerce / Quick Commerce
**Database:** PostgreSQL
**Analysis Tool:** SQL / pgAdmin
**Dataset:** Zepto Product Inventory
**Focus:** Pricing, Discounts, Inventory, Stock Availability & Revenue Potential

---

## 📜 License

This project is available under the **MIT License**.
