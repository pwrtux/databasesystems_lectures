# Overview
- Files are used to store databases and are organized in blocks on a disk
- Different file organizations include heap files, sequential files, and sorted files
- Disk concepts such as block size and contiguous block I/O affect file organization and access times
- Tables can be stored in a sorted order using clustering indices
- Indices are data structures that help find rows quickly, and a table can have multiple indices
- There are different types of indices, including primary, secondary, and clustering indices
- B-trees are a commonly used data structure for indices in databases
- B-trees allow for fast insertion, deletion, and search operations on indexed attributes
- Range queries and point queries can be efficiently executed using B-trees


# What did we learn in the last lecture?

In the last lecture, we covered:

- **Foreign keys and other constraints:** These are restrictions placed on the data in a database to ensure its integrity. A foreign key is a column or group of columns used to establish a link between the data in two tables.

- **Triggers:** Triggers are stored procedures in a database that automatically respond to a specific event (like an insert, update, or delete) on a particular table. Triggers can help maintain data consistency and enforce business rules.

- **Views:** In SQL, a view is a virtual table based on the result-set of an SQL statement. Views don't store data themselves, but display data stored in other tables. Views can simplify query execution and provide an additional layer of data security.

A quiz was presented to check our understanding of triggers.

# Quiz Question
```sql
CREATE TRIGGER AS MovedOut
AFTER DELETE ON People
REFERENCING OLD ROW AS OldEntry
NEW ROW AS NewEntry
FOR EACH ROW
WHEN (
  OldEntry.group = 'phd' AND
  NewEntry.office IS NOT NULL
)
DELETE FROM People 
WHERE userid = NewEntry.userid;
```

**What is the error count of the above SQL statement?**

- A. 0 errors
- B. 1 error
- C. 2 errors
- D. 3 errors
- E. I don’t know

Answer: The answer will be **C. 2 errors**. Triggers can't reference a "NEW ROW" for a DELETE operation as there's no new data coming in. And also, the DELETE statement inside the trigger body is unnecessary and could lead to recursive deletion which is generally avoided in database design.

# What will we learn in this lecture?

In this lecture, we aim to understand disk and file organization for databases, learn how to design and create indices in SQL, and get acquainted with B-trees.

# What are File Structures in a Database?

In databases, data is stored as a collection of files. These files are usually stored on a disk and are accessed randomly. Each file is a set of records, typically of the same type. A record is a sequence of fields. Records can have a fixed size or a variable size.

# How do Disk Concepts influence File Structures?

File systems are organized in such a way that blocks are not necessarily contiguous. This can lead to slower access times when the data is not organized properly on the disk. The use of contiguous block I/O is faster as it avoids disk seek. File systems can be reorganized to make block chains contiguous.

# How are Tables stored?

Tables in a database are typically stored on a disk, with several rows fitting into one disk block. Due to the slow nature of disks, it's important to maximize packing density and minimize response time. It's typical to model complexity by counting only I/Os, as one disk access equals about 1,000,000 RAM accesses.

# How does the organization of Heap Files affect performance?

In a heap file, records are stored in blocks with no particular order. The complexity of operations on a heap file are as follows:

- Searching for a record: $\lceil\frac{n}{2R}\rceil$ (O(n), slow)
- Insertion: 2 (O(1), fast)
- Deletion (k records): $\frac{n}{R}$ (O(n), slow)
- Modification (k records): $\frac{n}{R}$ (O(n), slow)

# What are Sequential Files?

Sequential files are suitable for applications that need sequential access. Records in the file are ordered by a search key, making search operations have a cost of O(log2(n)). The cost of reorganizing

 a sequential file to restore block contiguity and remove empty blocks is O(n).

# How do we Sort a Table?

Sorting a table on disk requires specialized versions of algorithms due to the importance of I/O accesses. Merge sort, for instance, can be used but is often impractical due to the large number of read/write operations and consequently high time consumption. The Two-Phase Multiway Merge Sort is a more efficient approach, with a significantly lower time consumption.

# How can Indices help in data retrieval?

Indices are data structures that help in quickly finding rows. They identify rows by a subset of the attributes. While they make some queries faster, they can slow down modifications. An index on several attributes also gives an index for any prefix of those attributes.

# Quiz Question

`CREATE INDEX Idx ON R(a1,a2,...,an);`

**What types of queries does this index benefit?**

- A. Benefits all queries on R
- B. Benefits queries such as a range query on a1, a2, and a3
- C. Benefits queries such as a range query on a17
- D. Benefits only join queries on R
- E. I don’t know

Answer: **B. Benefits queries such as a range query on a1, a2, and a3.** The index on multiple attributes helps in queries on a range of the indexed attributes or any prefix of those attributes.

# How to Use Indices?

Indices make some queries easier, such as range or point queries on the indexed attributes. However, indices may not necessarily help with all types of queries. For instance, a range query on a non-indexed attribute won't really benefit from the index. In the case of large table modifications, it might be beneficial to drop the index and rebuild it afterwards.

# What is an Index File?

An index file is an auxiliary file that helps with applications that require random access. It usually works in combination with a sequential file and contains entries of the format `<search-key, pointer to record>`, ordered on the search key.

# What is a Primary (Sparse) Index?

A primary index is defined on a data file ordered on the primary key. It can either be dense (having an entry for each search key value) or sparse (having fewer index entries than search key values). A primary index is particularly useful for enhancing the speed of data retrieval operations.

# What is a Primary Index in Databases?

A primary index is an index that is built on the primary key field of a database record. It is a sorted file that stores the primary key and its corresponding disk block ID. In a primary index, there is an entry for each search key value that appears in the data file. 

Let's consider a data file with customers' information:

```
CustomerID | Name     | Street     | City   | State
----------------------------------------------------
123456     | Melanie  | 701 Broad  | Tucson | AZ
246800     | Eric     | 701 Broad  | Tucson | AZ
336699     | Merrie   | 123 Speed  | Tucson | AZ
456789     | Tom      | 197 Cardiff| Houston| TX
555555     | Lee      | 221 Post   | Houston| TX
678900     | Jane     | 197 Cardiff| Houston| TX
890890     | Linda    | 234 Oak    | Houston| TX
993889     | David    | 564 Alberca| Tucson | AZ
```

In this case, the `CustomerID` is the primary key, and a primary index could look like this:

```
Index | CustomerID
------------------
1     | 123456
2     | 246800
3     | 336699
4     | 456789
5     | 555555
6     | 678900
7     | 890890
8     | 993889
```

# What is a Secondary Index in Databases?

A secondary index is an index defined on a data file that is not ordered on the index search key. It provides an alternate path to access the data. Unlike a primary index, a secondary index can be either sparse or dense.

The secondary index example from the slides, based on `Name`, would look like this:

```
Index | Name
--------------
1     | David
2     | Eric
3     | Jane
4     | Lee
5     | Linda
6     | Melanie
7     | Merrie
8     | Tom
```

# What is a Clustering Index in Databases?

A clustering index is defined on a data file ordered on a non-key field. There is only one index entry for each distinct value of the field. This index points to the first data block of records for the search key. 

A clustering index on `City` from our previous data could look like this:

```
Index | City
--------------
1     | Houston
2     | Tucson
3     | Wichita
```

# What is a Multi-level Index in Databases?

A multi-level index is an index on an index, until all entries of the top level fit in one disk block. This kind of index is useful when the data file is so large that the index can't fit into the main memory. Every level of the index is an ordered file.

# Quiz Question

**What is true for multi-level indices?**

A. All levels of the index must be sparse
B. All levels of the index except for the first-level index must be sparse
C. All levels of the index must be dense
D. All levels of the index must be dense except for the first-level index
E. I don’t know

**Answer:** The correct answer is **D. All levels of the index must be dense except for the first-level index**. In a multi-level index, the first-level index can be sparse, meaning there's an entry for each block of data. However, all subsequent levels of the index should be dense, containing an entry for every search key value.

# How to Search a Single-Level Index?

There are

 two primary ways to search a single-level index:

- **Sequential search**: This is faster than a linear search of the data file because the index is smaller than the data file. However, the worst-case search cost is still $O(n)$.
- **Binary search**: This is a more efficient way to search, especially if the key space is large. The search cost is $O(\log_2{n})$, where $n$ is the size of the index.

# What is a Clustering Index?

A clustering index is an index that keeps the rows of a table in the database in a specific order. This order is determined by the index key. In a clustering index, indexed rows are scattered in the table, but each index points to rows having consecutive key values. The result is equivalent to sorting the table. However, there can be at most one clustering index per table.

For example, to create a clustering index on the `Exams` table for the columns `vip`, `date`, and `time`, the following SQL command can be used:

```sql
CREATE INDEX ExamIndex ON Exams CLUSTER(vip,date,time);
```
This means the rows of the `Exams` table will be stored on the disk in the order of `vip`, `date`, and `time`.

# What is the Role of Indices in Database Queries?

Indices play a vital role in the optimization of database queries. If a query uses only indexed attributes, it can be evaluated in virtually constant time. The speedup is because the database can directly access the memory blocks containing the indexed attribute values without having to scan the entire table.

For example, in the query:

```sql
SELECT date
FROM Exams
WHERE vip = 'jan';
```
If `vip` is an indexed attribute, the database can quickly locate all entries where `vip` is 'jan' without scanning the entire table.

Similarly, if a query contains multiple conditions, we can use an index scan for each condition, then compute the intersection (for AND conditions) or disjunction (for OR conditions) of the result sets. For example:

```sql
SELECT *
FROM R
WHERE x = 42 AND y > 87;
```

If `x` and `y` are both indexed, we can first find all entries where `x` equals 42 and `y` is greater than 87, then intersect the two results to get the final result.

# What is a B-tree and Why is it Used in Databases?

A B-tree is a self-balancing tree data structure that maintains sorted data and allows for efficient insertion, deletion, and search operations. It's a variation of search trees that trades some extra space to gain better performance. Each node in a B-tree can contain multiple keys and links, unlike binary trees, where each node contains only one key and two links.

B-trees are widely used in databases and file systems because of their "perfect" suitability for disk storage. They have high fan-out (many keys per node), are robust to changes in data volumes, and are efficient in managing large amounts of data that won't fit in memory.

An example B-tree could look as follows:

```
      100
     /   \
   30     120
  /  \   /  |  \
3  5  11 101 110 130
```

In this B-tree, each node is stored in one disk block, and each row is pointed to by a leaf node.

# What are the Invariants of a B-tree?

There are several invariants, or rules, that a B-tree must maintain:

- Each node must hold at least $\lfloor (k+1)/2 \rfloor$ pointers, where $k$ is the maximum number of keys that a node can hold. The only exception is the root, which can have down to 2 pointers.
- All leaves (end nodes) must be at the same level. This rule ensures that the tree remains balanced.
- The height of the tree with $n$ rows is at most $1 + \log_{\frac{k}{2}}(n)$. In practice, the height is usually 3 or 4.

# Quiz Time

**Assuming that the root is held in main memory, and that each node is one page, how many I/Os for a range query from 101-166?**

A. 2
B. 3
C. 4
D. 5
E. 6
F. 9

**Answer:** The correct answer is **C. 4**. In a B-tree, a range query requires an I/O for each level of the tree from the root to the leaves, and for each leaf in the range. In the provided B-tree, the range 101-166 would need to traverse 2 levels down the tree and access 2 leaf nodes (101-110 and 120-130).

# How Does a Range Query Work in a B-tree?



A range query in a B-tree involves searching for a subset of keys between two values. The search is performed by following the path from the root to the leaf nodes, but only for the nodes falling within the query range. The time taken for this operation is proportional to the height of the tree and the extent of the range.

For example, if we wanted to perform a range query for keys between 101 and 166 in the B-tree example above, we would traverse down from the root, selecting the paths that lead us to nodes within our range. In this case, we would end up with the subtree consisting of keys 101-110 and 120-130.

# How Do We Insert a Key into a B-tree?

To insert a key into a B-tree, we start by finding the leaf node where the key should be inserted. If the node has fewer than the maximum allowed keys, we insert the key in the correct position. If the node is already full, we need to split it and adjust the tree to maintain the B-tree invariants. A simple case of insertion would be when the leaf node has space for the new key.

Let's consider inserting 33 into our B-tree. Since 33 is between 30 and 100, it should go into the left child of the root node. However, that node is already full. Hence, the node will need to be split, and the tree will need to be adjusted to accommodate the new key. The next slides likely explain this process in detail.

# How Does Insertion Work in a B-tree?

In a B-tree, the insertion process follows several steps, adjusting the tree as necessary to maintain the B-tree invariants. Let's go over some cases:

## Inserting a Key into a Leaf Node That Has Space

When inserting a key into a leaf node that has space for more keys, we simply put the key in the correct position. For instance, if we were to insert the key 33 into our previous B-tree example, it would fit perfectly into the left child of the root node, since that node has room for one more key.

## What Happens When We Insert a Key into a Full Leaf Node?

When we attempt to insert a key into a leaf node that is already full, the node has to be split. For example, if we want to insert the key 7, we see that the leaf node containing keys 3, 5, and 11 is full. We can split this node into two nodes, one containing keys 3 and 5, and another containing keys 7 and 11. The parent node then gets updated to contain the key values of its new children.

## What Happens When We Insert a Key that Causes an Internal Node to Split?

Inserting a key that leads to an internal node splitting is a slightly more complex case. Suppose we wish to insert the key 160. This would cause the internal node containing keys 120, 130, 150, 156, and 179 to split. The process is similar to splitting a leaf node, but we also need to update the parent node and possibly the root of the tree.

## What Happens When We Insert a Key that Causes the Root to Split?

In the rare case where we need to insert a key that causes the root to split, we create a new root and increase the height of the tree. For example, if we wanted to insert the key 45 into a tree with root containing keys 30, 32, and 40, we would split the root and create a new root containing keys 30, 40, and 45.

# Can We Delete Keys from a B-tree?

Yes, keys can be deleted from a B-tree. However, deleting keys from a B-tree involves rebalancing the tree to maintain the B-tree invariants. This process has similar case-based algorithms to the insertion process, and is a bit complex.

Because deletion can require significant overhead, in many cases, deleted rows are left as "tombstones" -- placeholders indicating a deletion -- rather than removing them and rebalancing the tree immediately. As most tables tend to grow with time, these tombstones quickly get reused for new entries.

However, if there are too many tombstones, the index may need to be periodically rebuilt or reorganized online to ensure the tree's efficiency.

# Summary

In this section, we learned about how B-trees manage data in databases, and how to perform various operations, including searches and insertions. This knowledge is crucial for understanding disk and file organization for databases and designing and creating indexes in SQL.

# Self-Review Questions

Let's recap the key concepts we've covered in this section. You should be able to answer the following questions:

## How Is Data Stored in Files?

Data in a database is typically stored in files on disk. Each file is structured in a specific format, such as a heap or a sorted file.

## How Are These Files Laid Out and Accessed?

Files can be laid out as either heap files or sorted files. Heap files allow for fast insertions, but searching can take a long time. On the other hand, sorted files are slower for insertions but allow binary search, which makes searching for specific data faster.

Data is retrieved from or added to these files through the process of reading from or writing to the disk. The DBMS handles these operations.

## How Does Sorting Impact Times for Query, Insertion, Deletion, and Modification?

Sorting can significantly impact the performance of queries, insertions, deletions, and modifications. For example, sorted files allow for fast binary search queries but slow down insertions because new data must be placed in the correct order. Conversely, heap files allow for fast insertions but slow down search queries.

## What Are Indices, and What Do the Terms "Sparse", "Dense", "Clustered", and "Multi-level" Mean?

Indices are data structures that allow quick lookup of data in a database. 

- Sparse indices only include an entry for some of the records, reducing the size of the index.
- Dense indices include an entry for every record, providing faster lookups but at the cost of larger index size.
- Clustered indices determine the physical order of data in a database. There can only be one clustered index per table.
- Multi-level indices involve creating an index on an index for larger databases, which improves search performance.

## How Can Multiple Indices Be Used?

Multiple indices can be used to speed up queries that involve more than one field. For example, one index could be used for the `vip` field and another for the `date` field in a table of `Exams`.

## What Is a B-Tree and How Do You Query It?

A B-Tree is a self-balancing tree data structure that maintains sorted data and allows for efficient insertion, deletion, and search operations. Each node in a B-Tree can contain multiple keys and links to child nodes.

To query a B-Tree, you start from the root and traverse the tree. Depending on whether the desired key is less than, equal to, or greater than the key in the current node, you go to the left child, return the current node, or go to the right child, respectively.

## How Is a B-Tree Kept Balanced?

A B-Tree is kept balanced by splitting full nodes during insertion and merging or redistributing keys during deletion. The tree always remains height-balanced after these operations.