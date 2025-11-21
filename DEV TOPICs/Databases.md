## ORMs

object-relational Mapping as the name suggest it maps tables to classes and rows to objects. ORM handle the translation between objects and database schemas , manage relationships and often provide features like lazy loading and caching.

example: Prisma(I have used )

Drawbacks :

- Performance Overhead
- Abstract Leaks
- Less Flexible
- Add a little complexity
- Performance is OK for usual queries, but a SQL master will always do better with his own SQL for big projects.

  

## ACID

4 properties that ensure reliable processing of database transactions.

Atomicity, Consistency, Isolation, and Durability

[https://retool.com/blog/whats-an-acid-compliant-database](https://retool.com/blog/whats-an-acid-compliant-database)

### Atomicity

transaction is treated as single unit , either completes entirely or fails completely .

### Consistency

maintains the database in a valid state before and after a transaction. Data integrity constraints must be followed .

### Isolation

concurrent transaction doesn’t interfere with each other . (it follow sequential way )

==Optimistic vs Pessimistic Concurrency Control==

### Durability

once transaction is committed , it remains so even in the event of system failure .

## CAP Theorem

- **Consistency (C)** – Every read receives the most recent write or an error.
- **Availability (A)** – Every request receives a (non-error) response, without guarantee that it contains the most recent write.
- **Partition Tolerance (P)** – The system continues to operate despite arbitrary message loss or failure between nodes.

[https://chatgpt.com/share/687134f3-75e0-8006-8c64-80b6f68d0a9c](https://chatgpt.com/share/687134f3-75e0-8006-8c64-80b6f68d0a9c)

  

  

## N+1 Problem

when an application performs a query to retrieve a list of items and then issues additional queries to fetch related data for each item individually. (inefficient and performance issues )

Example : you queried for books than run individual query on each book to fetch let say author name it’s waste , instead we can directly fetch for the author of books . Former took N+1 query where later took only 1 and correct optimized query .

==**Solutions to the N+1 problem typically involve optimizing queries to use joins or batching techniques to retrieve related data in fewer, more efficient queries.**==

## Normalization

a process of structuring a relational database in accordance with a series of so called normal forms in order to reduce data redundancy and improve data integrity.
also to reduce data dependency and minimize insertion, deletion and update anomalies 
## Different Level of Normalization

- 1NF - First Normal Form
    
    Rules:
    
    1. Every Column/ Attribute need to have a single value .
    2. Each row should be unique. Either through a single or combination of multiple columns. Not mandatory to have a primary key .
- 2NF- Second Normal Form
    
    Rules:
    
    1. Must be in 1NF
    2. All non key attributes must be fully dependent on candidate key. (if partially dependent on then split them into separate table )
    3. Every table should have primary key and relationship between the tables should be formed using foreign key . (Relation ship Table holds just relationship through primary key)
- 3NF- Third Normal Form
    
    Rule : Avoid Transitive Dependencies . create a separate table for all A, B, C as you can map or have relation between them .
    

4NF- Fourth Normal Form

BCNF

5NF

6NF

  

## Anomalies ( problems) faced by denormalized dataset

==Insertion Anomalies== ==: If we have just one table storing all info there would be col having same data with just a few col different (same col could be product name , customer name (repeated customer), and many) also i we have add certain data which might not have any other col filled fill cause poor query (like new product introduced need not to have customer attached )==

==Deletion Anomalies== : while deleting the entry you might delete col which hold data which is correct and can be used ( like a wrong order deletion doesn’t require to erase the product details associated with it from database )

==Updation Anomalies== : Update the item only once instead of every where it is present in denormalized data .

## Failure Modes

refer to the various ways in which a database system can malfunction or cease to operate correctly. Common failure modes involve data loss, system unavailability, replication lag in distributed databases, and deadlocks.

## Profiling Performance

Profiling is essential for diagnosing performance issues and ensuring that applications meet desired performance standards. Profiling tools can provide insights into how different parts of the code contribute to overall performance, highlighting slow or resource-intensive operations

  

[https://servebolt.com/articles/profiling-sql-queries/](https://servebolt.com/articles/profiling-sql-queries/)

ever need to improve performance of your SQL query use above doc . Discussed query optimization technique are useful

## Migrations

Database migrations are a version-controlled way to manage and apply incremental changes to a database schema over time, allowing developers to modify the database structure (e.g., adding tables, altering columns) without affecting existing data.

[https://www.prisma.io/dataguide/types/relational/what-are-database-migrations#how-do-you-use-database-migrations](https://www.prisma.io/dataguide/types/relational/what-are-database-migrations#how-do-you-use-database-migrations)

## Data Indexing

## Overview

Database indexes are data structures that ==improve the speed of data retrieval operations== in a database management system. They work similarly to book indexes, providing a quick way to look up information based on specific columns or sets of columns. Indexes ==create a separate structure== that holds a reference to the actual data, allowing the database engine to find information without scanning the entire table. While indexes significantly enhance query performance, especially for large datasets, they come with trade-offs. ==They increase storage space requirements and can slow down write operations as the index must be updated with each data modification.== Common types include ==B-tree indexes== for general purpose use, ==bitmap indexes== for low-cardinality data, and ==hash indexes== for equality comparisons. Proper index design is crucial for optimizing database performance, balancing faster reads against slower writes and increased storage needs.

  

## Data Replication

Data replication is the process of creating and maintaining multiple copies of the same data across different locations or nodes in a distributed system. It enhances data availability, reliability, and performance by ensuring that data remains accessible even if one or more nodes fail. Replication can be synchronous (changes are applied to all copies simultaneously) or asynchronous (changes are propagated after being applied to the primary copy). It's widely used in database systems, content delivery networks, and distributed file systems. ==Replication strategies include master-slave, multi-master, and peer-to-peer models==. While improving fault tolerance and read performance, replication introduces challenges in maintaining data consistency across copies and managing potential conflicts. Effective replication strategies must balance consistency, availability, and partition tolerance, ==often in line with the principles of the CAP theorem.==

https://docs.google.com/document/d/17-eZbHn_a93gb3NRY1A-6k3CclxlhZ-FSI8WVrJyN4E/edit?usp=sharing

## Database Sharding

Sharding strategy is a technique to ==split a large dataset into smaller chunks== (logical shard) in which we distribute these chunks in different machines/database nodes in order to distribute the traffic load. It’s a good mechanism to improve the scalability of an application. Many databases support sharding, but not all.

[https://g.co/gemini/share/f44e7002bf23](https://g.co/gemini/share/f44e7002bf23)