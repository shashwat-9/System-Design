# Caching

 - The generic example of caching is discussed, that a user queries for a record in the Database, and another user with similar background tries to access it, then rather than querying the DB again, we can cache the data, for an optimized performance.
 - We typically take a section of the Database, that is frequently accessed and put it in Cache.
 - For smaller DBs, ranging in few GBs, it might be a considerable thought to put the DB in the Cache.
 - At a high level, caching reduces latency by avoiding repeated work through storage.

Important questions for the Cache:
1. How do I manage writes? If there's an update in the Database, we should be updating the Cache as well.
2. What data do I evict on overflow?

These two questions form the cache policy. There are a few ML-based policies too.

 - When we do alot of eviction and loading in the cache, via some policies, and none of it appears fruitful, we term it as ```Thrashing```.
 - If there are some updates in the Database about the records currently in the cache, the cache has also to be updated. 
 - There could be frequency of updating the cache, and therefore it will be Eventually consistent.
 - We can have the Cache on the server, in the DB to return the frequently accessed records, or could be a global component.
 - Each has some tradeoffs, and typically in a large-scale system, all are applied, including the client.

Caching:
1. Reduce Network calls
2. Avoid Repeated computations
3. Reduce database load

Cache Policy and the placement matter.

### Write Back Policy
 - Write policy is different from Replacement policy.
 - Replacement policies(evict existing and add new) are triggered when you have to bring a new entry from the database to the cache.
 - Write to a Cache means Create/Update/Delete
 - Say, we have a writing to a cache, and this has to be consistent with the DB. We can use time based persistence in the cache(TTL -> Time to Live)
 - Once the time exceeds TTL, we kick out the record and this is when we persist in DB, making it Eventually consistent.
 - Another approach is that we can update the data event based, say there are 5 updates to a key-value in the cache, and after 5 updates we persist the changes in the DB.
 - Write Back is Cache writing the updated data to the source of truth.

### Write Through Policy
 - If there is a write to the cache, we take the key, kick it out of the cache, to get persisted in the DB.
 - A tricky case, what if we kicked the data out of cache, for it to be in the DB, and someone comes along to read the same entry
, which is currently unavailable in the cache, and the DB is yet not updated. Possible that the user might get the stale value.
 - The above case can be mitigated by acquiring a lock on the record.
 - Used in places where you need high level of consistency and persistence, but not in places where efficiency is the peak requirement.

### Write Around Policy
 - In this policy, we directly write in the DB. 
 - There may be some mechanism in the cache, like TTL etc, that will kick the stale data, and later on data will be loaded from DB, when required.


