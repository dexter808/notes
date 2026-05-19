# KV Store

- A distributed Hash Table [DHTs]
- The keys are hashed and stored in the map such that it maps to a value. The value can be some reference and hence the map structure remains value agnostic.
- The value can be anything from Blobs, images, payload, single value etc.. If the value is too large keep it somewhere like a blob store and store the link to it.

