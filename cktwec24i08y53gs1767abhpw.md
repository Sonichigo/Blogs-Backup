---
title: "The Cassandra Query Language"
datePublished: Thu Sep 23 2021 03:49:24 GMT+0000 (Coordinated Universal Time)
cuid: cktwec24i08y53gs1767abhpw
slug: the-cassandra-query-language
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1647140950697/fs47fg_P-.png
tags: databases

---

### What is SQL?

SQL stands for Structured Query Language, and is used to communicate with a database using certain statements. SQL statements are used to perform tasks such as update data on a database, or retrieve data. Companies like Oracle, Sybase, Microsoft SQL Server, Access, Ingres, etc are relational database systems that uses SQL.

### What is NoSQL?

NoSQL (“non SQL” or “not only SQL”) databases were developed with a focus on scaling, fast queries, allowing for frequent application changes, and making programming simpler for developers. Relational databases accessed with SQL (Structured Query Language) were developed with a focus on reducing data duplication as storage was much more costly than developer time. SQL databases tend to have rigid, complex, tabular schemas and typically require expensive vertical scaling.

<img src="https://www.improgrammer.net/wp-content/uploads/2020/04/NoSQL-Database-Types.jpg" width=200px,height=250px>


### What is CQL then?

CQL (Cassandra Query Language) is the language for managing database resources such as keyspaces, tables, functions, aggregates, user-defined types, roles, and access permissions. CQL can be used to insert, update, and query tables, plus much more. It is similar to SQL, so anyone familiar with the commands and statement will find it easy to work with.

## What is Astra DB?

DataStax Astra DB provides the ability to develop and deploy data-driven applications with a cloud-native service, without the hassles of database and infrastructure administration. By automating tuning and configuration, Astra DB radically simplifies database and streaming operations.

<img src="https://www.datastax.com/sites/default/files/content/blog/2021-03/astra-blog.png" width=200px,height=250px>

Astra DB simplifies cloud-native Cassandra application development. It reduces deployment time from weeks to minutes, and delivers an unprecedented combination of serverless, pay-as-you-go pricing with the freedom and agility of multi-cloud and open source.

The Cassandra Query Language SHell (CQLSH) is a command line shell for interacting with your database through Cassandra Query Language (CQL) provided by Datastax. This tool provides a useful interface for accessing the database and issuing CQL commands. Each DataStax Astra DB database includes an embedded CQL shell instance. In the Astra DB console, navigate to your database and click the CQL Console tab to open a CQLSH instance that is connected to your database. Issue CQL commands directly to your Astra DB database without navigating outside of your browser.