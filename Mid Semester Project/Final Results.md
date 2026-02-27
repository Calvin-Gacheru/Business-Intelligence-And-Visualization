# MID SEMESTER PRACTICAL EXAMINATION

You are required to act as a Business Intelligence Analyst solving a real-world organizational 
problem using Power BI.

You must complete a full end-to-end BI workflow: 
Dataset Selection → Data Preparation → Data Modelling → Dashboard Development → Publishing 

## Dataset Selection
My dataset is from Kaggle, and it is called [`"Restaurant Sales"`](https://www.kaggle.com/datasets/ahmedmohamed2003/restaurant-sales-dirty-data-for-cleaning-training). The Restaurant Sales Dataset with Dirt contains data for `17,534` transactions. 

You can find the dataset [here](https://www.kaggle.com/datasets/ahmedmohamed2003/restaurant-sales-dirty-data-for-cleaning-training). 

Details about the dataset can be found in the following image:
![alt text](image.png)

### Columns Description
| Column Name      | Description                                                                 | Example Values                 |
|------------------|-------------------------------------------------------------------------------|-------------------------------|
| Order ID         | A unique identifier for each order.                                           | ORD_123456                    |
| Customer ID      | A unique identifier for each customer.                                        | CUST_001                      |
| Category         | The category of the purchased item.                                           | Main Dishes, Drinks           |
| Item             | The name of the purchased item. May contain missing values due to data dirt.  | Grilled Chicken, None         |
| Price            | The static price of the item. May contain missing values.                     | 15.0, None                    |
| Quantity         | The quantity of the purchased item. May contain missing values.               | 1, None                       |
| Order Total      | The total price for the order (Price × Quantity). May contain missing values. | 45.0, None                    |
| Order Date       | The date when the order was placed. Always present.                           | 2022-01-15                    |
| Payment Method   | The payment method used for the transaction. May contain missing values.      | Cash, None                    |

A sample of the dataset is shown below:
![alt text](image-1.png)

To obtain multiple tables, I split the dataset into two tables: `Order Table` and `Customer Table`.
``` python
import pandas as pd
import numpy as np

# Load the dataset
data = pd.read_csv('restaurant_sales_data.csv')

# Splitting the data
table1 = data[
    ['Order ID', 'Order Total', 'Order Date', 'Payment Method']
]

table2 = data[
    ['Customer ID', 'Category', 'Item', 'Price', 'Quantity', 'Order ID']
]

# Save the tables to new CSV files
table1.to_csv('Order Table.csv', index=False)
table2.to_csv('Customer Table.csv', index=False)
```
The `Order Table` contains the following columns:
'Order ID', 'Order Total', 'Order Date', 'Payment Method'

![alt text](image-2.png)

The `Customer Table` contains the following columns:
'Customer ID', 'Category', 'Item', 'Price', 'Quantity', 'Order ID'

![alt text](image-3.png)

## Part A: Power Query Editor(Data Preparation)

### Loading the data in Power BI
- Loading process of the Customer Table

![alt text](image-4.png)

- Loading process of the Order Table
![alt text](image-5.png)

- Power Query Editor after loading the tables
![alt text](image-6.png)


### Data Cleaning and Transformation
- Removing duplicates in the Customer Table

- Handling missing or null values
    - Filled missing Items and Payment Method with "Unknown Item"
    ![alt text](image-7.png)

    - Filling the missing Price:
    ![alt text](image-12.png)

    - Filled missing Quantity with 1 because it is reasonable to assume that if an item was ordered, at least one unit was purchased.
    ![alt text](image-13.png)

- Standardizing inconsistent data entries
    - Removed leading and trailing spaces in the Item column
    ![alt text](image-10.png)

- Creating derived columns:
`Total Price = (Price × Quantity)` in the Customer Table
![alt text](image-14.png)

- Extracting:
    - Year from the Order Date column to analyze sales trends over time
    ![alt text](image-15.png)
    - Month from the Order Date column to identify seasonal patterns in sales
    ![alt text](image-16.png)
    - Day of the week from the Order Date column to determine which days are busiest for the restaurant
    ![alt text](image-17.png)

- Renaming queries professionally for better readability and organization
    - Before Renaming
    ![alt text](image-18.png)

    - After Renaming
    ![alt text](image-23.png)

- Fixing data types: 
    - Changed Total Price to Decimal Number from Any
    ![alt text](image-21.png)

- Removing unnecessary columns that do not contribute to the analysis
![alt text](image-22.png)

- Merging the two tables using the Order ID column to create a comprehensive dataset for analysis   
![alt text](image-20.png)

## Part B: Data Modelling
Design a clean and logical analytical data model.
- Create relationships between the tables.
- Creating Dimention table for Order Date
![alt text](image-24.png)
    - Dim_Date
        - Date (unique)
        - Year, Month Number, Month Name, Day Name, Day of Week, etc.

    - Dim_Customer
        - Customer ID (unique)

    - Dim_Product
        - Item (unique)
        - Category

    - Dim_PaymentMethod
        - Payment Method (unique)

- Create relationships between tables 
- Implement a Star Schema structure 
- Apply correct cardinality 

    - Dim_Product (1) → Fact_OrderItems (*)
    - Dim_Customer (1) → Fact_OrderItems (*)
    - Order Table (1) → Fact_OrderItems (*) via Order ID
    - Dim_Date (1) → Order Table (*)
    - Dim_PaymentMethod (1) → Order Table (*)

- Avoid many-to-many relationships 
![alt text](image-29.png)
- Ensure proper filter direction 
- Hide unnecessary technical fields 
- Organize tables logically within Model View
![alt text](image-27.png)

Model View after creating relationships and dimension tables:
![alt text](image-25.png)
![alt text](image-28.png)

## Part C: Dashboard Development

Trend Analysis:
- Created a line chart to show the trend of total sales over time (by month and year).
- Created a pie chart to show the distribution of Payment Methods used by customers.
- Created a histogram to show the distribution of Total Revenue per Item and per month.
- Create a bar chart to show total revenue by category.
- Created a table to show the top 10 best-selling items based on quantity sold.
![alt text](image-30.png)

### Links to the report and dashboard in Power BI Service
To view the full report, please click to get the interactivity [here](https://app.powerbi.com/links/1-WjQTQD5N?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare): https://app.powerbi.com/links/1-WjQTQD5N?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare

Click [here](https://app.powerbi.com/groups/me/reports/09309554-c21b-461b-ae29-5672ae075509/3f32b93068d2b8ee210e?experience=power-bi) to view the report in Power BI Service: https://app.powerbi.com/groups/me/reports/09309554-c21b-461b-ae29-5672ae075509/3f32b93068d2b8ee210e?experience=power-bi

Click here to view the dashboard in Power BI Service: [Dashboard Link](https://app.powerbi.com/groups/me/dashboards/219420dc-7d69-46b2-aeb8-06b325cf2101?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare):
https://app.powerbi.com/groups/me/dashboards/219420dc-7d69-46b2-aeb8-06b325cf2101?ctid=16d83ee6-254a-469d-a6cc-54e2ca2313e7&pbi_source=linkShare

*Note, the dashboard link will only work when I have given you access to the dashboard. Please let me know if you want to view the dashboard, and I will give you access.*

Click here to view the Github repository for the Power BI project: [GitHub Repository](https://github.com/Calvin-Gacheru/Business-Intelligence-And-Visualization): https://github.com/Calvin-Gacheru/Business-Intelligence-And-Visualization





