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


## Data replication Models

### Single Leader replication (primary - secondary)

- 1 Primary Node (Leader) -> Only one to support all the writes. Also supports reads. Data replication can be configured to be sync or async

- Multiple Secondary Nodes (Followers) -> Only supports read operation and provide enhanced perforance benefits to systems with less write operations.

- New leader election happens via a leader election process or a manual operation.
- Issues: If async, and primary fails, the data can be lost or partially copied and might lead to durability and consistency failures.

### P-S Replication Models

#### Statement-based replication

#### Write-Ahead Log (WAL) shipping

#### Logical (row-based) replication