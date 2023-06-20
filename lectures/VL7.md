## What Was Covered in the Last Lecture?

The previous lecture delved into the design theory for relational databases. We discussed anomalies that can arise in database design and how to handle them. We introduced concepts of functional dependencies and multivalued dependencies. We also talked about the process of closure and decomposition.

## Quiz Time: Can You Correctly Identify Functional Dependencies?

Which of the following are correct?

A. A functional dependency must be known  
B. A functional dependency is generated using a closure algorithm  
C. A functional dependency must be decomposed  
D. A multivalued dependency implies a functional dependency  

*(Here, the correct answers would depend on the exact context and specific rules applied in the course. In general, the statements A and C are typically correct. B might be correct depending on the specific interpretation, and D is usually incorrect as multivalued dependencies and functional dependencies are different concepts.)*

## What Are the Intended Learning Outcomes for Today's Lecture?

By the end of this lecture, you should be able to:
- Detect and remove redundancies by decomposing into Boyce-Codd Normal Form (BCNF) or Third Normal Form (3NF)
- Understand the Fourth Normal Form (4NF)

## How Can We Avoid Anomalies through Decomposition?

To avoid anomalies, we can decompose a table into two. For instance, a "Meetings" table can be decomposed into "Owners" and "Meetings" tables. The owners table will carry information about the owners, and the meetings table will carry information about the meetings.

## What is Functional Dependency in Databases?

A functional dependency is a relationship between two sets of attributes in a database. It is of the form $a_1…a_n \rightarrow b$ for some attributes $a_i$ and $b$. The values of $a_1,...,a_n$ determine the value of $b$ in any row. This generalizes the notion of a key. A key is a set of attributes ${a_1,..., a_n}$ such that for any other attribute $b$ and for any row, values of ${a_1,...,a_n}$ determine the value of $b$. A key is always minimal. A superkey is a superset of a key.

## What Properties Should a Good Decomposition Have?

When a relational schema $R$ with a set of functional dependencies $F$ is decomposed into $R_1$ and $R_2$, the following properties should hold:

1. **Lossless-join decomposition:** The original relation $R$ can be reconstructed from $R_1$ and $R_2$ using a natural join operation. This ensures that no information is lost during decomposition.

2. **Dependency preservation:** The set of functional dependencies $F$ is preserved in the decomposed relations. This ensures that all constraints of the original relation are still enforceable in the decomposed relations.

3. **No redundancy:** The decomposed relations should not contain any redundant information. This reduces the space required to store the data and prevents update anomalies.

## What is Boyce-Codd Normal Form (BCNF)?

BCNF is a stricter version of the Third Normal Form (3NF). A table is in BCNF if for all non-trivial dependencies $a_1… a_n \rightarrow b$, ${a_1,…,a_n}$ is a superkey.
In other words, the determinant of each functional dependency is a superkey. BCNF can always be obtained by systematic lossless decomposition.

## Is BCNF Decomposition Always Dependency Preserving?

The answer is no. BCNF (Boyce-Codd Normal Form) decomposition may not always preserve dependencies. For example, consider the relation R(location, city, artist) with functional dependencies (FDs) `location → city`, `artist city → location` and keys `{artist, city}` and `{artist, location}`. This relation is not in BCNF due to the FD `location → city`. Decomposing this relation would fail to preserve the FD `artist city → location`. Consequently, maintaining this dependency after decomposition would have to be done manually, which can be complex and error-prone. 

```
Example:
    - Original Relation: 
        location | city | artist
        ---------------------------
        VoxHall | Aarhus | Illdisposed
        Jakobshof | Aachen | Ina Müller
```

## What is the Third Normal Form (3NF) and Its Rules for MVDs?

The third normal form (3NF) is a database schema design that aims to improve database processing while minimizing redundancy. For any non-trivial functional dependency `A → B` in F+, one of the following holds:

- `A` is a superkey for the table, or
- `B` is contained in a key.

Here, multivalued dependencies (MVDs) are introduced. A functional dependency `A → B` means that attribute values `A` imply specific values for attributes in `B`. In contrast, `A → → B` (an MVD) means attribute values in `A` imply a set of values for attributes in `B`.

Also, any relation in BCNF is also in 3NF.

```
Example:
    - DB1 → DB2 is a multivalued dependency if for every pair of tuples t1, t2 in DB1, there exists a tuple t3 in DB2 such that t3[A] = t1[A] and t3[B] = t2[B].
```

## Does 3NF Eliminate All Redundancies?

Not all redundancies are eliminated in 3NF. For example, consider the same relation R(location, city, artist) with FDs `location → city`, `artist city → location` and keys `{artist, city}` and `{artist, location}`. This relation is in 3NF. However, some redundancy remains, as evidenced by the repeated entries for the location 'VoxHall'.

```
Example:
    - Relation in 3NF: 
        location | city | artist
        ---------------------------
        VoxHall | Aarhus | Illdisposed
        Jakobshof | Aachen | Ina Müller
        VoxHall | ? | Kurve
```

## What is a Minimal Basis for FDs?

A minimal basis for a set of dependencies F on a table is a set of dependencies G that is equivalent to F and satisfies the following:

- It is a basis.
- All dependencies are of the form `a1… an → b`.
- If any dependency is removed, `G` is no longer a basis.
- If any `ai` from a dependency is removed, `G` is no longer a basis.

In other words, a minimal basis is the most simplified version of a set of functional dependencies.

## How Do We Obtain a Minimal Basis for FDs?

Consider a relation `R(a,b,c,d)` with `F = {ab → c, c → b, a → d}`. The process of finding a minimal basis `G` for `F` involves:

- Constructing `G` from `F`.
- Verifying if any FD can be removed from `F` without changing the closure.
- Checking if any attribute from the left hand can be removed.

In this case, it is found that no FD or attribute can be removed. Therefore, `F` itself is a minimal basis.

## What is the 3NF Synthesis Algorithm?

The 3NF synthesis algorithm is a method to decompose a relation `R` into a set of sub-relations `{R1, R2, …, Rn}` such that each relation `Ri` is in 3NF, and the decomposition is lossless-join and dependency preserving. 

Steps:
1. Let `G` be a minimal basis for `F`.
2. For each FD `X → a` in `G`, make `(X,a)` the schema of a candidate sub-relation.
3. If none of the new sub-relations contains a superkey for `R`, create another sub-relation with any key for `R` as schema.
4. Combine `Ri` and `Rj` if `Ri` was obtained using `X → ai` and `Rj` was obtained using `X → aj`.
5. Drop proper sub-relations.

This algorithm allows us to transform a given database schema into 3NF.


## How to Apply the 3NF Synthesis Algorithm?

The 3NF Synthesis Algorithm is used to decompose a relation into a set of relations that adhere to the Third Normal Form. Here's an example using a relation `R(location, city, artist, genre, rating)`:

- First, create subrelations for each functional dependency. For instance, `R1(location, city)`, `R2(artist, city, location)`, and `R3(artist, genre)`.
- Then, if no combinations are present (no identical left sides), drop `R1` as it is a proper subset of `R2`.
- Lastly, if no key for `R` has been created, choose `R4(artist, location, rating)`. The final decomposition will be `R2`, `R3`, and `R4`.

## What are Multivalued Dependencies?

Multivalued dependencies are a generalization of functional dependencies where for each pair of tuples `t` and `u` that agree on the `a` attributes, there exists a tuple `v` that agrees with both `t` and `u` on `a` attributes, with `t` on the `b` attributes, and `u` on the rest of the attributes. This concept is useful for set-valued attributes, such as when an instructor teaches several courses or an actor stars in several movies.

## What is Fourth Normal Form (4NF)?

The Fourth Normal Form (4NF) further reduces redundancies from a table, specifically those caused by multivalued dependencies. It states that for all non-trivial multivalued dependencies, the determinant is a superkey. Normalization in 4NF involves decomposing the table with respect to multivalued dependencies, not functional dependencies.

## Quiz Question: What is the Relationship Between the Different Normal Forms?

**Quiz Question:** Which of the following is true about normal forms and table R?

A. If R is in BCNF, it is also in 3NF and 4NF
B. If R is in 4NF, it is also in 3NF and BCNF
C. If R is in 3NF, it is also in 4NF and BCNF
D. If R is in BCNF, it cannot be in 3NF and 4NF

**Answer:** B. If R is in 4NF, it is also in 3NF and BCNF.

## How Do Normal Forms Influence Redundancy and Database Operation Speed?

As you move from lower to higher normal forms, redundancy decreases and less space is used. However, normalization affects database operations. It often speeds up `SELECT` operations but can slow down `INSERT`, `UPDATE`, and `DELETE` operations due to the increased number of tables and joins.

## What is Denormalization and When Should It Be Used?

Denormalization is the process of adding redundancy to a database to improve performance, especially for `SELECT` operations. While it can speed up data access and reduce join operations, denormalization typically slows down `INSERT`, `UPDATE`, and `DELETE` operations.

## What Are the Key Properties of Good Relational Design?

Good relational design avoids redundancy, prevents anomalies, and ensures that all necessary information can be represented. When a design is suboptimal, the solution often involves decomposing larger relation schemas into smaller ones.

## How to Review Your Understanding of Normal Forms?

To assess your understanding of normal forms, try to explain what each normal form is, why it's used, and how to achieve it. For instance, explain what it means for a relation to be in BCNF, 3NF, or 4NF, and what steps should be taken if it isn't. Furthermore, consider when denormalization might be appropriate, and what it means for a decomposition to be lossless.
