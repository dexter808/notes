# Scaling
- Data grows we can not keep all data in a single node
- 2 options to grow:
    - ## Increase resource for your DB Instance (Vertical Scaling)
        - +tivs
            - Easy to add more resources (Mostly just a spec file change)
            - A lot of benefits keeping all the data on the same nodes. Like ACID Transaction. 
        - -tivs
            - Reaches a limit at a point beyond which growing is impossible / inefficient.
            - SPOF - Single Point of failure
    - ## Increase the number of DB instances (Horizontal Scaling)
        - +tivs
            - Fault Tolerant
            - Easy to scale out as demand grows
            - Maintainance does not require down time.
            - More managable
        - -tivs
            - Can be overkill for small data set
            - Difficult to maintain
            - does not offer transaction gurantees, etc.
- ## Which scaling option is correct?
    - Combination of both is what most of the applications need. 
    - Scaling up (Vertical scaling) becomes expensive after a certain amount of time and also does not offer Fault Tolerance, distributed access etc.
    - Scaling Out (Horizontal Scaling) is overkill for a small data set as it makes your data system difficult to operate / work with.
# Sharding
- Horizontal Scaling requires dividing data into multiple DB nodes.
- 2 ways you can divide data into multiple nodes - Horizontal and Vertical Sharding
## Vertical Sharding 
- Keep Data structures in different nodes. 
- Example: Keep Profile Table in N1, Post Data in N2 ..etc
- Requires Planing, is static and manual as it requires making decisions as to what to keep where.
## Horizontal Sharding
- Keep the same DB schema in all Nodes. 
- Data (Rows / Payload) is divided among different Nodes
- Example - Profile and Post table both exist in all Nodes, but id in range \(0,1000\] in N1, and \(1000,2000\] in N2, and so on ...
### Key Range Horizontal Sharding
- Data is divided into nodes based on continuous range of Keys
- +ivs
    - Related data lives in the same DB, hence makes data retrival easy and low cost.
    - Data can be kept in increasing order in a partition
- -ivs
    - Hotspots issue are common if distribution is not even and querries are not optimized for the same.

### Hash Based Horizontal Shardding
- We hash the keys and mod n and then store in that node of DB.
- Distribution using Hashing Solves the hotspot problem because the distribution is more even now.
- Now suppose We want to get the user with key = 13 , key = apple
    - We can get that using the above formula and then use the same DB as we know that is where the resource exists.
- Now suppose we want to get all users with REGION=Mumbai,
    - We cant use the above formula directly so now we have to Scatter-Gather (Very Expensive)
    - #### Global Secondary Indexes (GSI)
        - This can be solved with GSI which is essentially a operation of index(Column) and we store this secondary index map in a seperate cluster and everyone can now know where REGION=MUMBAI is stored.
        - It is not a good idea to prepare GSI for all columns so we should do it only for some columns and optimize our DB queries.
        - Updates:
            - When an existing data point is updated we need to update the GSI and if the update fails our queries might give inconsistent answers. Using a background job to update GSI is the better and reliable way but the data might be stale for some time. We can also use distributed transaction for doing both where we commit only if both the main row and GSI update is successfull, but again this is also expensive (Slower, complex and low availability).
            - Suppose 95% of the queries have REGION selection and not user_id selection (about 5%), then we should hash based on REGION and not user_id.
            - Only Scatter and gather works reliably if any other column is added in the query.
            - GSI again creates a hotspot problem.
            - GSI is a workaround and not a base design.
            - Design should be around which column is quried the most (Query Optimization).

### Consistent Hashing
- Premise:
    - For some large number -> N
    - Select some K values between [0,N]. Each of these become a virtual node.
    - Assign virtual nodes to actual DB nodes based on their capability.
    - Because of random selection the virtual nodes should be about evenly distributed, if not you can select virtual nodes as well.
- New Data arrives:
    - Hash the key and then Mod N
    - The value on the ring [0,N] where the key hash falls should be added to next k nodes. These will form replicas for that data.
    - Removal and addition of nodes requires minimal data migration as no k continuous vNode is going to fail, highly unlikely, or make it possible by chosing v Nodes evenly.
    - Data can be copied or retrieved as required.
- -ivs
    - Range Queries are inefficient because continuous data are usually not on the same DB.

## Request Routing
- How does the client know which node to reach out to?
- #### Client aware - Client already knows who has what data and thus client can easily determine whom to reach.
- #### Routing Tier / layer - A cluster of servers to tell clients where to reach out for the specefic data.
- #### Any node - Nodes know where the data exist and will redirect request to that node.
- Challenge in request routing is keeping the data synchronized / consistent.

### Zookeper
- A coordination software used for monitoring / tracking the cluster state and mapping partition to nodes. Zookeper Updates the specific clients when a node is added or removed. 
- HBase, Kafka etc.