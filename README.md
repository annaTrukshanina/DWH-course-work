# DWH-course-work

# Online Store Database & ETL Pipeline

## Overview
This project implements a relational database schema for an online store specializing in medical equipment. It includes an ETL (Extract, Transform, Load) pipeline for data ingestion and a data warehouse schema for analytical processing. The project also integrates Power BI for visualization and business intelligence insights.

## Database Schema
The database consists of **nine tables**, each serving a specific purpose for managing products, users, orders, and related information.

### Entity-Relationship (ER) Diagram
*(Insert ER diagram image here if available)*

### Tables Description
#### 1. **CountriesOfOrigin**
- Stores the countries from which products originate.
- **Columns:** `CountryID` (Primary Key), `CountryName` (Unique, Not Null).

#### 2. **Categories**
- Stores product categories.
- **Columns:** `CategoryID` (Primary Key), `CategoryName` (Unique, Not Null).

#### 3. **Brands**
- Stores brands associated with products.
- **Columns:** `BrandID` (Primary Key), `BrandName` (Unique, Not Null).

#### 4. **Manufacturers**
- Stores manufacturers and their country of origin.
- **Columns:** `ManufacturerID` (Primary Key), `ManufacturerName` (Unique, Not Null), `CountryID` (Foreign Key).

#### 5. **Products**
- Stores product details including brand, category, manufacturer, and price.
- **Columns:** `ProductID` (Primary Key), `ProductName`, `CategoryID`, `BrandID`, `ManufacturerID`, `Price`, `AvailabilityStatus` (Boolean), `unique_product` (Unique Constraint).

#### 6. **Users**
- Stores user account information.
- **Columns:** `UserID` (Primary Key), `username`, `email` (Unique, Not Null), `address`, `phone`.

#### 7. **Orders**
- Stores details about orders placed by users.
- **Columns:** `OrderID` (Primary Key), `UserID` (Foreign Key), `OrderDate` (Timestamp), `Status`, `TotalCost`.

#### 8. **OrderDetails**
- Stores products included in each order.
- **Columns:** `OrderDetailID` (Primary Key), `OrderID` (Foreign Key), `ProductID` (Foreign Key), `Quantity`, `UnitPrice`.

#### 9. **ProductRatings**
- Stores user ratings for products.
- **Columns:** `RatingID` (Primary Key), `ProductID` (Foreign Key), `UserID` (Foreign Key), `Rating` (1-5 scale).

## ETL Process
The **ETL (Extract, Transform, Load) pipeline** imports order data from CSV files into the database and ensures data consistency. It consists of two ETL scripts:

### **ETL1: Data Import from CSV Files**
1. **Temporary Tables**: Staging tables (`TempOrders`, `TempOrderDetails`, `TempProductRatings`) are created.
2. **Data Import**: CSV files (`orders.csv`, `order_details.csv`, `product_ratings.csv`) are loaded into staging tables.
3. **Data Transformation & Loading**:
   - Deduplication and insertion into main tables (`Categories`, `Brands`, `CountriesOfOrigin`, `Manufacturers`, `Users`, `Products`, `Orders`, `OrderDetails`, `ProductRatings`).
   - Computation of `TotalCost` for each order.

### **ETL2: Data Warehouse Loading**
1. **Dimension Tables**:
   - `DimCategories`, `DimCountries`, `DimBrands`, `DimManufacturers`, `DimUsers`, `DimDate`, `DimProducts`.
2. **Fact Tables**:
   - `Sales_Fact` (stores transactional sales data).
   - `ProductRatings_Fact` (stores product ratings).
3. **Slowly Changing Dimensions (SCD2)**:
   - Implements history tracking for `DimProducts` using triggers.

## Data Warehouse Schema
A separate **data warehouse schema** (`datawarehouse`) is created for analytical reporting.

### **Dimension Tables**
- `DimCategories`, `DimCountries`, `DimBrands`, `DimManufacturers`, `DimUsers`, `DimDate`, `DimProducts`.

### **Fact Tables**
- `Sales_Fact`: Stores transactional sales data.
- `ProductRatings_Fact`: Stores product ratings.

### **Views for Reporting**
- `sales_trend`: Total sales over time.
- `sales_by_user`: Total sales by each user.
- `sales_by_country_of_origin`: Sales by country.
- `sales_by_category`: Sales by category.
- `sales_by_brand`: Sales by brand.
- `average_rating_by_product`: Average product rating.
- `average_rating_by_brand`: Average brand rating.

## Power BI Report
A **Power BI dashboard** is created using data from the `datawarehouse` schema.

### **Sales Analysis Dashboards**
- **Sales by Brand** (Bar chart)
- **Sales Trend** (Area chart over time)
- **Sales by Category** (Pie chart)
- **Sales by User** (Bar chart by user)

### **Rating Analysis Dashboards**
- **Average Rating by Brand** (Bar chart)
- **Average Rating by Product** (Bar chart)

## How to Use
1. **Database Setup**:
   - Execute `data_warehouse.sql` to create the schema.
   - Load sample data using the ETL scripts.
2. **ETL Execution**:
   - Run `ETL1_csv.sql` for operational database import.
   - Run `ETL2_data_warehouse.sql` for data warehouse import.
3. **Power BI Report**:
   - Load data from `datawarehouse` into Power BI.
   - Apply filters (date, brand, user, category) for analysis.

## Technologies Used
- **Database**: PostgreSQL
- **ETL**: SQL scripts
- **Data Warehouse**: Star schema with Slowly Changing Dimensions (SCD2)
- **Visualization**: Power BI

## Author
*(Your Name or Team Name)*
