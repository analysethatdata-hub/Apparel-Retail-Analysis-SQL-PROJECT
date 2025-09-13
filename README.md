# Apparel-Retail-Analysis-SQL-PROJECT
This is an analysis of retail data
# Apparel SQL Project

## Project Overview

**Project Title**: Apparel Sales Analysis  
**Level**: Beginner  
**Database**: `SQL PROJECT`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.


## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `SQLProjects`.
- **Table Creation**: A table named `apparel` is created to store the sales data. The table structure includes columns for ['retailer', 'retailer_ID', 'invoice_date', 'region', 'state', 'city',
       'product', 'price_per_unit', 'units_sold', 'operating_profit',
       'sales_method', 'sales', 'cost_of_sales'].

```sql
CREATE DATABASE SQLProjects;

CREATE TABLE retail_sales
(
    create table dbo.apparel
(
	id int,
	retailer   varchar(100),
	retailer_ID int,
	invoice_date  date,          
	region  nvarchar(100) ,
	state nvarchar(100), 
	city nvarchar(100), 
	product nvarchar(25), 
	price_per_unit float,
	units_sold int , 
	operating_profit float,
	sales_method nvarchar(25), 
	sales float, 
	cost_of_sales float
	)
);
```

### 2. Data Preprocessing & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Retailer Count**: Find out how many unique retailers are in the dataset.
- **Region Count**: Identify all unique regions in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.
- **State Count**: Find out how many unique states are in the dataset.
- **City Count**: Identify all unique cities in the dataset.
- **Sales Channel**: Identify all unique sales channels in the dataset.
- **Product Count**:Find out how many unique products in the dataset.


```sql
select id,count(*) as no_of_rec from apparel group by id order by id

select distinct(retailer) from apparel

select distinct(retailer_ID) from apparel

select distinct(region) from apparel

select distinct(state) from apparel

select distinct(city) from apparel

select state,count(distinct City) as Cities from apparel
group by state
order by count(distinct City) desc

select region,count(distinct City) as Cities from apparel
group by region
order by count(distinct City) desc

select distinct(sales_method) from apparel

select * from apparel 
where id is null
or    retailer is null
or    retailer_ID is null
or    invoice_date is null
or    region is null
or    state is null
or    city is null
or    product is null
or    price_per_unit is null
or    units_sold is null
or    operating_profit is null
or    sales_method is null
or    sales  is null
or    cost_of_sales is null


select id,count(*) as no_of_records
from apparel
group by id
having count(*)>1

```

### 3. Data Exploration & Findings

The following SQL queries were developed to answer specific business questions:

1. **Analysis of Sales,Cost and Gross Margin**:
```sql
select sum(sales) as TotalSales 
from apparel

select sum(cost_of_sales) as TotalCostSales
from apparel

select (sum(sales)-sum(cost_of_sales)) as Gross_Margin from apparel

select round((sum(sales)-sum(cost_of_sales))*1.0/sum(sales),2) as Gross_Margin_perc from apparel
```

2. **SQL query to calculate Yoy % Change**:
```sql

with tsales as(
select year(invoice_date) as years, sum(sales) as TotalSales
from apparel
group by year(invoice_date)
)
, prev_sales as(
select *,
lag(TotalSales,1,TotalSales) over(order by years) as prev_sales
from tsales
)
select *,
(TotalSales-prev_sales)*1.0/prev_sales as YoY
from prev_sales
```

3. **SQL query to calculate MOM change**:
```sql

with tsales as(
select year(invoice_date) as years,month(invoice_date) as months, sum(sales) as TotalSales
from apparel
group by year(invoice_date),month(invoice_date)
)
, prev_sales as(
select *,
lag(TotalSales,1,TotalSales) over(order by years,months) as prev_sales
from tsales
)
select *,
(TotalSales-prev_sales)*1.0/prev_sales as MoM
from prev_sales

```

4. **Product Sales by value**:
```sql

select product,sum(sales) as product_sales
from apparel
group by product
order by product_sales desc

```

5. **Retailer Sales by Value**:
```sql

select retailer,sum(sales) as retailer_sales
from apparel
group by retailer
order by retailer_sales desc

```

6. **Region Sales by value**:
```sql

select region,sum(sales) as regional_sales
from apparel
group by region
order by regional_sales desc

```

7. **State Sales by value**:
```sql

select state,sum(sales) as state_sales
from apparel
group by state
order by state_sales desc

```

8. **City Sales by value**:
```sql

select city,sum(sales) as city_sales
from apparel
group by city
order by city_sales desc

```

9. **State/Region/City Sales by value**:
```sql

select state as states,region as region,city as city,sum(sales) as state_region_city_sales
from apparel
group by state,region,city
order by state_region_city_sales desc

```

10. **Sales Method Sales by Value**:
```sql

select sales_method,sum(sales) as sales_method_sales
from apparel
group by sales_method
order by sales_method_sales desc

```

11. **Total Quantities Sold**:
```sql

select sum(units_sold) as TotalQuantitySold
from apparel

```

12. **Total Quantities Sold by Year**:
```sql

select year(invoice_date) as years,sum(units_sold) as TotalQuantitySold
from apparel
group by year(invoice_date)

```

13. **Year over Year Analysis**:
```sql

with tquants as(
select year(invoice_date) as years, sum(units_sold) as TotalQuants
from apparel
group by year(invoice_date)
)
, prev_sales as(
select *,
lag(TotalQuants,1,TotalQuants) over(order by years) as prev_quants
from tquants
)
select *,
(TotalQuants-prev_quants)*1.0/prev_quants as YoY
from prev_sales

```

14. **Total Quantities Sold Product**:
```sql

select product,sum(units_sold) as TotalQuaPro
from apparel
group by product
order by TotalQuaPro desc

```

15. **Total Quantities Sold Region**:
```sql

select region,sum(units_sold) as TotalQuaReg
from apparel
group by region
order by TotalQuaReg desc

```

16. **Total Quantities Sold by State**:
```sql

select state,sum(units_sold) as TotalQuaState
from apparel
group by state
order by TotalQuaState desc
```

17. **Total Quantities Sold by City**:
```sql

select city,sum(units_sold) as TotalQuaCity
from apparel
group by city

```

18. **Total Quantities Sold by State/Region/City**:
```sql

select state,region,city,sum(units_sold) as TotalStateRegQua
from apparel
group by state,region,city
order by TotalStateRegQua desc

```

### Findings

- **Sales Trends**: Monthly analysis shows variations a lot of variation in sales.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.
- **Seasonality**:None was identified due to the variations in the monthly analysis.

## Conclusion

This project serves as a introduction to SQL for data analysts, covering database setup, data cleaning and exploratory data analysis. 

## Author - analysethatdata
