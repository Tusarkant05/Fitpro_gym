# Fitpro_gym_sql_project



Welcome to my SQL project, where I analyze real-time gym data from **FitPro Gym**! This project uses a dataset of **10,000 visit records** to explore and analyze gym membership and visit data, answering key business questions that can help a fitness center understand its customer base better and optimize its services.

## Table of Contents
- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Business Problems](#business-problems)
- [SQL Queries & Analysis](#sql-queries--analysis)
- [Contact Me](#contact-me)

---

## Introduction

This project aims to demonstrate essential SQL skills by analyzing a dataset from FitPro Gym. Using SQL, I explored membership details, member demographics, and visit patterns to derive actionable insights. This project showcases fundamental SQL techniques, including creating tables, writing queries, and analyzing data.

## Project Structure

1. **SQL Scripts**: Code to create the database schema and queries for analysis.
2. **Dataset**: Real-time data on gym visits, membership, and member demographics.
3. **Analysis**: SQL queries solving practical business problems, each one crafted to address specific questions.

---

## Database Schema

Here’s an overview of the database structure:

### 1. **Members Table**
- **member_id**: Unique identifier for each member
- **name**: Name of the member

### 2. **Memberships Table**
- **member_id**: Unique identifier linked to the `members` table
- **age**: Age of the member
- **gender**: Gender of the member ('M' or 'F')
- **membership_type**: Type of membership (e.g., Monthly, Quarterly)
- **join_date**: Date when the member joined
- **status**: Membership status (e.g., Active, Cancelled)

### 3. **Visits Table**
- **visit_id**: Unique identifier for each visit
- **member_id**: Linked to the `members` table
- **visit_date**: Date of the visit
- **check_in_time**: Check-in time of the visit
- **check_out_time**: Check-out time of the visit

- ## Business Problems

The following queries were created to solve specific business questions. Each query is designed to provide insights based on gym membership and visit data.

1. Retrieve the **name** and **membership_type** of female members.
2. Find members who have a **Monthly membership** and joined after **2023-11-01**.
3. List the **name** and **status** of active members over **25**.
4. Get details of **visits** on a specific date (**2024-01-01**).
5. List members with a **Quarterly membership** aged between **20 and 30**.
6. Find the total number visits made by each member.
7. Retrive the count of members by **membership_type** (e.g., how many monthly, weekly, and quartely members they are).
8. Get the **average age** of members.
9. Find the total number of visits for each **visit_date**.
10. Retrive the number of members with each status.
11. Retrive the top 3 members who have made the most visits, order by desc.
12. find the number of members with a monthly membership, who are active. Retrive the top 2.
13. Get the total number of visits for each member who has more than 2 visits, ordered by total_visits and display only the first 5 members.
14. Retrive the number of membrs who have joined in 2023, where the total number of members in each group is more than 100.
15. Get a list of distinct member ages.
16. Get the names and ages of members who have a monthly memberships and are younger than 25.
17. Find the average age of members who have a quarterly **membership_type**.
18. Find the name of the member who has made the most visits.

## Getting Started

## Creating Tables

```sql
CREATE TABLE members (
    member_id INT PRIMARY KEY,
    name VARCHAR(100)
);
```

```sql
CREATE TABLE memberships (
    member_id INT PRIMARY KEY,
    age INT,
    gender CHAR(1),
    membership_type VARCHAR(20),
    join_date DATE,
    status VARCHAR(20),
    FOREIGN KEY (member_id) REFERENCES members(member_id)
);
```

```sql
CREATE TABLE visits (
    visit_id INT PRIMARY KEY,
    member_id INT,
    visit_date DATE,
    check_in_time TIME,
    check_out_time TIME,
    FOREIGN KEY (member_id) REFERENCES members(member_id)
);
```

## Q&A
### 1.  Retrieve the **name** and **membership_type** of female members.
```sql
select 
	age,
	membership_type
from memberships
where gender = 'F';
```

### 2. Find members who have a **Monthly membership** and joined after **2023-11-01**.
```sql
select * 
from memberships
where 
	membership_type = 'Monthly' 
	and
	join_date > '2023-11-01';
```
### 3. List the **name** and **status** of active members over **25**.
```sql
select
	member_id,
	age,
	status
from memberships
where
	status = 'Active'
	and 
	age > 25;
```
### 4. Get details of **visits** on a specific date (**2024-01-01**).
```sql
select * 
from visits
where
	visit_date = '2024-01-01';
```
### 5. List members with a **Quarterly membership** aged between **20 and 30**.
```sql
select * 
from memberships
where
	membership_type = 'Quaterly'
	and 
	age between 20 and 30;
```
### 6. Find the total number visits made by each member.
```sql
select
	member_id,
	count(member_id)
from visits
group by
	member_id
order by 1;
```
### 7. Retrive the count of members by **membership_type** (e.g., how many monthly, weekly, and quartely members they are).
```sql
select 
	count(membership_type) as total_num,
	membership_type
from memberships
group by membership_type
order by 1;
```
### 8. Get the **average age** of members.
```sql
select 
	membership_type,
	avg(age) as avg_age
from memberships
group by 1;
```
### 9. Find the total number of visits for each **visit_date**.
```sql
select 
	count(*),
	visit_date
from visits
group by 2
order by 1;
```
### 10. Retrive the number of members with each status.
```sql
select
	membership_type,
	status
from memberships
where
	status = "Active" or "Cancelled"
group by 2;
```
### 11. Retrive the top 3 members who have made the most visits, order by desc.
```sql
select
	member_id,
	count(visit_date) as total_visit
from visits
group by 1 
order by 2 desc
limit 3;
```
### 12. find the number of members with a monthly membership, who are active. Retrive the top 2.
```sql
select 
	member_id,
	membership_type,
	status
from memberships
where status = 'Active'
	and
	membership_type = 'Monthly'
group by 2
order by 
limit 2
```
### 13. Get the total number of visits for each member who has more than 2 visits, ordered by total_visits and display only the first 5 members.
```sql
select
	member_id,
	count(*)as total_visit
from visits
group by 1
having count(*) >2
order by 2 desc
limit 5
```
### 14. Retrive the number of membrs who have joined in 2023, where the total number of members in each group is more than 100.
```sql
select 
	membership_type,
	count(*) as total_num_members
from memberships
where extract(year from join_date) = 2023
group by 1
having count(*) > 100
```
### 15. Get a list of distinct member ages.
```sql
select
	distinct(age) as unique_age
from memberships
```
### 16. Get the names and ages of members who have a monthly memberships and are younger than 25.
```sql
select 
	member_id,
	age,
	membership_type
from memberships
where 
	membership_type = 'Monthly'
	and
	age < 25
```
### 17. Find the average age of members who have a quarterly **membership_type**.
```sql
select
	membership_type,
	avg(age) as avg_age
from memberships
where membership_type = 'Quaterly'
group by 1
````
### 18. Find the name of the member who has made the most visits.
```sql
select 
	member_id,
	count(*) as total_visits
from visits
group by 1
order by 2 desc
```

## Contact Me

📄 **[Resume](#)**  
📧 **[Email](jtusarkant@gmail.com)**  
📞 **Phone**: +91-7684-028977  

--











