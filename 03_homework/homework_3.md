# Homework 3: Essential SQL

-  	Due on Saturday, May 17 at 11:59pm
-  	Weight: 8% of total grade
-  	Upload one .sql file with your queries

# AGGREGATE
1. Write a query that determines how many times each vendor has rented a booth at the farmer’s market by counting the vendor booth assignments per `vendor_id`.

SELECT vendor_id, Count(booth_number)
FROM vendor_booth_assignments
GROUP BY vendor_id

2. The Farmer’s Market Customer Appreciation Committee wants to give a bumper sticker to everyone who has ever spent more than $2000 at the market. Write a query that generates a list of customers for them to give stickers to, sorted by last name, then first name. 
**HINT**: This query requires you to join two tables, use an aggregate function, and use the HAVING keyword.

SELECT B.customer_last_name, B.customer_first_name,  SUM(A.quantity * A.cost_to_customer_per_qty) as total_spend
FROM customer_purchases A
JOIN customer B ON A.customer_id = B.customer_id
GROUP BY B.customer_first_name, B.customer_last_name
HAVING SUM(A.quantity * A.cost_to_customer_per_qty) >= 2000
ORDER BY total_spend DESC

# Temp Table
1. Insert the original vendor table into a temp.new_vendor and then add a 10th vendor: Thomass Superfood Store, a Fresh Focused store, owned by Thomas Rosenthal

CREATE TEMP TABLE new_vendor AS
SELECT *
FROM vendor

INSERT INTO new_vendor (vendor_id, vendor_name, vendor_type, vendor_owner_first_name, vendor_owner_last_name)
VALUES (10, 'Thomass Superfood', 'Fresh Focused', 'Thomas', 'Rosenthal');

SELECT * FROM new_vendor;

**HINT**: This is two total queries -- first create the table from the original, then insert the new 10th vendor. When inserting the new vendor, you need to appropriately align the columns to be inserted (there are five columns to be inserted, I've given you the details, but not the syntax)

To insert the new row use VALUES, specifying the value you want for each column:  
`VALUES(col1,col2,col3,col4,col5)`

# Date
1. Get the customer_id, month, and year (in separate columns) of every purchase in the customer_purchases table.

SELECT customer_id, strftime('%Y', market_date) Year, strftime('%m', market_date) Month
FROM customer_purchases


**HINT**: you might need to search for strfrtime modifers sqlite on the web to know what the modifers for month and year are!

2. Using the previous query as a base, determine how much money each customer spent in April 2019. Remember that money spent is `quantity*cost_to_customer_per_qty`.

SELECT customer_id, strftime('%Y', market_date) Year, strftime('%m', market_date) Month, (quantity*cost_to_customer_per_qty) spent
FROM customer_purchases
WHERE strftime('%m', market_date) = '04' AND strftime('%Y', market_date) = '2019'
GROUP BY customer_id

**HINTS**: you will need to AGGREGATE, GROUP BY, and filter...but remember, STRFTIME returns a STRING for your WHERE statement!!

