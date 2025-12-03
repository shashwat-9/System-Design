## The Oracle and Timer Wheel
- Each server has a capacity, and based on that capacity we should take the number of requests on each.
  If the rate is not limited, we might face the following:
1. Delayed Response
2. Out of Memory Exception
3. Resource droughts(I/O ports busy)
- In our system, there could be servers of different capacities, and say, one server took more requests than it could, and then crashes, if we don't have intelligent routing, we may distribute the requests on the crashed node equally to other servers of the system.
- Since, each have different capacity, some other server might end up having more load than it can handle, and then it will go down, and therefore the entire system.
- It will cause a cascading failures.
- We might have done `vertical scaling`, when the surge appeared on the node, but that may cost more.
- If we could have limited the number of requests that the server could have taken, the crash would not have started in the first place.
- What we can do to have more capable system, is we can use our system more effectively, that is :
1. Using efficient Messaging Protocols : gRPC , written over HTTP 2.0, don't have HOL blocking.
2. Message compression, kryo is a tool, compresses the message into a shorter format.
3. Client connections to be used effectively. If we have a connection to the server, say in a chat system, then there is no point of killing the connection as soon as the work is over, cuz there may be more message exchanges possible.
4. Pull/Push hybrid, pushing messages to a lot of consumer is a big task, rather we can ask each consumer to pull the specific information.
5. Graceful degradation. The example discussed is delivered and read receipts of chat application, in this flow, there is no need to client get acknowledgement of whether the other client received the message or not. If the server said that I've the receipt, then eventually the other client will receive it.

### Rate Limiter Design
- We have an API gateway, which will take and route all the requests.
- In the discussed design, there is a separate component, called Oracle, where the Gateway passes the request and get to know if and where should the request be processed?
- The oracle could be combination of Load Balancer plus Heartbeat Monitor, which knows what service can handle how many requests.
- All of the services are registered to the Oracle, of what's the capacity, and therefore  it provides intelligent routing. Further, if any service cannot handle the request, it sends maybe 5xx to the user.

### Implementing the Rate Limiter
- Mostly Sliding Window approach is used, and all the requests are queued in the window, for say 1 minute. That is, it can take 10 request/min in the queue. If there are more requests, it is discarded.
- If we can take 3 requests in the sliding window, and if the 1st request came on timestamp 60, and the 3rd on 119, we can't take the 4th request that came on 120, as there are 3 elements in the window, for within a minute.

#### Issues
1. Memory footprint
2. Garbage collection

### Timer Wheel
- It's a much simpler approach.
- There is a wheel of fixed size, where every segment of the wheel have a list of fixed size(say 3) which is the maximum number of requests it can have at once, in each segment. The pointer of the wheel rotates every unit of time, and any incoming request at that time gets into the list of the current segment pointed by the pointer, if the requests at the current moment exceeds the  bucket size, then the overflow will be discarded.
- When the pointer comes back to backet after rotation, we kick out each record in the bucket out, and load the new records in the bucket.


## Partitioning and Real life optimizations
- We can have SLA for how many requests we can have from within the system, so that there won't be any hammering from within the system.
- How can we measure the load on a system:
1. Average response time
2. Age of messages in queue. If the average time of request in a queue is high, it means some heavy processing is going on, maybe from internal services.
3. Monitor Dead Letter Queue. It's a queue where the services pushes all the requests that it has errors/failures/issues to process, and it could maybe a kafka topic, which is looked upon by support team to examine the issues. If the DLQ is filling frequently, then that's a solid indication of some tremendous load/issues in the service.

### How to deal with bad actors?
- Say there is a bad actor sending high number of requests. What we can do, we can hash the request ID of each user, and put it in another queue whose index which came as the hash.
- If the same user keeps on sending request again and again, it goes into the same queue, and thus after a point it rejects new requests.
- The cons is what if a good request has the same hash result as the bad actor, it will have to suffer then.
  The way to mitigate the issue is:
1. Increase the number of queue
2. Increase the size of the queue
3. Split overloaded queue -> put a different hash function for the records of this queue, and then segregate the records into another layer of queues, therefore more efficiently removing bad actors.
4. Rather than using request IDs, we can also use process task type, and this way, all computaionally heavier tasks goes in the same queue after hashing, and thus the server is safely loaded.
5. We can also have multi layer queue, where the layers have process types and request id each hashed.


### Real life Optimizations

##### Request collapsing -> We can cache the value, and therefore less number of requests will hit the server.

##### Requests Batching/Condensing -> Say we have three requests, all asking different results from the same resource. Then rather than hitting each as a different requests, we can hit the combined request. Like three requests to fetch different columns from the same table.

##### Client side Rate Limiting
- we can retry the requests based on error type, that is if permanent error then no, and if temporary then yes.
- Exponential backoff etc are discussed.