# Online sales
This project contains online sales data that can be used to analyze sales trends and performance across different regions. 
The CSV file includes information about the sales of individual products in various countries.

Columns:
  Order ID: Unique identifier for each sales order.
  Date:Date of the sales transaction.
  Category:Broad category of the product sold (e.g., Electronics, Home Appliances, Clothing, Books, Beauty Products, Sports).
  Product Name:Specific name or model of the product sold.
  Quantity:Number of units of the product sold in the transaction.
  Unit Price:Price of one unit of the product.
  Total Price: Total revenue generated from the sales transaction (Quantity * Unit Price).
  Region:Geographic region where the transaction occurred (e.g., North America, Europe, Asia).
  Payment Method: Method used for payment (e.g., Credit Card, PayPal, Debit Card)

In this project, we will analyze sales trends over time. We will examine the popularity of different product categories for each region. 
We will look into the most popular payment methods. We will check which products sell best in each category to tailor appropriate marketing campaigns.

After importing the data into MySQL, we proceed to check how sales look for each region and individual product, as well as the preferred payment method in each country.

`SELECT `Region`, COUNT(*) AS Result FROM online_sales_data GROUP BY `Region`;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/3c9ff300-b8d6-476f-ab45-7e5d1a61a23a)

`SELECT `Product Category`, Count(*) AS Result FROM online_sales_data GROUP BY `Product Category`;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/9832882f-a287-4ef5-8ae0-bee952046e8b)

`SELECT `Region`, `Payment Method`, COUNT(*) AS Popular_Method FROM online_sales_data 
GROUP BY `Region`, `Payment Method`
ORDER BY Popular_Method;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/8f3cc486-80cb-4fe9-b206-c29caeb0fbf1)

1.Uniform geographic sales indicate an effective marketing and distribution strategy that works well in the specified countries.

2.Balanced sales indicate even demand for specific products.

3.The preferred payment method is credit card, which constitutes 50% of all transactions. PayPal is in second place, and debit card is third.

  In Asia, the most popular payment methods are credit card and debit card.
  In Europe, PayPal is preferred.
  In North America, credit card is preferred.


Now we will check how expenditures on specific products look in different countries.

`SELECT `Product Category`, `Region`, SUM(`Units Sold`) AS TheBest_Category FROM online_sales_data 
GROUP BY `Product Category`, `Region`
ORDER BY TheBest_Category DESC;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/cc672f4f-1962-4783-bbc8-43f5ac8a8c8c)

`SELECT `Product Category`, `Region`, ROUND(SUM(`Total Revenue`),2) AS Total_Revenue FROM online_sales_data 
GROUP BY `Product Category`, `Region`
ORDER BY Total_Revenue DESC;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/66dcd61b-8a7b-4ec2-b1b0-49fc65788864)

Asia makes the most purchases in the category 'Clothing and Sports'.

Europe leads in the category 'Home Appliances and Cosmetics'.

North America dominates in the category 'Books and Electronics'.

North America spent 34,982.41 PLN on electronics alone.

Europe spent 18,646.16 PLN on cosmetic products.

Asia spent 14,326.52 PLN on purchases in the Sports category.


We will see which country spends the most on individual products and which country makes the most purchases.

`SELECT `Product Name`, `Region`, ROUND(SUM(`Unit Price`),2) AS Top_Unit_Price FROM online_sales_data 
GROUP BY `Product Name`, `Region`
ORDER BY Top_Unit_Price DESC LIMIT 10;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/f125156d-3722-474e-8925-d60755c9c9c6)

`SELECT `Product Name`, `Region`, SUM(`Units Sold`) AS Top_Unit_Sold FROM online_sales_data 
GROUP BY `Product Name`, `Region`
ORDER BY Top_Unit_Sold DESC LIMIT 10;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/27d47909-026b-4943-92ae-4fb568f0ac08)

Residents of Asia buy the largest quantity of specific products.
However, when it comes to making the most expensive transactions (in the Top 10), 80% of transactions belong to North America.

`SELECT `Region`, ROUND(SUM(`Total Revenue`), 2) AS Total_Revenue FROM online_sales_data
GROUP BY `Region`
ORDER BY `Total_Revenue` DESC;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/30b4e57c-d8ff-4ea5-b1e9-2edb58fe066e)

Throughout the entire period, North America generated the highest income.

- I visualized revenues over all available days using Power BI.

![obraz](https://github.com/biku89/Online-sales/assets/169537978/ff826b94-314f-4afd-90c5-d319da684158)

The chart indicates several periods with clear peaks in revenues. The highest values can be observed in early January, February, April, and August 2024.
Revenues are highly variable throughout the period with many peaks and troughs. 
This suggests a dynamic sales pattern, which could be due to seasonal trends or promotions.


Now we will check how sales looked on specific days and months.

`SELECT 
    CASE
        WHEN DAYOFWEEK(`Date`) = 1 THEN 7
        ELSE DAYOFWEEK(`Date`) - 1
    END AS day_of_week_number,
    DAYNAME(`Date`) AS day_of_week, 
    ROUND(SUM(`Total Revenue`), 2) AS Total_Revenue 
FROM online_sales_data
GROUP BY 
    day_of_week_number,
    day_of_week
ORDER BY 
    day_of_week_number;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/c3004dab-9efc-4a94-92a7-69750da8d85e)

`SELECT 
    DATE_FORMAT(`Date`, '%Y-%m') AS Month,
    ROUND(SUM(`Total Revenue`), 2) AS Total_Revenue
FROM 
    online_sales_data
GROUP BY 
    DATE_FORMAT(`Date`, '%Y-%m')
ORDER BY 
    DATE_FORMAT(`Date`, '%Y-%m');`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/895a2b40-8b66-4ccf-ad56-dadaca211a5f)

- Sales throughout the week appear to be quite stable, except for Thursday, which has noticeably lower revenue.
Tuesday is the most profitable day, which may suggest special promotions. Friday and the weekend (Saturday, Sunday) indicate increased customer traffic.

-Monthly sales show a trend of increasing revenue in each month of 2024. 
The highest revenue was achieved at the beginning of the year in January, and the lowest in August.

Let's examine the correlation between Unit Price and Total Revenue.

`WITH sums_and_counts AS (
    SELECT 
        Region,
        COUNT(*) AS n,
        SUM(`Unit Price`) AS sum_x,
        SUM(`Total Revenue`) AS sum_y,
        SUM(`Unit Price` * `Total Revenue`) AS sum_xy,
        SUM(POWER(`Unit Price`, 2)) AS sum_x2,
        SUM(POWER(`Total Revenue`, 2)) AS sum_y2
    FROM online_sales_data
    GROUP BY Region
)
SELECT 
    Region,
    (n * sum_xy - sum_x * sum_y) / 
    SQRT((n * sum_x2 - POWER(sum_x, 2)) * (n * sum_y2 - POWER(sum_y, 2))) AS correlation
FROM sums_and_counts;`

![obraz](https://github.com/biku89/Online-sales/assets/169537978/f8c6d903-6da3-4756-8fbb-7df156c8d9d5)

A correlation close to 1 indicates a strong positive linear relationship. These values suggest that as Unit Price increases, Total Revenue also increases in a predictable and proportional manner.

There is a strong positive correlation between Unit Price and Total Revenue in all three regions.

The strongest correlation is observed in North America, indicating that changes in unit prices have the greatest impact on total revenues here.

High correlation values across all regions may indicate that pricing strategy is a key factor influencing revenues in these areas. This suggests the need for careful management of unit prices to maximize revenue.


Conclusion:

  Effective marketing strategies, tailored product offerings, and understanding regional payment preferences are crucial for maintaining steady sales and maximizing revenue.
  Seasonal trends, promotional activities, and strategic pricing adjustments are key drivers influencing sales fluctuations and revenue peaks observed throughout the year.
  Continued monitoring and adjustment of marketing tactics and product strategies will be essential to sustain and enhance revenue growth across different markets.




The data has been downloaded from:
https://www.kaggle.com/datasets/shreyanshverma27/online-sales-dataset-popular-marketplace-data










