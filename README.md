# Online-sales
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

SELECT `Region`, COUNT(*) AS Result FROM online_sales_data GROUP BY `Region`;
![obraz](https://github.com/biku89/Online-sales/assets/169537978/3c9ff300-b8d6-476f-ab45-7e5d1a61a23a)
