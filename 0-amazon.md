>Remember to go over every technical skill, e.g. Docker, and project details that is listed on resume.

>Search and practice Amazon's object-oriented design and system design questions.

## How to answer system design questions

1. Start with one machine
2. Duplicate the problem over multiple machines
3. Design algorithms for distributing client requests or tasks over multiple servers.
  3.1 Session persistence: if sessions or caches are being stored on servers, we need to make sure subsequent requests from one user are always sent to the same server.
  3.2 How to share session data across multiple backend servers: shared database, in-memory session storage via Memcached
  3.3 Where to store client session data: on backend server, e.g. a shared state server, which costs more space on the backend; or at the client side, e.g. cookie, URL rewriting,  which takes more bandwidth to transfer and more time on the server to compute if the payload is big. It is always about making trade-offs between time and space.
4. Look for and account for single point of contention and failure in the system, e.g. single point of failure for load balancers, database servers. Solution: replication
5. Account for adding or removing computing resource

[Server load balancing architectures, Part 1: Transport-level load balancing](http://www.javaworld.com/article/2077921/architecture-scalability/server-load-balancing-architectures--part-1--transport-level-load-balancing.html)

[Server load balancing architectures, Part 2: Application-level load balancing](http://www.javaworld.com/article/2077922/architecture-scalability/server-load-balancing-architectures-part-2-application-level-load-balanci.html)

[Consistent caching](http://theory.stanford.edu/~tim/s16/l/l1.pdf)

[Google technology stack](http://michaelnielsen.org/blog/lecture-course-the-google-technology-stack/)

[What really happens when you navigate to a URL](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)

[DATABASE SHARDING](http://www.agildata.com/database-sharding/)

[How Sharding Works](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6#.8wc7t1o9z)

[Consistent hashing](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html)

[Consistent Hashing in memcache-client](http://www.mikeperham.com/2009/01/14/consistent-hashing-in-memcache-client/)

[Visual Representation of SQL Joins](https://www.codeproject.com/kb/database/visual_sql_joins.aspx)

[How do database indexes work? And, how do indexes help?](
http://www.programmerinterview.com/index.php/database-sql/what-is-an-index/)

[One-to-one, one-to-many, many-to-many relationships in database](https://code.tutsplus.com/articles/sql-for-beginners-part-3-database-relationships--net-8561)

[Amazon's Dynamo](http://www.allthingsdistributed.com/2007/10/amazons_dynamo.html)

### Load balancing algorithms

- Round robin
- Least connections
- IP hashing


## Object-Oriented Design

- Design patterns: singleton, factory, listener, composite, etc.
- Inheritance, aggregation, delegation

## Databases

- SQL basics, joins, indexing
- Vertical partition/normalization vs. horizontal partition/sharding
- Database sharding: algorithmic sharding/consistent hashing, dynamic sharding/locator service/hierarchical primary key
- Consistent hashing
- Relational DB vs Non-relational DB/NoSQL, normalization vs de-normalization
- NoSQL, document based
- In-memory key-value pair storage: Redis, DynamoDB
- Graph DB
- BigTable, column based
- ACID

## Distributed Computing

- NodeJS/ExpressJS
- Service oriented architecture
- Map-reduce, external sort
- [x] Distributed caching
- [x] Load balancing
- Data sharding
- Eventual consistency

## Operating Systems

- Processes, threads
- Memory management
- Synchronization
- Paging
- Multi-threading

## Networking

- RESTful services
- HTTPS
- DNS lookups
- TCP/IP
- Socket connections

---

## Algorithms and Data Structures

- Greedy
- Trie
- Topological-sort
- Disjoint-set
- Heap
- NP, NPC

## Java

- Memory management
- Collections
- Generics
- OOP, anonymous class
- Concurrency
- Exceptions
- Annotations
- Lambda expression basics

## JavaScript

- [x] Lexical scope, execution context/`this`, closure
- React, Redux
- Angular
- ES6 basics
- Deferred objects and promises
- Throttling events
- Webpack, Grunt

## Web

- [x] Authentication, JSON web token/jwt.io
- [x] RESTful verbs
- [x] Cookies, sessions, stateful vs stateless
- Threaded server vs async server, NodeJS, ExpressJS
- How event loop works in browser
- CORS ajax
- What happens after a user enters a URL in browser
- HTML page rendering, CSS, JS scripts loading
- HTML caching mechanism
- Image sprites, icon image maps

[Long polling](https://en.wikipedia.org/wiki/Push_technology#Long_polling)

## React

- React essentials
- How virtual DOM works
- Redux
- VS Angular
- Webpack
- React native

## Notes

Read all of my github notes
