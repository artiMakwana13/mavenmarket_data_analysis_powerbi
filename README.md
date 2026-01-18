# Maven Market Power BI Dashboard  


## Overview  
Maven Market, a multinational grocery chain operating across Canada, Mexico, and the United States, lacks a unified, data-driven view of its sales, profitability, and operational performance. Due to fragmented data across regions and limited analytical insights, management struggles to identify key revenue drivers, underperforming products, and market trends, making it difficult to optimize decision-making and improve overall business performance. So this interactive Power BI dashboard analyze transactions, revenue, profit, return rates, performance by country, and brand-level metrics along with weekly revenue trends and target tracking.

---

## Project Steps 

## Tools & Technologies  

| Tool | Purpose |
|-----------|-------------|
| **Power BI Desktop** | Dashboard design and analysis |
| **Power Query Editor** | Data transformation and cleaning |
| **DAX** | Calculations and business metrics |

### Data Loading  
- Imported all csv files into **PowerBI Desktop**.  

### Data Cleaning  
Data preparation was performed using Power Query with the following steps:
- Removed duplicates and missing values.  
- Standardized all date fields to `M/d/yyyy`.  
- Created new calculated columns such as:
  - `Weekend` – marks Saturdays and Sundays as "Y".  
  - `End of Month` – using `EOMONTH()`.  
  - `Current Age` – using `DATEDIFF([Birthdate], TODAY(), YEAR)`.  
  - `Short_Country` – using `UPPER(LEFT([Country],3))`.  
  - `Price_Tier` – categorized prices into High, Mid, and Low tiers.  
- Applied correct data types and ensured consistency across all tables.  

---

### Data Modeling  

#### Schema Design
A Star Schema was implemented with:
- **Fact Tables:** Transaction_Data, Returns_1997_1998  
- **Dimension Tables:** Customers, Products, Stores, Regions, Calendar  
- **Snowflake Relationship:** Stores → Regions  

#### Relationships
| From Table | Column | To Table | Column |
|-------------|----------|----------|---------|
| Calendar | date | Transaction_Data | transaction_date |
| Products | product_id | Transaction_Data | product_id |
| Stores | store_id | Transaction_Data | store_id |
| Customers | customer_id | Transaction_Data | customer_id |'
| Store | store_id | Returns_1997_1998 | store_id|
| Region | region_id | store | region_id |

---

#### Key DAX Measures  
- Total_Revenue = SUMX(Transaction_Data, Transaction_Data[quantity] * RELATED(Products[product_retail_price]))
- Total_Cost = SUMX(Transaction_Data, Transaction_Data[quantity] * RELATED(Products[product_cost]))
- Total_Profit = [Total_Revenue] - [Total_Cost]
- Profit_margin = DIVIDE([Total_Profit], [Total_Revenue])
- Return_Rate = [Total_Quantity_Returned] / [Total_Quantity_Sold]
- Last_Month_Revenue = CALCULATE([Total_Revenue], DATEADD('Calendar'[date],-1,MONTH))
- Revenue_Target = [Last_Month_Revenue] * 1.05
- YTD Revenue = YTD_Revenue = CALCULATE([Total_Revenue], DATESYTD('Calendar'[date])) 
- Weekend Transactions = CALCULATE([Total Transactions], 'Calendar'[Weekend] = "Y")
- % Weekend Transactions = DIVIDE([Weekend Transactions], [Total Transactions])

---

### Dashboard Development (Power BI)  
- Designed an interactive dashboard highlighting:
  - Map: Total Revenue by Country
  - Treemap: Transactions by Store Hierarchy (Country → State → City)
  - Matrix: Product brand level insights(Transactions, Profit Margin, Return Rate)
  - Column Chart: Weekly Revenue Trending (1998 only)
  - Gauge Chart: Revenue vs Target
  - KPI Cards: Transactions, Profit, Returns Rate

## 📈 Results & Insights  
- Identified **key business insights**, such as:
  - USA and Mexico generate 90 % of total revenue while canada has lower sale volume 
  - Hermanos, Ebony, Tell Tale lead in Profit and sales volumme and has lowest return rates (~0.8–1 %)
  - Plato, BBB Best, Cormorant has highest profit margin with 63.55%, 62.12%, 61.6% respectively
  - Low Return Rate shows good product reliability and customer satisfaction
  - Revenue vs Target: $119.5K vs $120.2K (≈ -0.6% gap). 
  - Horatio and Nationeel shows higher return rates (> 1.1 %) indicate potential quality issue
  - Weekly revenue ranges between 0k - 40k
  - Total Transaction: 113.66K

---

![Power BI Dashboard Preview](mavenmarket_reports.pdf)  
  
---
