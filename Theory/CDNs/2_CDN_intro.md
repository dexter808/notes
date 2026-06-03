# CDN (Content Delivery Network) / Allso know as edge servers
- A distributed system with servers around the world to serve static / dynamic content to users around the world to help decrease latency.

## Functional Requirements
- Retrieve: Should be able to pull from origin servers depeding on the type of CDNs
- Request: Proxy servers must allow requests and respond for content to these CDNs.
- Update: The system must propogate updates to peer proxy .. etc
- Delete: The system must support removing cached entries via TTL or purge requests.

## Non Function Requirements
- Performance: Minimize latency to ensure fast response times.
- Availability: Should be highly available and operational even during DDoS attacks.
- Scalability: Support horizontal scaling for the increasing or decreasing workloads.
- Reliability and Security: Eliminate SPOF and protected hosted content from attacks