LAB 3 - FILTERING ROWS (WHERE, LIKE, IN, BETWEEN)
=================================================

ABOUT THIS LAB
--------------
Filtering rows with WHERE: BETWEEN, IN, NOT IN, LIKE with % and _ wildcards, YEAR(), LENGTH().

Number of questions: 11
Database used: hr

FILES
-----
queries3.txt        The questions and their queries (one query per line).
hr_database.sql      Creates and fills the hr database.
Lab_3_Outputs.pdf   Screenshots of each question, its query and its output.

REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client

HOW TO USE
----------
1. Open the MySQL Command Line Client and log in.
2. Load the database:  source C:/sqlshots/hr_database.sql
3. Select it:  use hr;
4. Run each query from queries3.txt and check the output.

NOTES
-----
- The hr database has no department 30, so Q2 and Q3 only show department 100 employees.
- Q4 shows Steven King (hired 1987) with the current hr_database.sql. An older version of the database returned an empty set.
- Q6 uses the job code SH_CLERK for Shipping Clerk and IT_PROG for Programmer.
- Q11 returns only King. There is no Blake, Scott or Ford in the data. MySQL ignores upper and lower case, so 'KING' matches King.
- Row counts and some values in Lab_3_Outputs.pdf come from the database version used when the screenshots were taken. With the current hr_database.sql a few counts may differ slightly.

QUESTIONS AND QUERIES
---------------------

Q1. Write a query to display the name (first_name, last_name) and salary for all employees whose salary is not in the range $10,000 through $15,000.
Query: select first_name, last_name, salary from employees where salary not between 10000 and 15000;

Q2. Write a query to display the name (first_name, last_name) and department ID of all employees in departments 30 or 100 in ascending order.
Query: select first_name, last_name, department_id from employees where department_id in (30, 100) order by first_name asc, last_name asc;

Q3. Write a query to display the name (first_name, last_name) and salary for all employees whose salary is not in the range $10,000 through $15,000 and are in department 30 or 100.
Query: select first_name, last_name, salary from employees where salary not between 10000 and 15000 and department_id in (30, 100);

Q4. Write a query to display the name (first_name, last_name) and hire date for all employees who were hired in 1987.
Query: select first_name, last_name, hire_date from employees where year(hire_date) = 1987;

Q5. Write a query to display the first_name of all employees who have both "b" and "c" in their first name.
Query: select first_name from employees where first_name like '%b%' and first_name like '%c%';

Q6. Write a query to display the last name, job, and salary for all employees whose job is that of a Programmer or a Shipping Clerk, and whose salary is not equal to $4,500, $10,000, or $15,000.
Query: select last_name, job_id, salary from employees where job_id in ('IT_PROG', 'SH_CLERK') and salary not in (4500, 10000, 15000);

Q7. Write a query to display the last name of employees whose names have exactly 6 characters.
Query: select last_name from employees where length(last_name) = 6;

Q8. Write a query to display the last name of employees having 'e' as the third character.
Query: select last_name from employees where last_name like '__e%';

Q9. Write a query to display the jobs/designations available in the employees table.
Query: select distinct job_id from employees;

Q10. Write a query to display the name (first_name, last_name), salary and PF (15% of salary) of all employees.
Query: select first_name, last_name, salary, salary * 0.15 as PF from employees;

Q11. Write a query to select all record from employees where last name in 'BLAKE', 'SCOTT', 'KING' and 'FORD'.
Query: select * from employees where last_name in ('BLAKE', 'SCOTT', 'KING', 'FORD');
