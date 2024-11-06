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
     

