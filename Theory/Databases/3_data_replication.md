# Data Replication

- Availability -> Fault Tolerant (disk, nodes, network, or power outages ,etc.)
- Scalability -> Ability to handle increasing load (read, write, and traffic)
- Performance -> Low latency and high throughput

## Why Replicate?

- To maintain multiple copies of data across different nodes and regions to improve fault tolerance, availability and latency / performance.  

## Challenges of replication

- Consistency across nodes
- data recovery upon a node failure
- how to consistently identify where the data exist or in which node
- What happens in case of concurrent writes?
- Systems with high writes find it difficult to provide the data consistency if read happening via multiple nodes. Allowing reads via multiple nodes allow better performance but may lead to stale data (consistency issues).

## Types of replication

### Synchronus Replication

- Primary nodes wait for the acknowledgement of data replication from all the secondary nodes for that data before marking the operation as success.
- Low performance 
- Data remains consistent and durable even incase of node failures.

### Asynchronus Replication

- Primary node markes the operation success as soon as it is able to update itself and the relpication is handled asynchronusly in the bakcground.
- Provides better performance
- Data might not be consistent or even durable if primary fails and the data was not replicated.


# Data replication Models

## Single Leader replication (primary - secondary)

- 1 Primary Node (Leader) -> Only one to support all the writes. Also supports reads. Data replication can be configured to be sync or async

- Multiple Secondary Nodes (Followers) -> Only supports read operation and provide enhanced perforance benefits to systems with less write operations.

- New leader election happens via a leader election process or a manual operation.
- Issues: If async, and primary fails, the data can be lost or partially copied and might lead to durability and consistency failures.

### P-S Replication Models

#### Statement-based replication

- SQL statements are recorded and shipped to other replicas to be replicated.
- Used by Older Versions of MySQL
- Non deterministric functions like NOW() values can change from machine to machine

#### Write-Ahead Log (WAL) shipping

- Changes are recorded at low level of Bytes and then shipped to other replicas for replication.
- Used by PostgreSQL and Oracle
- Logs are hard coupled with the DB Engine and version upgrade can cause issues.

#### Logical (row-based) replication

- Row based changes like updating columns etc. are recorded and shipped.
- Used by PostgreSQL and MySQL
- Deterministic compared to SQL Statement based, less coupled with DB engine and thus balanced for accuracy and flexibility.

### Common Problems during async replication

- Data loss: If master node dies before shipping its logs to other replicas. There is a potential Data loss.
- Inconsistent Read - If the data is modified and read at the same time from some other DB, it may seem update failed.
    - Solution - Read-Your-Own-Writes (RYOW) - If a user access data they can modify, then the routing should happen towards the master node. Like Profile Data and passwords etc. Other Users may see the new Value after some time when replication completes. Although this solves this problem but it introduces a large overhead for master nodes. Not Always worth the latency.

## Multi - Leader Replication

- If multiple nodes in a cluster are write allowed and they replicate themselves to other leaders and its own followers, it becomes a multi-leader replication.
- Imporoves write performance and Fault tolerance.
- Overhead increases, Complexity increases, Conflict resolution becomes a difficult thing to manage.

### Common Data Migration / Distribution Patterns:

- Bidirectional - Reporting instance.
- Unidirectional - Instant Fail Over.
- Peer-to-peer - Load Balancing (HA)
- Broadcast - wide level data distribution to multiple instances
- Consolidation- Data warehouse (data storage)

### Conflict

- Multiple users try to modify data concurrently and both gets into some dbs.

### Conflict Avoidance

- Timestamp. Latest is correct. - Sometimes the data and time are not synchronized and might cause unforseen issues.
- Prompt the User to resolve 
- resolve either at read or during write
- Merge the data somehow...etc

### Topology

#### All to All
- Updates take a lot of time to reach all the nodes.
- Might end up with a lot of conflicts.
- Replication overhead can be huge.
- One node failure can inturrupt the flow.

## Peer-to-peer / Leaderless Replication

- No primary database. All are allowed to read / write.  
- Dynamo style dbs. Amazon Dynamo in 2007 introduced a DB sysyem with AP (Availability and Patition Tolerance)

### Balancing A and C with Quorums

- A number used to decide when a read / write operation is success.
    - R nodes should return the same value before confirming a read.
    - W nodes should acknowledge becfore we mark Write success.
- R + W > K, with R <= K and W <= K; K is the number of nodes the data is copied to. Notice we kept > than and not = to, this gurantees consistency.
- Conflict does not mean inconsistency.
- Quorum is sync (W = N, waiting for ack) as well as async(W = 1, does not wait for ack) as well as none / both (1< W < N). Basically depends on the configuration.
