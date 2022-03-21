# Research On Cassandra Database

This repo is for course 7280. I choose Cassandra and research on it. The entire complete and formal report of my research result can be found in the file named "Research Report".

I will elaborate on the following topics:
1. Introduction. 
2. Installation. 
3. Sample code to create database objects.
4. Sample code to use (CRUD) the database. 

## Introduction

Apache Cassandra is one of the most widely used NoSQL database. It was initially developed with intent to optimize Facebook inbox search feature and was first released in 2006. Written in Java, it supports cross-platform operating system. Cassandra is open source and distributed database; user could download and use it from its Github repository or official website. It is trusted by its linear scalability, fault-tolerance, performant, security, low latency, fast scalable, reliability, etc. Especially, according to CAP theorem, in a distributed system, it is common to see network failures and partition tolerant is crucial. Cassandra enables users to balance the level of consistency and availability. These result in it’s been used by thousands of companies who have to manage large active datasets. 

### Architechture
Cassandra architecture endows a database ability to scales and preforms with continuous availability. It is designed to be a best-in-class combination of meeting emerging largescale in data footprint, query volume and storage requirements. It has a masterless “ring” distributed architecture which is easy to scale up or down and manage. Benefiting from its built-for-scale architecture, it can handle large amount of data and allow allows large number of concurrent users/operations. To avoid token imbalance, the usage of “virtual nodes” is proposed upon the multiple tokens per physical node. Virtual nodes enable physical node to take multiple positions in the ring.

As for language, Cassandra uses Cassandra Query Language (CQL), which is a SQL-like language, to organize data within a cluster of Cassandra nodes. Cassandra is consisted of: 
1.	Keyspace: defines the replication of dataset per datacenter. 
2.	Table: defines the typed schema for a collection of partitions.
3.	Partition: defines the primary key that Cassandra must use to identify and locate the node that a row is stored. 
4.	Row: a collection of columns with same primary key.
5.	Column:  a single typed datum of a row.
Contain relations are like below:
Column --> Row --> Partition --> Table --> Keyspace

CAP theorem implies that no database could have all the three guarantees simultaneously, which are consistency, availability and partition tolerance. Nevertheless, Cassandra guarantees high scalability, high availability, durability, eventual consistency, lightweight transaction with linearizable consistency, batched writes and secondary indexes.

### Data Model
Cassandra data modeling is a top-down approach of model creation, in other words, it is query-driven. In the NoSQL data modeling, the workflow is from application to models and then to data. To elaborate, the process is to analyze user behavior, identify workflows and needs, and then define queries. With the queries, table could be designed using denormalization. BATCH is used to insert or update denormalized data of multiple tables. Conceptual data models evolve into logical data model and then to physical data model. 
Cassandra stores data across a cluster of nodes. It uses partition keys and a variant of consistent hashing to partition data among the nodes. Data partitioned into hash tables could be quickly looked up by partition keys. A partition key is generated from the first field of a primary key. If a table has more than one primary key, then apart from the first one, other will be used to sort within a partition rather than functioning as a partition key.

## Installation. 

To install on Mac, I follow the following steps. 

First, install homebrew (or update homebrew to newest version if already have it). 

Run command: 
```brew update```

Then use brew to install Cassandra. 

Run command: 

```brew install cassandra```

Cassandra is installed in the path   /usr/local/Cellar/cassandra .

Then we could start Cassandra by running command:

```brew services start cassandra```

Run command to login to cqsh using default credentials:

```cqlsh```

If we run command ```help```, we will get a list of commands and useful topics. 


## Sample code to create database objects.

First we need to create a keyspace.
Given below is an example of creating a KeySpace named tutorial. 

```CREATE KEYSPACE tutorial WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 3};```

```DESCRIBE keyspaces;```

```USE tutorial;```

Next we create a table named employee:

```CREATE TABLE employee(emp_id int PRIMARY KEY, emp_name text, emp_city text, emp_sal varint, emp_phone varint);```

CRUD stands for Create, Update, Read and Delete or Drop operations.

a)	Create.

Command INSERT is used to insert data into a certain table. We run the below commands to insert into table “employee” that we just created.

```INSERT INTO employee (emp_id, emp_name, emp_city, emp_phone, emp_sal) VALUES(1,'ram', 'Hyderabad', 9848022338, 50000);```

```INSERT INTO employee (emp_id, emp_name, emp_city, emp_phone, emp_sal) VALUES(2,'robin', 'Hyderabad', 9848022339, 40000);```

```INSERT INTO employee (emp_id, emp_name, emp_city, emp_phone, emp_sal) VALUES(3,'rahman', 'Chennai', 9848022330, 45000);```

We can verify the data by running:

```SELECT * FROM employee;```

b)	Update.

Command UPDATE is used to update data in a certain table. For example, we can update the city of employee with id 1 to “Sanjose”, and his salary to 100000:

```UPDATE employee SET emp_city='Sanjose',emp_sal=100000 WHERE emp_id=1;```

Run select command again and we can verify that the data has been updated.

c)	Read.

SELECT is used to read data. As showed above, we select a certain table and get all information about the table by running:

```SELECT * FROM employee;```

We can also select particular columns in the table:

```SELECT emp_name, emp_city from employee;```

We can also use “WHERE” clause to select specific data that meet our requirements. 

```SELECT * FROM employee WHERE emp_sal=10000;```

d)	Drop / Delete.

DELETE command is used to delete certain data from our table. We can add “WHERE” clause to delete certain data that is qualified. 

```DELETE emp_sal from employee where emp_id = 1; ```
 
Now we can see the data we delete become “null”.

DROP:

Command DROP can be used to drop a Keyspace, a table or an index. 
Command “DROP INDEX” is used to drop an index. 

First, we create an index:

```Create index emp_index on tutorial.employee(emp_sal);```

```Drop index emp_index;```

```DROP TABLE employee;```

```DROP KEYSPACE tutorial;```

4.	Exit Cassandra.

We could run this command to exit Cassandra:

```Exit```

We could stop Cassandra using:

```brew services stop cassandra```

