# Structured Query Language (SQL) (2/2)

### Introduction

In our previous lecture, we discussed basic SQL Data Definition Language (DDL) commands like `CREATE TABLE` and Data Manipulation Language (DML) queries for one table.

In this lecture, we will continue our discussion about SQL, focusing on:

- Writing more complex SQL queries involving multiple tables.
- Creation of subqueries, joins, and set operations.
- Uniquely addressing attributes.
- Modification of data using SQL.

## Basic SQL Concepts

**Quiz**: A simple SQL query needs to have at least a `SELECT` and a `FROM` clause.

## SQL with Multiple Relations

We can write SQL queries that involve multiple tables. SQL implements a *general loop semantics* for this process:

1. Loop through all rows in all tables.
2. For each combination, check if the condition is true.
3. If the condition is true, project the rows onto the desired attributes.

Note that duplicates are still kept in this process.

Here is an example of a SQL query that involves multiple tables:

```sql
SELECT 'group'
FROM
Meetings, People
WHERE date = '2017-06-09' AND
owner = userid;
```

**Quiz**: A correct SQL query with multiple tables is as follows:

```sql
SELECT name
FROM People, Rooms
WHERE capacity = '6' AND room = office;
```

## Avoiding Name Clashes

Prefixing attributes with table names can be used to avoid name clashes in SQL queries. For example:

```sql
SELECT P.name AS name
FROM
People P, Meetings M
WHERE M.date = '2017-06-09' AND
M.owner = P.userid;
```

**Quiz**: To find all pairs of roommates from the People table, we can use a self-join:

```sql
SELECT p1.name, p2.name
FROM
People p1, People p2
WHERE p1.office = p2.office AND p1.name < p2.name;
```

## Subqueries

A subquery is a query nested inside another SQL query and can be used in SELECT, FROM, or WHERE clauses. The result of a subquery can be used as a constant (SELECT and WHERE) or as a relation (FROM and WHERE).

For example:

```sql
-- Find Annika's roommates
SELECT name
FROM
People
WHERE office = (SELECT office
FROM
People
WHERE userid = 'aas');

-- Which people have projectors in their office?
SELECT name
FROM
People
WHERE office IN (SELECT room
FROM
Equipment
WHERE type = 'projector');
```

### Correlated Subqueries

A correlated subquery is a subquery that uses values from the outer query. Here is an example that identifies which meetings exceed the room capacity:

```sql
SELECT meetid
FROM
Meetings
WHERE (SELECT COUNT(DISTINCT pid)
FROM
Participants
WHERE meetid = Meetings.meetid AND
status <> 'declined' AND
pid NOT IN (SELECT room
FROM
Rooms)
)
>
(SELECT capacity
FROM Rooms, Participants
WHERE room = pid AND meetid = Meetings.meetid);
```

## Set Operations: UNION, INTERSECT and EXCEPT

SQL supports set operations that can be used to combine rows from two or more tables.

```sql
-- UNION
SELECT owner AS userid, meetid
FROM
Meetings
EXCEPT
SELECT pid AS userid, meetid
FROM
Participants;
```

**Note**: In MySQL, INTERSECT and EXCEPT are not supported. You can use EXISTS and NOT EXISTS instead.

## JOIN Operator

The JOIN operator is a syntactic convenience for having two tables

## SQL UPDATE Statement

The `UPDATE` statement is used to modify the existing records in a table. 

Here is the basic syntax of the SQL UPDATE statement:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

- `table_name`: Name of the table.
- `column1, column2, ...`: Columns in the table to be updated.
- `value1, value2, ...`: New values to replace the existing values of the specified columns.
- `WHERE condition`: Specifies which records should be updated. If omitted, all records will be updated!

**Note**: Be careful with the WHERE clause in the UPDATE statement. The WHERE clause specifies which record(s) should be updated. If you omit the WHERE clause, all records in the table will be updated!

Here's an example. Let's suppose we have a `Customers` table:

| ID | NAME | AGE | ADDRESS   | SALARY |
|----|------|-----|-----------|--------|
| 1  | Ravi | 32  | Ahmedabad | 2000.0 |
| 2  | Mohan| 30  | Delhi     | 4000.0 |
| 3  | Ali  | 19  | Mumbai    | 2000.0 |

And we want to update the ADDRESS for the customer whose ID equals 1, we'd use the following SQL query:

```sql
UPDATE Customers
SET ADDRESS = 'Pune'
WHERE ID = 1;
```

After executing the above SQL statement, the `Customers` table will be as follows:

| ID | NAME | AGE | ADDRESS | SALARY |
|----|------|-----|---------|--------|
| 1  | Ravi | 32  | Pune    | 2000.0 |
| 2  | Mohan| 30  | Delhi   | 4000.0 |
| 3  | Ali  | 19  | Mumbai  | 2000.0 |

## SQL DELETE Statement

The `DELETE` statement is used to delete existing records in a table.

Here is the basic syntax of the SQL DELETE statement:

```sql
DELETE FROM table_name
WHERE condition;
```

- `table_name`: Name of the table.
- `WHERE condition`: Specifies which records should be deleted. If you omit the WHERE clause, all records in the table will be deleted!

**Note**: Be very careful with the WHERE clause in the DELETE statement. The WHERE clause specifies which record or records should be deleted. If you omit the WHERE clause, all records will be deleted!

Let's look at an example. Suppose we have the same `Customers` table:

| ID | NAME | AGE | ADDRESS | SALARY |
|----|------|-----|---------|--------|
| 1  | Ravi | 32  | Pune    | 2000.0 |
| 2  | Mohan| 30  | Delhi   | 4000.0 |
| 3  | Ali  | 19  | Mumbai  | 2000.0 |

And we want to delete the record of the customer whose ID equals 1, we'd use the following SQL query:

```sql
DELETE FROM Customers
WHERE ID = 1;
```

After executing the above SQL statement, the `Customers` table will be as follows:

| ID  | NAME  | AGE | ADDRESS | SALARY |
| --- | ----- | --- | ------- | ------ |
| 2   | Mohan | 30  | Delhi   | 4000.0 |
| 3   | Ali   | 19  | Mumbai  | 2000.0 | 


In the next slide, we will discuss the SQL SELECT statement, which allows us to retrieve data from a database.