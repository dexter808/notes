# Non Relational DB / NoSQL DB

NoSQL DB is used to store unstructured data, or to store data in diverse data models. 

- Unstructured Data Model / Structure
- Large Amount of data handling
- Higher Throughput
- Horizontal Scaling
- Flexible Schema
- Lower Cost
- Eventual Consistency

## Types of No Sql DB

---
### Key Value Databases:
---

- Data Model: Simple unique Key maps to an opaque Value (scalar data, lists, or compound JSON documents).
- Availability: Extremely High Availability and seamless horizontal scaling due to a masterless or highly sharded architecture.
- Storage & Cost:In-Memory (Cache-focused): Stores data in RAM (e.g., Memcached, Redis). Higher cost per GB and limited by RAM constraints.
- Persistent (DB-focused): Stores data on SSDs (e.g., DynamoDB). Highly cost-effective with virtually limitless storage scaling.Durability Options:
    - Memcached: Volatile only; data is wiped upon server restart.
    - Redis: Offers point-in-time snapshots (RDB) and transaction logs (AOF). AOF can be tuned to minimize or completely eliminate data loss risks at the expense of speed.
    - Amazon DynamoDB: Fully persistent by default. Automatically records data across solid-state drives with built-in replication.

#### Usage

- Session Oriented application to stores session data.
- Key value data might be lost if server restarts.
- Faster reads

---
### Document DB
---

- Data stored in Tree or Hierarchical structure like JSON, XML etc.
- MongoDB and Google Cloud Firestore

#### Usage
- Unstructured Catalog data, like e-commerce - where products might have different attributes and not always the same attributes.
- Stores all data in a single place so reading and writing is easy, as compared to sql where you would need to store it in multiple tables.

---
### Graph Database
---

- Data Model is a graph with data being coordinates on the graph.
- Nodes are entities and edges represent the relationship. 
- Efficient traversal and relationship queries
- As compared to SQL where traversal would require visiting multiple tables.
- Noe4j etc.

#### Usage
- Social Media Apss (Friends of Friends relationship)
- Recommendation engines
- Fraud Detections
- Analytics
- Anywhere where we need to query relationships

---
### Columnar Database
---

- Data stored in Large columns to store a range of data instead of individual rows.
- Continuous data storage.
- Amazon RedShift, Google BigQuery

#### Usage
- Analytics
- Range Operations
- Recording a specific attribute

---
## No SQL DB Drawbacks

- Lack of standardization and compatibility across platforms - Unlike SQL no sql db end up with vendor lockin because APIs and syntax differs between different DBs
- Consistency - ACID transactions are expensive gurantees which unstrutured DB like NoSQL find hard to provide. BASE - Eventual Consistency can be guranteed by these DBs.