**B2B-Retail-Customer-Analysis-Report**

xyz.com is a B2B startup that sells various products not directly to end users but to other businesses/companies. Every quarter xyz.com will hold a town hall where all division representatives will gather to review the company performance during the last quarter.

Before starting to compose SQL queries and make analysis of query results, the first thing that needs to be done is to become familiar with the tables that will be used. This will be very useful in determining which columns are related to the problem to be analyzed, and what data manipulation process should be done for these columns, because not all columns in the table need to be used.

* Check table orders_1 :SELECT * FROM orders_1 limit 5;
* Check table orders_2 :SELECT * FROM orders_2 limit 5;
* Check customer table :SELECT * FROM customer limit 5;

````sql
SELECT * FROM orders_1 LIMIT 5;
SELECT * FROM orders_2 LIMIT 5;
SELECT * FROM customer LIMIT 5;
````
<details>
<summary>order_1</summary>

| orderNumber | orderDate  | requiredDate | shippedDate | status  | customerID | productCode | quantity | priceeach |
|-------------|------------|--------------|-------------|---------|------------|-------------|----------|-----------|
|       10234 | 2004-03-30 | 2004-04-05   | 2004-04-02  | Shipped |        412 | S72_1253    |       40 |     45690 |
|       10234 | 2004-03-30 | 2004-04-05   | 2004-04-02  | Shipped |        412 | S700_2047   |       29 |     83280 |
|       10234 | 2004-03-30 | 2004-04-05   | 2004-04-02  | Shipped |        412 | S24_3816    |       31 |     78830 |
|       10234 | 2004-03-30 | 2004-04-05   | 2004-04-02  | Shipped |        412 | S24_3420    |       25 |     65090 |
|       10234 | 2004-03-30 | 2004-04-05   | 2004-04-02  | Shipped |        412 | S24_2841    |       44 |     67140 |
</details>

<details>
<summary>order_2</summary>
  
| orderNumber | orderDate  | requiredDate | shippedDate | status  | customerID | productCode | quantity | priceeach |
|-------------|------------|--------------|-------------|---------|------------|-------------|----------|-----------|
|       10235 | 2004-04-02 | 2004-04-12   | 2004-04-06  | Shipped |        260 | S18_2581    |       24 |     81950 |
|       10235 | 2004-04-02 | 2004-04-12   | 2004-04-06  | Shipped |        260 | S24_1785    |       23 |     89720 |
|       10235 | 2004-04-02 | 2004-04-12   | 2004-04-06  | Shipped |        260 | S24_3949    |       33 |     55270 |
|       10235 | 2004-04-02 | 2004-04-12   | 2004-04-06  | Shipped |        260 | S24_4278    |       40 |     63030 |
|       10235 | 2004-04-02 | 2004-04-12   | 2004-04-06  | Shipped |        260 | S32_1374    |       41 |     90900 |
</details>

<details>
<summary>customer</summary>
  
| customerID | customerName               | contactLastName | contactFirstName | city      | country   | createDate |
|------------|----------------------------|-----------------|------------------|-----------|-----------|------------|
|        103 | Atelier graphique          | Schmitt         | Carine           | Nantes    | France    | 2004-02-05 |
|        112 | Signal Gift Stores         | King            | Jean             | Las Vegas | USA       | 2004-02-05 |
|        114 | Australian Collectors, Co. | Ferguson        | Peter            | Melbourne | Australia | 2004-02-20 |
|        119 | La Rochelle Gifts          | Labrune         | Janine           | Nantes    | France    | 2004-02-05 |
|        121 | Baane Mini Imports         | Bergulfsen      | Jonas            | Stavern   | Norway    | 2004-02-05 |
</details>








