## Primary Replica Architecture

### Data Replication

#### Data Migration → You manually move the data to a different location

#### Data Replication → Data automatically getting copied from one place to another.

- The typical case, where a server connects to DB, and if the DB fails, due to whatever reasons, be it software upgrade, heavy load etc etc, there should be a backup DB, having all the data copied to it.
- The primary DB gets all the write operations, and the replicas have those new entries copied to them by the primary.
- The read operation goes to all, the primary and the replicas.

Pros:
- Fault tolerance
- Improved read speeds
- Simple

Cons:
- Consistency
- Slower write speed

### WAL and Change Data Capture

#### Write ahead logging - WAL
- Sequence of actions that happened in the DB.
- Can be replayed and rolled back.
- Supports Transaction
- Timestamps are tricky, that is what should the replica write, should it write its own time, or should it use the time passed
- Based on the logging, the replicas receive the operation executed, along with other details like timestamp etc. Sometimes the replicas can be smart enough to make the changes in the query that is required on them, based on the details.

#### Change Data Capture
- It captures the changes in the DB, based on WAL, and sent over to other systems, maybe using MQ or other tools.
- Ideal for cross-database replication.
- This use case is ideal when you have different types of Databases, typically for different purposes, like say Cassandra
is really good for Write operations, so we can have Cassandra as the primary node and all writes are there, and say we get
another RDS(good for read operations) as replica for reading the database, then CDC could be ideal, as the changes are captured, 
somewhat transformed and sent over a DB  independent tool(typically MQ or MS, but could be something else) to another DB.
- Other benefits includes that there can be alot of different types and number of subscribers and replicas.

### Write Amplification and Split brain

- What if we have two primary replicas, each receiving write requests? Then in that case, each DB will think it has the 
correct value, and therefore in replication in either side, we may end up overwriting or discarding an entire operation executed on one node.
- This is called Split-Brain problem. It's a Multi-Primary Problem.
- Auto Reconciliation is very hard, this is the case when we may end up overwriting or discarding an operation.
- Manual reconciliation is slow. But this is the way, a manual reconciliation will be done and based on that an apology email might be sent to the user.
- The idea here in the multiple primary problem is that it should have an odd number of nodes, and therefore use quorum.

#### Write Amplification Problem
- Say, we have one node where a write request is incoming, and there are many replica nodes where they have to written.
- In this case, one write operation explodes into many change request.
- Put load on primary DB bandwidth.
- Choice between inconsistency and latency.
- Limited writer is one solution, and we take the maximum of all nodes and therefore the system is consistent.
- We may use different cluster architecture, like peer2peer and use consensus algorithms to agree on a value.

#### Database Migrations
- Herein, the current database is stopped, and a DB dump is taken.
- This DB dump is uploaded into the new DB. Then the new DB is started, and all clients are pointed to this new DB location.
- It has Perfect Consistency and Eventual Availability.

- The database migration is normal for general systems. Stop, take a dump, upload, and restart with clients pointing to new DB.
- For hot tables/DB, migrations requires to be startegic. The DB dump is taken in the live DB, till say the checkpoint x, and is uploaded in the new DB. Then again, a sb dump is taken till point y, and uploaded, this time the required time is very less. Now, there are few more populations, we can skip them, and add a trigger for new upserts in the old live DB to the new DB, and then finally take the dump in the new DB, from the checkpoint y to z. This way, live migration is carried of.
- One of the tools that's typically used is the Database logs.
- The other thing is that, we have to point clients to the new DB. We can either stop the entire operation before all the clients starts pointing to the new DB, or we can create a view of the new DB tables(tables in the new DB could have different schemas), and expose them in the old DB, and rename the old DB view to the table that was refered, so that the reading is not impacted, when a portion of clients have migrated.
- This live migration strategy above is loose, and can have unhandled edge cases. In real scenarios, we need to look at all the edge cases precisely, and if we miss anything, we might need to apologies or correct things to the customer.


### Migration across regions(Migration of cloud solutions)
- When we are migrating across cloud providers solutions, like AWS and Azure, the above strategy might not work exactly. Like, we can't have triggers in the DB to update in the new directly.
- Then we may use the design pattern, active-active, that is two datacenters are active, and put a DB proxy where the client connects and which reads from the old DB for the clients, an insert/update/delete. This way trigger problem is solved.
- Here the proxy design pattern is used, to carry out the migration.
- This approach is complex and requires 2 code restarts, once pointing to the proxy and other pointing to the new Datacenter DB. It should be used only when one can't afford any downtime. Otherwise, the brute force approach is fine, that is, take a dump, upload to the new location, and code restarts to point to the new DB.
- We can also use eventual consistency, and therefore, once the system is consistent, we switch.

[Refer this for more](./Resources/DatabaseMigrationSummary.pdf)