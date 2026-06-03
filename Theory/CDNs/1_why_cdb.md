# Why do we need CDNs?

## High Latency
- Single Data center / storage serving to all the users based accross different geographical locations can slow down the experience for end users.
- Buying / Setting up the whole infra or the application setup accross multiple data centers can be expensive and a nightmare for maintainance and management.

## Data Intensve Workloads
- Transferring large amounts of data from a data center to each user will require significant bandidth and will become expensive soon enough.

## Resouce Scarcity and SPOF
- A single data center has finite resources and has a single point of failure if resource scarcity happens or maybe due other reasons.

### Hence,  Better to have someone cache static data near the end users and only importan or uncached data gets streamed from the actual application from data centers