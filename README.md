# Pewlett Hackard Analysis with SQL

## Overview of Project

The following analysis is for the company Pewlett Hackard to determine the number of employees that will be retiring by title. Additionally, the Sales and Development Managers would like to institute a new mentor program where employees that are eligible for retirement transition to part-time status to train their replacement. This analysis identifies employees that would be eligible for said mentorship program. 

## Data and Resources

Data Source: 

- departments.csv
- dept_emp.csv
- dept_manager.csv
- employees.csv
- salaries.csv
- titles.csv
- Employee_Database_challenge.sql

Data Software: 

- PostgreSQL 11.16
- pgAdmin 4 v6.8

## Model 

The below Entity Relationship Diagram (ERD) visually represents the relationships between the six .csv files in the data source and identifies primary and foreign keys. </br>

![ERD](EmployeeDB.png)

I used the code below to determine the employees that were eligible for retirement and then the DISTINCT and COUNT functions to determine the number of upcoming retirees by title.

```
SELECT e.emp_no,
    e.first_name,
	e.last_name,
	t.title,
	t.from_date,
	t.to_date
INTO retirement_titles
FROM employees as e
LEFT JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY e.emp_no;
```

The following code determines the employees eligible for the mentorship program.

```
SELECT DISTINCT ON (e.emp_no) e.emp_no,
    e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	t.title
INTO mentorship_eligibility
FROM employees as e
LEFT JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
LEFT JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (de.to_date = '9999-01-01')
AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY e.emp_no;
```

## Results
- Of the 204,104 current Pewlett Hackard employees, there are **72,458 that are eligible for retirement**, which represents 36% of their current workforce.
- 36% of employees that are eligible for retirement are Senior Engineers and 34% are Senior Staff. 

<img width="243" alt="Screen Shot 2022-07-23 at 2 48 51 PM" src="https://user-images.githubusercontent.com/106405775/180620731-c4fbc5dc-885c-4a5c-8bdf-f6beb446aeae.png">

- There are **1,549 employees** that are eligible to participate as mentees in the new mentorship program.
- Of the employees that are eligible for the mentorship program, 708 are already in senior roles (Senior Staff and Senior Engineer).

## Summary
#### Silver Tsunami
More than a third of Pewlett Hackard's workforce is eligible for retirement. There are **72,458 roles** that will need to be filled in the next five years. 

#### Qualified, Retirement-Ready Employees for Mentorship Program 
There are enough qualified retirement-ready employees for the mentorship program. There are **50,842 employees** that are currently in senior roles (either Senior Staff or Senior Engineer). 

#### Prospective Mentees for Mentorship Program
As defined, Pewlett Hackard does not have enough prospective employees to be mentored by retiring employees. There are **1,549 prospective mentees** and 46% of these are already in senior roles. 

<img width="941" alt="Screen Shot 2022-07-23 at 3 59 58 PM" src="https://user-images.githubusercontent.com/106405775/180622776-93ee92cb-d303-4a0a-98f3-d0c07ce8c29b.png">

#### Expand the pool of Prospective Mentees
Currently the employees eligible to participate in the mentee program were only born in 1965, and almost half are already in senior roles. However, expanding the search to birth years between 1964 through 1972 and excluding people who are already senior staff (as show in the code below) brings in a more targeted list with 16,059 potential candidates.

```
SELECT DISTINCT ON (e.emp_no) e.emp_no,
    e.first_name,
	e.last_name,
	e.birth_date,
	de.from_date,
	de.to_date,
	t.title
INTO mentorship_eligibility_expanded
FROM employees as e
LEFT JOIN dept_emp as de
ON (e.emp_no = de.emp_no)
LEFT JOIN titles as t
ON (e.emp_no = t.emp_no)
WHERE (de.to_date = '9999-01-01')
AND (e.birth_date BETWEEN '1964-01-01' AND '1972-12-31')
AND t.title <> 'Senior Engineer' AND t.title <> 'Senior Staff'
ORDER BY e.emp_no;
```
