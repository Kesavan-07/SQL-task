### SQL Task: E-Commerce System

# Create a Database and Tables

Create a Database Named ecommerce
CREATE DATABASE ecommerce;
USE ecommerce;
2. Create the Tables: customers, orders, and products

## Creating the customers Table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,   -- Unique customer ID
    name VARCHAR(100),                   -- Customer's name
    email VARCHAR(100),                  -- Customer's email
    address TEXT                         -- Customer's address
);
## Creating the orders Table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,   -- Unique order ID
    customer_id INT,                     -- Foreign key referencing the `customers` table
    order_date DATE,                      -- Date the order was placed
    total_amount DECIMAL(10, 2),          -- Total amount of the order
    FOREIGN KEY (customer_id) REFERENCES customers(id)  -- Foreign key constraint
);
## Creating the products Table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,   -- Unique product ID
    name VARCHAR(100) NOT NULL,           -- Product's name
    price DECIMAL(10, 2) NOT NULL,        -- Product's price
    description TEXT                      -- Product's description
);
3. Insert Sample Data into the Tables

## Insert Sample Data into customers Table
INSERT INTO customers (name, email, address)
VALUES ('Sharoan', 'sharoan@gmail.com', 'No. 4 Lakshmi Nagar'),
       ('Yash', 'yash@gmail.com', 'No. 15 Mahesh Nagar'),
       ('Krithik', 'krithik@gmail.com', 'No. 7 Violin Street');
## Insert Sample Data into orders Table
INSERT INTO orders (customer_id, order_date, total_amount)
VALUES (1, CURDATE(), 50.00),                        -- Customer 1 placed an order today
       (2, CURDATE() - INTERVAL 10 DAY, 75.00),        -- Customer 2 placed an order 10 days ago
       (3, CURDATE() - INTERVAL 40 DAY, 30.00);         -- Customer 3 placed an order 40 days ago
## Insert Sample Data into products Table
INSERT INTO products (name, price, description)
VALUES ('Product A', 30.00, 'Description of Product A'),
       ('Product B', 40.00, 'Description of Product B'),
       ('Product C', 25.00, 'Description of Product C');
Queries
1. Retrieve all customers who have placed an order in the last 30 days

SELECT DISTINCT c.*
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;
-- This query retrieves customers who have placed an order in the last 30 days.
2. Get the total amount of all orders placed by each customer

SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;
-- This query calculates the total amount spent by each customer on all orders.
3. Update the price of Product C to 45.00

UPDATE products 
SET price = 45.00
WHERE name = 'Product C';
-- This query updates the price of 'Product C' to 45.00.
4. Add a new column discount to the products table

ALTER TABLE products
ADD COLUMN discount DECIMAL(5, 2) DEFAULT 0.00;
-- This query adds a new column `discount` to the `products` table with a default value of 0.00.
5. Retrieve the top 3 products with the highest price

SELECT * FROM products
ORDER BY price DESC
LIMIT 3;
-- This query retrieves the top 3 products with the highest price.
6. Get the names of customers who have ordered 'Product A'

SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';
-- This query retrieves the names of customers who have ordered 'Product A'.
7. Join the orders and customers tables to retrieve the customer's name and order date for each order

SELECT c.name, o.order_date
FROM orders o
JOIN customers c ON o.customer_id = c.id;
-- This query retrieves each order's corresponding customer name and the order date.
8. Retrieve the orders with a total amount greater than 150.00

SELECT * FROM orders
WHERE total_amount > 150.00;
-- This query retrieves orders where the total amount is greater than 150.00.
9. Normalize the database by creating a separate table for order_items and updating the orders table to reference the order_items table

CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,   -- Unique order item ID
    order_id INT,                        -- Foreign key referencing the `orders` table
    product_id INT,                      -- Foreign key referencing the `products` table
    quantity INT DEFAULT 1,              -- Quantity of the product in the order
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
-- This query creates the `order_items` table and establishes relationships with `orders` and `products`.
10. Retrieve the average total of all orders

SELECT AVG(total_amount) AS average_order_total
FROM orders;
-- This query calculates the average total amount of all orders.
