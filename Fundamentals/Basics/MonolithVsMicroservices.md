1. Contract
2. Router between MS
3. Simplify Deployments, when there are many services instead of monolith, the better approach for deployments would be to automate it.
4. Communication
5. Logging

 - We can collect all the logs from all the microservices(maybe on the same or different box) via a MQ.
 - We can use elastic search to search in this file.
 - We can use lucine(which is part of the same Elastic search stack), and can fetch the required logs using Elastic Search.
 - This way debugging will be easy, as all the logs are at one location.

Use the attached notes for more info.