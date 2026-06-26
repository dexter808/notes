# Important Considerations for Distributed Cache

## Writing policies
- This policy defines when do we write the data to cache or the Database. Each policy has its tradeoffs and design advantages.

### Write through
- Write in both the cache and the DB in the same atomic operation.
- +ives:
    - Strong consistency between cache and the DB
- -ives:
    - Increased write latency.

### Write-Back
- Data is written to the cache first and then async to DB
- \+ives
    - Low latency for write operations
- \-ives
    - Consistency issues
    - Risk of data loss if cache fails before the data is communicated to DB

### Write-Around
- Data is written directly to the DB, bypassing the cache. Data i loaded into the cache only if requested.
- +ives:
    - Lower latency than Write through
- -ives:
    - Consistency issues if stale data present in cache

## Eviction Policies
- Which item to remove when the cache is full
- Policies:
    - LRU (Least Recently Used)
    - MRU (Most Recently Used)
    - LFU (Least Frequently Used)
    - MFU (Most Frequently Used)

## Cache Invalidation
- How do we prevent stale data
- TTL (Time to live) is widely used for this.
    - Active Expiry: Background process to remove stale / expired data.
- Lease-Based Expiration policy is when a contract is established between the cache and the DB with a time window until which DB will post the new data to the cache. After the expiry window of the contract the cache has to renew the contract.

## Storage Mechanism
- What data is stored where?
- What data structure is used to store data in each server.

## Hash Functions
- Hash % N - Not good if a server is added or removed, as this would cause a massice cache redistribution ..etc
- Consistent Hashing for key distribution and identification.
    - Virtual Nodes to increase randomness
    - Hinted Handoff for temp down DB, removed if data is not requested back or if storage issue occurs.
    - Gossip Protocols for identifying down DBs.
    - Anti-Entropy recovery mechanisms like Merkels tree for recovery from surrounding nodes. 

## Linked List
- We can use a Doubly Linked List to store the cache as it makes it easy for eviction.
- We can also use a Hash Function and a List to keep track of the keys of what lives where.

## Bloom Filters
- Use by large DBs to tell whether a key is definetly not present in DB. 
- For a large subset of keys it can determine negatives with 100% accuracy.
- But its positive results can be missleading
- Multiple Hash Functions and correspong Hash List.
- Set a bit to 1 in the hash List if the key is added or keep 0 by default.
- A key when checked for presence has to be calculated for all the Hash, and if even one of the hash index value is 0 we know with 100% accuracy that this key has not been added yet.

## Sharding in Cache Servers

### Dedicated Cache Servers
- Cache servers are seperate from the application servers.
- These can provide Cache as a Service to multiple consumers.
- Key namespacing is required to serve to diffferent consumers

### Co-Located Cache
- Located on the same server as the application.
- Reduced CAPEX or OPEX because of sharing the same infrastructure.
- Server failure also fails the cache.

## Cache Client
- Library in application servers to handle the logic for storing and retriving the data from the correct cache server.
- Purpose is too offload the computation from the shared cache resurce to the consumer application server.
- Basically, this supports the dumb provider and smart consumer model which helps in scalling to multiple consumers.