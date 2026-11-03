# SQL_Bharat_mart_project
Beginner-friendly SQL practice project. Includes 5 linked Excel datasets (employees, departments, customers, products, orders) and 24 hands-on tasks with sample solutions covering SELECT, WHERE, GROUP BY, HAVING, JOINs, INSERT, UPDATE and DELETE. Works with MySQL, PostgreSQL and SQLite.

--Creating Table employees
Drop table if exists employees
Create table employees(
		emp_id serial primary key,
		first_name varchar(15),
		last_name varchar(15),
		gender varchar (2),
		department_id int references departments(department_id),
		job_title varchar(30),
		monthly_salary float,
		hire_date date,
		city varchar(25)
		);

--Creating table departments
Drop table if exists departments;
Create table departments(
		department_id int primary key,
		department_name varchar(20),
		location varchar(25)
		);

--Creating table Customers
Drop table if exists customers
Create table customers(
		customer_id int primary key,
		customer_name varchar(50),
		email varchar(50),
		city varchar(25),
		state varchar(25),
		signup_date date
		);

--Creating table products
Drop table if exists products;
Create table products(
		product_id int primary key,
		product_name varchar(50),
		category varchar(30),
		price float,
		stock_qty int
		);

--Creating table orders
Drop table if exists orders
Create table orders(
		order_id int primary key,
		customer_id int references customers(customer_id),
		product_id int references products(product_id),
		quantity int,
		order_date date,
		status varchar(15)
		);

Select * from departments;
Select * from employees;
Select * from customers;
Select * from products;
Select * from orders;

--Easy level
--Task 1: View every column and row of the employees table.
Select * from employees;

--Task 2: Show only first_name, last_name and monthly_salary of all employees.
Select
		first_name,
		last_name, 
		monthly_salary
from employees;

--Task 3: List all employees who live in Mumbai.
Select * from employees
where city = 'Mumbai';

--Task 4: Find employees whose monthly_salary is greater than 60000.
Select * from employees
where monthly_salary > 60000;

--Task 5: List products in the 'Electronics' category that cost less than 2000.
select * from products
where category = 'Electronics'
and price < 2000;

--Task 6: Show customers who live in Delhi, Pune or Kanpur.
select * from customers
where city in ('Delhi', 'Pune', 'Kanpur');

--Task 7: Show the 5 highest-paid employees (name and salary).
Select concat(first_name, ' ', last_name) as name,
		monthly_salary
From employees
order by 2 desc
limit 5;

--Task 8: List the unique cities where customers live.
select distinct(city) as city
from customers;


Select * from departments;
Select * from employees;
Select * from customers;
Select * from products;
Select * from orders;

--Task 9: Find customers whose name starts with the letter 'A'.
Select * From customers
where customer_name like 'A%';

--Task 10: Find employees hired between 2020-01-01 and 2022-12-31.
Select * From employees
where hire_date between '2020-01-01' 
and '2022-12-31';

--Task 11: Find customers who have no email address.
Select * From customers
where email is null;

--Task 12: How many employees are there in total?
Select count(*) as total_employees
from employees;

--Easy-medium level

Select * from departments;
Select * from employees;
Select * from customers;
Select * from products;
Select * from orders;

--Task 12: How many employees are there in total?
Select distinct(count(emp_id))
from employees;

--Task 13: Find the minimum, maximum and average monthly_salary of employees.
Select min(monthly_salary) as min_salary,
		max(monthly_salary) as max_salary,
		avg(monthly_salary) as avg_salary
From employees;

--Task 14: Show first_name and annual salary (monthly_salary * 12) with the alias annual_salary.
Select first_name, 
		monthly_salary*12 as annual_salary
From employees;

--Task 15: Count how many employees work in each department_id.
Select department_id,
		Count (emp_id) as total_employees
From employees
Group by 1;

--Task 16: Find the total quantity ordered for each product_id in the orders table.
Select product_id, sum(quantity) as total_quantity
from orders
Group by order_id;

--Task 17: Show departments (by department_id) whose average monthly_salary is above 55000.
Select department_id, avg(monthly_salary) as avg_salary
From employees
Group by 1
Having avg(monthly_salary) > 55000;

--Task 18: How many orders have the status 'Cancelled'?
Select Count(*)
From orders
where status = 'Cancelled';

--Medium level

--Task 19: Show each employee's first_name along with their department_name.
select e.first_name, d.department_name
from employees e 
join departments d
on e.department_id = d.department_id

--Task 20: Show order_id, customer_name and order_date for all orders.
Select c.customer_name, o.order_id, o.order_date
From customers c
Join orders o
On c.customer_id = o.customer_id;

--Task 21: Calculate the total revenue (quantity * price) of all 'Delivered' orders.
Select p.price*o.quantity as total_revenue
From products p Join orders o
on p.product_id = o.product_id
where o.status = 'Delivered';

--Task 22: Add a new product: product_id 26, 'Laptop Stand', 'Accessories', 999, stock 50.
Insert into products(
			product_id, product_name, category, price, stock_qty)
Values (26, 'Laptop Stand', 'Accessories', 999, 50)

--Task 23: Increase the price of every 'Sports' product by 10%.
Update products
set price = price*1.10
Where category = 'Sports';

--Task 24: Delete all orders with the status 'Cancelled'.
Delete From orders
Where status = 'Cancelled';
