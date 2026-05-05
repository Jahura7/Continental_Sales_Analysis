# Continental_Sales_Analysis
This is a Power Bi Dashboard showes the continent wise sales analysis of garment product
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
  - [Area-wise Total Sales](#area-wise-total-sales)
  - [Brand-wise Sales](#brand-wise-sales)
  - [Brand-wise Milk Category Sales](#brand-wise-milk-catagory-product-sales)
  - [Milk Category Sales Trend](#milk-catagory-product-sales-trend)
  - [December 2024 Sales & January 2025 Forecast](#sales-of-december-2024-and-forecasting-of-january-2025)
- [Dataset](#dataset)
- [Acknowledgements](#acknowledgements)

### Project Overview
This Power BI report delivers a comprehensive, continent-wise view of sales performance from 2019 to 2021. The underlying data originates from an ERP system export and encompasses dealer-level transactions across multiple countries, regions, product categories, and customer segments.
Business Questions Answered:

What are the Total Order Quantity, Total Revenue, Total Profit, and Profit Margin?
Which continent generates the highest Revenue and Profit?
What are the year-over-year Revenue and Profit differences for the period 2019–2021?
How does customer diversity impact Total Revenue?
Which are the Top 5 Products by Revenue?
How does Product Style influence country-wise Revenue?
What is the Total Revenue breakdown by country?
What does the Monthly Revenue and Profit trend look like over the report period?


### Tools & Technologies
* Excel
* Power Query
* Power BI

### Dataset Overview
Columns:
sales_date, sales_type_1_counter_sales, region_name, area_name, teritorry_name, dealer_id, db_erp_id, sr_name, route_id, route, outlet_id, outlet_name, category_name, brand_name, disc_tp, sales_qty_pcs, sales_in_ton, sales_dp

### Sample Preview
<img width="1832" height="685" alt="image" src="https://github.com/user-attachments/assets/20035770-fd0c-4f70-9008-af69166a7930" />

### Data Cleaning Process (Power Query)
    - Promoted Headers
    - Changed data Type 
	- Removed column that are not required for our analysis
	
<img width="1915" height="997" alt="image" src="https://github.com/user-attachments/assets/9d6223a3-4f23-4c5a-a0ac-381b5e098546" />

	
### Create "Calender Table 
1. Calender
 ``` dax
Calender = CALENDAR(MIN(Retail_Sales_Dataset[sales_date]), MAX(Retail_Sales_Dataset[sales_dp]))
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
<img width="1917" height="975" alt="image" src="https://github.com/user-attachments/assets/f7452da9-3055-42b7-b931-fde4c1f52f66" />

 ### Key Measures
 
 1. Total Sales
 ``` dax
total_sales = SUM(Retail_Sales_Dataset[sales_dp])
```

2. Total Sales in Ton
 ```dax
total_sales_in_ton = SUM(Retail_Sales_Dataset[sales_in_ton])
```

3. Average Price per Ton
```dax
average_price_per_ton = DIVIDE(
        SUM(Retail_Sales_Dataset[sales_dp]),
        SUM(Retail_Sales_Dataset[sales_in_ton]),
        0
    )
```    

### Data Modeling
<img width="1910" height="946" alt="image" src="https://github.com/user-attachments/assets/8350e911-da4b-4822-8bf4-466841f2b48b" />

The model has a classic star schema structure — a fact table and a date dimension linked by a single relationship.

-Retail_Sales_Dataset (Fact table — the * side)
-Calender (Dimension table — the 1 side)

The relationship: Many-to-one (* → 1)

Retail_Sales_Dataset[sales_date] → Calender[Date]

This means many transaction rows can share the same date, but each date in the calendar table is unique. This is the standard way to unlock time intelligence in Power BI — once this link exists, all visuals can be filtered and grouped by Year, Month, or Day from the Calender table, and the day-wise trend charts with the January 2025 forecast become possible.


### Power BI Dashboard
The Power BI dashboard inclues the fpllowing visuals:
- Cards for Total Sales, Total Sales in Ton and Average Price per Ton
- Clastered bar chart showing teritorry wise sales
- Clastered bar chart showing brand wise sales
- Donut chart showing brand wise milk category sales
- Day wise Milk catagory sales Trend in Line chart
- Day wise Sales Trends along with Forecasting of Jan'25 (Line Chart)
- Interactive slicer of Area, Territory, Category and Brand
 
<img width="1430" height="748" alt="image" src="https://github.com/user-attachments/assets/9c36667e-dd2f-4388-bcd8-5d9eb4093c01" />

 
 ### Key Insights
### Area wise Total Sales
------------------------------------
<img width="1222" height="706" alt="image" src="https://github.com/user-attachments/assets/07458364-f48d-4980-a793-1356c913bf9b" />

- Shantinagar dominates at 130M — 44% of the 298M total, and 53% more than the next area. It's carrying nearly half the business on its own.
- Cumilla and Chittagong are near-equal at 85M and 83M respectively — a 2M gap that's essentially parity. Both sit at roughly 28% share each, forming a balanced second tier.
- The top-to-bottom gap is steep — Shantinagar generates 57% more than Chittagong. If this were a healthier distribution, you'd expect the top area to lead by 15–25%, not 57%.
- Concentration risk — with 3 areas covering all revenue, losing traction in Shantinagar alone would wipe out ~44% of sales. No area acts as a safety buffer at comparable scale.
 
### Brand wise Sales:
-------------------------- 
<img width="1222" height="703" alt="image" src="https://github.com/user-attachments/assets/701bd310-5bbe-4d9a-b80e-65f779a2486f" />

#### Dominant Brand:
DIPLOMA is overwhelmingly the leader with 181.13M sales, accounting for 60.76% of the total.
	This indicates a highly concentrated market where one brand drives the majority of revenue.
#### Secondary Players:
HAPPY COW (31.05M, 10.42%) and POPPERS (13.6M, 4.56%) are the next strongest contributors.
Together with DIPLOMA, the top three brands make up ~76% of total sales.
#### Mid-tier Brands:
Brands like BELLEAME-HD, DETOS, BELLEAME-SD, DOODLES INSTANT, FARMLAND each contribute 2–3%.
Collectively, these mid-tier brands form about 14% of the market.
#### Small Brands:
The remaining brands (e.g., RC BUTTER OIL, DOODLES STICK, FARMLAND ATTA, etc.) each contribute <2%.
Their combined share is relatively small, but they diversify the portfolio.

 ### Brand wise Milk Catagory Product Sales:
 --------------------------------------------
 <img width="1220" height="702" alt="image" src="https://github.com/user-attachments/assets/dc0daf6a-136c-4d93-80fc-98d616b5e9ad" />

Four quick takeaways from this chart:

- DIPLOMA is essentially the MILK category. At 82.4% (181M), it's not a market leader — it's the market. No other brand comes close, which raises a real single-point-of-failure risk.
- HAPPY COW is a distant but meaningful second. At 14.1% (31M), it's the only brand with enough presence to cushion any DIPLOMA shortfall. Growing it strategically would reduce portfolio concentration.
- FARMLAND at 3.2% is a structural gap — it has name recognition (it appears in the legend) but nearly no sales traction. It's either underranged across outlets or has weak demand pull in this region.
- RED COW, CALCI-PRO, and SHAPE-UP are effectively absent — three SKUs together contribute less than 0.3M, which suggests they're listed but not actively pushed by SRs or pulled by outlets. Worth reviewing whether these remain worth distributing or should be rationalised.
 
 ### Milk Catagory Product Sales Trend:
 -------------------------------------- 
  <img width="1224" height="702" alt="image" src="https://github.com/user-attachments/assets/c5465caf-ba88-4852-b96f-549637893f4c" />
  
 - While the overall trend is stable, the extreme lows distort the average. Removing anomalies, the true daily average is ~7.5M.
 - The Day 29 (8.9M) and Day 31 (13.1M) peaks suggest strong month-end buying behavior.
 - The near-zero days are red flags. If these are stockouts, they represent lost revenue opportunities of ~7–8M per day.
 

### Sales of December, 2024 and forecasting of january, 2025:
---------------------------------------------------------------
<img width="1222" height="705" alt="image" src="https://github.com/user-attachments/assets/2aca2a5c-c252-4c5a-926c-74f145812adb" />

#### Recent Performance (Dec 2024):
- Sales fluctuated between 7.4M–11.6M, with some days showing 0M (likely stockouts or reporting gaps).
- A notable high point was Dec 22 (11.47M), showing strong demand before the holiday period.
#### Forecast for Jan 2025:
- The forecast line projects growth, with a peak of 16.8M expected.
- This is significantly higher than December’s average, suggesting positive momentum heading into the new year.
#### Trend Line Analysis:
- The linear trend (dashed black line) shows a steady upward trajectory. 
- The average benchmark (blue dashed line) sits around 8–9M, meaning forecasted highs are nearly double the baseline.

  
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

 

