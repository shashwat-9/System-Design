## NoSQL

### Difference between SQL and NoSQL
 - Most of the famous application (like Instagram, WhatsApp, YouTube etc) core functionalities run on RDBMS,but they probably
use NoSQL for analytics.
 - The pros of using a NoSQL DB are:
1. Insertion and Retrievals are fast. All the relevant data is contained in one JSON blob, so insertion and retrival are of the entire blob,
and thus faster.
2. Schema is flexible.
3. They have horizontal partitioning built in. Built for scale.
4. These kinds of DBs are built in for analytics.

 - The Cons of using a NoSQL DB are:
1. Not built for updates(consistency ACID). Financial system doesn't use this.
2. Read time is slower
3. Relations are not implicit.
4. Joining is hard.

### Cassandra Internals
 - In Cassandra, we have a cluster of DBs. Any request ID is hashed, and the hash directs the request to one of the node in the cluster.
 - Typically, the hash function ensures that the request is evenly distributed among the nodes.
 - We can also have a multi-layer cluster that if the request is directed to node 2, node 2 is itself a cluster.
 - This way load is managed. The nodes could be allocated country-wise, and each country have its own cluster.
 - We want to have replicas, so the probability of losing the data is very less. We can choose the next node to have the copy of data, of the current node.
 - So, if a request hits the cluster, it can be time optimized, cuz we have replicas.

Cassandra provides us with:
1. Load Balancing (reading for the less loaded replica node)
2. Redundancy/Replication

### Quorum
 - If a majority of the nodes agree on a particular value, we accept the case.
 - In some cases(where we don't have a majority), we can pick the value of with the latest timestamps.

### Write Heavy DB Design
 - How can we optimize the writings in our DB?
 - A user sends info to the DB server that writes into the DB.
 - The traditional Data structures use B+ trees. They have many child nodes.
 - Therefore, the insertion/search operations are O(log(n)).
 - Each request is an I/O call.
 - We can avoid unnecessary exchange of data, that is headers & data to avoid bandwidth consumption.
 - We can reduce the I/O call to save time & resources.
 - If we can condense all the information in one call, and get one ack, that would lead to less bandwidth utilization.
 - But this way, we have to allocate alot of memory(buffer) on the server, and thus it's a drawback.
 - The easiest way to write is to use Linked List, as writing is O(1). An example is log file.
 - But reading from a Linked list is terrible. We can use a Linked List, plus a sorted array, this way writing is O(1), and reading is O(log(n))

### Write Heavy DB Design—Merging Sorted String Tables
 - We can convert the Linked List in a sorted array. We don't do the sorting/insertion in the memory, but the Database.
 - After a certain point, when the limit of keeping data in memory is reached, we get the data, sort it, and persist it.
 - Rather than merging the arrays over and over again in the DB, that will lead to a tremendous amount of sorting in the Database,
with each writing, we can store the sorted arrays in chunks. So, there are many chunks of the sorted arrays.
 - But there will be many chunks, so say is there are `N` insertions, and each chunk is of size `x`, the total number of chunks would be `N/x`.
 - This is huge!!
 - What we can do, we can merge as many sorted arrays, such that the time complexity of sorting doesn't grow too much.

### Write Heavy DB Design - Query Optimizations
 - If we have `x` number of chunks, and each chunk have `y` number of records, then the total number of worst case operations
would be x * log(y). But if we merge, say `l` number of chunks together, the search time would be (x/ l) * log(l*y) in each chunk,
which is significantly less as the log part will reduce significantly.
 - This way we can have various chunks of data, each with a varying size.
 - We can use `bloom filters`. A `bloom filter` is a Data structure that we put above the data, to have faster queries.
 - Say, we have a dictionary of words, and we want to find whether the word `CAT` is present in it. We can use a Data Structure of size 26
each representing an Alphabet, that has value 1 is there exists words with letter `C`, and 0 if none.
 - Say the word `CORONA` is present and not `CAT`, then the positive at letter index `C` is called a false positive.
 - But we can optimize it by having combinations, like `CA`, `CO` instead of just `C` in the bloom filters.
 - Sometimes, the `bloom filters` do give false positives, but the yet it's effective holistically.
 - We can put `bloom filters` above all the chunks to speed up the query.

Refer the attached lecture notes for more.