


🚴‍♂️ Bike Store: Unlocking Insights for a Retail Business
<img width="946" height="640" alt="bikkkkeee" src="https://github.com/user-attachments/assets/3609cdfd-b090-4f33-bf8c-e869866477f4" />


📌 Project Overview 
This project dives into the Bike Store relational database, focusing on sales, customers, products, inventory, and staff performance. Through structured SQL analysis and exploratory data techniques, it aims to reveal valuable insights that support retail strategy, customer engagement, and operational improvements.
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               📂 Database Tables
 **brands** – Bike brands  
 **categories** – Product categories  
 **customers** – Customer details  
 **orders** – Orders placed by customers  
 **order_items** – Products in each order  
 **products** – Product details  
 **staffs** – Staff members  
 **stores** – Store details  
 **stocks** – Inventory per store                                                                                                                                                                                                                       
                                                                                                                                                                                                                    
🎯 Project Goals

🛒 Customer Insights

 * 📌 Identify customer profiles, locations, and order history for targeted campaigns.
 * 📊 Analyze customer purchase behavior to boost engagement and retention.

🚲 Product & Inventory Optimization
 * 🛠️ Track inventory across stores and categories to avoid stockouts.
 *  🔍 Identify best-selling and underperforming products.


💰 Revenue Enhancement
* 📈 Calculate total and category-wise revenue to spot trends.
* 🛍️ Evaluate pricing strategies and profit potential by product.
  

⚙️ Operational Efficiency
* 📌 Monitor staff activity and performance in processing orders.
* 🗂️ Track store-wise sales to identify growth opportunities.


🛠️ Tools & Technologies Used
* SQL – Data querying and analysis
* Bike Store Database – Relational retail dataset
* MySQL / PostgreSQL – Query execution engines
* DBeaver / MySQL Workbench – Database IDE
* Excel / CSV Files – Raw data tables

📊 Insights & Expected Outcomes
* 📢 Better customer segmentation for marketing strategies.
* 🚲 Smarter inventory planning based on demand patterns.
* 💵 Increased revenue tracking through product and store-level analysis.
*⚙️ Improved staff productivity monitoring and operational clarity.

📌 Conclusion

This project highlights the potential of SQL-driven business intelligence in the retail sector. From optimizing product inventory to tracking staff productivity and enhancing revenue through analytics, the Bike Store Database provides a practical, hands-on environment for applying real-world data skills.


✅ Bike_Store SQL Queries
This repository contains SQL queries and corresponding visualizations for analyzing the Bike_Store dataset.

* Queries and Visualizations

1. Show all customers from the city 'New York'                                                                                                                                                                         
![Query 1 Result](https://github.com/YadavBarkha/Bike_store/blob/aadb2f336a0e615a4c07e1da31c9c1d27242bf11/buil/bk1.png)

2.Find the number of products in each category

![Query 2 Result](https://github.com/YadavBarkha/Bike_store/blob/6c784a972497f53c9829faef84cfc80285430e38/bk2.png)


3.List all products with category, brand, and price 

![Query 2 Result](https://github.com/YadavBarkha/Bike_store/blob/6c784a972497f53c9829faef84cfc80285430e38/bk3.png)


-- Show all orders along with customer name and order date--

select o.order_id, c.first_name, c.last_name, o.order_date
from orders as o
 inner join customers as c ON o.customer_id = c.customer_id;


 -- Find the top 5 most expensive products --
 
 select product_name, list_price
 from products
 order by list_price desc
 limit 5;
 
 
 -- List products with no stock in any store --
 
select p.product_name
from products as p
left join  stocks as s on p.product_id = s.product_id
where s.quantity is null;
 
 
-- Get total revenue from each product --

select p.product_name, SUM(oi.quantity * oi.list_price) as revenue
from order_items  as oi
left join products p on oi.product_id = p.product_id
group by p.product_name;



-- Show order details with product names and quantity--

select o.order_id, p.product_name, oi.quantity
from orders as o
inner join order_items as oi on o.order_id = oi.order_id
inner join products as p on oi.product_id = p.product_id;


-- List all staff and the number of orders they handled --

select s.staff_id, s.first_name, s.last_name, COUNT(o.order_id) as total_orders
from staffs s
left join  orders o on  s.staff_id = o.staff_id
group by s.staff_id, s.first_name, s.last_name;



-- Revenue generated per category --

select c.category_name, SUM(oi.quantity * oi.list_price) as total_revenue
from  order_items as oi
inner join  products p on  oi.product_id = p.product_id
inner join categories c On p.category_id = c.category_id
group by c.category_name
order by total_revenue desc;


-- Total stock available in each store --

select  st.store_name, SUM(s.quantity) as total_stock
from stocks as s
inner join stores as st On s.store_id = st.store_id
group by st.store_name;


 -- Revenue generated by each store --


select  st.store_name, SUM(oi.quantity * oi.list_price) as  store_revenue
from orders  as o
JOIN order_items oi on  o.order_id = oi.order_id
inner join staffs s on  o.staff_id = s.staff_id
inner join stores st on  s.store_id = st.store_id
group by st.store_name
order by  store_revenue desc;


-- Most sold product by quantity --

select  p.product_name, SUM(oi.quantity) as total_quantity
from order_items  as oi
join products as p on oi.product_id = p.product_id
group by  p.product_name
order by total_quantity desc 
limit 1;


-- Most profitable product (based on revenue)--

select  p.product_name, SUM(oi.quantity * oi.list_price) as total_revenue
from order_items as oi
inner join  products as p on  oi.product_id = p.product_id
group by p.product_name
order by total_revenue desc
limit 1;


-- Percentage contribution of each category to total revenue --

select  c.category_name,
       ROUND(SUM(oi.quantity * oi.list_price) * 100.0 / 
             (select SUM(quantity * list_price) from  order_items), 2) as revenue_percentage
from order_items  as oi
inner join products as p on  oi.product_id = p.product_id
inner join  categories as c ON p.category_id = c.category_id
group by c.category_name;








