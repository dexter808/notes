# Failure detection and Recovery

## What happens when a node is temporarily Down?
- The Quoram cant be completed sometimes and can lead to availability issue.
- The system has to wait for the node to come back online or would have to give up on consistency for availability.
- We use Sloppy Quoram for such cases and allow the next node  / V. Node to handle the current write request. The node saves the data and gives it back to the node when it comes back (Hinted Hand off). If the node does not come back uptil a specific threshold the DB purges the data to avoid resource hogging and the system in such case relies on anti-entropy synchronization using merkle tree for recovery.

## Sloppy Quoram
- It just means easing off the rules by using an extra node when a node is temporarily down.

## Hinted hand-off
- The mechanism where a node saves the data for the temporarily down node.
- If the node does not come back soon, this node might delete the data to save resources.

### Permanant Down / Node recovery
- A node goes down and does not come back soon enough and becomes too stale.
- How do we recover ? Using an anti - entropy synchronization like merkle trees we restore data points from multiple nodes.

## Recovery using Merkle Tree
- Each node calculates and keeps the hash of data range from hash ring that it is responsible for.
- Since no 2 db handle the exact 100% data. We might need to ask for data range from different DBs
- So we can ask for some range from some DB and for others from others.
- We can calculate the hash on our node and compare and keep breaking down into 2 until we identify exactly which hash is not matching or inconsistent.
- We can then batch the data and transferto the requesting DB only the data that is absent or inconsistent.
- Low network Overhead and faster recovery as compared to getting a whole new DB.
- Can also be used for data transfer when new DBs are added or removed.

## Failure detection - Gossip Protocol
- Each node maintains a list of DB and their response time and whether they are responsive or not.
- Each node calls the other db and they share the data from each other and also spreads their own data, whichever is fastest is taken into account / latest, basically they merge their data - they gossip and spread the status.
- They also ping each other regularly to update this data.
- This way everyone quickly are able to know which DB is up and working and which are down. This is a decentralized and efficient way for status update of all DB.
- If this adds too much overhead we can assign a few nodes as seed DB to monitor and then spread the gossip.
