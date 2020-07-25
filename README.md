# Basic-SQL-sample

Below is a sample SQL code for everyday activities in a business. The schema used for this code is composed of two main sets of data, one with the information from the business' employees, and one with all the client's. The purpose of this code is to highlight some of my abilities querying, building and organizing databases.

## Creating Tables, Delimeters and inserting values

```sql
###Creating table of younger male employees
CREATE TABLE Young_Male_Employees(
emp_id INT PRIMARY KEY,
first_name VARCHAR(40),
last_name VARCHAR(40),
birth_day DATE,
sex VARCHAR(1),
SALARY INT,
branch_id INT,
super_id INT
);
```

```sql
##Populating table from main employee data
INSERT INTO Database1.Young_Male_Employees
SELECT * FROM Database1.Employee WHERE sex='M'
AND birth_day > '1993-01-01';
```

```mysql
##Creating a Trigger that will keep track of every employee hired by company
DELIMITER $$
CREATE
    TRIGGER my_trigger BEFORE INSERT
    ON employee 
    FOR EACH ROW BEGIN
        IF NEW.sex='M' THEN 
            INSERT INTO employee_tracker VALUES('Added male employee');
        ELSEIF NEW.sex='F' THEN
            INSERT INTO employee_tracker VALUES('Added female employee');
        ELSE 
            INSERT INTO employee_tracker VALUES('Added new employee');
        END IF;
    END$$
DELIMITER ;   
```

## Examples of Joining, Cases, Wildcards and Updating

```mysql
##Using a SELF JOIN to find a list of clients who live in the same city
SELECT a.city, CONCAT(a.first_name,' ',a.last_name client_1),
CONCAT(b.first_name,' ',b.last_name client_2)
FROM
Database1.client a
INNER JOIN Database1.client b ON (a.city=b.city)
WHERE a.client_id<>b.client_id
ORDER BY
city,
client_1,
client_2;
```

```mysql
##Using Cases to categorize client priority based on sales
SELECT client.client_name, 
                    CASE WHEN total_sales<10000 THEN 'Low'
                         WHEN total_sales BETWEEN 10000 AND 100000 THEN 'Medium'
                         WHEN total_sales>100000 THEN 'High' END AS Priority 
FROM client JOIN works_with ON (client.client_id=works_with.client_id)
ORDER BY total_sales;
```

```mysql
##Utilizing Wildcards to find all clients that are LLC's in the
SELECT client.client_name
FROM Database1.client
WHERE client_name LIKE '%LLC%';
```

```mysql
##Formating all client's phone numbers
UPDATE Database1.client
SET phone_number = CONCAT(SUBSTRING(phone_number,1,3),'-',
SUBSTRING(phone_number, 4,3),'-',
SUBSTRING(phone_number, 7,4));
```
