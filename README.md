# ADVANCE_SQL
Advanced SQL techniques like views, transactions, stored procedures, and joins 


# 1--VIEWS

**In SQL, a view is an alternative way of representing data that exists in one or more tables. Just like a real table, it contains rows and columns. The fields in a view are fields from one or more real tables in the database. Though views can be queried like a table, views are dynamic; only the definition of the view is stored, not the data.**


# Database Used in this Lab

**The database used in this lab is a sample HR database. This HR database schema consists of five tables called**

**EMPLOYEES**

**JOB_HISTORY**

**JOBS**

**DEPARTMENTS**

**LOCATIONS**

**Each table has a few rows of sample data.**


# CODE TO CREATE TABELS 

DROP TABLE IF EXISTS EMPLOYEES;
DROP TABLE IF EXISTS JOB_HISTORY;
DROP TABLE IF EXISTS JOBS;
DROP TABLE IF EXISTS DEPARTMENTS;
DROP TABLE IF EXISTS LOCATIONS;



CREATE TABLE EMPLOYEES (
                          EMP_ID CHAR(9) NOT NULL,
                          F_NAME VARCHAR(15) NOT NULL,
                          L_NAME VARCHAR(15) NOT NULL,
                          SSN CHAR(9),
                          B_DATE DATE,
                          SEX CHAR,
                          ADDRESS VARCHAR(30),
                          JOB_ID CHAR(9),
                          SALARY DECIMAL(10,2),
                          MANAGER_ID CHAR(9),
                          DEP_ID CHAR(9) NOT NULL,
                          PRIMARY KEY (EMP_ID)
                        );

CREATE TABLE JOB_HISTORY (
                            EMPL_ID CHAR(9) NOT NULL,
                            START_DATE DATE,
                            JOBS_ID CHAR(9) NOT NULL,
                            DEPT_ID CHAR(9),
                            PRIMARY KEY (EMPL_ID,JOBS_ID)
                          );

CREATE TABLE JOBS (
                    JOB_IDENT CHAR(9) NOT NULL,
                    JOB_TITLE VARCHAR(30) ,
                    MIN_SALARY DECIMAL(10,2),
                    MAX_SALARY DECIMAL(10,2),
                    PRIMARY KEY (JOB_IDENT)
                  );

CREATE TABLE DEPARTMENTS (
                            DEPT_ID_DEP CHAR(9) NOT NULL,
                            DEP_NAME VARCHAR(15) ,
                            MANAGER_ID CHAR(9),
                            LOC_ID CHAR(9),
                            PRIMARY KEY (DEPT_ID_DEP)
                          );

CREATE TABLE LOCATIONS (
                          LOCT_ID CHAR(9) NOT NULL,
                          DEP_ID_LOC CHAR(9) NOT NULL,
                          PRIMARY KEY (LOCT_ID,DEP_ID_LOC)
                        );


**Load all the tables with the data available in the CSV files shared below.**

Departments.csv --> [](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/datasets/HR_Database/Departments.csv)

Employees.csv --> [](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/MySQL/week2/data/Employees_updated.csv)

Jobs.csv --> [](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/datasets/HR_Database/Jobs.csv)

Locations.csv --> [](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/datasets/HR_Database/Locations.csv)

JobsHistory.csv --> [](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/MySQL/week2/data/JobsHistory.csv)

# Task 1: Create a View

**1)  Let's create a view called EMPSALARY to display salary along with some basic sensitive data of employees from the HR database. To create the EMPSALARY view from the EMPLOYEES**

CREATE VIEW EMPSALARY AS

SELECT EMP_ID, F_NAME, L_NAME, B_DATE, SEX, SALARY

FROM EMPLOYEES;

**2)  Using SELECT, query the EMPSALARY view to retrieve all the records.**

SELECT * FROM EMPSALARY;


# Task 2: Update a View

**Assume that the EMPSALARY view we created in Task 1 doesn't contain enough salary information, such as max/min salary and the job title of the employees. For this, we need to get information from other tables in the database. You need all columns from EMPLOYEES table used above, except for SALARY. You also need the columns JOB_TITLE, MIN_SALARY, MAX_SALARY of the JOBS table.**

CREATE OR REPLACE VIEW EMPSALARY AS

SELECT EMP_ID, F_NAME, L_NAME, B_DATE, SEX, JOB_TITLE,

MIN_SALARY, MAX_SALARY

FROM EMPLOYEES, JOBS

WHERE EMPLOYEES.JOB_ID = JOBS.JOB_IDENT;

# Task 3: Drop a View


DROP VIEW EMPSALARY;



# 2-- STORED PROCEDURE 

**Stored Procedures in SQL are a type of database object that allow you to encapsulate a series of SQL statements into a single routine. They are stored in the database data dictionary and can be invoked from an application program or from the database command interface. Stored procedures can accept input parameters and return multiple values of output parameters. They can also include control-of-flow constructs such as loops and conditional statements. Stored procedures offer several benefits including improved performance, higher productivity, ease of use, and increased scalability. They also provide a mechanism for enforcing business rules and data integrity in the database system.**

# Database Used in this Lab

**create a database PETS**

**create a new PETSALE table dropping any previous PETSALE table if exists, and will populate it with the required sample data.**

# CODE TO CREATE TABELS 

DROP TABLE IF EXISTS PETSALE;


 

CREATE TABLE PETSALE (
	ID INTEGER NOT NULL,
	ANIMAL VARCHAR(20),
	SALEPRICE DECIMAL(6,2),
	SALEDATE DATE,
	QUANTITY INTEGER,
	PRIMARY KEY (ID)
	);


INSERT INTO PETSALE VALUES
(1,'Cat',450.09,'2018-05-29',9),
(2,'Dog',666.66,'2018-06-01',3),
(3,'Parrot',50.00,'2018-06-04',2),
(4,'Hamster',60.60,'2018-06-11',6),
(5,'Goldfish',48.48,'2018-06-14',24);



SELECT * FROM PETSALE;



# Stored Procedure: Exercise 1

**1)  You will create a stored procedure routine named RETRIEVE_ALL.
This RETRIEVE_ALL routine will contain an SQL query to retrieve all the records from the PETSALE table, so you don't need to write the same query over and over again. You just call the stored procedure routine to execute the query everytime.**


DELIMITER //

CREATE PROCEDURE RETRIEVE_ALL()

BEGIN

   SELECT *  FROM PETSALE;
   
END //

DELIMITER ;

**2)  To call the RETRIEVE_ALL routine, open another SQL tab by clicking Open in new Tab**

CALL RETRIEVE_ALL;

**3)  If you wish to drop the stored procedure routine RETRIEVE_ALL,**

DROP PROCEDURE RETRIEVE_ALL;


# Stored Procedure: Exercise 2


**You will create a stored procedure routine named UPDATE_SALEPRICE with parameters Animal_ID and Animal_Health.**

**This UPDATE_SALEPRICE routine will contain SQL queries to update the sale price of the animals in the PETSALE table depending on their health conditions, BAD or WORSE.**

**This procedure routine will take animal ID and health conditon as parameters which will be used to update the sale price of animal in the PETSALE table by an amount depending on their health condition. Suppose that:**

1)  For animal with ID XX having BAD health condition, the sale price will be reduced further by 25%.

2)  For animal with ID YY having WORSE health condition, the sale price will be reduced further by 50%.

3)  For animal with ID ZZ having other health condition, the sale price won't change.

DELIMITER @

CREATE PROCEDURE UPDATE_SALEPRICE (IN Animal_ID INTEGER, IN Animal_Health VARCHAR(5)) 

BEGIN

    IF Animal_Health = 'BAD' THEN
    
        UPDATE PETSALE
        SET SALEPRICE = SALEPRICE - (SALEPRICE * 0.25)
        WHERE ID = Animal_ID;
    ELSEIF Animal_Health = 'WORSE' THEN
        UPDATE PETSALE
        SET SALEPRICE = SALEPRICE - (SALEPRICE * 0.5)
        WHERE ID = Animal_ID;
    ELSE
        UPDATE PETSALE
        SET SALEPRICE = SALEPRICE
        WHERE ID = Animal_ID;
    END IF;
END @

DELIMITER ;


# LETS CALL STORED PROCEDURES

CALL RETRIEVE_ALL;

CALL UPDATE_SALEPRICE(1, 'BAD');

CALL RETRIEVE_ALL;



# 3--ACID TRANSACTIONS


**A transaction is simply a sequence of operations performed using one or more SQL statements as a single logical unit of work. A database transaction must be ACID (Atomic, Consistent, Isolated and Durable). The effects of all the SQL statements in a transaction can either be applied to the database using the COMMIT command or undone from the database using the ROLLBACK command.**

**In this lab, you will learn some commonly used TCL (Transaction Control Language) commands of SQL through the creation of a stored procedure routine. You will learn about COMMIT, which is used to permanently save the changes done in the transactions in a table, and about ROLLBACK, which is used to undo the transactions that have not been saved in a table. ROLLBACK can only be used to undo the changes in the current unit of work.**


requires you to have the BankAccounts and ShoeShop tables populated with sample data. Download the BankAccounts-CREATE.sql and ShoeShop-CREATE.sql scripts below, and load them to the phpMyAdmin console. The scripts will create new tables called BankAccounts and ShoeShop and populate them with the sample data required for this lab.


# CODE 

**BankAccounts-CREATE.sql**



DROP TABLE IF EXISTS BankAccounts;


CREATE TABLE BankAccounts (
    AccountNumber VARCHAR(5) NOT NULL,
    AccountName VARCHAR(25) NOT NULL,
    Balance DECIMAL(8,2) CHECK(Balance>=0) NOT NULL,
    PRIMARY KEY (AccountNumber)
    );


    
INSERT INTO BankAccounts VALUES
('B001','Rose',300),
('B002','James',1345),
('B003','Shoe Shop',124200),
('B004','Corner Shop',76000);

-- Retrieve all records from the table

SELECT * FROM BankAccounts;


**ShoeShop-CREATE.sql**


DROP TABLE IF EXISTS ShoeShop;


CREATE TABLE ShoeShop (
    Product VARCHAR(25) NOT NULL,
    Stock INTEGER NOT NULL,
    Price DECIMAL(8,2) CHECK(Price>0) NOT NULL,
    PRIMARY KEY (Product)
    );

INSERT INTO ShoeShop VALUES
('Boots',11,200),
('High heels',8,600),
('Brogues',10,150),
('Trainers',14,300);



SELECT * FROM ShoeShop;


# CREATE TRANSACTION


Example of committing and rolling back a transaction.

Scenario: Rose is buying a pair of boots from ShoeShop. So we have to update Rose's balance as well as the ShoeShop balance in the BankAccounts table. Then we also have to update Boots stock in the ShoeShop table. After Boots, let's also attempt to buy Rose a pair of Trainers.
1) Once the tables are ready, create a stored procedure routine named TRANSACTION_ROSE that includes TCL commands like COMMIT and ROLLBACK.

2) Now develop the routine based on the given scenario to execute a transaction.
   
3) To create the stored procedure routine on MySQL, copy the code below and paste it to the textarea of the SQL page. Click Go.

DELIMITER //

CREATE PROCEDURE TRANSACTION_ROSE()
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        RESIGNAL;
    END;
    START TRANSACTION;
    UPDATE BankAccounts
    SET Balance = Balance-200
    WHERE AccountName = 'Rose';

    UPDATE BankAccounts
    SET Balance = Balance+200
    WHERE AccountName = 'Shoe Shop';

    UPDATE ShoeShop
    SET Stock = Stock-1
    WHERE Product = 'Boots';

    UPDATE BankAccounts
    SET Balance = Balance-300
    WHERE AccountName = 'Rose';

    COMMIT;
END //

DELIMITER ;

**Let's now check if the transaction can successfully be committed or not. Copy the code below in a new blank script and paste it to the textarea of the SQL page. Click Go**


CALL TRANSACTION_ROSE;

SELECT * FROM BankAccounts;

SELECT * FROM ShoeShop;

Observe that the transaction has been executed. But when we observe the tables, no changes have permanently been saved through COMMIT. All the possible changes happened might have been undone through ROLLBACK since the whole transaction fails due to the failure of a SQL statement or more. Let's go through the possible reason behind the failure of the transaction and how COMMIT - ROLLBACK works on a stored procedure:

The first three UPDATEs should run successfully. Both the balance of Rose and ShoeShop should have been updated in the BankAccounts table. The current balance of Rose should stand at 300 - 200 (price of a pair of Boots) = 100. The current balance of ShoeShop should stand at 124,200 + 200 = 124,400. The stock of Boots should also be updated in the ShoeShop table after the successful purchase for Rose, 11 - 1 = 10.

The last UPDATE statement tries to buy Rose a pair of Trainers, but her balance becomes insufficient (Current balance of Rose: 100 < Price of Trainers: 300) after buying a pair of Boots. So, the last UPDATE statement fails. Since the whole transaction fails if any of the SQL statements fail, the transaction won't be committed.





# 4--JOINS

**USING SAME TABLES FROM THE 'HR' DATABASE WHICH ARE USED EARLIER FOR VIEWS CONCEPT**

**1) Retrieve the names and job start dates of all employees who work for department number 5.**












