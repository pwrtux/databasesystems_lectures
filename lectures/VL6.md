## What Was Covered in the Last Lecture?

The previous lecture discussed the concept of Relational Algebra. This entails describing query results in the relational model using operators such as projection, selection, and renaming. Basic query optimization was also covered.


## What Are the Intended Learning Outcomes for Today?

By the end of this lecture, you should be able to:

1. Describe and detect anomalies in database systems.
2. Determine functional and multivalued dependencies in a database.
3. Avoid anomalies through database decomposition.



## Why Are Dependencies Important in Database Systems?

Dependencies are essential because they capture the semantics of the data. They express an "inherent connection" between pieces of information, such as inferring a city name from zip codes. Additionally, dependencies are used for detecting anomalies in relations, designing "good" relations using normal forms, and eliminating redundancy via decomposition of relations.



## What are Database Anomalies?

Database anomalies are issues that arise in the database, leading to inefficient data management. They include:

1. **Redundancy Anomaly**: This occurs when information is repeated unnecessarily, leading to data duplication. For example, information about the owners in a "MeetingsOwners" table might be duplicated.

2. **Update Anomaly**: This arises when updating a row causes inconsistent information. For example, if an owner's office information is updated in one row but not in others, the database becomes inconsistent.

3. **Deletion Anomaly**: This occurs when deleting a row causes too much information to disappear. For example, if a meeting is deleted from the "MeetingsOwners" table, all information about the owner may be lost.



## How Can Anomalies Be Avoided?

Anomalies can be avoided through decomposition. This involves breaking down a large table into smaller tables to avoid redundancy, update, and deletion anomalies. For instance, the "MeetingsOwners" table can be decomposed into two tables, "Owners" and "Meetings", to prevent data anomalies.



## What Are Functional Dependencies?

A Functional Dependency (FD) in a table is denoted as $a_1,...,a_n \rightarrow b$. This implies that the values of attributes $a_1,...,a_n$ determine the value of attribute $b$ in any row. FDs help to identify possible information loss from a given decomposition.

In FDs, a superkey is a superset of a key, i.e., a subset is a key. For instance, in the "Meetings" table, 'meetid' is a superkey as it determines other attributes.



## How Can We Inference Dependencies?

Inferring dependencies involves finding all dependencies that follow logically from an initial set of dependencies. For instance, if we have $a,b \rightarrow c$ and $a,d \rightarrow b$, it logically follows that $a,d \rightarrow c$.


## How to Compute Attribute Closure?

Attribute closure helps to compute all attributes that are functionally dependent on a set of attributes. The closure of set of attributes $A$ with dependencies $F$ is all attributes determined by $A^+$ = {$a | A \rightarrow a \in F^+$}.

The algorithm for computing $a+$:

```
result = a
while result changes do
  for each g → d in F do
    if g ⊆ result then result := result ∪ d
```


## How to Compute FD Set Closure?

The closure of a set of dependencies $F$, $F+$, includes all dependencies implied by dependencies in $F$. If $a \rightarrow R \in F+$, then $a$ is a superkey of $R$.

The algorithm for computing $F+$:
1. Initialize F+ as F. F+ will contain all functional dependencies that can be inferred from F.
2. Repeat the following steps until no new dependencies can be added to F+:
    - For each pair of dependencies X → Y and Z → W in F+:
        - If Y and Z have common attributes, then a new dependency can be inferred. This new dependency is XZ → YW.
        - Add this new inferred dependency to F+ if it's not already present in the set.
3. Return the final F+ as the closure of F. This set F+ contains all the functional dependencies that can be logically inferred from the initial dependencies in F. 

Note: The 'XZ' and 'YW' in the new inferred dependency XZ → YW represent the union of attributes in X and Z, and Y and W, respectively.

## What is Lossless Decomposition?

Lossless decomposition is an essential characteristic of a well-structured database. If we have a schema $R$ with attributes $a_1,...,a_n, b_1,...,b_k, c_1,...,c_m$, we can define two new relations $R1$ and $R2$ as follows:

- $R1 = \pi_{a1,...,an,b1,...,bk} (R)$
- $R2 = \pi_{a1,...,an,c1,...,cm} (R)$

A decomposition is considered lossless if the join of $R1$ and $R2$ is equal to $R$. This is ensured if $a_1,...,a_n$ is a superkey for either $R1$ or $R2$.

**Example**: In a database with Meetings and Owners, the 'userid' of the owner can serve as a superkey ensuring a lossless decomposition.

## What are the Properties of Decomposition?

When a relational schema $R$ with a set of functional dependencies $F$ is decomposed into $R1$ and $R2$, it should ideally fulfill three properties:

1. **Lossless-Join Decomposition**: For this, we test to see if at least one of the following dependencies is in $F+$: $R1 R2 \rightarrow R1$ or $R1 R2 \rightarrow R2$. If not, the decomposition may be lossy.

2. **Dependency Preservation**: In this case, let $Fi$ be the set of dependencies in $Ri$ ($Fi = \pi_{Ri} (F+)$). We need to test if $(F1 \cup F2)^+ = F+$. This ensures that when a relation is modified, no other relations need to be checked to preserve dependencies.

3. **No Redundancy**: The decomposition should eliminate any redundancy in the data. A functional dependency $A \rightarrow B$ implies specific values for attributes in $B$ based on attribute values $A$. 

## What are Multivalued Dependencies (MVDs)?

MVDs capture relationships between attributes that are not necessarily functional dependencies. For instance, in the context of a course, instructor, and TA relation, there may be no functional dependencies since neither course, TA nor instructor can uniquely determine the other attributes.

However, we could have multivalued dependencies where:

- Course implies a set of instructors and a set of TAs.
- TA implies a set of courses and a set of instructors.
- Instructor implies a set of courses and a set of TAs.

A multivalued dependency is represented as $a_1,a_2,…,a_n \twoheadrightarrow b_1,b_2,…,b_m$. This notation means that for every pair of tuples $t$ and $u$ in $R$ that agree on the $A$’s, we can find some tuple $v$ in $R$ that agrees with both $t$ and $u$ on the $A$’s, with $t$ on the $B$’s, and with $u$ on the attributes that are not $A$’s or $B$’s.

## What are the Rules for MVDs?

Trivial multivalued dependencies are always valid. We say that $A \twoheadrightarrow B$ is trivial if $B \subseteq A$ or $A \cup B$ equals the set of all attributes. Any other multivalued dependency is termed non-trivial.

Also, any functional dependency is a multivalued dependency. That is, if $A \rightarrow B

 for brevity. You can ask me to show all messages or configure your client to do so.
Quiz Time: Decoding dependencies

* R = (a,b,c), F = {a → b, b → c}
* R1 = (a,b), R2 = (a,c)
* F+ = {a → b, b → c, a → c (Transitivity)}
* F1 = {a → b}, F2 = {a → c}, (F1 U F2)+ != F+

**Question**: What does this imply about the decomposition of R into R1 and R2?

**Answer**: This implies that the decomposition is lossless, but it does not preserve dependencies. Therefore, option A is correct: Lossless-join decomposition: yes, dependency preserving: no.

## What are the properties of a good relational design?

* No anomalies: Any inconsistencies or duplication in data is minimized.
* Ability to represent all the information: The design should be able to accurately model the reality it is supposed to represent.

## How can we convert a bad design into a good one?

Bad design can be converted into good one by decomposing large relation schemas into smaller ones. However, this may result in needing more joins to answer queries. 

To ensure lossless join decomposition, we must specify semantic integrity constraints that must be satisfied by all instances of the schemas. Constraints can be used to argue that decompositions are lossless.

## What are functional dependencies and Armstrong's Axioms?

Functional dependencies are a type of constraint in a relational model. Given a relation R, a functional dependency between the attributes of R is an assertion that a particular set of attributes functionally determines another attribute, meaning the value of the second attribute is determined by the value of the first attribute.

Armstrong's Axioms are a set of rules used to infer all the functional dependencies on a relational database. They are a sound and complete set of inference rules, and therefore allow the inference of all functional dependencies on a relation schema based on a given set of functional dependencies.

## What are Multivalued Dependencies?

In a database, we sometimes encounter scenarios where there are independent pieces of information in the same relation. These scenarios may lead to the concept of Multivalued Dependencies (MVDs). MVDs are a type of data dependency where, for a single attribute, there exist multiple independent attributes or sets of attributes in a given relation.

A critical point to remember is that these dependencies are known from the real world, not inferred from the data!

Consider a university database having the attributes 'course', 'TA', and 'instructor'. The information that a course has a set of teaching assistants (TAs) and a set of instructors, or a TA is associated with a set of courses and a set of instructors, is independent information. This implies that there is no functional dependency between these attributes:

- 'Course' does not functionally determine 'TA' or 'Instructor'
- 'TA' does not functionally determine 'Course' or 'Instructor'
- 'Instructor' does not functionally determine 'Course' or 'TA'

However, multivalued dependencies do exist:

- 'Course' $\rightarrow\rightarrow$ 'Instructor' and 'Course' $\rightarrow\rightarrow$ 'TA'
- 'TA' $\rightarrow\rightarrow$ 'Course' and 'TA' $\rightarrow\rightarrow$ 'Instructor'
- 'Instructor' $\rightarrow\rightarrow$ 'Course' and 'Instructor' $\rightarrow\rightarrow$ 'TA'

These imply that a specific 'Course' is associated with a set of 'Instructors' and 'TAs', a 'TA' is associated with a set of 'Courses' and 'Instructors', and an 'Instructor' is associated with a set of 'Courses' and 'TAs'. It does not mean that a single 'Course', 'TA', or 'Instructor' value is determined by the other attributes.

## How are Multivalued Dependencies Identified in a Relation?

If we have a relation R with the form $a_1, a_2, ..., a_n \rightarrow\rightarrow b_1, b_2, ..., b_m$, for each pair of tuples 't' and 'u' of R that agree on the attributes 'A', we can find in R some tuple 'v' that:

- Agrees with both 't' and 'u' on the 'A's,
- Agrees with 't' on the 'B's,
- Agrees with 'u' on the attributes that are neither 'A's nor 'B's.

Here, "Tuple generating" implies all combinations must be present in the relation R. This means for each combination of 'A's and 'B's, a corresponding tuple must exist in R.

## How does Multivalued Dependency Relate to Functional Dependency?

MVDs can be seen as a generalization of Functional Dependencies (FDs). For example, in the given 'Course' $\rightarrow\rightarrow$ 'Instructor' MVD, it implies a course is associated with a set of instructors, but not necessarily a single instructor. This resembles a FD but encompasses a broader range of dependencies where one attribute or set of attributes can determine a set of values of another attribute.

## How can multivalued dependencies exist in a relational database?

In a relational database, independent pieces of information can exist in the same relation. A multivalued dependency (MVD) is a full constraint between two sets of attributes in a relation.

For example, given the below relation:

```
course  TA  instructor
----------------------
DB1     aas  [REDACTED]
DB2     aas  [REDACTED]
DB1     aas  john
```

The attribute `course` does not imply `TA` or `instructor`. However, `course` does imply a set of `TA`s and a set of `instructors`. Thus, there is a multivalued dependency between `course` and `TA`, and between `course` and `instructor`.


## How to Understand Multivalued Dependencies?

Continuing our exploration of multivalued dependencies (MVDs), we are considering the relation R with the form: $a_1, a_2, ..., a_n \rightarrow\rightarrow b_1, b_2, ..., b_m$. This indicates that the attributes $a_1, a_2, ..., a_n$ multivalued determine the attributes $b_1, b_2, ..., b_m$. 

Let's further break this down using an example:

We have a university course database with the attributes 'course', 'TA', and 'instructor'. For each course, there can be multiple TAs and instructors. The course could have TAs named 'aas', 'jan', and 'fra', and instructors named 'john', '[REDACTED]', and another '[REDACTED]'. This is an example of an MVD where 'course' $\rightarrow\rightarrow$ 'instructor'.

The key rules to keep in mind regarding multivalued dependencies are:

- For each pair of tuples 't' and 'u' of R that agree on the attributes 'A', we can find in R some tuple 'v' that agrees with:
  - both 't' and 'u' on the 'A's,
  - with 't' on the 'B's, and
  - with 'u' on the attributes that are neither 'A's or 'B's.
- "Tuple generating": all combinations must be there. This means that for every combination of 'A's and 'B's, there must be a corresponding tuple in the relation. If this is not the case, then the multivalued dependency is not satisfied.
- MVDs are a generalization of the notion of functional dependencies.

The notion of 'course' $\rightarrow\rightarrow$ 'instructor' implies that a specific course can be linked to a set of instructors, but not necessarily a single instructor. This is a key characteristic of multivalued dependencies.

These rules help to define and understand the structure of a database and aid in ensuring its proper normalization.


## What are the Rules for Multivalued Dependencies (MVDs)?

Multivalued dependencies occur in database normalization, where we want to ensure that each logical data relationship is depicted only once. A multivalued dependency (MVD) exists when there are three attributes (A, B, C) in a relation and for each value of A, there are multiple values of B and C that are independent of each other.

Two types of MVDs are:

1. **Trivial MVDs:** This is a dependency where attribute B is a subset of attribute A or attribute A union attribute B includes all attributes. It is always valid. This can be written as $A\rightarrow\rightarrow B$.

    Example: If we have a database of university courses (DB1) with attributes 'course', 'TA', and 'instructor', a trivial MVD might be 'course' $\rightarrow\rightarrow$ 'TA', meaning that for each course, there are multiple TAs that can be independently assigned to that course.

2. **Non-Trivial MVDs:** These are dependencies where neither $B \subseteq A$ nor $A \cup B = U$, where U is the universal set (the entire set of possible attributes).

    Example: In the same university courses database (DB2), 'course' $\rightarrow\rightarrow$ 'instructor' could be a non-trivial MVD, if there can be multiple independent instructors for each course.

Remember, any functional dependency is a multivalued dependency.

## Quiz: Understanding Dependencies

Consider the table below:

| beer | drinker | size |
|------|---------|------|
| Beck’s | ira | .5 |
| Jever | tore | .5 |
| Jever | tore | .3 |
| Beck’s | eric | .5 |
| Beck’s | ira | .3 |

**Question:** Which of the following dependencies apply to this table?

A. No dependencies with beer on the left side  
B. $beer \rightarrow\rightarrow drinker$  
C. $beer \rightarrow drinker$  
D. $beer \rightarrow\rightarrow drinker$ and $beer \rightarrow drinker$  

**Answer:** B. $beer \rightarrow\rightarrow drinker$. There are multiple drinkers for each beer type, suggesting a multivalued dependency.

## What are the Learning Outcomes of this Topic?

By the end of this topic, you should be able to:

- Describe and detect anomalies in databases.
- Determine functional dependencies and multivalued dependencies in databases.
- Reduce redundancy in databases via decomposition.

## Self-Review: Understanding Dependencies and Keys

- **Dependencies:** These describe how the values of certain attributes are determined by the values of other attributes. We determine them in order to identify and remove redundancy in our database.
- **Functional dependencies:** These are related to keys in that keys are a set of attributes on which other attributes are functionally dependent. They are defined by a set of tuples that map from a set of attribute A to a set of attribute B. A functional dependency is found by observing the relationship between attributes in the database. The closure of a set of functional dependencies is the set of all dependencies logically implied by that set. It is computed by applying inference rules.
- **Multivalued dependencies:** These describe a relationship between two sets of attributes in a database. A multivalued dependency is found by observing the relationship between attributes in the database. They are related to functional dependencies in that both are used in the normalization process of a database to reduce redundancy and improve the integrity of the database.


