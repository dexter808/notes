# Apache Kafka

## What is Apache Kafka? Who developed it?
Developed at LinkedIn, Apache Kafka is a distributed event streaming platform for building real-time data pipelines and event-driven systems.

It complements databases rather than replacing them.

Publishers and Subscribers to topics of different events can now work independently of each other, without worrying about 429s and other synchronous roadblocks. Publishers and Subscribers are now decoupled and can scale independently of each others. It provides asynchronous decoupling.

## Different Components of Kafka

### Events

An event logically is an occurrence of something at a specific time. In Kafka an event contains the following items:
Key (optional): identifier (e.g., user ID)
Value: actual data (e.g., “order placed”)
Timestamp: when it happened
Metadata: partition, offset, headers, etc.  
  
### Offset
Offset is not part of the event payload, but metadata assigned by Kafka. It is often tracked by Kafka per consumer per partition so that even if the consumer fails, they can restart from where they left.


### Topics

A topic is a logical category where events or messages are stored by the broker and read by the consumers. You can see them as channels where specific type or category of message / event is broadcasted.

### Partitions

Each topic is divided into partitions, to divide and store the data accross multiple partitions. Partitions of a topic allows that many number of parallel processors.

### Consumer Groups

These are group of consumers which together consume the whole topic.  
Each consumer in a group gets to read from a single partition.  
If number of consumers > partitions -> then extra consumers remain idle.

1 Topic can have n Partitions, each partition consumed by a consumer.   
1 Topic can have n Consumer Group.  
1 Consumer Group can subscribe to n Topics.

A topic is split into partitions
Each partition is consumed by one consumer per group
A consumer can consume multiple partitions
A consumer group collectively reads the entire topic
Max parallelism = number of partitions

### Producers

In Apache Kafka, a producer is the component that sends (writes) events to Kafka topics.  
  
What a producer does  
- Chooses which topic to send data to. Optionally sets a key (e.g., userId) and sends the event to Kafka (broker).  
- Important behavior :  
  
  - Partition selection  
  
    - If key is present → Kafka hashes the key → ensures same key goes to  same partition  
    - If no key → Kafka distributes events (round-robin or sticky partitioning)

  - Batching   
  
    - Producers batch multiple events → improves throughput.  
- Acknowledgements (acks)  
    - acks=0 → fire-and-forget (fast, unsafe)  
    - acks=1 → leader confirms write  
    - acks=all → leader + replicas confirm (most durable)

### Brokers

A broker is a Kafka server that:

- Stores data (events)  
- Serves read/write requests  
- Manages partitions  

A Kafka cluster = multiple brokers

What brokers handle?  
- Topic partitions storage
- Replication
    - Each partition has:
        - Leader (handles reads/writes)
        - Followers (replicas for fault tolerance)
- Client communication
    - Producers and consumers talk to brokers

#### Example
Topic: orders  
Partition 0 → Broker 1 (leader), Broker 2 (replica)  
Partition 1 → Broker 2 (leader), Broker 3 (replica)

#### How data is written into partitions

This is the most important flow.

Step-by-step
- Producer sends event → specifies:  
        - Topic  
        - (optional) Key  
- Kafka determines partition  
        - With key → hash(key) % partitions  
        - Without key → round-robin / sticky  
- Producer sends request to leader broker of that partition
- Broker:  
        - Appends event to partition log (disk)
        - Assigns offset
    - Replication (if enabled):
        - Followers copy the data
- Acknowledgement sent back to producer

#### Key guarantees
- Ordering  
        - Guaranteed within a partition only
- Durability  
        - Depends on replication + acks
- Scalability  
        - Achieved via partitions across brokers

## Common Doubts

### 1. What is sticky partitioning?

In Apache Kafka, sticky partitioning is used when no key is provided.

#### What it does?  
- Instead of round-robin per message, the producer:  
    - Picks one partition
    - Sends a batch of messages to it.
    - Then switches to another partition  
  
#### Why it exists?  
- Improves batching efficiency
- Reduces network overhead
- Increases throughput  
  
#### Summary
With key → hash-based partitioning.  
Without key → sticky partitioning (batch-friendly)  

### 2. How data is stored in partitions?

Kafka does NOT use a traditional database internally.

Storage model  
- Each partition = append-only log on disk  
- Stored as files (log segments) on the broker’s filesystem.

#### Structure

On disk:
```
topic-0/
  000000.log
  000001.log
  ...
```  


Each file contains:

- Sequential events
- Indexed for fast reads
  
#### Key properties
Sequential disk writes → very fast  
Immutable logs → no updates, only append  
Retention-based deletion (time/size)  

#### Important
Kafka is self-managed storage.  
It does not sit on top of MySQL/Oracle/etc.  

### 3. Leaders and followers (brokers). 

#### Can they be both?
  
Yes — brokers can be both.

A single broker can be:

Leader for some partitions.  
Follower for others.

Example:

Broker 1 → leader (Partition 0), follower (Partition 1)  
Broker 2 → leader (Partition 1), follower (Partition 0)  

#### Is this like databases?

Yes, similar concept, but:

Kafka applies leader–follower replication at partition level, not at entire database level.  


#### Does each broker maintain its own DB?

No.

Brokers do not run separate databases.  
They store:  
- partition log files directly on disk
- Replication is handled by Kafka itself  
### 4. Who decides and where does producer send data?

This is a very important subtle point.  

Partition decision  
Producer client decides the partition  

Uses:  
key hashing OR
sticky partitioning

Kafka cluster does not randomly redirect messages.

Then how does it reach the correct broker?

Flow:

Producer fetches metadata from Kafka:
-  Which partitions exist
-  Which broker is leader for each partition  

Producer:
-  Computes partition
-  Knows exact broker (leader)

Producer sends data directly to that broker  

#### So no redirection?

Correct:

No central routing layer  
No “Kafka deciding after receiving”

Instead:

Smart client (producer) + dumb broker (storage)  

Mental model:  
Producer = intelligent router  
Broker = storage node handling assigned partitions  

### Final summary

Sticky partitioning → improves batching when no key.  
Data stored as log files on disk, not DB.   

no key -> Sticky Partitioning (still decided by producer)
with key -> Hash based partitioning (still decided by producer)
  
Brokers: 
- Can be leader and follower simultaneously.  
- Do not run internal DBs.  
- Never decides partition
- Only responsible for Read / write of data, serving requests, and replication.

Producer:  
- Decides partition
- Sends data directly to correct broker  
- These are not part of Kafka server side components. These are just any service or program that wants to write to kafka message queues. Kafka provides libraries to use for the same.
   
Consumers:  
- These are not part of Kafka server side components. These are just any service or program that wants to read from the kafka message queues. Kafka provides libraries to use for the same.

Core components of Kafka:
- Brokers (servers storing data)
- Topics & partitions
- Replication mechanism
- Internal coordination (metadata, offsets, etc.)

## Interview Questions

### 1. How do you choose the number of partitions?

#### Key rules  
- Consider Ordering - is it important to you. Will producer handle the complexity of choosing the partion if yes then you can use multiple partition which will increase complexity for key based messages, but ordering for some keys will be guarenteed.
- Max parallelism = number of partitions  
- Start with:  
    - expected consumer count × safety factor (2–3x)  
- Consider:  
    - Throughput requirements
    - Key-based ordering needs
    - Future scaling (hard to reduce partitions later)

#### Trade-offs
- Too few → bottleneck  - Consumer might wait for data.
- Too many → overhead (open files, memory, rebalancing cost) - Consumers would have to manage multiple partition and thus increase network overhead.