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
  
Offset is not part of the event payload, but metadata assigned by Kafka


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
