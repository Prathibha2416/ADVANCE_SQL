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


# Load all the tables with the data available in the CSV files shared below.

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












