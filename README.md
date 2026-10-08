# E-Commerce Sales Data Analysis

An end-to-end **E-Commerce Sales Data Analysis** project built in Microsoft Power BI. This project demonstrates the complete data analytics workflow — from raw data preparation and transformation to data modeling, DAX development, dashboard visualization, business insights, and actionable recommendations.

The goal of the project is to analyze sales performance, product performance, customer behavior, and order-related issues to identify the factors affecting revenue and provide data-driven recommendations for improving business performance.

---

## 📌 Project Overview

This project simulates an e-commerce business environment where transactional, customer, product, and payment data are combined to answer key business questions.

The analysis focuses on:

* Sales and revenue performance
* Order trends and completion performance
* Product performance
* Customer activity and retention
* Order cancellations and returns
* Revenue loss
* Customer churn
* Revenue concentration
* Business opportunities and risks

Rather than creating a dashboard directly from raw data, the project follows a structured **Extract → Transform → Model → Analyze → Visualize → Recommend** workflow.

---

## 🎯 Business Objectives

The main objectives of this analysis are to:

1. Evaluate overall sales and revenue performance.
2. Identify trends and changes in revenue over time.
3. Determine which products and categories contribute most to revenue.
4. Analyze customer activity, retention, and churn.
5. Measure the impact of cancelled and returned orders.
6. Identify potential revenue losses.
7. Understand whether sales volume translates into revenue.
8. Provide actionable recommendations based on the findings.

---

## ❓ Business Questions

The analysis was designed around the following questions:

### Sales Performance

* How much revenue is being generated?
* How are sales changing over time?
* Which months perform best?
* How much revenue does each category contribute?
* How does average order value change over time?

### Product Performance

* Which products generate the most revenue?
* Which products have the highest sales volume?
* Which categories generate the most revenue?
* Does higher quantity sold necessarily result in higher revenue?
* Which products have higher revenue per unit?

### Customer Analysis

* How many customers are in the customer base?
* How many customers are actively purchasing?
* What percentage of customers are active?
* What is the customer churn rate?
* Which customer segments generate the most revenue?
* Are existing customers returning?

### Order Performance

* What percentage of orders are completed?
* What percentage of orders are cancelled?
* What percentage of orders are returned?
* How much revenue is potentially lost from failed orders?
* Are order failures concentrated in particular categories or periods?

---

# 🗂️ Dataset

The project uses four source datasets:

| Dataset         | Description                                       |         Records |
| --------------- | ------------------------------------------------- | --------------: |
| `customers.csv` | Customer demographic and segmentation information |          10,000 |
| `orders.csv`    | Transaction-level order information               | 50,120 raw rows |
| `payments.csv`  | Payment dates and payment statuses                |          50,000 |
| `products.csv`  | Product details and pricing                       |              20 |

### Customers

Contains:

* Customer ID
* Age
* City
* Signup Date
* Customer Segment

### Orders

Contains:

* Order ID
* Customer ID
* Order Date
* Product ID
* Quantity
* Discount
* Payment Method
* Order Status

### Payments

Contains:

* Payment ID
* Order ID
* Payment Date
* Payment Status

### Products

Contains:

* Product ID
* Product Name
* Category
* Unit Price

## ⚠️ Dataset Disclaimer

This project uses a **publicly available/synthetic e-commerce dataset for portfolio and educational purposes**. The dataset does not represent proprietary or confidential business data from an actual company.

The business context, analysis, insights, and recommendations presented in this project were developed as part of the portfolio analysis exercise to demonstrate an end-to-end data analytics workflow.

While the dataset is not from a real business, the project applies industry-relevant practices including **data cleaning, Power Query transformation, data modeling, star schema design, DAX development, dashboard visualization, exploratory analysis, and business-oriented recommendations**.


---

# 🛠️ Tools & Technologies

* **Microsoft Power BI**
* **Power Query**
* **DAX**
* **Data Modeling**
* **Star Schema**
* **Data Cleaning**
* **Data Visualization**
* **Business Intelligence**
* **CSV**

---

# 🔄 End-to-End Analytics Workflow

The project follows this workflow:

```text
Raw CSV Data
     ↓
Data Inspection
     ↓
Data Cleaning
     ↓
Power Query Transformation
     ↓
Data Integration
     ↓
Star Schema Modeling
     ↓
DAX Measures
     ↓
Dashboard Development
     ↓
Data Analysis
     ↓
Key Insights
     ↓
Business Recommendations
```

---

# 1. Data Inspection

Before building the dashboard, the source datasets were inspected to understand:

* Dataset structure
* Column names
* Data types
* Missing values
* Duplicate records
* Invalid values
* Relationships between datasets
* Possible data-quality issues

Several issues were identified in the raw data.

### Examples of Data Quality Issues

#### Duplicate Order IDs

The raw orders file contains **50,120 rows but only 50,000 unique Order IDs**, indicating duplicate order records that needed to be investigated and addressed.

#### Invalid Quantities

The orders dataset contained:

* Negative quantities
* Zero quantities

These records were treated as invalid transactional values during the cleaning process.

#### Missing Values

Missing values were identified in several fields, including:

* Customer attributes
* Order dates
* Discounts
* Payment methods

#### Customer Relationship Issues

Some order records contained customer IDs that were not present in the customer dimension. These records were handled in the model and represented as **Unknown** customers rather than silently removing the transactions.

This preserves the transactional information while making the data-quality issue visible.

---

# 2. Data Cleaning & Transformation with Power Query

Power Query was used as the primary data preparation layer before creating the analytical model.

### Main transformation steps

#### Data Type Standardization

Appropriate data types were applied to:

* IDs
* Dates
* Numeric quantities
* Discounts
* Unit prices
* Customer attributes

Date fields were converted into proper date formats to support time-based analysis.

---

### Duplicate Handling

Duplicate order records were identified using `OrderID`.

The objective was to ensure that each order was represented correctly in the analytical fact table and that duplicate records did not artificially inflate:

* Revenue
* Quantity
* Order counts
* Customer activity

---

### Invalid Quantity Handling

Transactions containing zero or negative quantities were identified and removed from the analytical dataset because they do not represent valid sales quantities for this analysis.

---

### Missing Value Handling

Missing values were reviewed based on business meaning rather than blindly deleting every incomplete row.

For example:

* Missing customer relationships were retained as `Unknown`
* Missing categorical values were standardized where appropriate
* Missing transactional values were reviewed before inclusion

This helped prevent unnecessary loss of potentially useful transactional data.

---

### Payment Data Integration

Payment information was integrated into the transactional model so that payment status could be analyzed alongside order performance.

This allowed the analysis to consider:

* Paid orders
* Failed payments
* Refunded transactions
* Order completion
* Potential revenue loss

---

# 3. Data Modeling

After transformation, the data was structured into a **Star Schema**.

The model contains:

```text
                  DimCustomers
                       |
                       |
DimProducts ---- FactOrders ---- DimDate
                       |
                       |
                  Measures
```

### Fact Table

#### `FactOrders`

The central transaction table containing order-level information.

It contains transactional attributes such as:

* Order ID
* Customer ID
* Product ID
* Order Date
* Quantity
* Discount
* Payment information
* Order status

The fact table is used as the foundation for sales and order analysis.

---

### Dimension Tables

#### `DimCustomers`

Contains customer-level information used for:

* Customer segmentation
* Demographic analysis
* Customer activity
* Retention analysis
* Churn analysis

---

#### `DimProducts`

Contains product information used for:

* Product performance
* Category analysis
* Unit price analysis
* Revenue contribution
* Quantity analysis

---

#### `DimDate`

A dedicated date dimension was created to support:

* Monthly trends
* Year analysis
* Time intelligence
* Revenue trends
* Order trends
* Customer activity trends

Using a dedicated date table provides a consistent time dimension across the report.

---

### Measures Table

A dedicated `_Measures` table was also created to organize DAX measures separately from the underlying data tables.

This improves model organization and makes measures easier to maintain.

---

# 4. DAX Measures

DAX was used to create reusable business metrics instead of relying solely on raw columns.

Examples of the analytical measures developed include:

### Sales Metrics

* Total Revenue
* Total Orders
* Total Quantity
* Average Order Value
* Revenue Growth
* Revenue % of Total
* Best Month

### Product Metrics

* Average Unit Price
* Average Discount
* Highest Unit Price
* Revenue per Unit
* Top Product

### Customer Metrics

* Total Customers
* Active Customers
* Active Customer Rate
* Customer Churn Rate
* Average Customer Lifespan
* Repeat Customer Revenue %

### Order Metrics

* Average Items per Order
* Cancelled Rate
* Returned Rate
* Average Discount per Order
* Revenue Lost

### Example Revenue Calculation

A core sales metric can be represented conceptually as:

```DAX
Revenue =
SUMX(
    FactOrders,
    FactOrders[Quantity]
        * RELATED(DimProducts[UnitPrice])
        * (1 - FactOrders[Discount])
)
```

This calculates revenue at the transaction level while accounting for product price and discount.

---

# 5. Dashboard Development

The Power BI report was designed as a multi-page analytical dashboard rather than a single crowded visualization.

The final report contains six analytical pages:

1. Executive Overview
2. Sales
3. Products
4. Customers
5. Orders
6. Insights & Recommendations

---

# 📊 Dashboard Pages

## 1. Executive Overview

The Executive Overview provides a high-level summary of the business.

### Key KPIs

* Total Revenue
* Total Orders
* Total Quantity
* Average Order Value
* Completion Rate

### Visualizations

* Revenue trend
* Revenue by category
* Order status distribution
* Key business insights

The purpose of this page is to allow stakeholders to understand overall performance quickly before exploring individual areas.

---

## 2. Sales Analysis

The Sales page focuses on revenue performance and sales trends.

### Key Metrics

* Total Revenue
* Total Orders
* Average Order Value
* Total Quantity
* Revenue Growth
* Revenue % of Total
* Best Month

### Analysis Areas

* Revenue trend over time
* Revenue growth
* Average order value trend
* Revenue contribution by category
* Monthly sales performance

This page helps identify when revenue is growing or declining and which areas contribute most to overall sales.

---

## 3. Product Analysis

The Products page evaluates product-level performance.

### Key Metrics

* Total Quantity
* Average Unit Price
* Average Discount
* Highest Unit Price
* Top Product
* Revenue per Unit

### Analysis Areas

* Quantity sold by category
* Revenue by product
* Average unit price
* Revenue per unit
* Product quantity vs. revenue relationship

A scatter plot was used to identify whether high-volume products necessarily generate high revenue.

This prevents the analysis from relying solely on quantity rankings.

---

## 4. Customer Analysis

The Customers page focuses on customer activity and retention.

### Key Metrics

* Total Customers
* Active Customers — 90 days
* Active Customer Rate
* Customer Churn Rate
* Average Customer Lifespan

### Analysis Areas

* Revenue by customer segment
* Active customer rate
* Customer churn rate
* Customer trends over time
* Customer segmentation

This page helps identify whether revenue performance is supported by healthy customer retention or an increasingly shrinking customer base.

---

## 5. Order Analysis

The Orders page focuses on operational performance and revenue leakage.

### Key Metrics

* Average Items per Order
* Cancelled Rate
* Returned Rate
* Average Discount per Order
* Revenue Lost

### Analysis Areas

* Order volume trend
* Order status
* Cancellation rate
* Return rate
* Revenue lost from failed orders

This section connects operational problems with their potential financial impact.

---

# 6. Key Insights

The dashboard revealed several important findings.

## 1. Electronics is the primary revenue driver

The Electronics category generates approximately **$1.75M, representing around 51% of total revenue**.

This indicates that overall business performance is strongly influenced by the performance of the Electronics category.

**Implication:** Changes in Electronics demand can have a significant effect on total company revenue.

---

## 2. Revenue declined significantly in 2026

Revenue dropped sharply after January 2026, reaching approximately **$75K in February** and remaining below previous-year levels during the displayed period.

Because the available 2026 data is incomplete, comparisons should be made carefully using equivalent periods rather than comparing full-year totals.

---

## 3. Customer retention is a major concern

Approximately **46.9% of the previous year's customers did not return**, while only around **27.9% of customers purchased within the last 90 days**.

Customer churn remains high across customer segments.

This suggests that declining revenue cannot be explained only by product performance.

Customer retention is a significant business issue.

---

## 4. Customer acquisition is not replacing customer loss

The analysis indicates that there are **no 2026 customer sign-ups** in the available dataset.

This creates a structural risk:

```text
Customers leaving
       +
No meaningful new customer inflow
       ↓
Shrinking active customer base
       ↓
Potential long-term revenue decline
```

The business is therefore highly dependent on existing customers.

---

## 5. Order failures create a meaningful revenue leak

Approximately:

* **5% of orders are cancelled**
* **3.1% are returned**

Together, these represent approximately **$282K in potential revenue loss**, or around **8% of revenue** based on the dashboard analysis.

The consistency of failure rates across categories suggests that the issue may be related to the overall order or payment process rather than one specific product category.

---

## 6. Sales volume does not always equal revenue performance

The product analysis shows that products with higher quantities sold do not necessarily generate the highest revenue.

This highlights the importance of evaluating both:

* Sales volume
* Revenue contribution

A product can sell frequently while generating relatively low revenue because of its lower unit price or discount level.

---

## 7. Some transactions have incomplete customer information

Approximately **3% of revenue** is associated with customer IDs that could not be matched to the customer dimension.

Instead of deleting these transactions, they were retained and categorized as **Unknown**.

This preserves revenue accuracy while highlighting a data-quality issue that should be investigated.

---

# 💡 Business Recommendations

Based on the analysis, the following recommendations are proposed.

## 1. Launch a customer win-back strategy

Prioritize customers who have not purchased recently.

Potential actions:

* Identify customers inactive for 90+ days
* Send targeted re-engagement campaigns
* Provide personalized offers
* Create incentives for repeat purchases
* Prioritize high-value inactive customers

The objective is to recover existing customers before spending heavily on new acquisition.

---

## 2. Restart and monitor customer acquisition

The absence of new customer sign-ups in 2026 is a major warning signal.

The business should:

* Establish monthly acquisition targets
* Track new customers by month
* Monitor acquisition channels
* Measure conversion from new customer to repeat customer
* Compare customer acquisition cost against customer value

---

## 3. Investigate the order and payment process

Because cancellations and returns occur consistently across categories, the business should investigate the broader transaction process.

Areas to review include:

* Payment failures
* Fulfillment delays
* Delivery issues
* Order processing
* Return reasons
* Payment methods

The objective should be to reduce operational revenue leakage rather than immediately assuming that product quality is the main problem.

---

## 4. Focus on high-value products, not only high-volume products

Product decisions should consider multiple metrics:

```text
Revenue
+
Quantity
+
Unit Price
+
Discount
+
Revenue per Unit
```

Products with strong revenue contribution should receive greater attention in:

* Inventory planning
* Promotions
* Product placement
* Cross-selling
* Marketing campaigns

---

## 5. Strengthen customer data quality

The presence of transactions associated with unmatched customer IDs indicates a data integration issue.

The business should establish validation rules to ensure:

* Customer IDs are consistent across systems
* New customers are properly registered
* Orders cannot reference invalid customer IDs
* Customer master data is regularly reconciled

Better data quality will improve the accuracy of customer-level analysis.

---

# 📈 Business Impact

The analysis demonstrates that revenue decline should not be viewed only as a sales problem.

The dashboard reveals a combination of:

```text
High Customer Churn
        +
Low/New Customer Acquisition
        +
Order & Return Revenue Leakage
        +
Revenue Concentration
        ↓
Potential Revenue Decline
```

This shifts the business conversation from:

> "Why are sales declining?"

to:

> "How can we retain existing customers, rebuild customer acquisition, and reduce revenue leakage?"

This is a more actionable approach to business analysis.

---

# 🧠 Skills Demonstrated

This project demonstrates practical Data Analyst skills across the complete analytics lifecycle.

### Data Preparation

* Data inspection
* Data cleaning
* Missing-value handling
* Duplicate detection
* Invalid-value handling
* Data type transformation
* Data integration

### Power Query

* Query transformation
* Data type conversion
* Filtering
* Cleaning
* Merging/integrating datasets
* Preparing analytical tables

### Data Modeling

* Star schema design
* Fact and dimension tables
* Relationships
* Date dimension
* Model organization

### DAX

* Measure creation
* Aggregations
* Time-based analysis
* Percentage calculations
* Growth calculations
* Customer activity metrics
* Churn metrics
* Revenue-loss metrics

### Data Visualization

* KPI cards
* Line charts
* Bar charts
* Column charts
* Donut charts
* Scatter plots
* Slicers
* Page navigation
* Executive dashboard design

### Business Analysis

* Trend analysis
* Product analysis
* Customer retention analysis
* Revenue analysis
* Operational analysis
* Root-cause thinking
* Insight generation
* Business recommendations

---

# 📁 Project Structure

```text
E-Commerce-Sales-Data-Analysis/
│
├── data/
│   ├── customers.csv
│   ├── orders.csv
│   ├── payments.csv
│   └── products.csv
│
├── E-commerce Sales Data Analysis.pbix
│
└── README.md
```

---

# 🚀 Project Outcome

This project demonstrates an end-to-end Power BI analytics workflow:

```text
Raw Data
   ↓
Data Quality Assessment
   ↓
Data Cleaning
   ↓
Power Query Transformation
   ↓
Data Integration
   ↓
Star Schema
   ↓
DAX Measures
   ↓
Interactive Dashboard
   ↓
Business Analysis
   ↓
Insights
   ↓
Recommendations
```

The final result is not simply a visualization of historical data. It is an analytical solution that transforms raw transactional data into **business insights that can support decisions around sales, customers, products, and operational performance.**

---

## 👤 Project Type

**End-to-End Data Analytics / Business Intelligence Project**

**Primary Tool:** Microsoft Power BI
**Focus:** Sales, Customer, Product & Order Analytics
**Approach:** Data Cleaning → Data Modeling → DAX → Visualization → Insights → Recommendations

--- 

## 👤 Author

**Kurt Aaron Delizo**

Aspiring Data Analyst | Computer Engineering Graduate

### Tools

`Power BI` `Power Query` `DAX` `Data Analysis` `Data Visualization`
