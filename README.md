# Continental_Sales_Analysis
An end-to-end sales analytics solution built in Power BI — covering data ingestion, cleaning, modeling, DAX measures, and an interactive dashboard spanning continental revenue, profit trends, customer diversity, and product performance from 2019 to 2021.
| Sales Data | From Raw Data to Business Insights|

## Table of Contents
- [Project Overview](#project-overview)
- [Tools & Technologies](#tools--technologies)
- [Dataset Overview](#dataset-overview)
- [Sample Preview](#sample-preview)
- [Data Cleaning Process](#data-cleaning-process-power-query)
- [Create Calendar Table](#create-calender-table)
- [Key Measures](#key-measures)
- [Data Modeling](#data-modeling)
- [Power BI Dashboard](#power-bi-dashboard)
- [Key Insights](#key-insights)
- [Dataset](#dataset)
- [Acknowledgements](#acknowledgements)

### Project Overview
The Continental Sales Analysis dashboard provides a unified, interactive view of global sales performance across three continents and six countries from 2019 to 2021. The project follows a complete BI pipeline:

Raw CSV Data → Power Query Cleaning → Star Schema Modeling → DAX Measures → Interactive Dashboard

Business Questions Answered:
- What are the Total Order Quantity, Total Revenue, Total Profit, and Profit Margin?
- Which continent generates the highest Revenue and Profit?
- What are the year-over-year Revenue and Profit differences for the period 2019–2021?
- How does customer diversity impact Total Revenue?
- Which are the Top 5 Products by Revenue?
- How does Product Style influence country-wise Revenue?
- What is the Total Revenue breakdown by country?
- What does the Monthly Revenue and Profit trend look like over the report period?


### Tools & Technologies
* Excel
* Power Query
* Power BI
* Git

### Dataset Overview
- #### "Sales_19-21(1)" Dataset Columns:
OrderDate, StockDate, OrderNumber, ProductKey,CustomerKey, teritorryKey, OrderLineItem, OrderQuantity 
#### Sample Preview
<img width="1231" height="237" alt="image" src="https://github.com/user-attachments/assets/72aa7783-800a-405f-bcd0-4683fe900ef4" />

- #### "Customer" Dataset Columns:
CustomerKey, Perfix, FirstName, LastName, BirthDate, MaritalStatus, Gender, EmailAddress, AnnualIncome, TotalChildren, EducationLevel, Occupation, HomeOwner 
#### Sample Preview 
<img width="1772" height="232" alt="image" src="https://github.com/user-attachments/assets/bef4bd2b-12f4-4a97-99ef-ba7ee235332a" />

- #### "Product" Dataset Columns:
ProductKey, ProductSubcatagoryKey, ProductSKU, ProductName, ModelName, ProductDescription, ProductColor, ProductSize, ProductStyle, ProductCost, ProductPrice  
#### Sample Preview 
<img width="1812" height="317" alt="image" src="https://github.com/user-attachments/assets/17ae7f17-4819-462b-beeb-7b656bbc93ee" />

- #### "Territories" Dataset Columns:
SalesTerritoryKey, Region, Country, Continent 
#### Sample Preview
<img width="507" height="287" alt="image" src="https://github.com/user-attachments/assets/6105ada5-f116-4269-903f-85b9de596f7f" />

### Data Cleaning Process (Power Query)
    - Promoted Headers
    - Changed data Type 
	- Replaced value
	
<img width="1891" height="533" alt="image" src="https://github.com/user-attachments/assets/371ba528-bcb3-419e-b018-02ca3ea93031" />
	
### Create "Calender Table 
1. Calender
 ``` dax
Calender = CALENDAR(MIN('Sales_19-21 (1)'[OrderDate]), MAX('Sales_19-21 (1)'[OrderDate]))
```
2. "Year" Column
 ``` dax
Year = YEAR(Calender[Date])
```
3. "Month" Column
 ``` dax
Month = FORMAT(Calender[Date],"MMM")
```
3. "Day" Column
 ``` dax
Day = FORMAT(Calender[Date],"DDD")
```
<img width="896" height="382" alt="image" src="https://github.com/user-attachments/assets/19403333-89d7-46bd-9692-168afebcfee6" />


 ### Key Measures
 
 1. Unit Price
 ``` dax
Unite Price = RELATED(Products[ProductPrice])
```

2. Revenue
 ```dax
Revenue = 'Sales_19-21 (1)'[OrderQuantity] * 'Sales_19-21 (1)'[Unite Price]
```

3. Profit
```dax
Profit = 'Sales_19-21 (1)'[Revenue] - RELATED(Products[ProductCost])
```

3.  % Profit Margin
```dax
% Profit Margin = (SUM('Sales_19-21 (1)'[Profit])/(SUM('Sales_19-21 (1)'[Revenue]))* 100)
```  

### Data Modeling
<img width="1512" height="738" alt="image" src="https://github.com/user-attachments/assets/6c82a484-d075-4d06-9923-cbab5569c60b" />

This is a Star Schema data model — the most common and recommended structure in Power BI. The Sales_19-21(1) table sits at the center as the fact table, surrounded by four dimension tables: Customers, Products, Territories, and Calendar.

Relationships
All relationships are one-to-many (1 to *), meaning one dimension row can relate to many fact rows — standard for star schemas:

- Customers[CustomerKey] → Sales_19-21(1)[CustomerKey]
- Products[ProductKey] → Sales_19-21(1)[ProductKey]
- Territories[SalesTerritoryKey] → Sales_19-21(1)[SalesTerritoryKey]
- Calendar[Date] → Sales_19-21(1)[OrderDate]

The arrow direction on the connector lines indicates the cross-filter direction — filters flow from the dimension (the 1 side) into the fact table (the * side).
 
## Power BI Dashboard 

 <img width="1120" height="742" alt="image" src="https://github.com/user-attachments/assets/9a67c8c2-84c0-4ecb-876d-1a0633454feb" />

 
### The Power BI dashboard inclues the following visuals:

| Visual | Chart Type | Purpose|
|--------|----------|----------|
| KPI Cards |Card | Total Orders, Revenue, Profit, Profit Margin |
| Revenue & Profit by Continent | Clustered Bar Chart | Continent-level performance comparison |
| Revenue vs Profit by Year |Clustered Column Chart | Year-over-year trend (2019–2021) |
| Diversity Impact on Sales |Clustered Column Chart | Gender-based revenue segmentation |
| Top 5 Products by Revenue |Clustered Bar Chart |Best-performing product ranking |
| Revenue by Country & Product Style |Clustered Column Chart | Country × product style cross-analysis |
| Revenue by Country | Treemap | Proportional geographic revenue view |
| Monthly Revenue & Profit Trend | Area + Line |Time-series trend tracking |

### Key Metrics (KPIs)

| Metric | Value |
|--------|-------|
| Total Order Quantity | 84K |
| Total Revenue | 24.91M |
| Total Profit | 10.58M |
| % Profit Margin | 42.47 |

All KPI cards update dynamically based on active slicer selections.

### Slicers / Filters

| Slicer | Options |
|--------|---------|
| Continent | All / North America / Europe / Pacific | 
| Country / Region | Cascades from Continent selection |
| Year | 2019 / 2020 / 2021 | 
| Month | January – December |
| Quarter | Q1 / Q2 / Q3 / Q4 |


##  Key Insights
#### Revenue by Continent

- North America leads all continents with $9.7M revenue and $4.2M profit.
- Europe follows with $7.8M revenue and $3.3M profit.
- Pacific contributes $7.4M revenue and $3.1M profit — competitive with Europe.
- Profit margins appear consistent (~43–44%) across all three continents.

#### Revenue vs Profit by Year

- Revenue grew from $6.4M (2019) → $9.3M (2020) → $9.2M (2021) — a 44% rise from 2019 to 2020.
- Profit followed the same trend: $2.6M → $4.0M → $4.0M.
- Growth plateaued in 2021 relative to 2020, suggesting market saturation or seasonal factors.

#### Top 5 Products by Revenue

All top 5 products are variants of the Mountain-200 series:

| Rank | Product | Revenue |
|------|----------|--------|
| 1 | Mountain-200 Black, 46 | $1.24M |
| 2 | Mountain-200 Black, 42 | $1.23M |
| 3 | Mountain-200 Silver, 38 | $1.21M |
| 4 |Mountain-200 Silver, 46| $1.18M |
| 5 |Mountain-200 Black, 38 |$1.17M |

The Mountain-200 line dominates revenue — a strong indicator for inventory prioritization and marketing focus.

#### Revenue by Country

- United States is the top market: $7.94M
- Australia is a strong second: $7.42M
- United Kingdom: $2.52M | France: $2.36M | Germany: $2.1M | Canada: $1.77M

The US and Australia together account for ~62% of total revenue.

 #### Diversity Impact on Sales

- Female customers generate $12.5M in revenue vs Male customers at $12.2M — nearly equal split.
- A minor NA segment ($0.2M) indicates some records lack gender classification.
- Gender parity in purchasing behavior suggests marketing strategies need not be gender-skewed.

#### Revenue by Country and Product Style

- United States shows the highest volume across all product styles (O, U, W), with style O leading at $6.5M.
- Australia mirrors US patterns with significant style O purchases ($6.3M).
- Other markets (UK, Germany, France, Canada) show more distributed style preferences with lower individual volumes.

#### Monthly Revenue and Profit Trend

- Revenue shows a clear upward trend: starting at $1.3M (Sep) and climbing to $3.0M (Jun).
- Profit holds steady between $0.5M–$1.3M monthly throughout the period.
- The Sep–Nov period shows the slowest revenue growth, while Feb–Jun accelerates significantly — suggesting a seasonal peak in the first half of the calendar year.
  
 ## Dataset

- **Source**: Provided by [Ostad.app](https://ostad.app) as part of a 
  structured Skill Development Course in Data & Analytics
- **Provided By**: Ostad — Bangladesh's leading tech skill development platform
- **Context**: This dataset was supplied as course material for hands-on 
  practice in HR data analysis, dashboard design, and business intelligence 
  reporting 


## Acknowledgements

This project was completed as part of a **Data & Analytics Skill 
Development Course** on [Ostad.app](https://ostad.app) — Bangladesh's 
leading platform for tech and professional skill development.

Special thanks to the Ostad course instructors for providing the 
HR dataset and structured learning path that made this project possible.

 

