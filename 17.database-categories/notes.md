Databas Fundamentals

- db provides organized and persistent storage of your data.
- you need to understand durability, availability, RTO, RPO, consistency and transactions to choose between different databases.
- to solve the problem of data centers crashing our database, you will create a standby database with synchronization replication of the original database.
- the standby database will have a snapshot running to take the snapshots of the database.
- the slowing down of the database will be solved and also the crashing of the data center becuase the standby database will be running.
-

Availability and Durability

1. Availabilit: being able to access my data now and when I need it.
   percentage of time an application provides the operations expected of it.

2. Durability: will my data be availabe after 10, 100 or 1000 years.

3. Measuring durability and availability
   4 9's -> 99.99
   11 9's -> 99.999999999
   Typically, an availability of four 9's is considered very good.
   Typically, a durability of eleven 9's is considred very good.

Availability;
99.95% means 22 mins downtime a month
99.99% (4 9's) 4 and 1/2 minutes downtime. Typical online apps aim for 99.99% availability.
99.999 (5 9's) 26 seconds down time. Achieving 5 9's availability is tough.

Durability;
11 9's means if you store one million files for 10 million years, you are expected to lose only one file.
durability should be high because losing data is dangerouse.

Increasing Availability and Durability of Databases.

1. Increasing Availability -> have multiple standby available or distribute the database.
   distribute db across multiple regions and multiple zones.
2. Increasing Durability -> have multiple copies of data (standbys,snapshots,transaction log and replicas) in multiple zones and multiple regions.

Replicating data hsa its own challenges

## Database Terminologies : RTO and RPO

- typical business are fine with downtime but not losing data.
- how to measure how quickly we can recover from data?

1. RPO (Recovery Point Objective): Maximum acceptable period of data loss.
2. RTO (Recovery Time Objective): Maximum acceptable downtime.

Achieving minimum RTO is objective.

Achieving RTO & RPO - Failover Examples.

1. Scenario: Small Data loss(RPO-1min) - Hot Standby - automatically synchronize data.
   Scenario: Very small downtime(RTO-5mins) - have standby ready to pick up load. Use failover from master to standby
2. Scenario: Very small data loss (RPO - 1 minutes) but can tolerate some downtimes (RTO - 15 minutes)
   Solution: Warm Standby, automatically synchronize data. Have standby with minimum infrastructure and scale it up when failure happens.
3. Scenario: Data is Critical (RPO - 1 minute) but can tolerate downtime of a few hours (RTO - few hours)
   Solution: Create regular data snapshots and transaction logs. Create database from snapshots and transactions logs when failure happens.
4. Scenario: Data Can be lost without a problem (example cached data)
   Solution: Failover to completely new server

5. Scenario: Reporting and Analytics Applications
   new reporting and analytics applications are being launched useing the same database.
   these applications will only read data
   after some times, you will notice the database is not first
   Solution 1: Vertically scale the database - increase CPU and memory
   Solution 2: Create a database cluster (distribute the database) - Typically database clusters are expensive to set up.
   Solution 3: Create a read only replica. You can connect your reporting and analytics replica to read only. This reduces the loss on master database.

Consistency
How do you ensure data in multiple database instances (standby and replicas) is updated simultenously.
Levels of Consistency

1. Strong Consistency - Synchronous replication to all replicas. It will be slow if you have multiple replicas or standbys
2. Eventual Consistency - uses asynchronous replication. A little lag - few seconds before the changes are available in all replicas. in the intermediate period, different replicas might return different values. Used when scalability is more important than data integrity. eg Social media posts
3. Read-after-Write Consistency - Inserts are immediately available. Updates, however, will be using eventual consistency.

Database Categories

- there are several database categories like relational databases, key value, graph, memory, file etc
- choosing type of db for your use case is not easy but very important
- consider a few factors;
  do you want fixed schema
  what level of transaction properties do you need
  what kind of latency do you want
  how many transactions do you expect
  how much data will be stored in your database

1. Relational Databases

- was the only option till a decade ago
- most popular/unpopular type of databases.
- has predefined schema with tables and relationships
- has strong transactional capabilities i.e changes can be made but will only be commited to all the tables when it is updated on all tables.
- Good for Online Transaction processing.
- Good for Online Analytics processing use cases.

a) Relational Database - Online Transaction Processing

- applications where large number of users make large number of small transactions eg small data reads, update, deletes.
- popular db in this space are SQL
- recommend GCP for this service is Cloud SQL which supports PostgreSQL, MySql, Sql for regional relational databases upto a few terrabytes.
- another option is Cloud Scanner: has unlimited scale and is available 99.999% for global application with horizontal scaling.
- rows in this type of database are stored together

b) Relational Database - Online Analytics Processing

- OLAP are applications allowing users to analyze petabytes of data.
- examples are reporting applications, data ware hoses, business intelligence applications and analytics systems.
- recommended GCP Service for this scenarios are BigQuery
- columns in this type of database is stored together.
- the advantage is data is compressed and the column has the same type of data.
- columns in this type of data can be stored in different nodes. and this allow you to apply single across multiple nodes.

2. NoSQL Databases

- this a new approach to build your databases.
- has flexible schema
- structures data the way your application needs it.
- lets the schema evolve with time.
- horizontally scals to petabytes of data with millions of TPS
- they are scalable and high perfomant.
- GCP services for this are Cloud Firestore, and Cloud Big Table.

a) Cloud Datastore

- managed serverless NoSQL document database
- provides ACID transactions, sql like queries and indexes.
- designed for transactional mobile and web applications
- recommended to small and medium databases.

b) Cloud BigTable

- managed, scalable nosql wide column database.
- it is not serverless as you need to create an instance.
- recommended for data sizes greater than 10 TB to several Petabytes
- recommended for large analytical and operational workloads.
- does not support multi rows applications
- supports only single row transactions

3. In Memory Databases

- retrieving data from in memory is faster than retrieving data from a disk
- in memory datasase like Redis deliver microseconds latency by storing persistent data in memory.
- Recommended GCP service for this is **Memory Store**
- use cases are for caching, session management, gaming leader boards, geospatical applications.

Database Scenario

1. Scenario: A startup with quickly evolving schema (table structure)
   Solution: Cloud Datastore/Firestore
2. Scenario: Non Relational Data with less storage (10GB)
   Solution: Cloud Datastore
3. Scenario: Transactional global database with predefined schema needing to process millions of transactions per second.
   Solution: Use Cloud spanner
4. Scenario: Transactional local database processing thousands of transactions per second
   Solution: Use CloudSQL
5. Cache data (from database) for web applications
   Solution: Use Memory Store
6. Scenario: Database for analytics processing of petabytes of data
   Solution: Use BigQuery
7. Scenario: Database for storing huge volumes of stream data from IOT devices
   Solution: Use BigTable
8. Scenario: Database for storing huge stream of time series data
   Solution:Use big table
