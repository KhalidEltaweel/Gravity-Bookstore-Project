# End-to-End Data Analysis Solution

## OLTP Database: Gravity Bookstore

Gravity Bookstore is an online transactional processing (OLTP) database designed for a fictional bookstore. It captures and manages detailed information about books, customers, and sales transactions. We will be utilizing this database for our project.
You can find the database

### Tables Description

- **book**: A list of all books available in the store.
- **book_author**: Stores the authors for each book, which is a many-to-many relationship.
- **author**: A list of all authors.
- **book_language**: A list of possible languages of books.
- **publisher**: A list of publishers for books.
- **customer**: A list of the customers of the Gravity Bookstore.
- **customer_address**: A list of addresses for customers, as a customer can have more than one address, and an address has more than one customer.
- **address_status**: A list of statuses for an address, because addresses can be current or old.
- **address**: A list of addresses in the system.
- **country**: A list of countries that addresses are in.
- **cust_order**: A list of orders placed by customers.
- **order_line**: A list of books that are a part of each order.
- **shipping_method**: The possible shipping methods for an order.
- **order_history**: The history of an order, such as ordered, cancelled, delivered.
- **order_status**: The possible statuses of an order.

### Tables Contents

1. **book**
   - **Primary Key**: `book_id`
   - **Attributes**: `title`, `isbn13`, `language_id`, `num_pages`, `publication_date`, `publisher_id`.

2. **book_author**
   - **Composite Primary Key**: `book_id`, `author_id`

3. **Author Table**
   - **Primary Key**: `author_id`
   - **Attributes**: `title`, `author_name`

4. **book_language**
   - **Primary Key**: `language_id`
   - **Attributes**: `title`, `language_code`, `language_name`.

5. **Publisher**
   - **Primary Key**: `publisher_id`
   - **Attributes**: `title`, `publisher_name`.

6. **Customer**
   - **Primary Key**: `customer_id`
   - **Attributes**: `title`, `first_name`, `last_name`, `email`

7. **customer_address**
   - **Primary Key**: `customer_id`, `address_id`
   - **Attributes**: `status_id`

8. **address_status**
   - **Primary Key**: `status_id`
   - **Attributes**: `address_status`

9. **address**
   - **Primary Key**: `address_id`
   - **Attributes**: `street_number`, `street_name`, `city`, `country_id`

10. **country**
    - **Primary Key**: `country_id`
    - **Attributes**: `country_name`

11. **cust_order**
    - **Primary Key**: `order_id`
    - **Attributes**: `order_date`, `customer_id`, `shipping_method_id`, `des_address_id`

12. **order_line**
    - **Primary Key**: `line_id`
    - **Attributes**: `order_id`, `book_id`, `price`

13. **shipping_method**
    - **Primary Key**: `method_id`
    - **Attributes**: `method_name`, `cost`

14. **order_history**
    - **Primary Key**: `history_id`
    - **Attributes**: `order_id`, `status_id`, `status_date`

15. **order_status**
    - **Primary Key**: `status_id`
    - **Attributes**: `status_value`

### ERD

We created this database in SQL Server Management Studio (SSMS). By correctly connecting the tables, we generated the following Entity-Relationship Diagram (ERD): 
![ERD Schema](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/ERD/OLTP%20ERD/SSMS%20Screenshot.png)

For a clearer view of the ERD diagram, you can download the attached `.drawio` file and open it on [https://app.diagrams.net](https://app.diagrams.net).

[def]: https://github.com/bbrumm/databasestar/tree/main/sample_databases/sample_db_gravity/gravity_sqlserver

## Data Warehouse
### Data Modeling:
- **Star Schema:**
![DWH Schema](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/DWH%20model/data_model.jpg)

### Schema and Tables:
#### Tables Overview:

##### Fact_book Table:
**Fields:**
   - `order_id_SK` (Primary Key)
   - `order_id` (Business Key)
   - `book_id` (Foreign Key)
   - `customer_id` (Foreign Key)
   - `shipping_id` (Foreign Key)
   - `date_id` (Foreign Key)
   - `order_status`(dd)
   - `total price` (measure)
   - `shipping_cost` (measure)
   - `quantity_sold` (measure)


##### Dim_shipping Table:
 **Fields:**
   - `shipping_SK` (Primary Key)
   - `shipping_bk` (Business Key)
   - `shipping_method`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Book Table:
 **Fields:**
   - `book_id_SK` (Primary Key)
   - `book_id_BK` (Business Key)
   - `isbn13`
   - `title`
   - `publication_date`
   - `num_pages`
   - `language_name`
   - `publisher_name`
   - `author_name`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Customer Table:
 **Fields:**
   - `customer_id_SK` (Primary Key)
   - `customer_id_BK` (Business Key)
   - `first_name`
   - `last_name`
   - `email`
   - `country`
   - `street_name`
   - `city`
   - `address_status`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Date Table:
 **Fields:**
   - `date_Id_SK` (Primary Key)
   - `year`
   - `month`
   - `day`
   - `quarter`
   - `dayofweek`

##### Dim_status Table:
 **Fields:**
 - `status_SK` (Primary Key)
 - `order_status_id` (business Key)
 - `order_status`


#### Schema Architecture:

- Each table is connected to the **Fact_book** table via foreign keys, forming a **star schema**.
- The **Fact_book** table is positioned at the center, surrounded by dimension tables (**Dim_shipping, Dim_Book, Dim_Customer, Dim_Date**).
- This star schema architecture facilitates efficient querying and data analysis, a common practice in Data Warehousing.

####  Why This Approach is Optimal:

Alignment with Analytical Use Cases:
The star schema is specifically designed for analytical workloads, such as aggregating sales data, analyzing trends, and generating reports. It simplifies complex queries and allows for fast data retrieval, which is critical for business intelligence and decision-making.

Centralized Fact Table:
The fact_book_sales table acts as the single source of truth for transactional data (e.g., sales orders). This centralization ensures consistency and simplifies the process of joining data with dimensions for analysis.

Dimension Tables for Context:
Dimension tables (dim_book, dim_customer, dim_shipping, etc.) provide the necessary context for the facts. For example, you can easily analyze sales by book, customer, or shipping method by joining the fact table with the relevant dimension.

Scalability for Large Datasets:
The schema is designed to handle large volumes of data efficiently. The fact table stores transactional data, while dimension tables store descriptive attributes, reducing redundancy and improving storage efficiency.

Historical Data Tracking:
The inclusion of start_date, end_date, and is_current fields in dimension tables (e.g., dim_customer, dim_shipping) allows you to track changes over time (slowly changing dimensions). This is essential for historical analysis and trend identification.

Benefits of the Star Schema for Your Model:
Improved Query Performance:
Star schemas are optimized for read-heavy operations. Queries often involve simple joins between the fact table and dimension tables, which are typically smaller and indexed, resulting in faster query execution.

Ease of Use for Analysts:
The schema is intuitive and easy to understand, making it accessible for analysts and business users who may not have deep technical expertise. This reduces the learning curve and speeds up report generation.

Flexibility in Analysis:
You can easily slice and dice data across multiple dimensions (e.g., sales by year, customer, book, or shipping method). This flexibility is crucial for answering a wide range of business questions.

Reduced Data Redundancy:
Dimension tables are normalized, reducing redundancy and ensuring consistent data storage. For example, customer details are stored once in dim_customer and referenced in the fact table via a foreign key.

Support for Aggregation:
The schema is well-suited for aggregating data (e.g., total sales, average order value, sales by region), which is a common requirement in analytical workloads.
Advantages of the Star Schema

Simplicity:
The structure is easy to design, implement, and maintain compared to more complex schemas like snowflake or normalized schemas.
Fast Query Performance:
Optimized for read-heavy operations, making it ideal for analytical queries.

Scalability:
Can handle large datasets efficiently, especially when combined with modern data warehouse technologies (e.g., columnar storage, partitioning).
Business-Friendly:

The schema aligns well with how business users think about data (e.g., sales by customer, book, or time period).
Historical Analysis:
Supports slowly changing dimensions (SCDs), enabling historical tracking and trend analysis.
Disadvantages of the Star Schema
Data Redundancy in Dimensions:

While dimension tables are normalized, they can still contain redundant data if not carefully managed. For example, if a book’s author appears in multiple rows, there could be duplication.
Limited Flexibility for Complex Relationships:
Star schemas are not ideal for modeling complex relationships between dimensions. If your use case requires multi-level hierarchies or many-to-many relationships, a snowflake schema might be more appropriate.

Storage Overhead:
The denormalized nature of the fact table can lead to large storage requirements, especially for high-volume transactional data.
Maintenance Challenges:
Managing slowly changing dimensions (SCDs) can be complex, particularly when tracking historical changes over time.
Not Ideal for Transactional Systems:
Star schemas are designed for analytical workloads, not transactional systems. If your use case involves frequent updates or complex transactions, a normalized schema would be more suitable.

When to Use the Star Schema:
1-Your primary goal is analytical reporting and business intelligence.
2-You need fast query performance for aggregations and joins.
3-Your data relationships are relatively simple and hierarchical.
4-You want a schema that is easy for business users to understand and use.

Avoid it when:
Your use case involves complex, multi-level relationships between dimensions.
You need to support frequent updates or transactional workloads.
Storage efficiency is a critical concern, and you cannot tolerate redundancy.
--------------------------------------------------------------------------------
Conclusion:
Your star schema is an excellent choice for a book sales analysis model. It provides the simplicity, performance, and flexibility needed for analytical workloads while supporting historical data tracking and scalability. However, be mindful of its limitations, such as potential data redundancy and challenges with complex relationships. By leveraging modern data warehouse technologies (e.g., columnar storage, partitioning, and indexing), you can further optimize the performance and scalability of your star schema.
# ETL Project with SSIS - Data Warehouse Schema
This project involves building an ETL pipeline using SQL Server Integration Services (SSIS) to populate and manage a data warehouse. The schema includes a Fact Table and several Dimension Tables to support analytics and reporting for book orders, shipping, and customer data.

### Data Warehouse Schema
### Fact_Book Table
This table stores transactional data related to book orders.
![DWH Fact_Book_Sales_Table](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSIS/Fact%20Table.PNG)

# Fields:

### Dim_Shipping Table
![DWH Dim_Shipping Table](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSIS/DimShippingMethod.PNG)

### Dim_Book Table
![DWH_Dim_Book Table](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSIS/DimBooks.PNG)

### Dim_Customer Table
![DWH Dim_Customer Table](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSIS/DimCustomer.PNG)

## DimDate
Created Using SQL SERVER QUERIES
## DimStatus
![DWH DimStatus](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSIS/DimStatus.PNG)


--------------------------------------------------------------------------------
# SSAS Tabular Model for Data Warehouse
In this project, I used SQL Server Analysis Services (SSAS) in Tabular Mode to build a semantic model for the data warehouse. Below are the key steps involved:

1. **Data Model Structure: :** The data warehouse consists of a star schema with fact and dimension tables. The central fact table FactBookSales stores transactional data, while the dimension tables (DimBooks, DimCust, DimDate, DimShipping, DimStatus) provide descriptive information for different entities.

2. **Key Measures Created: :** Several important measures were developed using DAX (Data Analysis Expressions) to provide insights into the data:

- `No. of Books Sold`
- `No. of Customers`
- `No. of Orders`
- `No. of Publishers`
- `Total Due`
- `Total Freight`
- `Total Tax`
- `Total Subtotal`
- `Total Due by No. of Orders`

Below is an image showing the measures created in the Tabular Model for reference:

![DWH SSAS](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSAS/Measures%20in%20fact%20table.PNG)
![DWH SSAS](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSAS/DWH%20Model.jpeg)
![DWH SSAS](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/SSAS/Model%20With%20Measure.jpeg)

3. **Data Sources::** The data source used for this SSAS tabular model is an SQL Server database connected to the Gravity_Books_DWH dataset. 

4. **Visualization::** The Tabular Model Explorer showcases the structure of the model, relationships between tables, and measures used to provide key insights. 

By leveraging SSAS Tabular, I was able to create a fast, scalable model that enables multidimensional analysis and reporting. 

--------------------------------------------------------------------------------
# Book Sales Analysis Using Power BI

## Overview
This project analyzes book sales data to derive insights into sales performance, customer segmentation, and publisher analysis. The goal is to identify key trends and provide actionable insights for improving sales strategies.

## Data Sources
The data used in this project is sourced from SSAS Tabular Model For Gravity_Books_DWH.

## Methodology
1. **Data Cleaning**: The raw data was cleaned to handle missing values and inconsistencies.
2. **Data Processing**: The data was processed to create meaningful metrics such as total sales, number of orders, and customer segmentation.
3. **SSAS Tabular Model**: A SQL Server Analysis Services (SSAS) Tabular Model was created to facilitate advanced data analysis and provide a robust framework for generating insights.
4. **Data Visualization**: Power BI was used to create interactive dashboards and visualizations, leveraging the SSAS Tabular Model for enhanced performance and deeper insights.

## Key Insights
- **Top Authors**: Barbaba Yusash and Johannes Mirrora are among the top authors by total sales.
- **Top Publishers**: Penguin Books and Vintage are leading publishers by total sales.
- **Customer Segmentation**: The majority of customers are from Afghanistan and Albania.
- **Advanced Insights**: Utilizing the SSAS Tabular Model, we identified key trends such as seasonal sales patterns and high-performing regions.


## Power BI Reports
Below are the images of the Power BI reports created for this project:

### Sales Performance Overview
![Sales Performance Overview](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/Reports%26Dashboard/Sales%20Performance%20Overview.PNG)

### Books Analysis
![Books Analysis](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/Reports%26Dashboard/Books%20Analysis.PNG)

### Publisher Analysis
![Publisher Analysis](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/Reports%26Dashboard/Publisher%20Analysis.PNG)

### Customer Segmentation Overview
![Customer Segmentation Overview](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/Reports%26Dashboard/Customer%20Segmentation%20Overview.PNG)

## Visualizations
- **Sales Performance**: Line graphs showing sales trends over time.
- **Customer Segmentation**: Pie and graphs charts showing the distribution of customers by region.
- **Publisher Analysis**: Bar charts comparing the performance of different publishers.
- **Advanced Metrics**: Dashboards incorporating DAX calculations and time intelligence functions from the SSAS Tabular Model.

## How to Use
To replicate this analysis, follow these steps:
1. Clone the repository.
2. Install the necessary dependencies.
3. Run the data processing scripts.
4. Deploy the SSAS Tabular Model using SQL Server Data Tools (SSDT).
5. Open the Power BI file and connect it to the SSAS Tabular Model to view the visualizations.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any improvements or suggestions.
