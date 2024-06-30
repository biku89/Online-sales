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



