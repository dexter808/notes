# Version and Configurability

## Data versioning
- Data can have multiple versions accross nodes.
- Stale data can corrupt business data and can cause huge inconsistency issues.
- Data versioning is used to identify the conflicts and resolve them before them cascading.

### Synchronus
- Read and Write requests wait for the data versioning and the request to be acknowledged by all the relavant nodes.

### Asynchronus
- Background replication just copies the data with the same vector clock to all the nodes.

## Vector Clocks for Conflict Identification
- A list of tuples, where each tuple has 2 elements -> node_id, version
- This exists for each kv pair.
- Everyone maintains there own vector clock -> Client, Coordinator, replicas
- Anyone can detect conflicts. 
- If version of a node is less than the one i have, but there is some other node whose version is more than what I have then there is a conflict.
- If all the versions are greater than what I have then I have the stale data and just replcae with what I recieved.
- If all the versions are smaller then your context is outdated, so reject the update / write.

## Get Request
- Request just contains the key. Read can happen without a context
- Response contains the value and context. 
- Response might ask you to resolve the conslict if configured that way.
- Conflict resolution for amazon shopping cart is merging etc..

## Put / Post / Update Request
- Request must contain the key and a context that the application has.
- The context contains metadata and the vector clock which is used for conflict detection.
- If you dont know the context then you read the latest context and make decisions on it and thus modify the data based on that context.
- If the node sees that all the versions in the vector clock is smaller or larger then it knows whther it is an outdated data or the latest data and thus can handdle accordingly.

## Quoram for consistency
- W + R > N makes sure atleast one overlap and the machine is able to identify if any data is stale.