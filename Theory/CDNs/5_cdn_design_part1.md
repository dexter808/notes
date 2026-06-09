# Content Consistency in CDN

## Periodic Puling
- Pulling all the data periodically.
- TTR (Time To Refresh) can be used to control ho frequent this polling occurs.

## TTL Fetch
- Each Data Point has a TTL after which it expires.
- Upon expiry of a data point, the CDN has to revalidate the content and get a new TTL.
- The Origin can control the TTL and thus control how frequent data fetch occurs and also smoothen out the traffic on itself.

## Lease Model
- The Origin server creates a contract with the CDN that it will notify the CDN of any data changes.
- Once lease is expired the CDN has to request to create a new Lease / renewal.
- A dynamic balance between a pull and a push method.
- Better for static data where updates are infrequent.
- Adaptive leasing offers the option to dynamically change the Lease duration if proxy load increases or decreases.

# Deployment

## Placement of CDN Proxy Servers

### On Premise (Small Data Centers near IXPs)
- IXP (Internet Exchange Point) is a place where several ISP's meet and exchange data.
- Placing a data center here offers closeness to the end consumers of several ISPs.
- A balance between having to place CDNs inside the ISP itself vs Placing at the edge where multiple ISPs meet offers number advantage and also cost advantage.
- Google would place its servers here.

### Off Premise / Deep Caching (Placing proxies directly inside ISPs networks)
- Placing a Proxy server directly inside the ISP.
- Costly as more infra required.
- Performance boost is very high for end users.
- Good for content that do not change often.
- This is the closest we can get to the end users.
- Netflix and Akamai

#### ISP Benefits from proxy cdn
- ISPs customers percieved latency is much lower compared to ISPs who dont have this.
- CDN responds to queries inside the network itself and thus does not have to travel outside the network and hence easier for ISP to provide SLAs.

#### Solution
- Dynamic content, lets place it at IXP if we have money for that and not for placing inside ISP.
- Comparatively Static content like a movie, lets place inside the proxy CDNs. Quick Load time for End users

## CDN as a service
- Cloudflare, AWS, GCP ..etc offers CDN as a Service.
- Pay as you go model.
- Content lives in a sharing model with other customers.
- Outages - Down time out of our hands.
- Easy Start as we get a plug and play model.
- Blocking - If a shared CDN gets banned due to serving banned content, our service also shares the hit.

## Specialized CDNs
- Netflix OCA (Open Connect Appliance), Google also uses its own specialized CDNs.
- Serves petabytes of data and provides the direct access to the business.
- Often coexists with publics shared CDN to add redundancy and remain operational and fault tolerant.
- Netflix OCA directly reports the consumer behaviour to AWS Open Connect which monitors their health.


# Anology real world
1. The Monitoring System (The Analyst)

    Role: Sits in a room full of screens, watching real-time charts of traffic, server temperatures, and fiber-optic cable health.

    Task: Identifies that "The line at the London office is getting too long" or "The road to the Tokyo office is blocked by a storm." It sends these reports to Management.

2. The Routing System (The Executive Management)

    Role: The "C-Suite" (CEO/COO). They don't talk to customers directly. They take the Analyst's reports and make strategic policies.

    Task: They decide: "Okay, if London is full, tell the Dispatchers to send UK customers to our Paris or Amsterdam offices instead."

3. The CDN Auth DNS (The Dispatcher / Receptionist)

    Role: This is the first person the customer (the User) talks to. They don't do the work themselves, but they tell the customer exactly where to go.

    Task: When a customer asks "Where can I get this file?", the DNS looks at the Management's current policy and says, "Go to the Edge Server at this specific IP address."

4. The Proxy Edge Servers (The Employees)

    Role: The front-line workers in the local branch offices.

    Task: They are the ones who actually hand the product (the content) to the customer. They are the only ones the customer actually interacts with once they have the address.