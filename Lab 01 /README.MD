LAB 1 - CREATE TABLE AND CONSTRAINTS
====================================

ABOUT THIS LAB
--------------
Creating tables and using constraints: NOT NULL, CHECK, DEFAULT, UNIQUE, PRIMARY KEY, AUTO_INCREMENT and FOREIGN KEY (with ON UPDATE / ON DELETE actions).

Number of questions: 20
Database used: lab 


REQUIREMENTS
------------
MySQL 8.0 with the Command Line Client


NOTES
-----
- Uses a separate database called lab, so your hr tables are never touched.
- Many questions create tables with the same name (countries, jobs, employees). Before every question shots_v2.py silently drops the old tables, so no 'table already exists' errors appear.
- Q2, Q3 and Q4 need a countries table first. Q14 to Q20 need a parent table (jobs or departments) for the foreign key. shots_v2.py creates these first and clears the screen, so only the question and its answer appear in the screenshot.
- Needs MySQL 8.0.16 or newer for the CHECK constraints in Q6, Q7 and Q8 (older versions accept CHECK but ignore it).
- Q5 uses NOT NULL on every column. Q8 uses varchar(10) for end_date because a DATE column cannot store the format --/--/----.
- Run the script again after any change to the query file to get fresh screenshots.

QUESTIONS AND QUERIES
---------------------

Q1. Write a SQL statement to create a simple table countries including columns country_id,country_name and region_id.
Query: create table countries (country_id varchar(2), country_name varchar(40), region_id int);

Q2. Write a SQL statement to create a simple table countries including columns country_id,country_name and region_id which is already exists.
Query: create table if not exists countries (country_id varchar(2), country_name varchar(40), region_id int);

Q3. Write a SQL statement to create the structure of a table dup_countries similar to countries.
Query: create table dup_countries like countries;

Q4. Write a SQL statement to create a duplicate copy of countries table including structure and data by name dup_countries.
Query: create table dup_countries as select * from countries;

Q5. Write a SQL statement to create a table countries set a constraint NULL.
Query: create table countries (country_id varchar(2) not null, country_name varchar(40) not null, region_id int not null);

Q6. Write a SQL statement to create a table named jobs including columns job_id, job_title, min_salary, max_salary and check whether the max_salary amount exceeding the upper limit 25000.
Query: create table jobs (job_id varchar(10), job_title varchar(35), min_salary decimal(6,0), max_salary decimal(6,0), check (max_salary <= 25000));

Q7. Write a SQL statement to create a table named countries including columns country_id, country_name and region_id and make sure that no countries except Italy, India and China will be entered in the table.
Query: create table countries (country_id varchar(2), country_name varchar(40) check (country_name in ('Italy', 'India', 'China')), region_id int);

Q8. Write a SQL statement to create a table named job_histry including columns employee_id, start_date, end_date, job_id and department_id and make sure that the value against column end_date will be entered at the time of insertion to the format like '--/--/----'.
Query: create table job_histry (employee_id int, start_date date, end_date varchar(10) check (end_date like '__/__/____'), job_id varchar(10), department_id int);

Q9. Write a SQL statement to create a table named countries including columns country_id,country_name and region_id and make sure that no duplicate data against column country_id will be allowed at the time of insertion.
Query: create table countries (country_id varchar(2) unique, country_name varchar(40), region_id int);

Q10. Write a SQL statement to create a table named jobs including columns job_id, job_title, min_salary and max_salary, and make sure that, the default value for job_title is blank and min_salary is 8000 and max_salary is NULL will be entered automatically at the time of insertion if no value assigned for the specified columns.
Query: create table jobs (job_id varchar(10), job_title varchar(35) default '', min_salary decimal(6,0) default 8000, max_salary decimal(6,0) default null);

Q11. Write a SQL statement to create a table named countries including columns country_id, country_name and region_id and make sure that the country_id column will be a key field which will not contain any duplicate data at the time of insertion.
Query: create table countries (country_id varchar(2) primary key, country_name varchar(40), region_id int);

Q12. Write a SQL statement to create a table countries including columns country_id, country_name and region_id and make sure that the column country_id will be unique and store an auto incremented value.
Query: create table countries (country_id int auto_increment unique, country_name varchar(40), region_id int);

Q13. Write a SQL statement to create a table countries including columns country_id, country_name and region_id and make sure that the combination of columns country_id and region_id will be unique.
Query: create table countries (country_id varchar(2), country_name varchar(40), region_id int, unique (country_id, region_id));

Q14. Write a SQL statement to create a table job_history including columns employee_id, start_date, end_date, job_id and department_id and make sure that, the employee_id column does not contain any duplicate value at the time of insertion and the foreign key column job_id contain only those values which are exists in the jobs table. Here is the structure of the table jobs;
Query: create table job_history (employee_id int unique, start_date date, end_date date, job_id varchar(10), department_id int, foreign key (job_id) references jobs (job_id));

Q15. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, email, phone_number hire_date, job_id, salary, commission, manager_id and department_id and make sure that, the employee_id column does not contain any duplicate value at the time of insertion and the foreign key columns combined by department_id and manager_id columns contain only those unique combination values, which combinations are exists in the departments table. Assume the structure of departments table below.
Query: create table employees (employee_id int unique, first_name varchar(20), last_name varchar(25), email varchar(25), phone_number varchar(20), hire_date date, job_id varchar(10), salary decimal(8,2), commission decimal(2,2), manager_id int, department_id int, foreign key (department_id, manager_id) references departments (department_id, manager_id));

Q16. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, email, phone_number hire_date, job_id, salary, commission, manager_id and department_id and make sure that, the employee_id column does not contain any duplicate value at the time of insertion, and the foreign key column department_id, reference by the column department_id of departments table, can contain only those values which are exists in the departments table and another foreign key column job_id, referenced by the column job_id of jobs table, can contain only those values which are exists in the jobs table. The InnoDB Engine have been used to create the tables. "A foreign key constraint is not required merely to join two tables. For storage engines other than InnoDB, it is possible when defining a column to use a REFERENCES tbl_name(col_name) clause, which has no actual effect, and serves only as a memo or comment to you that the column which you are currently defining is intended to refer to a column in another table." - Reference dev.mysql.com Assume that the structure of two tables departments and jobs.
Query: create table employees (employee_id int unique, first_name varchar(20), last_name varchar(25), email varchar(25), phone_number varchar(20), hire_date date, job_id varchar(10), salary decimal(8,2), commission decimal(2,2), manager_id int, department_id int, foreign key (department_id) references departments (department_id), foreign key (job_id) references jobs (job_id)) engine=innodb;

Q17. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, job_id, salary and make sure that, the employee_id column does not contain any duplicate value at the time of insertion, and the foreign key column job_id, referenced by the column job_id of jobs table, can contain only those values which are exists in the jobs table. The InnoDB Engine have been used to create the tables. The specialty of the statement is that, The ON UPDATE CASCADE action allows you to perform cross-table update and ON DELETE RESTRICT action reject the deletion. The default action is ON DELETE RESTRICT. Assume that the structure of the table jobs and InnoDB Engine have been used to create the table jobs. CREATE TABLE IF NOT EXISTS jobs ( JOB_ID integer NOT NULL UNIQUE PRIMARY KEY, JOB_TITLE varchar(35) NOT NULL DEFAULT ' ', MIN_SALARY decimal(6,0) DEFAULT 8000, MAX_SALARY decimal(6,0) DEFAULT NULL )ENGINE=InnoDB;
Query: create table employees (employee_id int unique, first_name varchar(25), last_name varchar(25), job_id int, salary decimal(8,2), foreign key (job_id) references jobs (job_id) on update cascade on delete restrict) engine=innodb;

Q18. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, job_id, salary and make sure that, the employee_id column does not contain any duplicate value at the time of insertion, and the foreign key column job_id, referenced by the column job_id of jobs table, can contain only those values which are exists in the jobs table. The InnoDB Engine have been used to create the tables. The specialty of the statement is that, The ON DELETE CASCADE that lets you allow to delete records in the employees(child) table that refer to a record in the jobs(parent) table when the record in the parent table is deleted and the ON UPDATE RESTRICT actions reject any updates. Assume that the structure of the table jobs and InnoDB Engine have been used to create the table jobs. CREATE TABLE IF NOT EXISTS jobs ( JOB_ID integer NOT NULL UNIQUE PRIMARY KEY, JOB_TITLE varchar(35) NOT NULL DEFAULT ' ', MIN_SALARY decimal(6,0) DEFAULT 8000, MAX_SALARY decimal(6,0) DEFAULT NULL )ENGINE=InnoDB;
Query: create table employees (employee_id int unique, first_name varchar(25), last_name varchar(25), job_id int, salary decimal(8,2), foreign key (job_id) references jobs (job_id) on delete cascade on update restrict) engine=innodb;

Q19. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, job_id, salary and make sure that, the employee_id column does not contain any duplicate value at the time of insertion, and the foreign key column job_id, referenced by the column job_id of jobs table, can contain only those values which are exists in the jobs table. The InnoDB Engine have been used to create the tables. The specialty of the statement is that, The ON DELETE SET NULL action will set the foreign key column values in the child table(employees) to NULL when the record in the parent table(jobs) is deleted, with a condition that the foreign key column in the child table must accept NULL values and the ON UPDATE SET NULL action resets the values in the rows in the child table(employees) to NULL values when the rows in the parent table(jobs) are updated. Assume that the structure of two table jobs and InnoDB Engine have been used to create the table jobs. CREATE TABLE IF NOT EXISTS jobs ( JOB_ID integer NOT NULL UNIQUE PRIMARY KEY, JOB_TITLE varchar(35) NOT NULL DEFAULT ' ', MIN_SALARY decimal(6,0) DEFAULT 8000, MAX_SALARY decimal(6,0) DEFAULT NULL )ENGINE=InnoDB;
Query: create table employees (employee_id int unique, first_name varchar(25), last_name varchar(25), job_id int, salary decimal(8,2), foreign key (job_id) references jobs (job_id) on delete set null on update set null) engine=innodb;

Q20. Write a SQL statement to create a table employees including columns employee_id, first_name, last_name, job_id, salary and make sure that, the employee_id column does not contain any duplicate value at the time of insertion, and the foreign key column job_id, referenced by the column job_id of jobs table, can contain only those values which are exists in the jobs table. The InnoDB Engine have been used to create the tables. The specialty of the statement is that, The ON DELETE NO ACTION and the ON UPDATE NO ACTION actions will reject the deletion and any updates. Assume that the structure of two table jobs and InnoDB Engine have been used to create the table jobs. CREATE TABLE IF NOT EXISTS jobs ( JOB_ID integer NOT NULL UNIQUE PRIMARY KEY, JOB_TITLE varchar(35) NOT NULL DEFAULT ' ', MIN_SALARY decimal(6,0) DEFAULT 8000, MAX_SALARY decimal(6,0) DEFAULT NULL )ENGINE=InnoDB;
Query: create table employees (employee_id int unique, first_name varchar(25), last_name varchar(25), job_id int, salary decimal(8,2), foreign key (job_id) references jobs (job_id) on delete no action on update no action) engine=innodb;
