# MYSQL-Electronics
Datasert based on customers and products tables for Electronics database

Create table for electronic products. There should not be duplicates of product_id, this must be unique.
CREATE TABLE products 
(
    product_id 		INT PRIMARY KEY AUTO_INCREMENT,
    product_name 	VARCHAR(50) NOT NULL,
    quantity_in_stock 	INT NOT NULL,
    cost 		DECIMAL(6,2) NOT NULL,
    
);

Insert data into products table (no need to include product_id since it is auto_increment and will automatically create the id data, so the column name must be listed in the INSERT INTO statement).
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("keyboard piano", 10, 20.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("90 inch TV", 20, 1000.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("cd player", 55, 40.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("dvd player", 15, 60.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("keyboard computer", 50, 25.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("85 inch TV", 72, 600.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("15 inch laptop", 50, 1500.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("32 inch computer monitor", 50, 150.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("NintenIdo WII console", 10, 500.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Mario & Luigi in Space game", 100, 75.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Donkey Kong Special game", 100, 75.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("ACME desktop computer", 100, 1500.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("6 CD JAZZ Collection", 50, 40.00)
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Office Software", 50, 100.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Office Desk", 50, 125.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Office Chair", 50, 100.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("6 CD Christmas JAZZ ", 50, 40.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("Earbuds", 50, 15.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("computer mouse", 50, 10.00 )
INSERT INTO products (product_name, quantity_in_stock, cost) VALUES("I Love Lucy DVD Collection", 50, 10.00 )

Add new column called category:
ALTER TABLE products ADD COLUMN category VARCHAR(20)

Enter data for category:
UPDATE products
SET category = "Audio"
WHERE product_id IN (1, 3, 13, 17, 18)

UPDATE products
SET category = "Video"
WHERE product_id IN (2, 4, 6, 20)

UPDATE products
SET category = "Computer"
WHERE product_id IN (5, 7, 8, 12, 14, 15, 16, 17, 19)

UPDATE products
SET category = "Games"
WHERE product_id IN (9, 10, 11)

Get the different types of categories:
SELECT  distinct(category)
FROM products

Which category has highest amount of inventory
SELECT category, SUM(quantity_in_stock) AS TotalQuantity
FROM products
GROUP BY category
ORDER BY 2 DESC LIMIT 1

List Computer products:
select *
FROM products
WHERE category = "Computer"

6 CD Jazz Collection is incorrectly categorized as Computer. Change to Audio category:
UPDATE products
SET category = "Audio"
WHERE product_name = “6 CD Jazz Collection”

Calculate Inventory Cost and add commas to this new column:
SELECT product_name, sum(quantity_in_stock), format(quantity_in_stock * cost,2) AS Inventory_Cost
FROM products
GROUP BY product_name, quantity_in_stock, cost 


CREATE TABLE customers 
(
cust_id 		VARCHAR(50) NOT NULL,
first_name		VARCHAR(50) NOT NULL,
last_name		VARCHAR(50) NOT NULL,
product_id		INT NOT NULL,
quantity_purchased 	INT NOT NULL,
date_purchased	DATE NOT NULL
    
);

Insert data into customers table:
INSERT INTO customers  VALUES ("LBall1", "Lucy", "Ball", 2, 1, '2024_06_02')
INSERT INTO customers  VALUES ("LBall1", "Lucy", "Ball", 4, 1,'2024_06_02' )
INSERT INTO customers  VALUES ("LBall1", "Lucy", "Ball", 20, 1,'2024_06_02' )
INSERT INTO customers  VALUES ("CHarp2", "Chelsea","Harper", 3, 1, '2024_07_15' )
INSERT INTO customers  VALUES ("CHarp2", "Chelsea","Harper", 13, 1, '2024_07_15')
INSERT INTO customers  VALUES ("CHarp2", "Chelsea","Harper", 17, 1, '2024_07_15')
INSERT INTO customers  VALUES ("CHarp2", "Chelsea","Harper", 18, 1, '2024_07_15')
INSERT INTO customers  VALUES ("DRope3", "Dennis", "Roper", 9, 1, '2024_05_29')
INSERT INTO customers  VALUES ("DRope3", "Dennis", "Roper", 10, 1, '2024_05_29')
INSERT INTO customers  VALUES ("DRope3", "Dennis", "Roper", 11, 1, '2024_05_29')
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 7, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 5, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 8, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 12, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 14, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 19, 10, "2024_01_10"  )
INSERT INTO customers  VALUES("JTech4", "Junior Tech","Max Wilson", 16, 5, "2024_01_10"  )

What products did Chelsea Harper purchased?
SELECT cust_id, first_name, last_name, date_purchased, product_name AS Description, quantity_purchased as QTY
FROM customers C JOIN products P ON C.product_id = P.product_id
WHERE last_name = "Harper"


What products did LBall1 purchased? Used concatenate function to combine product_id and product_name.
SELECT cust_id, first_name, last_name, date_purchased, CONCAT(P. product_id, " ", product_name) AS Description, quantity_purchased
FROM customers C JOIN products P ON C.product_id = P.product_id
WHERE cust_id = 'LBall1'

Create list of products sold to Dennis Roper that shows a new Price column for each product. To calculate Price, add 30% as markup to cost: 
select cust_id, first_name, last_name, product_name, quantity_purchased as QTY, FORMAT(cost * 1.30, 2) AS PRICE
FROM customers C JOIN products P on C.product_id = P.product_id
WHERE last_name = "Roper"

Create list of products sold to Junior Tech that shows 
a new Price column for each product, add 30% as markup to cost. 
Calculate 10% Discount for each item
Calculate SalesPrice column = price - discount
select cust_id, first_name, product_name, cost, FORMAT(cost * 1.30, 2) AS MarkupPrice, quantity_purchased as QTY, quantity_purchased * cost*1.3 AS Subtotal,
    (cost * 1.30) * .1 AS '10% Discount', cost * 1.30- cost * 1.30 * .1 as SalesPrice
FROM customers C JOIN products P on C.product_id = P.product_id
WHERE first_name = "Junior Tech" AND quantity_purchased * cost >= 1000

Who ordered computer equipment?
select first_name, last_name, category, product_name
FROM customers C JOIN products P ON C.product_id = P.product_id
WHERE category = "Computer"

Create list of type of customers:
select first_name, last_name, quantity_purchased AS QTY, product_name,
    CASE
        WHEN quantity_purchased > 4 THEN 'Company or Education Institution'
        WHEN quantity_purchased < 5 THEN 'Individual'
        ELSE 'Other'
    END Customer_Type
from customers C JOIN products P ON C.product_id = P.product_id

Find customers who purchased products with cost of $1000 or more
SELECT first_name, last_name
FROM customers 
WHERE product_id IN (
    SELECT product_id
    FROM products
    WHERE cost >= 1000
);

