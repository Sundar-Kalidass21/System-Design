# System Design – Feature Learning (Complete)

This document covers **Sections 1–7** with **all requested topics**.
Each topic includes:
- About the Topic (≈5 lines)
- Key Points (bullets)
- Example (≈5 lines)

---

## SECTION 1 – CORE SYSTEM COMPONENTS

### Client–Server
**About the Topic**  
Client–server architecture separates user-facing components from backend logic. The client handles UI and sends requests. The server processes requests, applies business rules, and manages data. This separation improves security and maintainability. It is the foundation of most web and mobile systems.

**Key Points**
- Client handles presentation
- Server handles logic and data
- Improves security
- Centralized control
- Independent scaling

**Example**  
A mobile app requests user details.  
The server authenticates the user.  
Data is fetched from the database.  
Response is formatted.  
Client renders the UI.

---

### Scaling
**About the Topic**  
Scaling is the ability to handle growth in traffic and users. Systems must scale to avoid performance degradation. Scaling decisions are architectural. Poor scaling causes outages. There are vertical and horizontal approaches.

**Key Points**
- Handles growth
- Prevents overload
- Architectural concern
- Impacts availability
- Essential for success

**Example**  
Traffic increases daily.  
Response time slows down.  
Scaling strategy is applied.  
System handles more load.  
Users see stable performance.

---

### Vertical Scaling
**About the Topic**  
Vertical scaling increases resources of a single server. This includes RAM, CPU, or storage. It is easy to implement. Hardware limits restrict growth. It creates a single point of failure.

**Key Points**
- Upgrade one server
- Easy setup
- Hardware limit
- Expensive
- SPOF risk

**Example**  
Server has 8GB RAM.  
Traffic increases.  
RAM upgraded to 32GB.  
Performance improves temporarily.  
Further growth becomes impossible.

---

### Horizontal Scaling
**About the Topic**  
Horizontal scaling adds more servers. Load is distributed across machines. This improves fault tolerance. It supports large systems. Common in cloud setups.

**Key Points**
- Add servers
- Fault tolerant
- High scalability
- Needs load balancer
- Cloud friendly

**Example**  
Multiple servers are added.  
Requests are distributed.  
One server fails.  
Others handle traffic.  
System stays online.

---

### Load Balancer
**About the Topic**  
Load balancers distribute incoming requests across servers. They prevent overload. Health checks detect failures. Traffic is routed dynamically. They are critical for horizontal scaling.

**Key Points**
- Traffic distribution
- Health checks
- High availability
- Fault tolerance
- Enables scaling

**Example**  
Requests hit load balancer.  
Servers receive balanced traffic.  
One server crashes.  
Traffic rerouted automatically.  
Users notice no failure.

---

### Database
**About the Topic**  
Databases store persistent data. They support queries and transactions. Databases ensure consistency. They can become bottlenecks. Scaling strategies are required.

**Key Points**
- Persistent storage
- Transactions
- Consistency
- Performance sensitive
- Needs scaling

**Example**  
User data stored in DB.  
Traffic increases.  
Queries slow down.  
Indexes added.  
Performance improves.

---

### Database Sharding
**About the Topic**  
Sharding splits data across databases. Each shard stores a subset. This improves write scalability. Complexity increases. Shard key choice is critical.

**Key Points**
- Split data
- Write scalability
- Reduced load
- Complex queries
- Shard key needed

**Example**  
Users split by ID.  
Writes distributed.  
No DB overloaded.  
Queries routed correctly.  
System scales.

---

### Database Replication
**About the Topic**  
Replication copies data to replicas. Writes go to primary. Reads go to replicas. Improves availability. May cause lag.

**Key Points**
- Primary writes
- Replica reads
- High availability
- Failover support
- Replication lag

**Example**  
Orders written to primary.  
Reads from replicas.  
Primary goes down.  
Reads still work.  
System partially available.

---

### Object Storage
**About the Topic**  
Object storage holds unstructured data. Used for images and videos. Scales independently. DB stores only URLs. Reduces DB load.

**Key Points**
- Unstructured data
- Scalable
- Cost efficient
- URL based
- DB lightweight

**Example**  
User uploads image.  
Image stored in object storage.  
URL saved in DB.  
Client loads image.  
DB remains fast.

---

### Cache
**About the Topic**  
Cache stores frequently accessed data in memory. It reduces DB calls. Improves latency. Data is temporary. Eviction policies apply.

**Key Points**
- Fast access
- DB offload
- Temporary data
- Improves latency
- Uses eviction

**Example**  
Product data cached.  
Requests served instantly.  
Cache expires.  
Data reloads from DB.  
Cache refreshed.

---

### CDN
**About the Topic**  
CDN distributes static content globally. Content served closer to users. Reduces latency. Offloads backend. Improves UX.

**Key Points**
- Global delivery
- Low latency
- Static content
- Backend offload
- High availability

**Example**  
Images served via CDN.  
User loads page faster.  
Backend handles APIs.  
Traffic reduced.  
Performance improves.

---

### Monolithic Architecture
**About the Topic**  
Monoliths combine all features in one app. Easy to start. Hard to scale parts. Failures affect entire system. Becomes complex.

**Key Points**
- Single codebase
- Easy initially
- Tight coupling
- Hard scaling
- High risk

**Example**  
Login and payment in one app.  
Payment bug occurs.  
Entire app crashes.  
Scaling affects all modules.  
Deployment risky.

---

### Microservices Architecture
**About the Topic**  
Microservices split features into services. Each service is independent. Scales separately. Better fault isolation. Higher complexity.

**Key Points**
- Independent services
- Separate scaling
- Fault isolation
- Faster deploys
- Ops complexity

**Example**  
Payment service spikes.  
Only payment scales.  
Order service stable.  
Failure isolated.  
System resilient.

---

### Message Queue
**About the Topic**  
Message queues enable async communication. Producers send messages. Consumers process later. Improves reliability. Handles spikes.

**Key Points**
- Async processing
- Decoupling
- Spike handling
- Reliability
- Event-driven

**Example**  
Order placed.  
Message sent to queue.  
User gets instant response.  
Email sent later.  
System responsive.

---

### API Gateway
**About the Topic**  
API Gateway is single entry point. Handles auth and routing. Applies rate limits. Simplifies clients. Centralizes control.

**Key Points**
- Single entry
- Auth handling
- Rate limiting
- Routing
- Client simplicity

**Example**  
App calls gateway.  
Gateway validates token.  
Routes request.  
Applies limits.  
Backend protected.

---

## SECTION 2 – SYSTEM QUALITIES

### Scalability
**About the Topic**  
Scalability defines growth handling. Systems must grow without failure. Requires planning. Linked to architecture. Critical for success.

**Key Points**
- Growth support
- Architectural
- Prevents crashes
- Long-term
- Business critical

**Example**  
Users increase daily.  
Traffic spikes occur.  
System scales horizontally.  
No downtime seen.  
Growth sustained.

---

### Availability
**About the Topic**  
Availability is uptime. High availability minimizes downtime. Uses redundancy. Failover mechanisms help. Measured in percentages.

**Key Points**
- Uptime focused
- Redundancy
- Failover
- User trust
- Business impact

**Example**  
One server fails.  
Traffic rerouted.  
Users unaffected.  
System stays online.  
Availability maintained.

---

### Consistency
**About the Topic**  
Consistency ensures data correctness. Distributed systems struggle with it. Trade-offs exist. Business decides model. Impacts UX.

**Key Points**
- Data accuracy
- Distributed challenge
- Trade-offs
- Business driven
- CAP related

**Example**  
User updates data.  
Some nodes lag.  
System syncs later.  
Consistency achieved.  
Model defines behavior.

---

### Strong Consistency
**About the Topic**  
Strong consistency shows latest data always. No stale reads. Slower systems. Used in finance. Accuracy critical.

**Key Points**
- Immediate accuracy
- No stale reads
- Slower
- Banking systems
- High reliability

**Example**  
Bank transfer occurs.  
Balance updates instantly.  
All users see same data.  
No delay allowed.  
Consistency enforced.

---

### Eventual Consistency
**About the Topic**  
Eventual consistency allows delay. Data syncs later. Improves performance. Used in social apps. Temporary mismatch acceptable.

**Key Points**
- Temporary inconsistency
- High performance
- Scalable
- Social feeds
- Distributed friendly

**Example**  
Post created.  
Some users see later.  
Data propagates.  
Eventually synced.  
UX acceptable.

---

### Fault Tolerance
**About the Topic**  
Fault tolerance allows operation during failures. Failures are expected. Uses redundancy. Prevents total outage. Improves reliability.

**Key Points**
- Failure handling
- Redundancy
- High reliability
- Graceful degradation
- Distributed systems

**Example**  
Server crashes.  
Others continue.  
Traffic rerouted.  
System alive.  
Users unaffected.

---

### SPOF
**About the Topic**  
Single Point of Failure causes full outage. One component fails system. Must be removed. Redundancy fixes SPOF. Critical design concern.

**Key Points**
- One failure risk
- System outage
- Must eliminate
- Redundancy needed
- Reliability issue

**Example**  
Single DB used.  
DB crashes.  
System goes down.  
Replica added.  
SPOF removed.

---

### IP Address
**About the Topic**  
IP address identifies devices. Enables network communication. Used for routing. IPv4 and IPv6 exist. Fundamental to networking.

**Key Points**
- Unique identifier
- Network routing
- IPv4/IPv6
- Internet core
- Device addressing

**Example**  
Server assigned IP.  
Client sends request.  
Router forwards traffic.  
Server receives packet.  
Communication succeeds.

---

### DNS
**About the Topic**  
DNS maps domain names to IPs. Improves usability. Distributed system. Cached globally. Critical internet service.

**Key Points**
- Name resolution
- Human friendly
- Distributed
- Cached
- Internet backbone

**Example**  
User types domain.  
DNS resolves IP.  
Browser connects.  
Server responds.  
Page loads.

---

### Client–Server Relationship
**About the Topic**  
Relationship follows request-response. Client initiates. Server responds. Stateless in modern APIs. Scales easily.

**Key Points**
- Request-response
- Client initiated
- Stateless
- Scalable
- HTTP based

**Example**  
Client sends request.  
Server processes.  
Response returned.  
Connection closes.  
Next request independent.

---

## SECTION 3 – PROTOCOLS

### TCP
**About the Topic**  
TCP ensures reliable delivery. Maintains order. Retransmits lost packets. Slower but accurate. Used for critical data.

**Key Points**
- Reliable
- Ordered
- Error correction
- Slower
- Connection based

**Example**  
File download starts.  
Packets sent.  
Loss detected.  
Packets resent.  
File intact.

---

### UDP
**About the Topic**  
UDP is connectionless. No guarantee of delivery. Very fast. Used for real-time apps. Loss acceptable.

**Key Points**
- Fast
- No reliability
- Low latency
- Connectionless
- Real-time use

**Example**  
Video stream starts.  
Packets sent quickly.  
Some lost.  
Video continues.  
Latency minimal.

---

### HTTP
**About the Topic**  
HTTP is stateless protocol. Request-response based. Foundation of web. Simple. Widely supported.

**Key Points**
- Stateless
- Web standard
- Request-response
- Simple
- REST base

**Example**  
Browser sends request.  
Server processes.  
Response returned.  
Connection closed.  
Next request new.

---

### WebSocket
**About the Topic**  
WebSocket enables real-time communication. Persistent connection. Bidirectional. Low latency. Used in chat apps.

**Key Points**
- Persistent
- Real-time
- Bidirectional
- Low latency
- Event driven

**Example**  
Chat connection opened.  
Messages sent instantly.  
Server pushes updates.  
No polling needed.  
Real-time UX.

---

### Forward Proxy
**About the Topic**  
Forward proxy hides clients. Controls outbound traffic. Used in enterprises. Improves security. Enables filtering.

**Key Points**
- Client hiding
- Outbound control
- Security
- Caching
- Monitoring

**Example**  
Employee accesses web.  
Request goes to proxy.  
Proxy forwards request.  
Response returned.  
Client hidden.

---

### Reverse Proxy
**About the Topic**  
Reverse proxy hides servers. Handles inbound traffic. Used for load balancing. Improves security. Common with Nginx.

**Key Points**
- Server hiding
- Inbound traffic
- Load balancing
- Security
- Caching

**Example**  
Client hits proxy.  
Proxy routes to backend.  
Server hidden.  
Response returned.  
Security enforced.

---

## SECTION 4 – COMMUNICATION

### REST API
**About the Topic**  
REST uses HTTP methods. Stateless design. Resource-based. Simple and scalable. Widely adopted.

**Key Points**
- Stateless
- HTTP verbs
- Resource based
- Scalable
- Simple

**Example**  
Client calls GET /users.  
Server processes.  
JSON returned.  
Client renders.  
Independent request.

---

### GraphQL
**About the Topic**  
GraphQL allows client-defined queries. Avoids over-fetching. Single endpoint. Flexible. Frontend optimized.

**Key Points**
- Client controlled
- No over-fetching
- Single endpoint
- Flexible
- Frontend friendly

**Example**  
Client requests name only.  
Server returns only name.  
Payload small.  
Performance better.  
Network optimized.

---

### gRPC
**About the Topic**  
gRPC is binary protocol. Uses HTTP/2. Fast communication. Strong typing. Used between services.

**Key Points**
- Binary
- High performance
- Strong typing
- HTTP/2
- Internal services

**Example**  
Service A calls Service B.  
Binary data sent.  
Low latency.  
Fast response.  
Efficient communication.

---

### Message Queues
**About the Topic**  
Queues enable async messaging. Decouple systems. Improve reliability. Handle spikes. Event-driven design.

**Key Points**
- Async
- Decoupled
- Reliable
- Scalable
- Event based

**Example**  
Event sent to queue.  
Consumer processes later.  
Producer not blocked.  
System responsive.  
Failure isolated.

---

## SECTION 5 – DATABASE & STORAGE

### Relational (SQL) Database
**About the Topic**  
SQL databases use tables. Fixed schema. ACID transactions. Strong consistency. Used in finance.

**Key Points**
- Structured
- ACID
- Strong consistency
- Relational
- Reliable

**Example**  
Bank stores transactions.  
Each transaction atomic.  
Rollback on failure.  
Data consistent.  
System correct.

---

### Non-Relational (NoSQL) Database
**About the Topic**  
NoSQL uses flexible schema. Scales horizontally. High performance. Eventual consistency. Big data friendly.

**Key Points**
- Schema-less
- Scalable
- Fast
- Distributed
- Eventual consistency

**Example**  
Posts stored in NoSQL.  
Schema evolves.  
High traffic handled.  
Delay acceptable.  
System scales.

---

### SQL vs NoSQL
**About the Topic**  
SQL prioritizes consistency. NoSQL prioritizes scalability. Choice depends on use case. Trade-offs exist. Architecture decision.

**Key Points**
- Consistency vs scale
- Schema vs flexibility
- Use-case driven
- CAP trade-off
- Design choice

**Example**  
Payments use SQL.  
Feeds use NoSQL.  
Different needs.  
Different systems.  
Correct choices made.

---

### Object Storage
(refer Section 1)

### Cache
(refer Section 1)

### CDN
(refer Section 1)

---

## SECTION 6 – CACHING

### Read-Aside Cache
**About the Topic**  
Application checks cache first. On miss, DB queried. Cache updated. Simple pattern. Widely used.

**Key Points**
- Cache first
- DB fallback
- Lazy loading
- Simple
- Common

**Example**  
Request checks cache.  
Cache miss.  
DB queried.  
Cache updated.  
Next request fast.

---

### Write-Through Cache
**About the Topic**  
Writes go to cache and DB. Cache always updated. Strong consistency. Slower writes. Simple logic.

**Key Points**
- Cache always fresh
- DB updated
- Strong consistency
- Slower writes
- Simple reads

**Example**  
Update request received.  
Cache updated first.  
DB updated next.  
Reads hit cache.  
Consistent data.

---

### Read & Write Cache Strategies
**About the Topic**  
Systems combine strategies. Read-heavy uses read-aside. Write-heavy uses write-through. Choice depends on access pattern. Impacts performance.

**Key Points**
- Strategy based
- Access pattern driven
- Performance impact
- Consistency trade-off
- Design choice

**Example**  
Product catalog read-heavy.  
Read-aside used.  
User profile write-heavy.  
Write-through used.  
System optimized.

---

## SECTION 7 – UTILS

### Cloud
**About the Topic**  
Cloud provides on-demand resources. Scales easily. Pay-as-you-go. High availability. Managed services.

**Key Points**
- On-demand
- Scalable
- Cost efficient
- Managed infra
- Global reach

**Example**  
Server created on demand.  
Traffic increases.  
Auto-scaling triggered.  
Cost optimized.  
System scales.

---

### Logging & Monitoring
**About the Topic**  
Logging records events. Monitoring tracks metrics. Alerts detect failures. Critical for ops. Improves reliability.

**Key Points**
- Visibility
- Debugging
- Alerting
- Metrics
- Reliability

**Example**  
Error logged.  
Alert triggered.  
Engineer notified.  
Issue fixed quickly.  
Downtime minimized.

---

### Hashing
**About the Topic**  
Hashing converts input to fixed output. Used for security. Fast lookup. Irreversible. Used in passwords.

**Key Points**
- Fixed size
- One-way
- Fast
- Secure
- Distributed systems

**Example**  
Password hashed.  
Stored securely.  
Login compares hash.  
Original hidden.  
Security ensured.

---

### Consistent Hashing
**About the Topic**  
Consistent hashing distributes keys evenly. Minimizes rebalancing. Used in caches. Scales well. Distributed friendly.

**Key Points**
- Even distribution
- Minimal movement
- Cache friendly
- Scalable
- Distributed

**Example**  
Cache nodes added.  
Few keys remapped.  
Most cache preserved.  
Performance stable.  
Scales smoothly.

---

### CAP Theorem
**About the Topic**  
CAP states only two of C, A, P possible. Trade-offs unavoidable. Guides design. Distributed systems only. Critical concept.

**Key Points**
- C vs A trade-off
- Partition tolerance required
- Design constraint
- Distributed systems
- Architecture guide

**Example**  
Bank chooses consistency.  
Feed chooses availability.  
Partition occurs.  
System behaves as designed.  
CAP explains behavior.

---

### Database Indexing
**About the Topic**  
Indexing speeds up queries. Uses extra data structures. Improves read performance. Increases write cost. Essential for DBs.

**Key Points**
- Faster reads
- Extra storage
- Write overhead
- Query optimization
- DB performance

**Example**  
Email column indexed.  
Search faster.  
Insert slightly slower.  
Query optimized.  
Performance improved.

---

### Capacity Estimation
**About the Topic**  
Capacity estimation predicts resource needs. Prevents outages. Controls cost. Done early. Interview critical.

**Key Points**
- Traffic prediction
- Resource planning
- Cost control
- Prevent outages
- Design phase

**Example**  
DAU estimated.  
Requests calculated.  
Servers provisioned.  
System handles load.  
No downtime.

---

### Event Streaming
**About the Topic**  
Event streaming processes real-time events. Producers emit events. Consumers react. Scales well. Used in analytics.

**Key Points**
- Real-time
- Async
- Scalable
- Event driven
- Analytics friendly

**Example**  
User action emitted.  
Event streamed.  
Analytics consumes.  
Dashboard updates.  
Insights generated.

---

## END OF DOCUMENT
