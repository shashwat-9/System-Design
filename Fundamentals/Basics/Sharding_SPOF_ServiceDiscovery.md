# Sharding

[Refer this for more on this](./Resources/Sharding.pdf)

-------------------------------------------------------

Database Sharding is the process of dividing the data into partitions which can be stored in multiple db instances(servers).
 - Consistency
 - Availability

## Problem
1. Joins across shards will be a computationally heavy operation.
2. No of shards are fixed. If we want to increase it, we can have multiple shards in one shard and a route control point.
3. The particular shard might go down. So there we can use the master-slave architecture.

# Single point of failure
 - A single point in the system that can turn the entire system down.
 - Once all the requirements of a design is complete, the _interviewer_ will test the resilience of the system like SPOF


#### Ways to Mitigate the risk
1. Add more nodes.
2. Master-Slave Architecture.
3. Put the system in Multiple Regions

#### Other points
1. A DNS Server can have multiple ip addresses, and these addresses are of the Load Balancers(or api gateways), which are
there, so that there is no single point of failure.
2. Netflix has something called as chaos monkey, that goes on the production and takes down the a random node to ensure
the resilience.

# Server discovery and HeartBeats
 - Reliability and Availability is more important than efficiency initially.
 - There's a health check service which works for the service discovery and is co-ordinated by the load balancers.
 - The health service query the server and if it doesn't respond, then the server is marked as critical. If it misses
the second consecutive query, then the service is marked as dead. This information is passed to Load Balancers.
 - Maybe the health service will call a service to restart the down system.
 - There could be a case where the service is alive but not capable of processing requests, in avoid such scenarios 
we send a heartbeat to the health check along with the checks performed by the Health service.
 - In service discovery, whenever a service comes alive, it sends the details of ip, port etc to the load balancer, this
information is stored by the load balancer in DB(and maybe in the cache too). Furthermore, if any service would have to 
interact with this one, it will fetch the details from load balancer.
 - The health check detects the new servers from the load balancers and open connections to them.

Approaches for Service Discovery:
1. DNS-Based Service Discovery -> Each service is assigned a DNS name, and the DNS server is updated dynamically.
2. Service Registry/Discovery Agents -> Services register themselves with a central registry upon startup, and clients query 
this registry to find services.
3. Client-Side Service Discovery -> Clients are responsible for determining the network locations of available service instances and load balancing requests across them.
Example: In a microservice architecture, when a service is horizontally scaled by adding more instances, these new 
instances register with a service registry. The client-side service discovery mechanism queries the registry and balances 
the load among all available instances.
4. Server-Side Service Discovery -> The client makes a request to an intermediate router or load balancer, which is 
responsible for directing the request to an available service instance.
5. Self-Announcing Services -> Services publish their availability on a common message bus or distributed system like Apache ZooKeeper, where clients
can discover them.
6. API Gateway -> An API Gateway acts as a single entry point for all clients. It can handle service discovery internally and route 
requests to the appropriate service.