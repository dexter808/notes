# Sequencer

## Requirements
- Uniqueness - Distinct for te entire application for that data type.
- Scalability - 1 billion per day
- Availability - Simultaneous generation in milliseconds should also be allowed
- 64 bit numeric ID

## Approaches for IDs

### UUID
- Universally Unique ID.
- 128 bit hexadecimal number.
- Scalable
- Available as systems generte indepedently without any coordination
- Collision probability very low
- 128 but indexing slows down the db or other operations
- size is exceeding the 64 bit limit
- Collision is possible, though highly improbable.
- Ordering is never guranteed with time.

### Centralized DB for ID generation
- Generated ID will be unique
- SPOF

### Decentralized DB for ID generation
- M nodes will always increment by M and hence never overlap each other.
- SPOF avoided
- Scalable because of distributed approach.
- Multi DC scaling will be difficult
- Not time based if one db goes down and might also cause duplicates if that happens

### Range Handler
- Servers which assign range of IDs to each server.
- Scalable by assigning mutually exclusive range to each range handlers
- Available because range handlers can be increased.
- Gurantees unique IDs. No duplicates.
- 64 bit satisifed
- If a server goes down, then that range is missed and might create gaps in the system. Can be mitigated by assigning smaller ranges, but it might increase the network overhead.

### Twitter Snowflake
- 64 bit length
- 1 bit reserved for '+' IDs. We dont use unsigned values beacuse some languages do not support them. Like Java, Python etc.
- Epoch Timestamp - Seconds passed since certain date, we can keep our own certain date. Utilizes the next 41 bits. We also include the milliseconds by adding more bits and hence 41 bits.
- 1 of the bellow:
    - 5 bits for each
        - 5 data center ID - Offers 32 data centers
        - 5 for machine IDs - Offers 32 machines
    or
    - 10 bits for total
        - 10 for machines - Offers 1024 machines in total.
- 12 for concurrency - Same millisecond request can be accomodated.

### Using Logical Clocks

#### Lampart Clocks
- Each process maintains a counter initialized to 0.
- Before every local event the counter is incremented.
- When a node send a message to another node to do something, it sends its clock + 1 (because sending a message is also counted as an event) timestamp to the other machine.
- The oher node upon recieving the timestamp uses a timestamp using max(clock, recievedClock) + 1
- This ensures the causality is maintained across nodes. 
- Lamport timestamp alone is not enough to maintain globally unique timestamp. We can attach node id to make the timestamps unique.
- Limitations -> Ordering is not maintained between 2 different order of events. 


#### Vector Clocks
