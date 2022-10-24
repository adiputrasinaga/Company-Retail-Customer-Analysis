<u><h3>B2B-Retail Customer-Analysis Report</h3></u>
<br/>

**Understanding tables**

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

**Total Sales and Revenue in Quarter-1 (Jan, Feb, Mar) and Quarter-2 (Apr,May,Jun)**

From the orders_1 table, add the quantity column with the aggregate sum() function and name it “total_penjualan”, multiply the quantity column with the priceEach column then add up the multiplication results of the two columns and name it “revenue”

The company only wants to count sales from products shipped, so we need to filter the 'status' column so that it only shows orders with the status “Shipped”.

Perform Steps 1 & 2, for table **orders_2**.
````sql
SELECT 
  SUM(quantity) AS total_penjualan, 
  SUM(quantity*priceeach) AS revenue
FROM 
  orders_1
WHERE 
  status="shipped";
  ````
  <details>
<summary>OUTPUT</summary>
  
| total_penjualan | revenue   |
|-----------------|-----------|
|            8694 | 799579310 |  
</details>

````sql
SELECT 
  SUM(quantity) AS total_penjualan, 
  SUM(quantity*priceeach) AS revenue
FROM 
  orders_2
WHERE 
  status="shipped";
````
  <details>
<summary>OUTPUT</summary>

| total_penjualan | revenue   |
|-----------------|-----------|
|            6717 | 607548320 |
</details>

**Calculating the percentage of overall sales**

The two tables **orders_1** and **orders_2** are still separate, to calculate the percentage of overall sales from the two tables need to be combined:

* Select the “orderNumber”, “status”, “quantity”, “priceEach” column in the orders_1 table, and add a new column with the name “quarter” and fill it with the value “1”. Do the same with the orders_2 table, and fill it with the value “2”, then join the two tables.

* Use the statement from Step 1 as the subquery and alias it “table_a”.

* From "table_a", do the sum on the "quantity" column with the aggregate sum() function and name it "total_penjualan", and multiply the quantity column with the priceEach column then add up the product of the two columns and name it "revenue".

* Filter the 'status' column so that it only displays orders with the status “Shipped”.
* Group sales_total based on the “quarter” column, and don't forget to add this column to the select section.

````sql
SELECT 
  quarter, 
  SUM(quantity) AS total_penjualan,
  SUM(quantity*priceeach) AS revenue
FROM
  (SELECT ordernumber, status, quantity, priceeach, 1 AS quarter from orders_1
UNION
  SELECT ordernumber, status, quantity, priceeach, 2 AS quarter from orders_2) AS table_a
WHERE 
  status="shipped"
GROUP BY 
  quarter;
````
  <details>
<summary>OUTPUT</summary>
  
| quarter | total_penjualan | revenue   |
|---------|-----------------|-----------|
|       1 |            8694 | 799579310 |
|       2 |            6717 | 607548320 |  
  
</details>

**Calculation of Sales and Revenue Growth**

For this project, sales growth calculation will be done manually using the formula provided below.
* **% Sales Growth** = (6717 – 8694)/8694 = -22%
* **%Growth Revenue** = (607548320 – 799579310)/ 799579310 = -24%


**Costumer Analytics**

**Is the number of xyz.com customers growing?**
* The increase in the number of customers can be measured by comparing the total number of customers who registered in the current period with the total number of customers who registered at the end of the previous period.
* From the customer table, select the customerID column, createDate and add a new column using the QUARTER(…) function to extract the quarter value from CreateDate and name it “quarter”
* Filter the “createDate” column so that it only shows rows with createDate between January 1, 2004 and June 30, 2004
* Use statements Steps 1 & 2 as subquery with alias table_b
* Count the number of unique customers no duplication of customers and name it “total_customers”
* Group total_customer based on the “quarter” column, and don't forget to add this column to the select section.

````sql
SELECT 
  quarter, 
  count(distinct customerid) AS total_customers
FROM
  (SELECT customerid, createdate, quarter(createdate) AS quarter 
  FROM customer WHERE QUARTER(createdate) <= 2) AS table_b
GROUP BY quarter;
````
  <details>
<summary>OUTPUT</summary>
  
| quarter | total_customers |
|---------|-----------------|
|       1 |              43 |
|       2 |              35 |
  
</details>

**How many of these customers have made transactions?**

This problem is a continuation of the previous problem, namely from a number of customers who registered in the quarter-1 and quarter-2 periods, how many have made transactions.

* From the customer table, select the customerID column, createDate and add a new column using the QUARTER(…) function to extract the quarter value from CreateDate and name it “quarter”
* Filter the “createDate” column so that it only shows rows with createDate between January 1, 2004 and June 30, 2004.
* Use the Steps A&B statement as a subquery with the alias table_b.
* From the tables orders_1 and orders_2, select the customerID column, use DISTINCT to remove duplication, then join the two tables with UNION.
* Filter table_b with the IN() operator using 'Select statement step 4' , so that only customerIDs that have transacted (customerID recorded in the orders table) are taken into account.
* Count the number of unique customers (no duplicate customers) in the SELECT statement and name it “total_customers”.
* Group total_customer based on the “quarter” column, and don't forget to add this column to the select section.

````sql
SELECT 
  quarter, 
  count(distinct customerid) AS total_customers 
FROM 
  (SELECT 
    customerid, 
    createdate, 
    quarter(createdate) AS quarter 
  FROM 
    customer
  WHERE 
    createdate between "2004-01-01" and "2004-06-30") AS table_b
WHERE 
  customerid 
  IN
    (SELECT DISTINCT customerid
    FROM
      orders_1
    UNION
    SELECT DISTINCT customerid
    FROM
      orders_2)
GROUP BY 
  quarter;
````
  <details>
<summary>OUTPUT</summary>
  
| quarter | total_customers |
|---------|-----------------|
|       1 |              25 |
|       2 |              19 |
  
</details>

**What product categories are most ordered by customers in Quarter-2?**

To find out which product categories are purchased the most, it can be done by calculating the total orders and the number of sales from each product category.

* From the orders_2 column, select productCode, orderNumber, quantity, status
* Add a new column by extracting the first 3 characters from the productCode which is the ID for the product category; and name it categoryID
* Filter the “status” column so that only products with the status “Shipped” are taken into account
* Use the statements of Steps 1, 2, and 3 as subqueries with the alias table_c
* Calculate the total order from the “orderNumber” column and name it “total_order”, and the sales amount from the “quantity” column and name it “total_sales”
* Group by categoryID, and don't forget to add this column in the select section.
* Sort by “total_order” from largest to smallest.

````sql
SELECT  * FROM
  (SELECT 
    categoryid, 
    count(distinct ordernumber) AS total_order, 
    sum(quantity) AS total_penjualan
  FROM
    (SELECT productcode, 
      ordernumber, 
      quantity, 
      status,
      LEFT(productcode,3) AS categoryid 
    FROM 
      orders_2
    WHERE status="shipped") AS table_c
  GROUP BY categoryid) a
ORDER BY 
  total_order
DESC;
````
  <details>
<summary>OUTPUT</summary>
  
| categoryid | total_order | total_penjualan |
|------------|-------------|-----------------|
| S18        |          25 |            2264 |
| S24        |          21 |            1826 |
| S32        |          11 |             616 |
| S12        |          10 |             491 |
| S50        |           8 |             292 |
| S10        |           8 |             492 |
| S70        |           7 |             675 |
| S72        |           2 |              61 |
  
</details>

**How many customers remain active in transactions after their first transaction?**

Because there are only 2 periods, Quarter 1 and Quarter 2, the retention that can be calculated is the retention of customers who shop in Quarter 1 and return to shopping in Quarter 2, while for customers who shop in Quarter 2, the retention can only be calculated in Quarter 3.

* From the orders_1 table, add a new column with the value “1” and name it “quarter”
* From the orders_2 table, select the customerID column, use distinct to eliminate duplication
* Filter the orders_1 table with the IN() operator using the 'Select statement step 2', so that only customerIDs that have transacted in quarter 2 (customerID recorded in table orders_2) are taken into account.
* Count the number of unique customers (no duplicate customers) divided by total_ customers in percentage, in the Select statement and name it “Q2”.
````sql
#Calculating the total unique customers who made transactions in quarter_1
SELECT 
  COUNT(DISTINCT customerID) AS total_customers 
FROM 
  orders_1;
  ````
  <details>
<summary>OUTPUT</summary>

| total_customers |
|-----------------|
|              25 |
  
</details>

````sql
SELECT
  1 as quarter, 
  (COUNT(DISTINCT customerid)/25*100) AS Q2 
FROM 
  orders_1 
WHERE 
  customerid 
  IN(SELECT DISTINCT customerid FROM orders_2);
  ````
  <details>
<summary>OUTPUT</summary>

| quarter | Q2      |
|---------|---------|
|       1 | 24.0000 |
  
</details>


**thankyou!**

