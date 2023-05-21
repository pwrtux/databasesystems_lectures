# Structured Query Language (1/2)

## What Did We Cover in the Last Lecture?

- Milestones in the history of database systems
- The relational data model: relations, columns, rows, attributes
- Entity-Relationship (E/R) modeling: entity sets, attributes, relationships, multiplicity, weak entity sets, subclasses, keys, design principles, and rules for good design
- Mapping E/R diagrams to relational schemas

## What is the Manager-Employee Relationship in a Database Context?

Quiz Question: Manager supervises Employee. The corresponding relationship is:
- Many-Many
- Many-One
- One-Many
- One-One

## What Are Our Learning Outcomes Today?

- Storing data in the relational data model
- Writing basic SQL DDL (Data Definition Language) 
- Writing SQL DML (Data Manipulation Language) queries on one table 
- Understanding simple query templates
- Using aggregations, handling duplicates, and using scalar functions 

## What is an Example Database?

Example Database consists of tables like People, Equipment, Rooms, Meetings, and Participants with various attributes.

## What is SQL and Its Basics?

SQL (Structured Query Language) was invented by IBM in the 1970s, with various versions implemented by different systems (MySQL, DB2, Oracle, SQL Server). SQL consists of DDL for schema definitions, DML for data handling, is high-level and "declarative", and has algebraic foundations for query optimization.

## How to Declare Tables in SQL DDL?

Table declarations use the `CREATE TABLE` command, specifying the name and datatype for each column.

```sql
CREATE TABLE Rooms (
  room VARCHAR(15),
  capacity INT
);
```

## What are the SQL Data Types?

SQL has several datatypes, including INT, CHAR, VARCHAR, FLOAT, DATE, TIME, TEXT, BLOB, and more.

## What are SQL Constraints?

Constraints can be used in SQL to enforce certain conditions on columns, including NOT NULL, DEFAULT value, UNIQUE, and PRIMARY KEY.

## How to Refine SQL Tables?

Tables can be refined by adding constraints during table creation.

```sql
CREATE TABLE Rooms (
  room VARCHAR(15) PRIMARY KEY,
  capacity INT NOT NULL
);
```

## What is the Basic Form of an SQL Query?

The basic form of an SQL query uses SELECT-FROM-WHERE structure.

```sql
SELECT desired_attributes
FROM one_or_more_tables
WHERE conditions_about_involved_tuples;
```

## How to Rename Attributes in SQL?

Attributes can be renamed using AS keyword.

```sql
SELECT name, 'group' AS category
FROM People
WHERE office = '03-338';
```

## How to Use Conditions in SQL WHERE Clause?

The WHERE clause can use logical operators like AND, OR, NOT, and comparison operators like =, <>, <, >, <=, >=, LIKE.

## How to Handle NULL in SQL?

Arithmetic operations on NULL yield NULL. Any comparison with NULL yields unknown. You can check for NULL values using the `IS NULL` condition.

```sql
SELECT userid
FROM People
WHERE office IS NULL;
```

## How to Order SQL Results?

The `ORDER BY` keyword is used to sort the results of an SQL query.

```sql
SELECT *
FROM Meetings
WHERE time_slot > 10
ORDER BY owner;
```

## How to Use Scalar Functions in SQL?

Scalar functions can be used in the SELECT clause to perform operations on column values.

## How to Use Aggregation in SQL?

Aggregate functions such as SUM, AVG, COUNT, MIN, MAX can be used in the SELECT clause. 

```sql
SELECT AVG(capacity) AS to reduce length of output by 2638 characters average_capacity
FROM Rooms;
```

This query calculates the average capacity of all rooms in the "Rooms" table.

## How to Use SQL Aggregates and GROUP BY?

To partition data into groups and apply aggregate functions to each group, use the `GROUP BY` clause.

```sql
SELECT room, COUNT(*)
FROM Meetings
GROUP BY room;
```

This query counts the number of meetings in each room.

## How to Filter Aggregated Data in SQL?

The `HAVING` clause is used to filter results from a `GROUP BY` operation. It is similar to the `WHERE` clause but operates on the result of the aggregation.

```sql
SELECT room, COUNT(*)
FROM Meetings
GROUP BY room
HAVING COUNT(*) > 2;
```

This query retrieves rooms where more than 2 meetings occurred.

## How to Use Subqueries in SQL?

Subqueries can be used to solve more complex problems. Subqueries are nested within another SQL query, and they can return a single value or a set of values.

```sql
SELECT name
FROM People
WHERE office IN (
  SELECT room
  FROM Rooms
  WHERE capacity > 4
);
```

This query retrieves the names of people whose offices have a capacity greater than 4.

That's it for today's lecture. Next time, we'll go more in-depth with subqueries, joins, and more complex SQL constructs.