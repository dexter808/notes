# Load Balancers

## What are Load Balancers ?

As the name suggests, these are piece of software or a hardware which sits in between requestors and responders and help distribute some work load between the responders.   
  
Generally used as an entrypoint for requests coming from the web.


## Types of LB Algorithms (Classification on type of configuration)

### Static Algorithms - Fixed Configuration

These are algorithms where we dont need to monitor the states of the servers, the distribution of request is based on some fixed configuration. These algos are simple and stateless and but provides less features as compared to the dynamic ones.
  
Eg: Round Robin, Weighted round robin

### Dynamic Algorithms - Server Monitoring and Dynamic Load Balancing

These are algorithms where LBs need to actively monitored for their health or some status, which is used to deetermine the distribution of requests. A little complexity gets added but load distribution is better.  
  
Eg: Least connection

## LB Algorithms

### 1. Round Robin Scheduling 

Requests forwarded ina circular fashion - one by one and then repeating once it reaches the bottom.  

### 2.  Weighted Round Robin

Servers are assigned weights according to their capacity, and then the load is distributed in that ratio.

### 3. Least Connections

Request are sent to the servers with the least active conection, so if any server is overwhelmed due to a heavy work load request, then meanwhile other servers which have completed their requests get the request. This is helpfull when requests are not of same work load. 

### 4. Least Response Time

Servers with the least or the best response time gets the requests, this helps in maintaining low latency for the end user. As a server gets overwhelmed, the number of requests going to it reduces. Performance sensitive servers.

### 5. IP Hash

Hashes the IP address of clients such that all request of that client is mapped and served by the same server. Usefull for sticky session. Hash Ring or Consistent Hashing for mapping requests to the servers.  
  
### 6. URL Hash
  
This is hashing based on URL i.e. requests are mapped to specific servers based on a specific path. Basically routing the requests to set of servers from path of the request.


## Types of LBs (Classification on OSI Layer)

Remember the Open System Intercommunication protocol of 7 layers - Physical, Data Link, Network, Transport, Session, Presentation, Application

### Layer 4 LBs - Transport Layer LBs

These are aware of

### Layer 7 LBs - Application Layer LBs

## Types of LBs (Classification Based on Session Synchronization between LBs)

### Stateful - LB synchronization and sharing state

The LBs maintain a record of sessions b/n servers and clients. These records are later used for routing decisions. This increases complexity and reduces scalability as Mapping has to be synchronized to other LBs.

Stateful here just means LB have a state and must be shared across.

### Stateless - Independent LBs

These LB might maintain record of sessions or not but they dont have to synchronize their states across LBs. They depend on local state for routing decisions.