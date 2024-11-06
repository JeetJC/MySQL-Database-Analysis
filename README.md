# MySQL-Database-Analysis

Project Title : Retail and Sales Analysis <br />
Level : Beginner to Advanced <br />
Database : classicmodels <br />

The aim of this project is to demonstrate SQL skills and techniques typically used for data analysis purpose, which include querying and extracting the data based on specific business questions through SQL Queries, and creating new tables with different checks and constraints for further analysis.

1. >**Fetch the employee number, first name and last name of those employees who are working as Sales Rep reporting to employee with employeenumber 1102.**

        SELECT distinct(employeeNumber), firstName, lastName
        FROM classicmodels.employees
        where jobTitle like 'Sales Rep' and reportsTo = 1102;
