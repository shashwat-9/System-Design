# System Design
 - Vertical Scaling
 - Preprocessing using Cron-job
 - Backup Server
 - Horizontal Scaling
 - MicroService Architecture
 - Distributed Systems
 - Load Balancing
 - Decoupling
 - Logging and metrics calculation
 - Extensible

# Scalability
Buying more machines to cater more requests.
 - Horizontal Scaling -> Adding more machines to cater more requests.
 - Vertical Scaling -> Buying a bigger machine to increase throughput.

Trade-off(s) between both:

| Horizontal Scaling               | Vertical Scaling            |
|----------------------------------|-----------------------------|
| Load Balancing Required          | N/A                         |
| Resilient                        | Single point of failure     |
| Network Calls (RPC)              | Inter Process Communication |
| Data Inconsistency               | Consistent                  |
| Scales Well (as users increases) | HardWare limits             |

# Microservice vs Monolith Architecture
 - Monolith architecture is when the entire system is under one service.
 - MicroService architecture is when the entire system is divided into multiple services. A client interacts with an
API gateway to get redirected to the right microservice.

## Advantages of Monolith Architecture
 - Good when the team is smaller or cohesive, as it saves times.
 - Less Complex.
 - Faster as no network calls are made within the system.

## Disadvantage of Monolith
 - More context required. A new joiner will suffer.
 - Complicated Deployment as the system is highly coupled.
 - Single point of failure.

## Advantages of Microservice Architecture
 - Scalability.
 - Easier for new team member.
 - Parallel Development is easy.
 - Easier to reason about. Knowing what requires scaling.
 - Needs skilled architect.
 - 

Advantages:

1) The microservice architecture is easier to reason about/design for a complicated system.
2) They allow new members to train for shorter periods and have less context before touching a system.
3) Deployments are fluid and continuous for each service.
4) They allow decoupling service logic on the basis of business responsibility
5) They are more available as a single service having a bug does not bring down the entire system. This is called a single point of failure.
6) Individual services can be written in different languages.
7) The developer teams can talk to each other through API sheets instead of working on the same repository, which requires conflict resolution.
8) New services can be tested easily and individually. The testing structure is close to unit testing compared to a monolith.

Microservices are at a disadvantage to Monoliths in some cases. Monoliths are favorable when:

1) The technical/developer team is very small
2) The service is simple to think of as a whole.
3) The service requires very high efficiency, where network calls are avoided as much as possible.
4) All developers must have context of all services.

