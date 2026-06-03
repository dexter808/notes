# CDN Design

## Components
- Clients: The end user application on browsers, desktop and smartphones that request contents from CDN.
- Routing systems: Directs to the closeset optimal CDN facility. Uses input regarding content placement, reqest volume and proximity to the closeset server.
- Scrubber servers: Seperates legitimate traffic from malicious traffice. These are typically engaged when an attack is identified to clean traffi before routing it to the destination.
- Proxy servers: Edge servers that serve to users. 
- Distribution system: Intelligent broadcast mechanism to distribute the content to edge proxy servers.
- Management System: Monitors business and operational metrics, such as latency, downtime, packet loss and server load.
- Origin servers: The source of truth for the mapping data, they provide the actual content. If a content is expired or not present in the system then it is fetched from these servers.

---
## CDN Design High Level
![CDN Design High Level](../../Diagrams/CDN%20Design%20Practice.drawio.svg)

---

## API Design

### Retrieve Content
- Used by the edge proxy servers to get content from the origin servers
- proxy_server_id, content_info

### Deliver Content
- Used by Origin Servers to deliver the content to the distribution system to be distributed to the proxy edge servers.
- content_info, server_list, origin_id

### Request Content
- Used by the Clients to request from the Proxy Edge servers
- user_id, content_info

### Search Content (P2P APIs)
- Used by Proxy servers among themselves (peer proxy servers) to search for some requested content.
- proxy_server_id, content_info

### Update Content (P2P APIs)
- Used by the Proxy Servers to update content in peer proxy servers
- proxy_server_id, content_info

### Delete Content (Most oftne not used as we depend on cache eviction policies)
- Used by origin servers / proxy servers to peer proxy ..etc