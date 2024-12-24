### Find Customer Referee

Table: `Customer`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| referee_id  | int     |
+-------------+---------+
In SQL, id is the primary key column for this table.
Each row of this table indicates the id of a customer, their name, and the id of the customer who referred them.
```

Find the names of the customer that are **not referred by** the customer with `id = 2`.

Return the result table in **any order**.

```SQL
SELECT name FROM Customer
WHERE referee_id <> 2 OR referee_id is null
```

Key point: In SQL, a comparison between a `null` value and any other value (including another `null`) using a comparison operator (eg `=`, `!=`, `<`, etc) will result in a `null`, which is considered as `false` for the purposes of a where clause (strictly speaking, it's "not true", rather than "false", but the effect is the same).

### Rising Temperature

Table: `Weather`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the column with unique values for this table.
There are no different rows with the same recordDate.
This table contains information about the temperature on a certain day.
```

 

Write a solution to find all dates' `id` with higher temperatures compared to its previous dates (yesterday).

Return the result table in **any order**.

*This question is hard for me as I do not know how to deal with dates and differences.*

```SQL
SELECT w1.id
FROM Weather w1, Weather w2
WHERE DATEDIFF(w1.recordDate, w2.recordDate) = 1 AND w1.temperature > w2.temperature;
```

### Average Time of Process per Machine

Table: `Activity`

```
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
It is guaranteed that each (machine_id, process_id) pair has a 'start' and 'end' timestamp.
```

 

There is a factory website that has several machines each running the **same number of processes**. Write a solution to find the **average time** each machine takes to complete a process.

The time to complete a process is the `'end' timestamp` minus the `'start' timestamp`. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the `machine_id` along with the **average time** as `processing_time`, which should be **rounded to 3 decimal places**.

Return the result table in **any order**.

*I could not solve this problem as I am not familiar with sub-queries*

```SQL
SELECT 
    machine_id,
    ROUND(AVG(processing_time), 3) AS processing_time
FROM (
    SELECT 
        machine_id, 
        process_id, 
        MAX(timestamp) - MIN(timestamp) AS processing_time
    FROM Activity
    GROUP BY machine_id, process_id
) AS ProcessTimes
GROUP BY machine_id;
```

Start thinking from the inner one and the larger one. 

### Students and Examinations

Table: `Students`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the primary key (column with unique values) for this table.
Each row of this table contains the ID and the name of one student in the school.
```

 

Table: `Subjects`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
subject_name is the primary key (column with unique values) for this table.
Each row of this table contains the name of one subject in the school.
```

 

Table: `Examinations`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each student from the Students table takes every course from the Subjects table.
Each row of this table indicates that a student with ID student_id attended the exam of subject_name.
```

 

Write a solution to find the number of times each student attended each exam.

Return the result table ordered by `student_id` and `subject_name`.

I did not solve this problem, so I copied from solutions

```SQL
SELECT 
    S.student_id,
    S.student_name,
    SU.subject_name,
    COUNT(E.student_id) attended_exams
FROM 
    Students S
CROSS JOIN 
    Subjects SU 
LEFT JOIN 
    Examinations E ON S.student_id=E.student_id AND SU.subject_name = E.subject_name
GROUP BY S.student_id, S.student_name, SU.subject_name
ORDER BY S.student_id, S.student_name, SU.subject_name
;
```

Points to remember:

1. `CROSS JOIN` can join tables without common fields. 
2. Before using `COUNT`, make sure we understand the groups.

### Managers with at Least 5 Direct Reports

Table: `Employee`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the name of an employee, their department, and the id of their manager.
If managerId is null, then the employee does not have a manager.
No employee will be the manager of themself.
```

 

Write a solution to find managers with at least **five direct reports**.

Return the result table in **any order**.

My answer:

```SQL
SELECT name 
FROM Employee
LEFT JOIN (
    SELECT managerID, COUNT(*) direct_report
    FROM Employee
    GROUP BY managerID
) AS temp
ON Employee.id = temp.managerID
WHERE direct_report >= 5 
```

More concise answer from solutions:

```SQL
SELECT e.name
FROM Employee AS e 
INNER JOIN Employee AS m ON e.id=m.managerId 
GROUP BY m.managerId 
HAVING COUNT(m.managerId) >= 5
```

### Confirmation Rate

Table: `Signups`

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
user_id is the column of unique values for this table.
Each row contains information about the signup time for the user with ID user_id.
```

 

Table: `Confirmations`

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
(user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.
user_id is a foreign key (reference column) to the Signups table.
action is an ENUM (category) of the type ('confirmed', 'timeout')
Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').
```

 

The **confirmation rate** of a user is the number of `'confirmed'` messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is `0`. Round the confirmation rate to **two decimal** places.

Write a solution to find the **confirmation rate** of each user.

Return the result table in **any order**.

I copied this answer from the solutions. The only thing I did not figure out is `round(avg(if(c.action="confirmed",1,0)),2)`.

```SQL
SELECT S.user_id, round(avg(if(c.action="confirmed",1,0)),2) confirmation_rate
FROM Signups S
LEFT JOIN Confirmations C
ON S.user_id=C.user_id
GROUP BY S.user_id;
```

Points:

1. This `IF` function can create a column from the action column! This basically replaces the strings with integers
2. The author cleverly converted the problem of calculating rates into calculating the average of 0's and 1's. 

### Not Boring Movies

Table: `Cinema`

```
+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
id is the primary key (column with unique values) for this table.
Each row contains information about the name of a movie, its genre, and its rating.
rating is a 2 decimal places float in the range [0, 10]
```

 

Write a solution to report the movies with an odd-numbered ID and a description that is not `"boring"`.

Return the result table ordered by `rating` **in descending order**.

```SQL
select *
from Cinema
where id % 2 != 0 and description != 'boring'
order by rating desc;
```

### Average Selling Price

Table: `Prices`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
(product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the price of the product_id in the period from start_date to end_date.
For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.
```

 

Table: `UnitsSold`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
This table may contain duplicate rows.
Each row of this table indicates the date, units, and product_id of each product sold. 
```

 

Write a solution to find the average selling price for each product. `average_price` should be **rounded to 2 decimal places**. If a product does not have any sold units, its average selling price is assumed to be 0.

Return the result table in **any order**.

```SQL
select product_id, ifnull(round(sum(price * units)/sum(units), 2), 0) average_price
from (
select p.product_id, p.price, u.units
from Prices p
left join UnitsSold u
on p.product_id=u.product_id and p.start_date <= u.purchase_date and u.purchase_date <= p.end_date
) as n
group by product_id;
```

### Percentage of Users Attended a Contest

Table: `Users`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+
user_id is the primary key (column with unique values) for this table.
Each row of this table contains the name and the id of a user.
```

 

Table: `Register`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+
(contest_id, user_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id of a user and the contest they registered into.
```

 

Write a solution to find the percentage of the users registered in each contest rounded to **two decimals**.

Return the result table ordered by `percentage` in **descending order**. In case of a tie, order it by `contest_id` in **ascending order**.

```SQL
select contest_id, round(count(*) * 100 / (
    select count(*)
    from Users
), 2) as percentage
from Register
group by contest_id
order by percentage desc, contest_id asc;
```

### Queries Quality and Percentage

Table: `Queries`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
+-------------+---------+
This table may have duplicate rows.
This table contains information collected from some queries on a database.
The position column has a value from 1 to 500.
The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.
```

 

We define query `quality` as:

> The average of the ratio between query rating and its position.

We also define `poor query percentage` as:

> The percentage of all queries with rating less than 3.

Write a solution to find each `query_name`, the `quality` and `poor_query_percentage`.

Both `quality` and `poor_query_percentage` should be **rounded to 2 decimal places**.

Return the result table in **any order**.

```SQL
select 
    query_name,
    round(avg(rating/position), 2) as quality,
    round(avg(if(rating < 3, 1, 0))*100, 2) as poor_query_percentage
from Queries
group by query_name;
```

### Monthly Transactions I

Table: `Transactions`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| country       | varchar |
| state         | enum    |
| amount        | int     |
| trans_date    | date    |
+---------------+---------+
id is the primary key of this table.
The table has information about incoming transactions.
The state column is an enum of type ["approved", "declined"].
```

 

Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

Return the result table in **any order**.

```sql
select 
    date_format(trans_date, '%Y-%m') as month,
    country,
    count(*) as trans_count,
    sum(if(state='approved', 1, 0)) as approved_count,
    sum(amount) as trans_total_amount,
    sum(amount * if(state='approved', 1, 0)) as approved_total_amount 
from Transactions 
group by country, month;
```

I learned a new function here: `date_format(trans_date, '%Y-%m')` changed from the regular date to only year and month.

### Immediate Food Delivery II

Table: `Delivery`

```
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
delivery_id is the column of unique values of this table.
The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).
```

 

If the customer's preferred delivery date is the same as the order date, then the order is called **immediate;** otherwise, it is called **scheduled**.

The **first order** of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the percentage of immediate orders in the first orders of all customers, **rounded to 2 decimal places**.

```SQL
select round(avg(order_date = customer_pref_delivery_date)*100, 2) as immediate_percentage 
from Delivery
where (customer_id, order_date) in (
    select customer_id, min(order_date) # find the order date of the first order
    from Delivery
    group by customer_id
);
```

Point: Remember where in statement (use with subquery)

### Game Play Analysis IV

Table: `Activity`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
(player_id, event_date) is the primary key (combination of columns with unique values) of this table.
This table shows the activity of players of some games.
Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on someday using some device.
```

 

Write a solution to report the **fraction** of players that logged in again on the day after the day they first logged in, **rounded to 2 decimal places**. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

```SQL
select round(count(distinct player_id) / (select count(distinct player_id) from Activity), 2) as fraction
from Activity 
where (player_id, event_date - interval 1 day) in (
    select player_id, min(event_date) from Activity group by player_id # find the first login date
);
```

Points:

1. where in statement (use with subquery)
2. The arithmetic with dates are different. 

### User Activity for the Past 30 Days I

Table: `Activity`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |
+---------------+---------+
This table may have duplicate rows.
The activity_type column is an ENUM (category) of type ('open_session', 'end_session', 'scroll_down', 'send_message').
The table shows the user activities for a social media website. 
Note that each session belongs to exactly one user.
```

 

Write a solution to find the daily active user count for a period of `30` days ending `2019-07-27` inclusively. A user was active on someday if they made at least one activity on that day.

Return the result table in **any order**.

```sql
select activity_date as day, count(distinct user_id) as active_users
from Activity
where activity_date <= '2019-07-27' and activity_date >= '2019-07-27' - interval 29 day
group by activity_date;
```

### Product Sales Analysis III

Table: `Sales`

```
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.
```

 

Table: `Product`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.
```

 

Write a solution to select the **product id**, **year**, **quantity**, and **price** for the **first year** of every product sold.

Return the resulting table in **any order**.

```sql
select product_id, year as first_year, quantity, price
from Sales
where (product_id, year) in (
    select product_id, min(year) as first_year
    from Sales 
    group by product_id
);
```

### Classes More Than 5 Students

Table: `Courses`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the name of a student and the class in which they are enrolled.
```

 

Write a solution to find all the classes that have **at least five students**.

Return the result table in **any order**.

```sql
select class 
from (
    select class, count(*) as cnt
    from courses 
    group by class
) as temp
where cnt >= 5;
```

The above is my answer, but I think it is necessary to learn the difference between `where` and `having`.

### Biggest Single Number

Table: `MyNumbers`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
+-------------+------+
This table may contain duplicates (In other words, there is no primary key for this table in SQL).
Each row of this table contains an integer.
```

 

A **single number** is a number that appeared only once in the `MyNumbers` table.

Find the largest **single number**. If there is no **single number**, report `null`.

```sql
select max(num) as num from (
    select num, count(*) as cnt
    from MyNumbers
    group by num
    having cnt=1
) as temp;
```

I learned how to use `having` clause in this question. Good job!

### The Number of Employees Which Report to Each Employee

Table: `Employees`

```
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |
+-------------+----------+
employee_id is the column with unique values for this table.
This table contains information about the employees and the id of the manager they report to. Some employees do not report to anyone (reports_to is null). 
```

 

For this problem, we will consider a **manager** an employee who has at least 1 other employee reporting to them.

Write a solution to report the ids and the names of all **managers**, the number of employees who report **directly** to them, and the average age of the reports rounded to the nearest integer.

Return the result table ordered by `employee_id`.

```sql
select e1.employee_id, e1.name, count(e2.employee_id) as reports_count, round(avg(e2.age)) as average_age
from Employees e1
join Employees e2
on e1.employee_id=e2.reports_to
group by employee_id
order by employee_id;
```

This question took me a long time. I did not want to use `join` to solve this. But it turns out to be more convenient in this way. 

### Consecutive Numbers

Table: `Logs`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
In SQL, id is the primary key for this table.
id is an autoincrement column starting from 1.
```

 

Find all numbers that appear at least three times consecutively.

Return the result table in **any order**.

*I didn't solve this question by myself.*

```sql 
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l1.id = l2.id - 1
JOIN Logs l3 ON l1.id = l3.id - 2
WHERE l1.num = l2.num AND l2.num = l3.num;
```

I joined once and didn't get the correct solution. It looks like we should join two times to solve.

### Product Price at a Given Date

Table: `Products`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
(product_id, change_date) is the primary key (combination of columns with unique values) of this table.
Each row of this table indicates that the price of some product was changed to a new price at some date.
```

 

Write a solution to find the prices of all products on `2019-08-16`. Assume the price of all products before any change is `10`.

Return the result table in **any order**.

*This question is really hard for me. I could not solve it. Need to review for sure.*

```sql
select product_id, new_price as price
from Products
where (product_id, change_date) in 
 (# find the most recent date before 2019-08-16 for each product id 
select product_id, max(change_date)
from Products
where change_date <= '20190816'
group by product_id
) 
union 
select product_id, 10 as price # second part, all products whose date are all later than ...
from Products
group by product_id
having min(change_date) > '20190816'
```

### Count Salary Categories

Table: `Accounts`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
account_id is the primary key (column with unique values) for this table.
Each row contains information about the monthly income for one bank account.
```

 

Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

- `"Low Salary"`: All the salaries **strictly less** than `$20000`.
- `"Average Salary"`: All the salaries in the **inclusive** range `[$20000, $50000]`.
- `"High Salary"`: All the salaries **strictly greater** than `$50000`.

The result table **must** contain all three categories. If there are no accounts in a category, return `0`.

Return the result table in **any order**.

*I could not solve this either.*

```sql 
select 'Low Salary' as Category, count(*) as accounts_count
from Accounts
where income < 20000
union
select 'Average Salary' as Category, count(*) as accounts_count
from Accounts
where income between 20000 and 50000
union
select 'High Salary' as Category, count(*) as accounts_count
from Accounts
where income > 50000
```

### Exchange Seats

Table: `Seat`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
id is the primary key (unique value) column for this table.
Each row of this table indicates the name and the ID of a student.
The ID sequence always starts from 1 and increments continuously.
```

 

Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by `id` **in ascending order**.

```sql
select row_number() OVER (ORDER BY swap_id) AS id, student
from 
(select 
    case 
    when id % 2 = 0 then id - 1
    when id % 2 = 1 then id + 1
    end
    as swap_id, student
from Seat
order by swap_id) temp
```

Points:

1. `row_number` can create a new id for me.
2. `over` function needs to be summarized

```sql
<window_function>() OVER (    
    [PARTITION BY column_name]    
    [ORDER BY column_name ASC|DESC]    
    [window_frame] 
)
```

### Restaurant Growth

Table: `Customer`

```
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
In SQL,(customer_id, visited_on) is the primary key for this table.
This table contains data about customer transactions in a restaurant.
visited_on is the date on which the customer with ID (customer_id) has visited the restaurant.
amount is the total paid by a customer.
```

 

You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).

Compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). `average_amount` should be **rounded to two decimal places**.

Return the result table ordered by `visited_on` **in ascending order**.

*Really hard. I don't know how to solve it.*

```sql 
SELECT visited_on, amount, ROUND(amount/7, 2) average_amount
FROM (
    SELECT DISTINCT visited_on, 
    SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY   PRECEDING AND CURRENT ROW) amount, 
    MIN(visited_on) OVER() 1st_date 
    FROM Customer
) t
WHERE visited_on>= 1st_date+6;
```

Points:

1. ` RANGE BETWEEN INTERVAL 6 DAY   PRECEDING AND CURRENT ROW` very hard to think of.
2. empty `over()` would help 1 answer for each row. 

### Department Top Three Salaries

Table: `Employee`

```
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
id is the primary key (column with unique values) for this table.
departmentId is a foreign key (reference column) of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.
```

 

Table: `Department`

```
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of a department and its name.
```

 

A company's executives are interested in seeing who earns the most money in each of the company's departments. A **high earner** in a department is an employee who has a salary in the **top three unique** salaries for that department.

Write a solution to find the employees who are **high earners** in each of the departments.

Return the result table **in any order**.

*This is the only **hard** question in this SQL50 list.*

```sql
with cte as (
    select 
        e.id, 
        e.name, 
        e.salary, 
        d.name as d_name, 
        dense_rank() over (partition by e.departmentid order by e.salary desc) as rnk
    from employee e
    join department d
    on e.departmentid=d.id
)
select d_name as Department, name as Employee, Salary
from cte
where rnk <= 3;
```

Points:

1. Learn to use `dense_rank()` function to find top 3 unique high earners
2. `with` function is sometimes easier to read than subqueries.

### Second Highest Salary

Table: `Employee`

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.
```

 

Write a solution to find the second highest **distinct** salary from the `Employee` table. If there is no second highest salary, return `null (return None in Pandas)`.

```sql
SELECT
    (SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1) AS SecondHighestSalary
```

Points:

1. `offset` to skip



## The comparison between `where` and `having`



```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

| Feature                | `WHERE` Clause                                               | `HAVING` Clause                                              |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Purpose**            | Filters rows before grouping or aggregation.                 | Filters groups after grouping or aggregation.                |
| **Applies To**         | Individual rows in the table.                                | Aggregated data or grouped results.                          |
| **Use With**           | Non-aggregated conditions (columns, literals).               | Aggregated conditions (e.g., `SUM()`, `COUNT()`).            |
| **Order of Execution** | Executed before the `GROUP BY` clause.                       | Executed after the `GROUP BY` clause.                        |
| **Example**            | `SELECT department_id, salary FROM Employees WHERE salary > 50000;` | `SELECT department_id, SUM(salary) AS total_salary FROM Employees GROUP BY department_id HAVING total_salary > 200000;` |
| **Main Difference**    | Filters data at the row level.                               | Filters data at the group level.                             |



## How to use `over`?

#### Components:

1. **`<window_function>()`**:
   - A function to perform the operation (e.g., `SUM()`, `AVG()`, `ROW_NUMBER()`, etc.).
2. **`PARTITION BY`** *(optional)*:
   - Divides the data into partitions (subgroups) for the calculation.
   - If omitted, the function operates on the entire dataset.
3. **`ORDER BY`** *(optional)*:
   - Specifies the order of rows within each partition.
   - Necessary for operations like cumulative totals or rankings.
4. **`window_frame`** *(optional)*:
   - Defines the range of rows included in the calculation relative to the current row.
   - Common clauses:
     - `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` (all rows up to the current row).
     - `ROWS BETWEEN n PRECEDING AND CURRENT ROW` (a fixed number of rows preceding the current row).

Common Window Functions:

1. **Aggregate Functions**:
   - `SUM()`, `AVG()`, `MAX()`, `MIN()`, `COUNT()`.
2. **Ranking Functions**:
   - `ROW_NUMBER()`: Assigns a unique row number to each row.
   - `RANK()`: Assigns ranks with gaps for ties.
   - `DENSE_RANK()`: Assigns ranks without gaps for ties.
3. **Value Functions**:
   - `FIRST_VALUE()`: Returns the first value in the window.
   - `LAST_VALUE()`: Returns the last value in the window.
   - `LAG()`, `LEAD()`: Access previous or next row values.



## String and Regexp

1. RegExp operations

```sql
WHERE column_name REGEXP '\\bword\\b' # match exact word
WHERE column_name REGEXP '^word' # match at the start of a string
WHERE column_name REGEXP 'word$' # match at the end of a string
WHERE column_name REGEXP 'word' # match anywhere of a string
WHERE column_name REGEXP '(^|\\s)word($|\\s)' # match a distinct word
WHERE column_name REGEXP '[0-9]+' # match digits
WHERE column_name REGEXP '^[a-zA-Z]+$' # match letters only
WHERE column_name REGEXP '^[a-zA-Z0-9]+$' # Match Alphanumeric Characters
WHERE column_name REGEXP '\\s' # Match Whitespace
```

2. Strings Operations

```sql
SELECT CONCAT(string1, string2) AS concatenated_string; # concatenate strings
SELECT SUBSTRING(column_name, start_position, length) AS substring; # extract substring
SELECT REPLACE(column_name, 'old_value', 'new_value') AS replaced_string; # replace subtring
SELECT POSITION('substring' IN column_name) AS position; # find position of substring
SELECT UPPER(column_name) AS uppercase_string, LOWER(column_name) AS lowercase_string; # convert to uppercase / lowercase
SELECT TRIM(column_name) AS trimmed_string; # trim whitespace
SELECT LPAD(column_name, total_length, 'pad_char') AS left_padded,
       RPAD(column_name, total_length, 'pad_char') AS right_padded; # pad strings
```

