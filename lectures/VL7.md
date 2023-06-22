
## What did we learn in the last lecture?

In the previous lecture, we covered the design theory for relational databases. We discussed anomalies that can occur in databases and how to avoid them using functional dependencies and multivalued dependencies. We also learned about closure and decomposition, which are techniques used to ensure data integrity and reduce redundancy in databases.

## Quiz Time

**Question:** Which of the following are correct?

A. A functional dependency must be known
B. A functional dependency is generated using a closure algorithm
C. A functional dependency must be decomposed
D. A multivalued dependency implies a functional dependency

**Answer:** A. A functional dependency must be known

## What are the intended learning outcomes of this lecture?

By the end of this lecture, you should be able to:

- Detect and remove redundancies by decomposing into Boyce-Codd Normal Form (BCNF) or Third Normal Form (3NF).
- Understand Fourth Normal Form (4NF).

## How can we avoid anomalies through decomposition?

Anomalies in databases can be avoided by decomposing the database into smaller, more manageable parts. For example, consider a database with the following attributes: `meetid`, `topic`, `date`, `time_slot`, `userid`, `name`, `group`, and `office`. 
This database can be decomposed into two smaller databases: `Meetings` and `Owners`. The `Meetings` database would contain the attributes `meetid`, `topic`, `date`, `time_slot`, and `userid`, while the `Owners` database would contain the attributes `userid`, `name`, `group`, and `office`.
This decomposition is lossless, meaning that no information is lost in the process.

## What are functional dependencies?

A functional dependency is a relationship between two sets of attributes in a database. It is of the form $a_1…a_n \rightarrow b$ for some attributes $a_i$ and $b$. The values of $a_1,...,a_n$ determine the value of $b$ in any row. This generalizes the notion of a key. A key is a set of attributes ${a_1,..., a_n}$ such that for any other attribute $b$ and for any row, values of ${a_1,...,a_n}$ determine the value of $b$. 
- A key is always minimal. 
- A superkey is a superset of a key.

## What are the properties of decomposition?

When a relational schema $R$ with a set of functional dependencies $F$ is decomposed into $R_1$ and $R_2$, the following properties should hold:

1. **Lossless-join decomposition:** The original relation $R$ can be reconstructed from $R_1$ and $R_2$ using a natural join operation. This ensures that no information is lost during decomposition.

2. **Dependency preservation:** The set of functional dependencies $F$ is preserved in the decomposed relations. This ensures that all constraints of the original relation are still enforceable in the decomposed relations.

3. **No redundancy:** The decomposed relations should not contain any redundant information. This reduces the space required to store the data and prevents update anomalies.

## What is Boyce-Codd Normal Form (BCNF)?

BCNF is a stricter version of the Third Normal Form (3NF). A table is in BCNF if for all non-trivial dependencies $a_1… a_n \rightarrow b$, ${a_1,…,a_n}$ is a superkey.
In other words, the determinant of each functional dependency is a superkey. BCNF can always be obtained by systematic lossless decomposition.

## How can we normalize a table into BCNF?

To normalize a table into BCNF, we follow these steps:

1. Find a table $R$ with a non-trivial, non-reducible functional dependency $a_1… a_n \rightarrow b_1$ where ${a_1,…, a_n}$ is not a superkey.
2. Collect all other functional dependencies with the same left side: $a_1… a_n \rightarrow b_1 ... a_1 … a_n \rightarrow b_k$.
3. Assume that the schema of $R$ is $(a_1,...,a_n,b_1,...,b_k,c_1,...,c_m)$.
4. Decompose $R$ into: $R_1 = \pi_{a_1,...,a_n,b_1,...,b_k} (R)$; $R_2 = \pi_{a_1,...,a_n,c_1,...,c_m} (R)$.
5. Repeat this process until all non-trivial functional dependencies with left side $a_1… a_n$ are removed, since $R_2$ has none of those.

## Quiz Time

**Question:** Consider $R(a,b,c,d)$, and key ${a,d}$. Which of the FDs $acd \rightarrow b$, $d \rightarrow c$ (if any) cause BCNF violation?

A. None, $R$ is in BCNF
B. $acd \rightarrow b$
C. $d\rightarrow c$
D. Both

**Answer:** C. $d\rightarrow c$

## What is Dependency Preservation?

BCNF decomposition is not always dependency preserving. Dependency preservation is important because it ensures that all constraints of the original relation are still enforceable in the decomposed relations. If a decomposition is not dependency preserving, we may need to maintain the dependency manually, which can be difficult and error-prone.

## What is Third Normal Form (3NF)?

Third Normal Form (3NF) is a property of a relational database schema. A schema is in 3NF if for all functional dependencies $A \rightarrow B$ in $F^+$ one of the following holds:

- $A \rightarrow B$ is trivial (i.e., $B \subseteq A$), or
- $A$ is a superkey for the table, or
- $B$ is contained in a key.

If a relation is in BCNF, it is also in 3NF.

## What is a minimal basis for functional dependencies?

A minimal basis $G$ for a set of functional dependencies $F$ is a set of dependencies that satisfies the following properties:

- It is a basis.
- All dependencies are of the form $a_1… a_n \rightarrow b$.
- If any dependency is removed, $G$ is no longer a basis.
- If any $a_i$ from a dependency is removed, $G$ is no longer a basis.

## What is the 3NF Synthesis Algorithm?

The 3NF Synthesis Algorithm is a method to decompose a relation $R$ into ${R_1, R_2, …, R_n}$ such that each relation $R_i$ is in 3NF, and the decomposition is lossless-join and dependency preserving.

## What are multivalued dependencies?

Multivalued dependencies are a generalization of the notion of functional dependencies. They are of the form $a_1… a_n \rightarrow\rightarrow b_1… b_m$. They are typical for set-valued attributes and mean that two independent one-many relationships are mixed in one relation, e.g., $A-B$, and $A-C$ in $R(A,B,C)$.

## What is Fourth Normal Form (4NF)?

Fourth Normal Form (4NF) is a level of database normalization where there are no non-trivial multivalued dependencies other than a candidate key.
It is used to handle complex data types such as arrays and lists. A relation is in 4NF if, for all non-trivial multivalued dependencies $a_1…a_n \rightarrow\rightarrow b_1…b_m$, ${a_1,…,a_n}$ is a superkey.

## Quiz Time

**Question:** Which of the following is true about normal forms and table R?

A. If R is in BCNF, it is also in 3NF and 4NF
B. If R is in 4NF, it is also in 3NF and BCNF
C. If R is in 3NF, it is also in 4NF and BCNF
D. If R is in BCNF, it cannot be in 3NF and 4NF

**Answer:** B. If R is in 4NF, it is also in 3NF and BCNF

## What are the properties of different normal forms?

Different normal forms have different properties. For example, 3NF removes redundancy due to functional dependencies, but it does not remove redundancy due to multivalued dependencies. BCNF removes redundancy due to functional dependencies. 4NF removes redundancy due to both functional and multivalued dependencies. However, none of these normal forms guarantee dependency preservation.

# **Minimal basis fehlt**

# 3NF Synthesis Algorithm fehlt 

# 4nf

## Quiz Time

**Question:** Which of the following is true about normalization?

A. Speeds up SELECT, INSERT, UPDATE, DELETE
B. Slows down SELECT, INSERT, UPDATE, DELETE
C. Speeds up SELECT, but slows down INSERT, UPDATE, DELETE
D. Slows down SELECT, but speeds up INSERT, UPDATE, DELETE

**Answer:** C. Speeds up SELECT, but slows down INSERT, UPDATE, DELETE

## What is denormalization?

Denormalization is the process of introducing redundancy into a database design to improve performance. It can speed up data access, reduce the number of join operations, and reduce wait time during locking. However, it can also slow down INSERT, UPDATE, and DELETE operations.

## What are the properties of a good relational design?

A good relational design should have the following properties:

- No redundancy: The same data should not be duplicated in multiple places.
- No anomalies: The database should not have insertion, deletion, or update anomalies.
- Ability to represent all necessary information: The database should be able to store all the data that is needed.

## How can we convert a bad design to a good design?

We can convert a bad design to a good design by decomposing large relation schemas into smaller ones. This process needs to be done carefully to ensure that the decomposition is lossless and dependency preserving. 

## What are the different normal forms?

There are several normal forms in relational database design:

- First Normal Form (1NF)
- Second Normal Form (2NF)
- Third Normal Form (3NF)
- Boyce-Codd Normal Form (BCNF)
- Fourth Normal Form (4NF)
- Fifth Normal Form (5NF)

Each normal form has a set of properties that a database schema must satisfy to be in that normal form. The higher the normal form, the less redundancy the database schema has, and thus less space is used.

## Self-Reviewing

To review the concepts covered in this lecture, consider the following questions:

- What are normal forms used for in database design?
- How do different normal forms differ with respect to the types of redundancy they remove?
- What conditions must a relation satisfy to be in BCNF, 3NF, or 4NF?
- What steps can be taken to normalize a relation that is not in BCNF, 3NF, or 4NF?
- Why might we choose to denormalize a database?
- What properties do we want to ensure when decomposing a relation?