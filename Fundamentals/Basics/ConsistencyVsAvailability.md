## Consistency
- If we have multiple copies of data, they should be same.
- Trivial discussions.

## Data consistency across continents
- We may cache some information of one continent to another, but there could be a cache miss.
- Rest of the discussion is trivial.

## Leader follower Architecture
- What if a follower DB is down, should it retry the updates command on the follower?
- How often, infinite/finite? Should it give up? If yes, won't the data be inconsistent?
- What if the change has been done, but ack is lost?
- It brings us to a fundamental problem of consistency in distributed systems, that is acknowledgement for consistency.
- If we wait for the acknowledgement of acknowledgement, then we will be in the Two Generals problem.
- The way around is, let's have a leader node and rest as follower nodes.
- What if the leader returns the acknowledgement to the client as soon as it has completed the writing. We may have an inconsistent system.
- What if the leader waits for all the nodes to get in sync, the availability will be impacted. So, if we have high consistency, availability will be somewhat impacted. If highly available, then consistency will be impacted.
- The idea is when things go wrong, you can't have both. What if one of the node goes down, the user have timeout for the response. It's an unavailable system.

## 2 Phase commit - tradeoffs
- We may have many followers, that have to persist a record, given by the leader.
- We as a leader, can send first a prepare statement, that is run all the cmds, and get ready to commit, and send back the acknowledgement to the leader, that the node is prepared to commit.
- If the leader doesn't get all the acknowledgements, from all the systems, the leader fails the transaction (the leader will send the rollback statement to all followers)
- The leader will also rollback then.
- Second the leader asks all the followers to commit, and receives an acknowledgement. But before sending the commit command, it will commit in it's own system first.
- The followers shouldn't rollback themselves based on any timeouts or any cmds, cuz what if the commit statement fails at the follower, and the follower rolls back based on timeouts etc, the system becomes inconsistent.
- If the commit fails, we can keep on sending  retries on the system.
- If the acknowledgement of commit sent by the follower fails, the leader can resend the commit command, that the follower have already did, and thus ignored and sends back the acknowledgement.
- If we want to make the system highly consistent, then we will take a lock of the particular record on all the nodes, so that the same record gets visible on all the nodes, to make it highly consistent. Then the availability will be weak.
- A better approach would be to get eventually consistent, rather than this strict consistency, iff the system can accept this flexibility.