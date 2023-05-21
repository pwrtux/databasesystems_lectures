# Relational Algebra in Database Systems

Relational algebra serves as the formal foundation for operations in the relational model. It provides a sequence of operations that represent the resulting relation of a database query. The understanding of relational algebra is fundamental for implementing and optimizing queries and some of its concepts are incorporated into SQL.

## The Components of Algebra

Algebra comprises values, operators, and rules. This includes the closure property, stating that operations yield values. Examples include integers with addition, subtraction, multiplication, sets with union, intersection, and difference, and relations with query operators.

## Understanding Relations

Relations, in the context of databases, refer to a schema of attribute names and a finite n-ary relation on a given data set. This concept can be visualized as a table where columns have the same generic type, duplicates are not allowed, and no other constraints are imposed.

## Operations in Relational Algebra

The key operations in relational algebra include:

- **Union, Intersection, Difference**: These operations work on the relations that have the same schema. 
- **Projection (π)**: This operation selects certain columns from the table and discards the others.
- **Selection (σ)**: This operation selects certain rows from the table based on a given condition.
- **Renaming (ρ)**: This operation changes the names of the schema.
- **Cartesian Product (x)**: This operation generates all possible combinations of tuples from two relations.
- **Natural Join (⋈)**: This operation merges two tables on the condition that they have an equal set of values in one or more common attribute(s).

## Translating Relational Algebra to SQL

Here are the SQL counterparts for the operations in relational algebra:

- Projection (π a1,...,an (R)) -> **SELECT** DISTINCT a1,…,an FROM R;
- Selection (σC (R)) -> SELECT * FROM R **WHERE** C;
- Renaming (ρa->b(R)) -> SELECT a **AS** b FROM R;
- Cartesian Product (RxS) -> SELECT * **FROM R,S**;
	- No Conditions for WHERE Part
- Natural Join (R⋈S) -> SELECT * FROM R **NATURAL JOIN** S;
- Conditional Join (R ⋈θ S = σθ(R x S)) -> SELECT * FROM R,S WHERE θ;
- Union, Intersection, Difference (R U S, R S, R \ S) -> R UNION S; R INTERSECT S; R EXCEPT S;

| Relational Algebra | SQL |
| --- | --- |
| $\pi_{a_1,...,a_n} (R)$ | `SELECT DISTINCT a1 ,...,an FROM R;` |
| $\sigma_{C} (R)$ | `SELECT * FROM R WHERE C;` |
| $\rho_{a\to b}(R)$ | `SELECT a AS b FROM R;` |
| $R \times S$ | `SELECT * FROM R,S;` |
| $R \bowtie S$ | `SELECT * FROM R NATURAL JOIN S;` |
| $R \bowtie_{\theta} S = \sigma_{\theta}(R \times S)$ | `SELECT * FROM R,S WHERE θ;` |
| $R \cup S$, $R \cap S$, $R \setminus S$ | `R UNION S; R INTERSECT S; R EXCEPT S;` |

## Query Trees

Query trees provide a graphical representation of the operations to perform on a database in order to execute a particular SQL command.

## Limitations of Relational Algebra

While powerful, relational algebra cannot answer all types of queries, particularly those that require transitive closures, such as finding all cities reachable from a given city via a series of flights. Some of these limitations can be overcome using recursion in SQL or by introducing a special closure operator.

## Algebraic Laws

Relational algebra follows several algebraic laws like idempotence, commutativity, associativity, distributivity, splitting, and cancellation. These properties can be used to optimize queries for efficient computation.

## Query Optimization

There are several ways to optimize queries using the concepts of relational algebra. Some of these include:

- Pushing selections down the expression tree to reduce the number of rows.
- Pushing projections down the expression tree to reduce the number of columns.
- Ordering joins based on size estimates.
