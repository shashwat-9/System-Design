## Introduction to Design Tradeoffs

Design Tradeoff: The situation in which one attribute of a system is made less usable because another attribute has been given priority.

- Every functional requirement has many non-functional requirement.
- An example could be, the function of uploading image on Instagram, it has various non-functional requirements, like high quality with compression, reduced cost, safe and persistent, recovery, etc etc.
- Likewise, take the case of the functional requirement of viewing an image on Instagram. The non-functional requirements include lesser response time, ability to cache etc etc.


## Push vs Pull Architecture

- Suppose, Christiano Ronaldo uploaded an image on Instagram. How many business requirements could be there?
- Instagram wants to show the followers this image on their feeds. But, it could be, that some are die hard fans and want the image immediately on their feeds, some followers are passive, some may want it on certain time etc etc.
- If we segment the users based on such requirements, we can design different strategies for each. Like the die hard fans getting the notifications at exactly the time the image is posted and so on.

#### Push
- We push the message to each client.
- Instead of sending the message to each client at once, we can send in batches, within a specified interval of time. So, our servers are not overloaded, and every required client gets the image within a specified time.
- Another way is to scale the hardware. Like, say there are many orders during lunch time on Zomato, and we have planned to send the notifications for discount etc, so we can scale the Hardware during that time.


#### Pull
- In this architecture, the user pulls in the data from the service. So, when a user opens the instagram app, the user asks for updates from a certain followings in order.
- This way the load peak is distributed, and the notification service is not overloaded at once.

- We can have a MQ in front of Notification service, this way the load to the notification service is managed to pushing the event to the MQ only, and the distribution is handled by the MQ.

- The hybrid model is most popular, that have push and pull both, for different segments of users.


## Memory vs Latency
- Typical example is having records in the `Cache`.  If we reduce the size of, or remove the cache at all, we have high latency.


## ThroughPut vs Latency
- Assume you have a server that can run almost 20instructions/sec
- Here, throughput can be defined as, how much of the processing power I have is being spent on the task, which are relevant to the application.
- If 10 instructions that are executed are of wait, 2 are for garbage collection etc, and 8 are working on the application itself.

Then, the throughput is :

8/20

Throughput = Application Work/Processing Power.

- 100% throughput is not possible, because alot of background work is in progress, unless you don't have all the background process running as the part of the application.
- How often the background tasks run determines what will be your latency and throughput will be?

- An example is discussed, where a background task runs after every 3 seconds out out 100 seconds of application runtime.
- In this case, the memory where the objects are kept, keeps on filling, and when the background tasks comes, say runs for 3 seconds, in that 3 seconds it has a huge amount of memory to cleanUp, and therefore the entire application will suffer.
- Any person coming up in this 3 seconds will wait, and therefore it will have a lot of connection requests piled up. After 3 seconds, when the application resumes, it might overflow.
- The other approach could be, we can run background tasks in small-small parts, so say 0.1 second of background task, but more frequently, say 90 times.
- So throughput with this case will be 91% < 97%(in the case where the Background task runs for 3 seconds out of 100)
- This way, the worst response time will be `R+3` with the big background task working at once for 3 seconds, but the worst response time will be `R + 0.1` in the later case.
- Many trade-offs between these two approaches, the first centralises all of things at once, and clears them out, utilising more resources. The later have more context switching, but lesser resource utilisation, and frequent clearance leading to lesser bulk up.
- Ideally, we should increase throughput and reduce latency.
- Thrashing of cache is a typical example of lower throughput and higher latency, and it should be avoided.
- If we have alot of threads, then context switching will be there eating of time, leading to lesser throughput. It's also a case of thrashing.


### Consistency vs Availability
