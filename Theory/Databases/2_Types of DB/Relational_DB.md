# Relational DB / SQL DB

- Fixed schema.
- Data organized into relations - Tabes, rows, columns.
- Each Row has to be identified by a unique key.
- SQL is used for CRUD operations.


## ACID Transactions

- Atomicity - A specific transaction remains atomic in nature, meaning it will either complete all statements fully or none by rolling back to Starting status.

- Consistency - The DB will remain consistent (data integrity will be maintained) before and after the transaction. If a transaction makes the DB in a inconsistent shape then the specific transaction is rolled back.

- Isolation - Concurrent transactions do not interfare with each other. The DB implements a lock on a row if it is shared between two tranactions - the lock blocks based on read or write event. Both reads then both are allowed simultaneously, but if one or both write then the transaction which comes late must wait.

- Durability - Once a transaction is committed, the changes are presisted in a durable storage, and can survive system failure.

## Popular SQL DBs

- MySQL
- Oracle DB
- PostGres
- SQLite

## When to use SQL DB

- ACID Gurantee
- Consistency / Acurracy is more important than Availability.
- Complex Operation (Joins, Lookup etc.)
- Reduced Data Redundancy - Storing data in normalized forms
- Example - Banking, Payment, Inventory etc