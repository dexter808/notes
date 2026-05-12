# Load Balancers

## What are Load Balancers ?

As the name suggests, these are piece of software or a hardware which sits in between requestors and responders and help distribute some work load between the responders.   
  
Generally used as an entrypoint for requests coming from the web.

---
## Types of LB Algorithms (Classification on type of configuration)

### Static Algorithms - Fixed Configuration

These are algorithms where we dont need to monitor the states of the servers, the distribution of request is based on some fixed configuration. These algos are simple and stateless and but provides less features as compared to the dynamic ones.
  
Eg: Round Robin, Weighted round robin

### Dynamic Algorithms - Server Monitoring and Dynamic Load Balancing

These are algorithms where LBs need to actively monitored for their health or some status, which is used to deetermine the distribution of requests. A little complexity gets added but load distribution is better.  
  
Eg: Least connection

---
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

---
## Types of LBs (Classification on OSI Layer)

Remember the Open System Intercommunication protocol of 7 layers - Physical, Data Link, Network, Transport, Session, Presentation, Application

### Layer 4 LBs - Transport Layer LBs

These ensure packets from a specific client always reaches the same server for a specific connection session. Content Agnostic. Some Layer 4 LBs do support TLS termination.

Faster because of low processing overhead.

It has access to 5 Tuple information.
- Src Host
- Src Port
- Dest Host
- Dest Port
- Transport layer protocol - TCP / UDP -> It needs to know this info to know whether to keep the connection open and how? For TCP -> It performs the usual TLS Handshake and verification but for UDP it just keeps a timer if no communication was made like for about 60 sec then it reassigns the port to some other connection and the client has to request a new connection.

#### Note
Because of knowing the TCP /UDP protocol it can perform TLS termination - but performing the SSL offloading involves reading the content and decrypting it then it is not purely a layer 4 LB in that case. Although it might not perform intelligent routing but it is not a purely Layer 4 LB anymore.

### Layer 7 LBs - Application Layer LBs

Operating at layer 7 - Application level of OSI model, These LBs have access to the request content like HEADERS, URLs, Cookies and therefore can make load distribution decisions based on these parameters. These are more flexible and can adopt to various needs with their intelligent routing decisions. TLS termination, rate limiting, and header rewriting is supported by these LBs.

Slower but intelligent beacuse of larger context.

### DNS LBs (uses ECMP router behind the scene)

Cloudflare and Google also provides the DNS LB, which internally uses ECMP routers and Anycast to distribute traffic to the closest region. How this works is, multiple servers accross the globe advertise the same IP to their local routers and the routers choose the topoligically closest server and hence the users are easily distributed to the closest servers.

In case of a DDos Attack the load is equally distributed, or when a failure in one region happens, the local server of that region stops advertising the IP address and the users are redirected to the next closest servers.

#### Note
CDNs also use cloudflare DNS LBs.

---
## Types of LBs (Classification Based on Session Synchronization between LBs)

### Stateful - LB synchronization and sharing state

The LBs maintain a record of sessions b/n servers and clients. These records are later used for routing decisions. This increases complexity and reduces scalability as Mapping has to be synchronized to other LBs.

Stateful here just means LB have a state and must be shared across.

In most of the cases we use statefull LBs

### Stateless - Independent LBs

These LB might or might not maintain record of sessions but they dont have to synchronize their states across LBs. They depend on local state for routing decisions.

---

## LB Deployment / Stack

3 Tier LBs

### Tier 1 - DNS LB (uses ECMP routers at layer 3 (Network) of OSI)

These face the internet and use Anycast for global load Balancing.

### Tier 2 - Layer 4 LB

These act as a smooth transistioning and consistency layer. They use consistent hashing if required to map network requests from a specific host to a spepcific Tier 3 / Layer 7 LB

### Tier 3 - Layer 7 LB (eg HAPROXY)

These do the actual Load Balancing and can do it intelligently if required.
This is where we terminate TLS. We can do Rate limiting at any level or at multiple levels.

---

## Notes:

### Does a response needs to follow the same path back to the client form the backend servers?

No, the backend servers can directly send the response to where TLS termination happened (most probably tier 3 or layer 7 LBs), if no TLS was involved the response can be sent directly to Tier 1 LBs.

### Sometimes the Tier 3 LBs are more in number than Tier 2, why?

Tier 3 does more complex computing as compared to Tier 2, as it needs to do Load Balancing based on the content and Tier 2 has a low overhead for the same number of requests, hence, Tier 3 needs more computation and processing power for the same number of requests. Additionally, the tier 3 may also tracks the health of the backend server.

## LB Implementation

### Hardware

Actual machines / hardwares which are expensive but offer high performance but comes with a fixed cost. Expensive and difficult to configure. Limited flexibility.

### Software

These are softwares like the HAPROXY, which runs on commodity hardwares, making them easy to install, configure and scale. More features at a lesser cost.

### Cloud

The Cloud services like AWS and GCP offers the LB as a service. LBaaS. Pay per usage. Really good at GSLB. ease of use, metered costs and auditing and monitoring.

### Client side LB

Client manages everything on their own. Twitter.

## Questions

### How does LB identify or mimtigate DDos Attacks?
- Track Source IP patterns.
- Apply rate limiting per IP if required.
- Redistribute load globally.
- Add Challenges like CAPTCHA to stop automated requests.
- Use ML behavorial analysis for blocking IPs, etc.
