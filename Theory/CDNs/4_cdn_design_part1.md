# Content Caching Strategies in CDN

## Push CDN
- Origin Server -> CDN system
- API used -> The Deliver API
- Content provider is the active manager in this model and is responsible for content update in CDNs.
- Best for Static content because these do not expire frequently and do not need to be pulled from Origin again and again.
- Drawback - Inefficient for rapidly changing content because they will be large in number and the origin server has to alone manage the update, instead of CDN who distribute the load among themselves in a  pull model.

## Pull CDN (TTL for expiry)
- CDN system request -> Origin server
- API used -> The retrieve API
- Responsibility of latest data on the CDN system using TTL
- Best for frequently changing / dynamic data as the high load is being handled by a distributed CDN system as opposed to the origin servers.
- Might be slower but is cheaper. Slowness comes from the fact that a network call might be required for a expired or uncached content.

### Dynamic Content caching optimization / Features in a CDN

#### Edge scripting
- We can run scripts on edge servers to generate dynamic content based on the end users location, time, status ..etc

#### Compression
- Tools like Cloudflare's Railgun compresses dynamic content to reduce bandwidth usage between the orginn and proxy servers.

#### ESI - Edge Side Includes
- A markup language used in a CDN where the pages are divided into static and dynamic bits and only dynamic content is fetched from origin server and hence avoids the re-fetching of entire web pages.

##### Note: DASH (Dynamic Adaptive Streaming over HTTP) uses a manifest file which defines which URI to fetch based on network condition. Netflix uses a private / proprietry version of DASH.

## Multi-tier CDN architecture

- 
