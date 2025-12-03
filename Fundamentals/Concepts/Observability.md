## Introduction to Observability

#### Observability in Distributed Systems
- Observing a system externally and understanding if the system is working fine or not
- Multiple topics under Observability :
1. Logging and Monitoring
2. Alerts
3. Anomaly detection
4. Root cause analysis


#### Logging best practices
- Getting the work data of the system.
- Hide confidential informations in the log files.


#### Monitoring Metrics
- track metrics of the system across time
- It could be Memory usage, Number of I/O operations, or could be business data like how many sales done by the system or the number of visitors etc.
- It could be viewed by someone at regular intervals.
- AWS have cloudwatch, which does this monitoring. Like say Load balancer have high usage, this could indicate a requirement to add new.


### Anomaly Detection
- The system keeps on getting monitored, and in case of unusual peaks and drops, we can alert the authority.
- The graph is plotted in a way, such that, the current point shows the confidence line for this point, that what could be the interval within which the value could lie for the next point.
- We can look for spike in the immediate graph, the differentiated graphs, like velocity, accelerations(ideally, the acceleration should be zero, but certain businesses have growing curve, so reporting should be decided based on the nature of business), jerk.
- If the jerk graph has spikes, it typically should be reported.
- Mathematically, we can compute anomalies, using the `Holt winter Algorithms`.


#### Root cause analysis
- There are two ways to do the Root cause analysis, Automatic(which is very hard to do), and manual(ideal)
##### Manual
- You look at the log lines, and prepare an incident report, with details like
1. When did the fault occured?
2. What was the root cause?
3. Technique to find the root cause.

##### Automated
- If a metric have gone wrong, what factors have contributed to it, amongst them which one(or many) behaved weirdly.
- In a mathematical sense, we may be looking at algorithms like, `Principle Component Analysis`, `Spearman's Coefficient`(used in Gorilla DB)
- We can use `AWS CloudWatch`, or `AWS SageMaker`.