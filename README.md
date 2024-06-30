# Online sales
This project contains online sales data that can be used to analyze sales trends and performance across different regions. 
The CSV file includes information about the sales of individual products in various countries.

**Columns:**
  - **Order ID:** Unique identifier for each sales order.
  - **Date:** Date of the sales transaction.
  - **Category:** Broad category of the product sold (e.g., Electronics, Home Appliances, Clothing, Books, Beauty Products, Sports).
  - **Product Name:** Specific name or model of the product sold.
  - **Quantity:** Number of units of the product sold in the transaction.
  - **Unit Price:** Price of one unit of the product.
  - **Total Price:** Total revenue generated from the sales transaction (Quantity * Unit Price).
  - **Region:** Geographic region where the transaction occurred (e.g., North America, Europe, Asia).
  - **Payment Method:** Method used for payment (e.g., Credit Card, PayPal, Debit Card)

## Analysis Plan
In this project, we will:
1. Analyze sales trends over time.
2. Examine the popularity of different product categories by region.
3. Identify the most popular payment methods.
4. Determine which products sell best in each category to tailor marketing campaigns.

## Initial Queries

After importing the data into MySQL, we proceed to check how sales look for each region and individual product, as well as the preferred payment method in each country.

1. **Sales by Region**
```sql
SELECT Region, COUNT(*) AS Result 
FROM online_sales_data 
GROUP BY Region;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/3c9ff300-b8d6-476f-ab45-7e5d1a61a23a)

2. **Sales by Product Category**
```sql
SELECT Category, COUNT(*) AS Result 
FROM online_sales_data 
GROUP BY Category;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/9832882f-a287-4ef5-8ae0-bee952046e8b)

3. **Preferred Payment Methods by Region**
```sql
SELECT Region, Payment Method, COUNT(*) AS Popular_Method 
FROM online_sales_data  
GROUP BY Region, Payment Method 
ORDER BY Popular_Method;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/8f3cc486-80cb-4fe9-b206-c29caeb0fbf1)

## Key Insights

1. **Uniform Geographic Sales**: Effective marketing and distribution strategies are indicated by uniform sales across regions.
2. **Balanced Sales**: Even demand for specific products.
3. **Preferred Payment Methods**: 
    - Credit card: 50% of transactions.
    - PayPal: Second most popular.
    - Debit card: Third place.

Regional Preferences:
- Asia: Credit card and debit card.
- Europe: PayPal.
- North America: Credit card.

## Detailed Expenditures by Region and Category

1. **Top Categories by Region**
```sql
SELECT Product Category, Region, SUM(Units Sold) AS TheBest_Category 
FROM online_sales_data  
GROUP BY Product Category, Region 
ORDER BY TheBest_Category DESC;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/cc672f4f-1962-4783-bbc8-43f5ac8a8c8c)

2. **Total Revenue by Category and Region**
```sql
SELECT Product Category, Region, ROUND(SUM(Total Revenue), 2) AS Total_Revenue 
FROM online_sales_data  
GROUP BY Product Category, Region 
ORDER BY Total_Revenue DESC;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/66dcd61b-8a7b-4ec2-b1b0-49fc65788864)

## Regional Spending Highlights

- **Asia**: Highest purchases in 'Clothing and Sports'.
- **Europe**: Leads in 'Home Appliances and Beauty Products'.
- **North America**: Dominates in 'Books and Electronics'.

**North America** spent 34,982.41 PLN on electronics alone.

**Europe** spent 18,646.16 PLN on cosmetic products.

**Asia** spent 14,326.52 PLN on purchases in the Sports category.


## High-Value Transactions

1. **Top Spending by Product and Region**
```sql
SELECT Product Name, Region, ROUND(SUM(Unit Price), 2) AS Top_Unit_Price 
FROM online_sales_data  
GROUP BY Product Name, Region 
ORDER BY Top_Unit_Price DESC 
LIMIT 10;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/f125156d-3722-474e-8925-d60755c9c9c6)

2. **Top Quantity Sold by Product and Region**
```sql
SELECT Product Name, Region, SUM(Units Sold) AS Top_Unit_Sold 
FROM online_sales_data  
GROUP BY Product Name, Region 
ORDER BY Top_Unit_Sold DESC 
LIMIT 10;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/27d47909-026b-4943-92ae-4fb568f0ac08)

Observations:
- Asia buys the largest quantities.
- North America leads in high-value transactions (80% of top 10).

## Total Revenue by Region
```sql
SELECT Region, ROUND(SUM(Total Revenue), 2) AS Total_Revenue 
FROM online_sales_data 
GROUP BY Region 
ORDER BY Total_Revenue DESC;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/30b4e57c-d8ff-4ea5-b1e9-2edb58fe066e)

**Throughout the entire period, North America generated the highest income.**

- *I visualized revenues over all available days using Power BI.*

![obraz](https://github.com/biku89/Online-sales/assets/169537978/ff826b94-314f-4afd-90c5-d319da684158)

**Visualization Insights**:
- Highest revenue peaks: Early January, February, April, and August 2024.
- Dynamic sales pattern: Seasonal trends or promotions.


## Sales Patterns by Day and Month

1. **Daily Sales Trends**
```sql
SELECT  
    CASE WHEN DAYOFWEEK(Date) = 1 THEN 7 ELSE DAYOFWEEK(Date) - 1 END AS day_of_week_number, 
    DAYNAME(Date) AS day_of_week,  
    ROUND(SUM(Total Price), 2) AS Total_Revenue  
FROM online_sales_data 
GROUP BY  day_of_week_number, day_of_week 
ORDER BY  day_of_week_number;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/c3004dab-9efc-4a94-92a7-69750da8d85e)

2. **Monthly Sales Trends**
```sql
SELECT  
    DATE_FORMAT(Date, '%Y-%m') AS Month, 
    ROUND(SUM(Total Price), 2) AS Total_Revenue 
FROM online_sales_data 
GROUP BY  DATE_FORMAT(Date, '%Y-%m') 
ORDER BY  DATE_FORMAT(Date, '%Y-%m');
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/895a2b40-8b66-4ccf-ad56-dadaca211a5f)

**Observations**:
- Stable sales throughout the week, except Thursday.
- Most profitable day: Tuesday (possible special promotions).
- Increased sales on weekends.
- Increasing monthly revenue in 2024, highest in January, lowest in August.

## Correlation Between Unit Price and Total Revenue

```sql
WITH sums_and_counts AS (
    SELECT  
        Region, 
        COUNT(*) AS n, 
        SUM(Unit Price) AS sum_x, 
        SUM(Total Price) AS sum_y, 
        SUM(Unit Price * Total Price) AS sum_xy, 
        SUM(POWER(Unit Price, 2)) AS sum_x2, 
        SUM(POWER(Total Price, 2)) AS sum_y2 
    FROM online_sales_data 
    GROUP BY Region 
)
SELECT  
    Region, 
    (n * sum_xy - sum_x * sum_y) /  
    SQRT((n * sum_x2 - POWER(sum_x, 2)) * (n * sum_y2 - POWER(sum_y, 2))) AS correlation 
FROM sums_and_counts;
```

![obraz](https://github.com/biku89/Online-sales/assets/169537978/f8c6d903-6da3-4756-8fbb-7df156c8d9d5)

**Findings**:
- Strong positive correlation between Unit Price and Total Revenue in all regions.
- Highest correlation in North America.

High correlation values across all regions may indicate that pricing strategy is a key factor influencing revenues in these areas. This suggests the need for careful management of unit prices to maximize revenue.


## Conclusion

Effective marketing strategies, tailored product offerings, and understanding regional payment preferences are crucial for maintaining steady sales and maximizing revenue. Seasonal trends, promotional activities, and strategic pricing adjustments are key drivers influencing sales fluctuations and revenue peaks observed throughout the year. Continued monitoring and adjustment of marketing tactics and product strategies will be essential to sustain and enhance revenue growth across different markets.




Data source:
https://www.kaggle.com/datasets/shreyanshverma27/online-sales-dataset-popular-marketplace-data










