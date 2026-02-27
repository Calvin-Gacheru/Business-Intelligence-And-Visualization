# Mid Semester Project: Restaurant Sales Analytics

## Project Overview

This project is a comprehensive Business Intelligence (BI) analysis of restaurant sales data. It demonstrates a complete end-to-end BI workflow, from raw data through data preparation, data modeling, and interactive dashboard development in Power BI.

**Project Scope**: Analyze 17,534 restaurant transactions to uncover sales trends, customer payment preferences, top-selling items, and revenue patterns across different product categories and time periods.

---

## Folder Structure

```
Mid Semester Project/
├── README.md                          # This file - project documentation
├── Final Results.md                   # Detailed analysis results and Power BI dashboard links
├── Data Split.ipynb                   # Jupyter notebook: Python script for data splitting
└── Data/
    ├── restaurant_sales_data.csv      # Original raw dataset (17,534 transactions)
    ├── Order Table.csv                # Processed file: Order-level data
    └── Customer Table.csv             # Processed file: Customer item-level data
```

---

## Dataset Information

### Source
- **Dataset Name**: Restaurant Sales (Dirty Data for Cleaning Training)
- **Source**: [Kaggle - Restaurant Sales Dataset](https://www.kaggle.com/datasets/ahmedmohamed2003/restaurant-sales-dirty-data-for-cleaning-training)
- **Record Count**: 17,534 transactions
- **Time Period**: 2022-2023

### Original Data Columns
| Column | Type | Description |
|--------|------|-------------|
| Order ID | String | Unique identifier for each order (e.g., ORD_123456) |
| Customer ID | String | Unique identifier for each customer (e.g., CUST_001) |
| Category | String | Product category (Main Dishes, Drinks, Desserts, Side Dishes) |
| Item | String | Name of the purchased item (may contain missing values) |
| Price | Numeric | Static price of the item per unit |
| Quantity | Numeric | Quantity ordered |
| Order Total | Numeric | Total transaction amount (Price × Quantity) |
| Order Date | Date | Date when the order was placed |
| Payment Method | String | Payment type used (Cash, Credit Card, Digital Wallet) |

---

## 🔧 Data Preparation & Processing

### Step 1: Data Splitting
The raw dataset was split into two normalized tables using Python (see `Data Split.ipynb`):

**Order Table** - Order-level aggregates
- Order ID (Primary Key)
- Order Total
- Order Date
- Payment Method

**Customer Table** - Customer item-level details
- Customer ID
- Category
- Item
- Price
- Quantity
- Order ID (Foreign Key)

### Step 2: Data Cleaning & Transformation (Power Query)
Data quality improvements performed in Power BI Power Query Editor:
- ✅ **Removed duplicates** in Customer and Order tables
- ✅ **Handled missing values**:
  - Null Items → "Unknown Item"
  - Null Payment Methods → "Unknown"
  - Null Prices → Filled with median category price
  - Null Quantities → Filled with 1 (reasonably assumed minimum)
- ✅ **Standardized data**: Trimmed leading/trailing spaces in Item names
- ✅ **Created derived columns**: Total Price = Price × Quantity
- ✅ **Extracted temporal features**:
  - Year (for annual trends)
  - Month (for seasonal analysis)
  - Day of Week (for day-based patterns)
- ✅ **Corrected data types**: Changed Price and Total Price to Decimal
- ✅ **Removed unnecessary columns** not needed for analysis

### Step 3: Data Modeling
Implemented a **Star Schema** architecture with proper relationships:

**Dimension Tables**:
- **Dim_Date**: Date, Year, Month Number, Month Name, Day Name, Day of Week
- **Dim_Customer**: Customer ID
- **Dim_Product**: Item, Category
- **Dim_PaymentMethod**: Payment Method

**Fact Tables**:
- **Fact_OrderItems**: Contains grain of one row per customer-item per order
- **Order Table**: Contains grain of one row per order

**Relationships**:
- Dim_Product (1:*) → Fact_OrderItems
- Dim_Customer (1:*) → Fact_OrderItems
- Dim_PaymentMethod (1:*) → Order Table
- Dim_Date (1:*) → Order Table

---

## File Descriptions

### `Data/restaurant_sales_data.csv`
- **Purpose**: Original raw dataset as downloaded from Kaggle
- **Records**: 17,534 rows
- **Columns**: 9 (Order ID, Customer ID, Category, Item, Price, Quantity, Order Total, Order Date, Payment Method)
- **Status**: Contains data quality issues (nulls, inconsistencies)
- **Use Case**: Reference for understanding source data structure

### `Data/Order Table.csv`
- **Purpose**: Normalized order-level transaction data
- **Records**: 17,535 rows (1 row per order)
- **Columns**: Order ID, Order Total, Order Date, Payment Method
- **Status**: Cleaned and processed
- **Use Case**: Load into Power BI for order-level analysis and time-series trends

### `Data/Customer Table.csv`
- **Purpose**: Normalized customer item-level transaction data
- **Records**: 17,535 rows (1 row per item per order)
- **Columns**: Customer ID, Category, Item, Price, Quantity, Order ID
- **Status**: Cleaned and processed
- **Use Case**: Load into Power BI for product and category analysis

### `Data Split.ipynb`
- **Purpose**: Python Jupyter notebook documenting the data splitting logic
- **Contains**: 
  - Markdown cell explaining the splitting process
  - Python code using pandas to split raw data into Order and Customer tables
- **Language**: Python 3
- **Dependencies**: pandas, numpy
- **Status**: Not executed (reference/documentation only)
- **Use Case**: Understanding or reproducing the data split process

### `Final Results.md`
- **Purpose**: Comprehensive documentation of the entire BI project
- **Contents**:
  - Complete Power Query transformation steps with screenshots
  - Data modeling approach and relationship diagrams
  - Dashboard design details
  - Interactive visualization descriptions
  - Links to Power BI service reports and dashboards
- **Use Case**: Detailed reference for all analysis work performed

---

## Key Insights & Visualizations

The Power BI dashboard includes:
- **Sales Trend Line Chart**: Total sales trends by month and year
- **Payment Method Distribution**: Pie chart showing customer payment preferences
- **Revenue by Category**: Bar chart of total revenue per product category
- **Top 10 Best-Selling Items**: Table ranked by quantity sold
- **Revenue Analysis**: Histogram showing distribution of revenue by item and time period

---

## Power BI Resources

### Interactive Report & Dashboard Links
All resources are available in Power BI Service (requires access):

1. **Interactive Report Link** (Full BI Report with drill-through):
   - [View Report](https://app.powerbi.com/links/1-WjQTQD5N?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare)

2. **Power BI Service Report**:
   - [View Report in Power BI Service](https://app.powerbi.com/groups/me/reports/09309554-c21b-461b-ae29-5672ae075509/3f32b93068d2b8ee210e?experience=power-bi)

3. **Executive Dashboard**:
   - [View Dashboard](https://app.powerbi.com/groups/me/dashboards/219420dc-7d69-46b2-aeb8-06b325cf2101?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare)
   - *Note: Request access if link doesn't work*

---

## How to Use These Files

### Option 1: Load into Power BI (Recommended)
1. Open Power BI Desktop
2. Create a new project
3. Import `Order Table.csv` and `Customer Table.csv` from the Data folder
4. Follow the data modeling approach documented in `Final Results.md`
5. Create visualizations using the specifications provided

### Option 2: Further Analysis with Python
1. Use the provided Jupyter notebook as a template
2. Load the cleaned CSV files using pandas
3. Perform additional statistical analysis or predictions
4. Generate custom reports

### Option 3: Data Integration
- Use the cleaned CSV files with your preferred BI tool (Tableau, Looker, Excel, etc.)
- CSV files are properly formatted and ready for import

---

## Assumptions & Data Quality Notes

- **Missing Item Names**: Filled with "Unknown Item" to preserve transaction totals
- **Missing Prices**: Filled using category-level median prices
- **Null Quantities**: Default assumption of 1 unit minimum
- **Date Range**: Analysis covers 2022-2023 period only
- **Customer IDs**: Anonymized and sequentially numbered (CUST_001, CUST_002, etc.)

---

## Project Deliverables Checklist

- ✅ Dataset selected and sourced
- ✅ Data processed and normalized
- ✅ Data cleaning and transformation completed
- ✅ Data model designed with proper relationships
- ✅ Star schema implemented
- ✅ Dashboard developed with key metrics
- ✅ Interactive report published in Power BI Service
- ✅ Project documentation completed

---

## Author

Calvin Gacheru  
[GitHub Repository](https://github.com/Calvin-Gacheru/Business-Intelligence-And-Visualization)

---

## Related Files

- Assignment One Requirements: See parent `Assignment One/` folder
- Additional assignments: See `Assignment Two/` folder

---

