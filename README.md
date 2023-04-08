# Data_Science_Assessment

This is the repository that contains the solutions to the Data Science Assessment assignment. 

Exercise 1

For exercise 1, the classification problem was chosen: the task is to implement a binary classifier. The code can be found in the file named 'Exercise_1.ipynb'. 

## Technical Questions

Exercise 2

2.1	Can you explain the purpose of the orders_items table? 

The orders_items table is used to log the items present in each order. The orders table contains information about each order, but no information about what items are in that order. Similarly, the products table contains information about the products, but not about who ordered them, what order they are part of or in what quantity. Thus, the orders_items table serves as a 'bridge' between the two tables and contains more specific information about how many items of a product are in a certain order. This information is necessary to process the order, because it will show what specific items belong to an order. 

2.2	Can you write a SQL query to find the average order cost per country?

SELECT AVG(orders.order_cost) 
FROM customers, orders
WHERE customers.country = country AND orders.id_customer = customers.id


2.3	Can you write a SQL query to find the name of the highest price product sold to an Italian customer?

SELECT products.name, MAX(products.price) 
FROM customers, orders, orders_items, products 
WHERE customers.country = 'Italy' AND orders.id_customer = customers.id AND orders_items.id_order = orders.id AND products.id = orders_items.id_product