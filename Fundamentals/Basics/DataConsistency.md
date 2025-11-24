## Data Consistency

Consistency is a measure of how recent or UP-TO-DATE a piece of data inside a system is.

Why consistency is Important:
1. Easier to understand. If all the systems have the same data, it is easier to reason about.
2. User experience. Example, a user likes a post and is reflected is appreciated by the user.

Lesson content:
1. Consistency levels
2. Tradeoffs
3. Examples

### Linearizable consistency
In this consistency, all read requests lead to the most recent result.

There are many requests, and each is processed one by one in the System, and therefore the system is said to be linearly consistent.

Benefits are:
1. Highly consistent
2. Easy to debug

There are drawbacks of this level too:
1. What if a record takes immensely high time to process? The others in the queue will wait, and this is called 'Head of Line Blocking'
2. What if one record can't be processed or breaks the system, the other in line will suffer and won't be taken into the system. Hence single point of failure.
3. Therefore such systems have low availability and high latency.

Implementations:
1. Single Thread -> a single Thread takes request in a server, and therefore the thread can be considered as a queue,
2. RAFT -> Distributed protocols like raft can be used.


### Eventual consistency:
- Eventual consistency is when the system will eventually be consistent.
- There might be many requests to a system, say read and write both to the same record, the concurrent behaviour might place read first before the update, leading to a stale value shown to the user.
- In some cases, like where the user is updating some values, and they get the stale read, it could be an issue.
- In other cases, like sending emails, the request is sent, and is in the outbox, and eventually gets in the receivers mailbox, is completely fine.

### Causal Consistency
- Suppose we have many read/write/update/delete requests incoming in the system. If we can order the requests that are on the same set of records, that is say a read(k1) came first followed by update(k1) and read(k1), then the operations are executed in the same order.
- This was, the consistency is based on cause and effect, that is if some operations was executed first, it will have effects on the following requests.
- The systems where we can't order the similar requests, or there is a contention in between all the related requests on who can acquire the lock, this consistency becomes tough.
- We will have problems in casual ordering, where aggregate functions are used. Example sum(all) sum(all), update(k1), sum update(k2), sum ...., in this order of requests if we place all the aggregate(sum) functions in one ordering, the results would be variable based on the permuation of executions with the update key.

### Quorum
- This is another consistency level, called quorum.
- In this case, we have multiple replicas of the database, that may or may not be consistent among themselves.
- We query all the nodes, for a read requests, and take the choose the element based on some logic, like the the element which occurred the most among the results received from all nodes, or the record that has been updated latest among all the records.
- Quorum works on some kind of consensus in distributed systems.
- Quorum is eventually consistent in most cases, as all the replicas have to be in sync.
- But what if the wrote node fails, the system would be inconsistent, and therefore not even eventual.
- We can make it strongly consistent by applying the rule, that R + W > N, that is the number of nodes from where we are reading the data and the number of nodes to which we are writing the data(in the first go, that is not considering the process of syncing between replicas) should add more than the total number of nodes. If the relation fails, we return the user 5xx, that don't have enough replicas to return consistent result.
- Quorum gives fault tolerance, by the nature of its design
- Eventual consistency if we choose `R + W <= N`.
- Quorum is the minimum number of votes that a distributed transaction has to obtain in order to be allowed to perform an operation in a distributed system.

### Tradeoffs
- The Linearizable Consistency is highly consistent but weakly efficient.
- The Eventual consistency is weakly consistent but highly efficient.
- The Causal Consistency is subjectively highly consistent and efficient.
- The Quorum is configurable, as there are multiple nodes required, and hence could be expensive. Multiple Problems could arise with Quorum like Split brain, or single point of failure(when the number of nodes is one).

## Transaction Isolation Levels

#### Read Uncommited Data
 - Say we have two transactions, T1 and T2, T1 is updating a record, and T2 is reading that record.
 - If the update(and not the following commit) in T1 is executed before the read in T2, and we can set the isolation level to Read Uncommitted
to read the current(committed or uncommited, whichever is latest) value of records.
 - This reads the latest copy of data.
 - This trades offs level of isolation with efficiency.

#### Read Committed
 - The Read Committed isolation is the isolation that ensures only commited data is read. That's it!
 - Implementations might change things, but it returns a committed value only, either latest or older.

#### Repeatable Read
 - In this Isolation level, we read A committed value that has been snapshot-ed at the beginning.
 - We are not taking the latest committed values to get more efficiency in repeated reads.
 - Say a transaction T1 changed a value of a record, that is getting read in another transaction T2 twice, so in the first attempt it gives 'a',
and in the second attempt it gives 'b', which is the committed value by T1. The repeated reads of the same record giving different values is a problem, as it can
make wrong decision.
 - A transaction running in Repeatable Read Isolation mode takes a snapshot of the DB at the start and reads value from there only.
 - When it goes for committing the transaction, it checks whether any values that were read in the transactions are changed in the actual DB,
if no commit succeeds, if yes, any such value was changed, then transaction is aborted.

#### Serializable
 - The highest level of isolation comes in Serializable Isolation level.
 - Guarantees transactions appear to execute __one after another__(as if serialized), even when running concurrently.


#### Isolation levels 1. Serializable > 2. Repeatable Read > 3. Read Committed > 4. Read Uncommitted
#### Efficiency levels 1. Serializable < 2. Repeatable Read < 3. Read Committed < 4. Read Uncommitted

[Refer this for more](Resources/consistency.pdf)