# MySQL-Database-Analysis Project

Project Title : Retail and Sales Analysis <br />
Level : Beginner to Advanced <br />
Database : classicmodels <br />

The aim of this project is to demonstrate SQL skills and techniques typically used for data analysis purpose, which include querying and extracting the data based on specific business questions through SQL Queries, and creating new tables with different checks and constraints for further analysis.
        

1. >**Fetch the employee number, first name and last name of those employees who are working as Sales Rep reporting to employee with employeenumber 1102.**

        SELECT distinct(employeeNumber), firstName, lastName
        FROM classicmodels.employees
        where jobTitle like 'Sales Rep' and reportsTo = 1102;

2. >**Show the unique productline values containing the word cars at the end from the products table.**
   
        SELECT distinct(productLine)
        FROM classicmodels.products
        where productLine like '%cars';

3. >**Using a CASE statement, segment customers into three categories based on their country: <br />
      a. "North America" for customers from USA or Canada <br />
      b. "Europe" for customers from UK, France, or Germany <br />
      c. "Other" for all remaining countries**

        SELECT customerNumber, customerName,
        case when country = 'USA' or  country = 'Canada' then 'North America'
        when country = 'UK' or country = 'France' or country = 'Germany' then 'Europe'
        else 'Other'
        end as CustomerSegment
        from classicmodels.customers;

4. >**Using the OrderDetails table, identify the top 10 products (by productCode) with the highest total order quantity across all orders.**
   
        SELECT productCode, sum(quantityOrdered) as total_ordered
        FROM classicmodels.orderdetails
        group by productCode
        order by total_ordered desc
        limit 10;

5. >**Company wants to analyse payment frequency by month. Extract the month name from the payment date to count the total number of payments for each month and include only those months with a payment count exceeding 20. Sort the results by total number of payments in descending order.**
   
       SELECT monthname(paymentDate) as payment_month, count(monthname(paymentDate)) as num_payments
       FROM classicmodels.payments
       group by payment_month
       having num_payments > 20
       order by num_payments desc;

 6. >**Create a new database named Customers_Orders and add the following tables as per the description
         Create a table named Customers to store customer information. Include the following columns.
         Add a NOT NULL constraint to the first_name and last_name columns to ensure they always have a value.**
    
        A. customer_id: This should be an integer set as the PRIMARY KEY and AUTO_INCREMENT.
        B. first_name: This should be a VARCHAR(50) to store the customer's first name.
        C. last_name: This should be a VARCHAR(50) to store the customer's last name.
        D. email: This should be a VARCHAR(255) set as UNIQUE to ensure no duplicate email addresses exist.
        E. phone_number: This can be a VARCHAR(20) to allow for different phone number formats.

            create database if not exists customer_orders;
            use customer_orders;
            create table customers (
                customer_id int auto_increment,
                first_name varchar(50) not null,
                last_name varchar(50) not null,
                email varchar(255) unique,
                phone_number varchar(20),
                primary key (customer_id)
                );
           select * from customers;
           desc customers;

7. >**Create a table named Orders to store information about customer orders. Include the following columns:**

    	order_id: This should be an integer set as the PRIMARY KEY and AUTO_INCREMENT.
        customer_id: This should be an integer referencing the customer_id in the Customers table  (FOREIGN KEY).
        order_date: This should be a DATE data type to store the order date.
        total_amount: This should be a DECIMAL(10,2) to store the total order amount.
     	
        Constraints:
        a) Set a FOREIGN KEY constraint on customer_id to reference the Customers table.
        b) Add a CHECK constraint to ensure the total_amount is always a positive value.

           create table orders(
                     order_id int auto_increment,
                     customer_id int,
                     order_date date,
                     total_amount decimal(10,2) check(total_amount>0),
                     primary key (order_id),
                     foreign key (customer_id) references customers(customer_id)
                     );
                     select * from orders;
                     desc orders;

8. >**List the top 5 countries (by order count) that Classic Models ships to.**
   
          SELECT customers.country, count(customers.country) as order_count
          FROM customers
          inner join orders ON customers.customerNumber = orders.customerNumber
          group by country
          order by order_count desc
          limit 5;

9. >**Create a table project with below fields.**
     ●	EmployeeID : integer set as the PRIMARY KEY and AUTO_INCREMENT. <br />
     ●	FullName: varchar(50) with no null values <br />
     ●	Gender : Values should be only ‘Male’  or ‘Female’ <br />
     ●	ManagerID: integer <br />
     ●  Find out the names of employees and their related managers.

           create table project(
			 EmployeeID int auto_increment,
             FullName varchar(50) not null,
             Gender varchar(10),
             Manager_id int,
             primary key (EmployeeID)
             );
           insert into project values (1,'Pranaya','Male',3),
                (2,'Priyanka','Female',1),
                (3,'Preety','Female',null),
                (4,'Anurag','Male',1),
                (5,'Sambit','Male',1),
                (6,'Rajesh','Male',3),
                (7,'Hina','Female',3);
                             
                select p1.FullName, p2.FullName
                from project p1
                join project p2
                on p1.EmployeeID = p2.Manager_id
                order by p1.FullName;

10. >**Create table facility. Add the below fields into it.** <br />
        ● Facility_ID <br />
        ● Name <br />
        ● State <br />
        ● Country <br />
        i) Alter the table by adding the primary key and auto increment to Facility_ID column. <br />
        ii) Add a new column city after name with data type as varchar which should not accept any null values.

        use classicmodels;
        create table facility(
             Facility_id int,
             FullName Varchar(100),
             State Varchar(100),
             Country Varchar(100)
             );
        alter table facility modify column Facility_id int auto_increment primary key;
        alter table facility add column City varchar(100) after FullName;
        alter table facility modify column City varchar(100) not null;
        select * from facility;
        desc facility;

11. >**Create a view named product_category_sales that provides insights into sales performance by product category. This view should include the following information:** <br />
       ● productLine: The category name of the product (from the ProductLines table). <br />
       ● total_sales: The total revenue generated by products within that category (calculated by summing the orderDetails.quantity * orderDetails.priceEach for each product in the category). <br />
       ● number_of_orders: The total number of orders containing products from that category. <br />

              CREATE VIEW product_category_sales AS
                    SELECT
                    pl.productLine AS productline,
                    SUM(od.quantityOrdered * od.priceEach) AS total_sales,
                    COUNT(DISTINCT o.orderNumber) AS number_of_orders
                    FROM
                    productlines pl
                    JOIN
                    products p ON pl.productLine = p.productLine
                    JOIN
                    orderdetails od ON p.productCode = od.productCode
                    JOIN
                    orders o ON od.orderNumber = o.orderNumber
                    GROUP BY
                    pl.productLine;
                    select * from product_category_sales;

12. >**Rank the customers based on their order frequency**

    		SELECT c.customerName,
    		 COUNT(o.orderNumber) AS order_count,
    		 RANK() OVER (ORDER BY COUNT(o.orderNumber) DESC) AS customer_rank
		 FROM customers c
		 JOIN 
    		 orders o ON c.customerNumber = o.customerNumber
		 GROUP BY 
    		 c.customerNumber, c.customerName
		 ORDER BY 
    		 customer_rank;
     		

