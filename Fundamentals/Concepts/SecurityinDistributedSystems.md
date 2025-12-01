### Security Aspects in distributed systems

#### Verification
- The idea is to verify if the request is even coming from a human being or not.
  The tools for this are :
1. CAPTCHA
2. SSL/TLS -> Security algorithms, we typically requires to know the version of SSL/TLS used, so as to gauge performance.
3. Encryption algorithms -> ECC, RSA, DIFFE-HELLMAN, all these are maths heavy algorithms and thus not required in depth from system design perspective.

#### Digital Right Management
- The idea is how to ensure the content goes only to one who have the right to do so.
- Like a video is supposed to be viewed by a user in India only, and a consumer using the service in US must not. Netflix faces this issue.
- Fair play is a good example to read over.

###### The above two topics are not covered from system design perspective, but can be read online.

### Three parts are there from security aspects:
1. Authentication
- OAuth
- SSO
- Token

2. Authorization
- ACL(Access control lists)
- Rule-Engine(leasing)
- Secret keys/client keys

3. Protection
- Hackers
- Employees
- Malicious code


### Token based Auth
- Tokens are signed zibbrish by the server using a private key, where the raw doc have the user info and Authorization info.
- Public key can be used to decrypt the information. But the server requires the signed zibbrish to believe this was given by the server.
- This doesn't saves from replay attacks, that is what if someone steals your token, and use it to login.
- A user whose token has been stolen and used can prevent it by logging out of the account. This way the token will be invalidated.


### SSO and OAUTH
- SSO is a SAML technique?
- We use another service provider(uber, Google, Microsoft etc etc) to verify if it's their user, and if they say yes, we allow them to use our service.

#### OAUTH
- OAuth is used for Authorization, but it's also used for authentication.
- It's just opposite of SSO.
- Herein, the requester is the server and not the client. Example, a server(business) wants to make updates to the google
calender of a client, if the service(google) says yes, then it will be able to do. If not, then no. It's the client that gives the permission to the business to make such updates.
- OAUTH(ideally) is for Authorization on external systems.
- OAUTH0 is there for Authentication, but everyone uses OAUTH for all Authentication/Authorizations.
- The general public uses OAUTH for authentication, like they ask the permission from the user to get their name and profile photo,
user says yes, and they get it. It is basically a Authorization request, but since google has the details, the business takes it as the authentication of the client. Funny :)


### Authorization

1. Access Control List
2. Rule Engine (Learning)
3. Secret Keys / Client Keys

### ACLs and Rule Engines
- A set of things an object (or user) can do are all defined in a list known as Access Control List(ACL).
- ACLs when grouped together forms a `Access Control Matrix` of the entire system.

ACLs can be of various types :
1. User-based
2. Role-based
3. Resource-based
4. Group-based

User and Resources fall under the Object based ACLs.

- Whenever a request comes in, it must get authenticated first. Once authenticated, then it can refer to the ACL for it, and get whether it can perform the tasks it is intending to.
- The ACLs can be modelled as a bunch of `if` statements.
- Generally, Object based rules takes precedence over Group based rules. Like all admins are allowed to delete, except Shashwat (who's an admin). This is an object based ACL taking precedence over Group based.
- All sort of whitelisting/blacklisting can happen here.

#### Rule Engine
- It have rules that can be performed before making any decisions.
- Like, if the user belongs to India, the subscription should expire in 30 days, if in America, it should within 15 days.
- Example includes AWS rules engine, etc etc

#### Secret Key/Client Keys
- The user sends the secret Key to the server, gets authenticated and therefore is authorised to do the certain requested activity.
- Not very nice, but an additional layer of security.

### Protection
- Distributed Denial of Service(DDoS) attack is a cybercrime, in which the attackers floods a server with internet traffic to prevent users from accessing connected online resources.
- WAF(Web application Firewall) can block access to such activities.
- To prohibit an employee from doing malicious activity, ACLs can be used effectively, thereby restricting access to certain privileges.
-  We should open the least number of entry points on the server, so that if the attack occurs we don't have 100s of entries to look at. So therefore, restricting access to the system from different angles.
-  If a service is meant to talk to only another few services, other accesses should be blocked.


### How videos are protected inside CDNs?
- CDNs have the contents cacahed.
- A user comes in to watch a particular video. We check with the server if the user is allowed to watch the content. If yes, then streamed.
  Pros:
1. Simple
2. Strong auth
   Cons:
1. slow
2. Unsecured

- Another approach is Domain Restriction, that is CDN says I'm going to serve you a request iff you came from a certain url. That is it checks for the headers to have a certain web url, and if it has that then served.
  The pros:
- Simple
- Fast
  Cons
- Definitely not Secure

- Server Side Auth
- In this approach, the user directly get the token from the service(e.g. Google) and then forwards it to the CDNs. The CDNs with the public key can decrypt and see that ok, the user is valid and thus serve the content.