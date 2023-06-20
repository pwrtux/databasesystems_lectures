r## What was the last lecture about?
- We learned about the Boyce-Codd Normal Form (BCNF), Third Normal Form (3NF) and Fourth Normal Form (4NF). These are forms of database normalization, a process used to organize a database into tables and columns to avoid data redundancy and improve data integrity.

## Quiz Time
**Question:**
What is true about a relation schema R with set of dependencies F, where for all X →B in F+ at least one of the following holds:
- X →B is trivial (i.e., B ⊆ X)
- X is a superkey for R
- B is contained in a key

Options:
A. BCNF
B. 3NF
C. 4NF
D. De-normalized

**Answer:** B. 3NF

## What are the intended learning outcomes of this lecture?
- Be able to write foreign key constraints and other constraints in SQL.
- Use triggers to capture more complex constraints.
- Use views.

## What are Constraints in Database Systems?
Constraints are rules enforced on the data in a table. They maintain the integrity and reliability of the data. The constraints in SQL are:
- Single-attribute keys (seen before)
- Multi-attribute keys (seen before)
- Foreign keys
- Attribute constraints
- Row constraints
- Assertions
- Triggers

There are different scopes of constraints and different policies for their enforcement.

## What are Single-attribute Constraints?
- `NOT NULL`: The value cannot be `NULL`.
- `DEFAULT`: A default value is specified.
- `UNIQUE`: The value is unique in the table unless it is `NULL`.
- `PRIMARY KEY`: The value is unique in the table, never `NULL`.

Example:

```SQL
CREATE TABLE People (
  name VARCHAR(40) NOT NULL,
  office VARCHAR(15),
  userid VARCHAR(15) PRIMARY KEY,
  'group' CHAR(3)
);
```

## What are Multi-attribute Constraints?
`PRIMARY KEY (a1, a2, ..., an)`: A combination of attributes that uniquely identify a record in the table. This constraint is a top-level element of the `CREATE` statement.

Example:

```SQL
CREATE TABLE Exams (
  studid CHAR(2),
  date DATE,
  time TIME,
  vip VARCHAR(15),
  room VARCHAR(40),
  PRIMARY KEY (studid, date)
);
```

## How are Constraints Enforced?
Constraints are checked during insert or update operations. If a constraint is violated, a runtime error occurs.

## What is a Foreign Key?
A foreign key specifies that an attribute must reference an attribute in another table, maintaining "referential integrity". The referenced attribute must be a primary key.

## Quiz Time
**Question:**
Given the following SQL statements:

```SQL
CREATE TABLE Rooms(
  room VARCHAR(15) PRIMARY KEY,
  capacity INT
);
CREATE TABLE People(
  name VARCHAR(40) NOT NULL,
  office VARCHAR(15) REFERENCES Rooms(room),
  userid VARCHAR(15) PRIMARY KEY,
  'group' CHAR(3)
);
```

Which statement is correct?

Options:
A. A tuple in People has a pointer to a tuple in Rooms
B. A tuple in Rooms has a pointer to a tuple in People
C. A tuple in People has an office value that exists in Rooms
D. A tuple in Rooms has a room value that exists in People
E. A tuple in People is repeated in Rooms
F. A tuple in Rooms is repeated in People

**Answer:** C. A tuple in People has an office value that exists in Rooms

## How are Foreign Key Constraints Enforced?

When we talk about enforcing foreign key constraints, we are essentially ensuring the integrity of data between related tables. For instance, let's consider two tables: People (source) and Rooms (target). The People table references the Rooms table, which means a tuple in People has an 'office' value that exists in the Rooms table. 

```sql
CREATE TABLE People (
    name VARCHAR(40) NOT NULL,
    office VARCHAR(15) REFERENCES Rooms(room)
    ON DELETE SET NULL ON UPDATE CASCADE,
    userid VARCHAR(15) PRIMARY KEY,
    'group' CHAR(3)
);
```

Better Example:
```sql
CREATE TABLE track(
  trackid     INTEGER, 
  trackname   TEXT, 
  trackartist INTEGER,
  FOREIGN KEY(trackartist) REFERENCES artist(artistid),
);
```

Foreign key constraints come into play when either the source or target tables change:

1. **Spurious value due to insert or update in source:** If an attempt is made to insert or update a record in the People table with an 'office' value that doesn't exist in the Rooms table, the DBMS would reject the insert or update.

2. **Dangling value due to delete or update in target:** If a record in the Rooms table that is being referenced by the People table is deleted or updated, the DBMS can react in several ways:
    - **Reject the delete or update** (default behaviour)
    - **CASCADE:** The same changes are made in the source table.
    - **SET NULL:** The source values are changed to NULL.

## How do Attribute Constraints Work?

Attribute constraints are conditions that can be set on individual columns in a table. These conditions are checked during an insert or update operation on the attribute, but not for other modifications. Thus, foreign keys cannot be enforced.

The CHECK constraint is used to enforce a condition on an attribute. Here is an example:

```sql
CREATE TABLE People(
    name VARCHAR(40) NOT NULL,
    office VARCHAR(15),
    userid VARCHAR(15) PRIMARY KEY,
    'group' CHAR(3) CHECK ('group' = 'vip' OR 'group' = 'phd' OR 'group' = 'tap')
);
```
In this example, the CHECK constraint enforces that the 'group' attribute can only have one of the following values: 'vip', 'phd', or 'tap'.

## How do Row Constraints Work?

Row constraints are conditions that are set on an entire row rather than on individual attributes. These conditions are checked during the insert or update of any attribute.

The CHECK constraint can also be used for row-level checks. Here's an example:

```sql
CREATE TABLE Meetings (
    meetid INT,
    date DATE,
    time_slot INT,
    owner VARCHAR(15),
    topic VARCHAR(40),
    CHECK (time_slot > 16 OR topic NOT LIKE '%beer%')
);
```
In this example, the CHECK constraint ensures that either the 'time_slot' is greater than 16 or the 'topic' does not contain the word 'beer'.

## What are Assertions in SQL?

Assertions are global constraints that are checked after every database modification. They can be used to ensure a condition holds for the entire database. Note that MySQL does not support assertions.

Here's an example of an assertion:

```sql
CREATE ASSERTION StayOutOfMyOffice
CHECK ((SELECT COUNT(*) FROM People WHERE office= '03-216') <= 1);
```
This assertion ensures that there is at most one person with the office '03-216'.

## What are Triggers in SQL?

Triggers are database operations that are automatically performed when a specified database event occurs, such as insertions, updates, or deletions. They have the potential to be both efficient and expressive and can do much more than just checks.

A trigger has three components:
- **event:** Defines when the trigger should be activated. It could be AFTER or BEFORE an INSERT, DELETE, or UPDATE.
- **condition:** Any SQL Boolean expression.
- **action:** Any sequence of SQL modifications.

Here is an example of a trigger:

```sql
CREATE TRIGGER LogInsert
AFTER INSERT ON People
FOR EACH STATEMENT
INSERT INTO LogFile VALUES ('People', CURRENT_TIME);
```
In this example, after each INSERT operation on the People table, a new record is inserted into the LogFile table.

Triggers can also be defined at the row level (FOR EACH ROW) or statement level (FOR EACH STATEMENT), and can handle both old and new values.

## Quiz

Consider the following schema: Sells(shop, product, price) and RipoffShops(shop). We want to maintain a list of shops that raise the price of any product by more than 1. Fill in the ???:

```sql
CREATE TRIGGER PriceTrig
??? UPDATE OF price ON Sells
REFERENCING
OLD ROW AS ooo
NEW ROW AS nnn
FOR EACH ROW
WHEN(nnn.price > ooo.price + 1.00)
INSERT INTO RipoffShops
VALUES(nnn.shop);
```
Options:
A. AFTER
B. BEFORE
C. INSTEAD OF
D. USING

**Answer: A. AFTER**

The trigger should be created to activate AFTER an UPDATE of the price on the Sells table. This is because we want to check the new price against the old one after the update operation is completed. If the new price is more than $1 higher than the old price, the shop is added to the RipoffShops table.


## How Can Triggers Be Put to Work?

Triggers are database operations that are automatically performed when a specified database event occurs, such as insertions, updates, or deletions. The following example shows how a trigger can be used to maintain a list of shops that raise the price of any product by more than 1 in a `Sells` table:

```sql
CREATE TRIGGER PriceTrig
AFTER UPDATE OF price ON Sells
REFERENCING
OLD ROW AS ooo
NEW ROW AS nnn
FOR EACH ROW
WHEN(nnn.price > ooo.price + 1.00)
INSERT INTO RipoffShops
VALUES(nnn.shop);
```
This trigger will be activated after the price of a product is updated in the `Sells` table. If the new price (`nnn.price`) is more than the old price (`ooo.price`) by more than 1, the shop will be added to the `RipoffShops` table.

## What Are Views in SQL?

In SQL, a view is a virtual table or a named query defined as a function of base tables or other views. The following are examples of creating views:

```sql
CREATE VIEW BusyDays AS
SELECT name, date
FROM
People, Meetings
WHERE People.userid=Meetings.owner;

CREATE VIEW Vips AS
SELECT * FROM People
WHERE 'group' = 'vip';
```

These views can be used as tables in SQL queries. For instance:

```sql
SELECT *
FROM
BusyDays, Vips
WHERE BusyDays.name = Vips.name;
```

## What Are Materialized Views in SQL?

Materialized views are views that are stored as tables. They require recomputation whenever involved tables may have changed. Materialized views are not available in MySQL.

```sql
CREATE MATERIALIZED VIEW VipsM AS
SELECT * FROM People
WHERE 'group' = 'vip';
```

The trade-off of materialized views is between query speed and modification speed. They are faster for queries but slower for modifications. A typical compromise is to recompute the view periodically.

## Quiz

Given the following view:

```sql
CREATE VIEW AvgCap AS
SELECT AVG(capacity) AS average
FROM
Rooms;
UPDATE AvgCap SET average=117;
```

What will happen when you try to update `average` in the `AvgCap` view?

A. The average value is recomputed
B. The average value is stored in the view
C. The average value is stored in the base table separately
D. The average value cannot be updated

**Answer: D. The average value cannot be updated**

Views are typically read-only and do not allow modifications. This is especially true for views like `AvgCap`, which is computed from the `Rooms` table. Attempting to directly update a computed value in a view will usually result in an error.

## Can Views Be Modified?

In general, it doesn't make sense to update a view, especially if the view is created using aggregate functions like in the following example:

```sql
CREATE VIEW AvgCap AS
SELECT AVG(capacity) AS average
FROM
Rooms;
UPDATE AvgCap SET average=117;
```

However, some simple views may be modified. For example, if the view only involves a single table and only includes simple attributes, the view might be updatable. For instance:

```sql
CREATE VIEW Vips AS
SELECT * FROM People
WHERE 'group' = 'vip';
INSERT INTO Vips VALUES ('John Doe', '02222', 'jdoe', 'vip');
```

In addition, INSTEAD-OF triggers can be used to capture modifications to views and perform the intended action on the underlying base tables.

## What Are Some MySQL Syntax Examples for Triggers?

Here are some examples of creating triggers in MySQL:

```sql
DELIMITER $$

CREATE TRIGGER PromotionOfficeDefault
AFTER UPDATE 
ON People
FOR EACH ROW
BEGIN
    IF (
        OLD.group = 'phd' AND
        NEW.group = 'vip' AND
        NEW.office IS NULL
    ) THEN
        UPDATE People
        SET office = '02-224'
        WHERE userid = NEW.userid;
    END IF;
END$$

DELIMITER ;

```

```sql
DELIMITER $$

CREATE TRIGGER ZombieOffice
BEFORE INSERT 
ON People
FOR EACH ROW
BEGIN
    IF (
        NEW.office NOT IN (
            SELECT room 
            FROM Rooms
        )
    ) THEN
        INSERT INTO Rooms(room,capacity)
        VALUES(NEW.office,4);
    END IF;
END$$

DELIMITER ;

```

```sql
DELIMITER $$

CREATE TRIGGER GetOutOfMyOffice
BEFORE INSERT 
ON People
FOR EACH ROW
BEGIN
    IF (NEW.office = '03-216') THEN
        SET NEW.office = '03-224';
    END IF;
END$$

DELIMITER ;

```

```sql
DELIMITER $$

CREATE TRIGGER GetOutOfMyOffice2
BEFORE UPDATE 
ON People
FOR EACH ROW
BEGIN
    IF (NEW.office = '03-216') THEN
        SET NEW.office = '03-224';
    END IF;
END$$

DELIMITER ;

```

## How to Review This Lecture?

When reviewing the material from this lecture, consider the following questions:

- What are the different types of constraints in SQL, and what do they capture?
- Why are foreign keys particularly important, and why can't their functionality be enforced using other constructs?
- What form do triggers take, and how do they allow us to specify database operations?
- What are views in SQL, and how are they used?