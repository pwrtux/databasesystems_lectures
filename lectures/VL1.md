# History & Data Models

## What are the Learning Outcomes of this Lecture?
1. Outline the milestones in database systems history.
2. Describe the relational data model using correct terminology.

## What is a Database?
A database is a large collection of data organized specifically for rapid search and retrieval. A Database Management System (DBMS) ensures efficient, reliable, convenient, and safe storage, and multi-user access to large amounts of persistent data. Querying is a key feature of databases which is more general than just searching files. For example, querying can be used to find all students in Computer Science who have taken more than 20 courses and passed with an average of at least 2.3.

## What were the Early Applications of Database Systems?
Some of the early applications of database systems included banking systems, airline reservation systems, and record-keeping systems.

## What are some Database Vendors?
The lecture does not mention specific vendors but the landscape includes many players such as Oracle, Microsoft, IBM, and open-source solutions like MySQL and PostgreSQL, among others.

## What are the Milestones in Database Systems History?

### The Ancient Times (Until 1960)
The earliest data storage methods evolved from punched cards to magnetic tapes. IBM's Ramac in 1959 allowed for non-sequential data reading, making file access feasible.

### The Early Days (1960's)
This era saw the introduction of the first database systems which used filesystems to store data, marking the start of the flat-files era. During this period, the network data model (Bachman's Integrated Data Store) and hierarchical data model (IBM's Information Management System) emerged.

### The Breakthrough (1970's)
During this decade, commercialization took hold and the hierarchical and network model dominated. Codd's paper laid the foundations of the relational data model between 1970-1972. The first query languages appeared and the first relational database systems research projects began, such as IBM's System R and UC Berkeley's Ingres.

### The Takeover (1980's)
The 1980s saw the full adoption and commercialization of relational database systems, the development of the Structured Query Language (SQL), the implementation of the client-server architecture, and the appearance of the first object-oriented database systems.

### Extend and Scale (1990's)
The 1990s saw the development of object-relational database systems and the ability to handle complex data such as multimedia, spatial, and temporal data. The era also saw the rise of distributed database systems, parallel database systems, and data warehouses.

### The Future (2000 onwards)
In recent years, we've seen the rise of novel technologies such as column-oriented data models, in-memory database systems, No-SQL, key-value stores, and hybrids like HandoopDB. Furthermore, other types of databases like graph databases have gained popularity.

## What are Data Models?
A data model is a (mathematical) representation of data, which could be in the form of tables/relations, sets, multisets, lists, trees, or graphs. They contain constraints on data such as data types, uniqueness, and dependencies, and define operations on data like insert, delete, update, and query.

### What are the Key Characteristics of Hierarchical, Network, and Relational Data Models?

#### Hierarchical Data Model
This model represents data in a tree-like structure with records and links. While it was an improvement over flat files, it had challenges in updates, flexibility, and complexity on the application side.

#### Network Data Model
This model, appearing in the late 1960s, represents data in a graph-like structure, providing more flexibility than the hierarchical model. However, it also had issues with updates, flexibility, and complexity on the application side.

#### Relational Data Model
In this model, data are stored in tables or relations with attributes representing different types of information within a particular table. Rows or tuples store entries – values for a particular piece of information. Columns contain the name and values for a particular type of information. 

## What are NULL values in the Relational Data Model?
In the relational data model, an attribute value can be NULL if it's unknown, no value exists, or it's unknown whether a value exists or not. These NULL values are treated specially.

## What are the Advantages of the Relational Data Model?
The relational model is simple and intuitive, often convenient for real-life data. Other models can be useful in certain settings such as XML for semi-structured data, RDF for semantic Web data. It has an elegant mathematical foundation based on set and multi-set theory, relational algebra and calculi, allows efficient algorithms and has industrial strength implementations available.

## Quiz Time
The correct answer is C. A column contains the values for a particular attribute.

## What is a Running Example of a Database?
A database behind a tiny calendar system would have tables for Rooms, People, Meetings, Participants, Equipment.

## What does the Summary of the Lecture entail?
The lecture teaches how to outline the milestones in database systems history and describe the relational data model using correct terminology.

## Self-Reviewing Questions
- The relational model consists of tables or relations that have attributes representing different types of information. Rows or tuples in these tables store entries which are values for a particular piece of information.
- A schema contains the name of the relation, the names of the attributes, the types of the attributes, and constraints.
- NULL values are used to represent unknown data, non-existing data, or when it is uncertain whether a value exists or not.
