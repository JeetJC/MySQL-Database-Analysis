# MySQL-Database-Analysis

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
                first_name varchar(30) not null,
                last_name varchar(30) not null,
                email varchar(250) unique,
                phone_number varchar(20),
                primary key (customer_id)
                );
           select * from customers;
           desc customers;
            




     

