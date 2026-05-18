# Trade Offs in DB Architecture

## Centralized DB
- +ivs
    - Maintainance (Updates and Backups) are simple
    - ACID gurantees are easy to ensure compared to distributed architecture.
    - Simpler programming models for the developers.
    - Efficient for smaller data sets
- -vs
    - High Latency when data heavy sets are used.
    - SPOF
    - Scaling becomes expensive as hardware limit is realized.

## Distributed DB
- +vs
    - Fault Tolerance
    - Faster data access from the geo nearest db node.
    - Complex queries can be split into smaller queries and can be processed parallely.
    - More flexibility
- -vs
    - Maintainance is difficult
    - Complexity for developers
    - Cross site data retrieval is slow
    - Sync and backup is much slower.
    - Overkill and thus expensive for smaller data sets.

## Query Optimization
- Larger data sets to stay closer to end users.
- Smaller data sets can stay farther at other sites to minimize the transfer delay time.

## Conclusion
- Reliability (Fault Tolerance)
- Performance (latency)
- Consistency
- Cost 