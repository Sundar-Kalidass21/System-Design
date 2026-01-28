# Section 1: Core System Components

## Topic 1.1: Client–Server Architecture

### About the Topic
Client–server architecture is a fundamental design model where responsibilities are divided between clients and servers. The client focuses on user interaction and presentation, while the server is responsible for business logic, security, and data management. This separation exists to improve maintainability and security by centralizing critical logic on the server side. It allows teams to evolve frontend and backend independently without tightly coupling changes. Understanding this model is essential because almost all modern web, mobile, and distributed systems are built on it.

### Key Points
- The client initiates requests and handles user-facing concerns such as UI and input validation.
- The server processes requests, enforces business rules, and controls access to data.
- Direct database access from clients is avoided to maintain security and integrity.
- The architecture supports independent scaling of clients and servers.
- Clear separation of concerns makes systems easier to maintain and extend.

### Example
A mobile application displays a user profile screen that needs account information.  
The app sends an authenticated request to the backend server asking for profile data.  
The server validates the user token, checks permissions, and queries the database.  
It formats the response into a safe JSON structure and sends it back to the client.  
This example shows how responsibility is cleanly divided between client and server.

---

## Topic 1.2: Scaling

### About the Topic
Scaling refers to a system’s ability to handle increased load without degrading performance. As the number of users, requests, or data grows, the system must adapt to meet demand. Scaling is not an afterthought but a core architectural concern during system design. Poor scaling decisions often lead to outages or slow response times. Learning scaling concepts helps engineers design systems that grow smoothly with business needs.

### Key Points
- Scaling addresses growth in traffic, users, or data volume.
- It directly impacts performance, reliability, and user experience.
- Scaling decisions should be made early in system design.
- Both infrastructure and application design influence scalability.
- Vertical and horizontal scaling are the two primary approaches.

### Example
A startup launches an application with a small user base.  
As marketing campaigns succeed, daily active users increase rapidly.  
The original setup starts to slow down under higher request volume.  
Engineers apply scaling strategies to handle the increased load.  
This shows why scaling must be planned for growth scenarios.

---

## Topic 1.3: Vertical Scaling (Scale Up)

### About the Topic
Vertical scaling increases the capacity of a single server by adding more resources. This usually involves upgrading CPU, RAM, or storage on an existing machine. It is attractive because it requires minimal changes to application architecture. However, it is limited by hardware constraints and cost. Vertical scaling also introduces a single point of failure, which can reduce reliability.

### Key Points
- Involves upgrading resources on a single machine.
- Easy to implement without architectural changes.
- Limited by physical hardware constraints.
- Becomes expensive at higher scales.
- Introduces a single point of failure.

### Example
A backend server starts with 8 GB of RAM and handles requests comfortably.  
As traffic increases, memory usage grows and response time degrades.  
The team upgrades the server to 32 GB of RAM.  
Performance improves for a while without code changes.  
This example shows how vertical scaling provides quick but limited relief.

---

## Topic 1.4: Horizontal Scaling (Scale Out)

### About the Topic
Horizontal scaling increases capacity by adding more servers instead of upgrading one. Traffic is distributed across multiple machines, allowing the system to handle much larger loads. This approach improves fault tolerance because failure of one server does not bring down the system. Horizontal scaling is a core principle of cloud-native systems. It is essential for building highly available and resilient applications.

### Key Points
- Adds more machines to handle increased load.
- Improves fault tolerance and availability.
- Supports very large-scale systems.
- Requires traffic distribution mechanisms.
- Commonly used in cloud environments.

### Example
An e-commerce platform deploys multiple backend servers.  
Incoming traffic is distributed across these servers.  
During peak traffic, one server crashes unexpectedly.  
Other servers continue processing requests without interruption.  
This demonstrates how horizontal scaling improves resilience.

---

## Topic 1.5: Load Balancer

### About the Topic
A load balancer sits between clients and backend servers to distribute incoming requests. Its purpose is to prevent any single server from becoming overloaded. Load balancers also monitor server health and avoid routing traffic to unhealthy instances. They are a key enabler of horizontal scaling. Without load balancers, adding servers would not effectively increase capacity.

### Key Points
- Distributes traffic across multiple backend servers.
- Prevents overload on individual servers.
- Performs health checks on server instances.
- Improves availability and reliability.
- Essential for horizontal scaling.

### Example
Thousands of users access a website at the same time.  
All requests first reach a load balancer.  
The load balancer routes each request to a healthy server.  
When one server becomes unresponsive, it is removed from rotation.  
This example shows how load balancers protect system stability.

---

## Topic 1.6: Database

### About the Topic
A database is responsible for storing application data persistently. It supports operations such as creating, reading, updating, and deleting data. Databases ensure durability and consistency of information. As systems scale, databases often become performance bottlenecks. Understanding database behavior is crucial for designing scalable systems.

### Key Points
- Provides persistent data storage.
- Supports transactions and queries.
- Ensures data consistency and durability.
- Often a bottleneck at scale.
- Requires careful design and optimization.

### Example
An application stores user accounts and orders in a database.  
As traffic grows, the number of database queries increases.  
Query response times start to slow down.  
Engineers add indexes and optimize queries.  
This illustrates how database performance impacts overall systems.

---

## Topic 1.7: Database Sharding

### About the Topic
Database sharding divides data across multiple databases or nodes. Each shard holds a subset of the total data. This approach improves write scalability by spreading load. Sharding introduces complexity in query routing and data management. Choosing a proper shard key is critical for success.

### Key Points
- Splits data across multiple databases.
- Improves write scalability.
- Reduces load on a single database.
- Increases operational complexity.
- Requires careful shard key design.

### Example
A system with millions of users shards data by user ID ranges.  
Each shard stores a portion of the users.  
Write traffic is distributed evenly across shards.  
Queries are routed to the correct shard based on user ID.  
This example shows how sharding enables large-scale growth.

---

## Topic 1.8: Database Replication

### About the Topic
Database replication involves copying data from a primary database to replicas. The primary database handles write operations, while replicas handle read operations. This improves read performance and availability. Replication also supports failover scenarios. However, replication may introduce slight delays in data synchronization.

### Key Points
- Primary database handles writes.
- Replicas serve read requests.
- Improves read performance.
- Increases availability.
- Can introduce replication lag.

### Example
An online store writes orders to a primary database.  
Product browsing queries are served from replicas.  
If the primary database becomes unavailable, replicas still respond to reads.  
Users can continue browsing even during partial failures.  
This demonstrates how replication improves availability.

---

## Topic 1.9: Object Storage

### About the Topic
Object storage is designed to store unstructured data such as images, videos, and documents. It provides high durability and scalability. Objects are accessed using unique identifiers or URLs. Databases usually store references to objects rather than the objects themselves. This design keeps databases lightweight and efficient.

### Key Points
- Stores unstructured data.
- Highly scalable and durable.
- Accessed via URLs or object IDs.
- Reduces load on databases.
- Common in cloud environments.

### Example
A user uploads a profile photo to an application.  
The image is stored in an object storage service.  
The database saves only the image URL.  
When the profile page loads, the app fetches the image via the URL.  
This example shows how object storage complements databases.

---

## Topic 1.10: Cache

### About the Topic
Cache stores frequently accessed data in memory to reduce latency. It reduces the number of database queries required. Cached data is typically temporary and may expire. Proper cache design greatly improves performance. Cache invalidation strategies are an important consideration.

### Key Points
- Provides fast in-memory access.
- Reduces database load.
- Improves response time.
- Stores temporary data.
- Requires eviction and invalidation strategies.

### Example
An application caches popular product details in memory.  
User requests are served directly from the cache.  
Database load decreases significantly.  
When cache entries expire, data is reloaded from the database.  
This example highlights how caching boosts performance.

---

## Topic 1.11: CDN (Content Delivery Network)

### About the Topic
A CDN distributes static content across geographically distributed servers. Content is served from locations closer to users. This reduces latency and improves page load times. CDNs also offload traffic from origin servers. They are essential for globally distributed applications.

### Key Points
- Distributes content globally.
- Reduces latency for users.
- Serves static assets efficiently.
- Offloads backend servers.
- Improves user experience.

### Example
A website serves images through a CDN.  
Users in different countries access nearby CDN nodes.  
Page load times decrease significantly.  
Backend servers focus on dynamic API requests.  
This example shows how CDNs improve global performance.

---

## Topic 1.12: Monolithic Architecture

### About the Topic
Monolithic architecture packages all application features into a single codebase. It is easy to develop and deploy initially. As the system grows, scaling individual components becomes difficult. Failures can affect the entire application. Maintenance becomes challenging with increased complexity.

### Key Points
- Single codebase and deployment.
- Easy initial development.
- Tight coupling of components.
- Difficult to scale parts independently.
- High impact of failures.

### Example
An application contains login, orders, and payments in one service.  
Traffic spikes in the payment module.  
The entire application must be scaled.  
A bug in payments affects all features.  
This demonstrates limitations of monolithic design.

---

## Topic 1.13: Microservices Architecture

### About the Topic
Microservices architecture breaks applications into independent services. Each service owns a specific business capability. Services can be developed, deployed, and scaled independently. This improves fault isolation. However, it increases operational complexity.

### Key Points
- Independent services.
- Separate deployment and scaling.
- Better fault isolation.
- Faster development cycles.
- Higher operational overhead.

### Example
Order and payment services are deployed separately.  
A surge in payments requires scaling only the payment service.  
Order processing remains unaffected.  
Failures are isolated to one service.  
This example highlights the benefits of microservices.

---

## Topic 1.14: Message Queue

### About the Topic
Message queues enable asynchronous communication between services. Producers send messages without waiting for processing. Consumers process messages independently. This decouples services and improves reliability. Message queues help handle traffic spikes gracefully.

### Key Points
- Asynchronous communication.
- Decouples producers and consumers.
- Improves reliability.
- Handles traffic spikes.
- Enables event-driven systems.

### Example
An order service places an order and sends a message to a queue.  
The user receives an immediate confirmation.  
An email service consumes the message later.  
Order processing is not blocked by email sending.  
This example shows how queues improve responsiveness.

---

## Topic 1.15: API Gateway

### About the Topic
An API gateway acts as a single entry point for client requests. It handles cross-cutting concerns like authentication, authorization, and rate limiting. The gateway routes requests to appropriate backend services. This simplifies client logic. It also centralizes security and monitoring.

### Key Points
- Single entry point for clients.
- Centralized authentication and authorization.
- Rate limiting and request routing.
- Simplifies client-side logic.
- Improves observability and security.

### Example
A mobile app sends all requests to an API gateway.  
The gateway validates authentication tokens.  
It applies rate limits and routes requests to services.  
Backend services remain protected from direct exposure.  
This example shows how API gateways simplify system design.


# Section 2: System Qualities & Networking Fundamentals

## Topic 2.1: Scalability

### About the Topic
Scalability describes a system’s ability to handle growth in users, requests, or data without losing performance. It focuses on how efficiently a system can expand when demand increases. Scalability is a design-time concern, not something added after failures occur. Both software architecture and infrastructure choices influence scalability. Learning scalability helps engineers design systems that grow smoothly with business needs.

### Key Points
- Scalability addresses growth in traffic, users, and data volume.
- It depends on architecture, not just hardware upgrades.
- Horizontal and vertical strategies are commonly combined.
- Poor scalability leads to outages and degraded performance.
- Scalability planning is essential for long-term system success.

### Example
A startup application gains popularity and user traffic doubles every month.  
The original single-server setup starts to slow down under load.  
Engineers redesign the system to add more servers and distribute traffic.  
The system continues to respond quickly despite increased usage.  
This example shows why scalability must be planned early.

---

## Topic 2.2: Availability

### About the Topic
Availability measures how often a system is operational and accessible to users. High availability systems are designed to minimize downtime. This is achieved using redundancy, failover mechanisms, and monitoring. Availability is usually expressed as uptime percentage, such as 99.9%. It is critical for user trust and business continuity.

### Key Points
- Availability focuses on system uptime.
- Redundancy is a key technique to improve availability.
- Failover mechanisms reduce service disruption.
- Monitoring helps detect and recover from failures quickly.
- High availability is essential for user-facing systems.

### Example
An online shopping platform deploys multiple backend servers.  
When one server fails, traffic is routed to healthy servers.  
Users continue browsing and placing orders without interruption.  
Monitoring alerts engineers about the failed server.  
This demonstrates how availability is maintained through redundancy.

---

## Topic 2.3: Consistency

### About the Topic
Consistency refers to how up-to-date and accurate data is across a system. In distributed systems, maintaining perfect consistency can be difficult. Different consistency models exist to balance correctness and performance. The choice depends on business requirements. Understanding consistency helps in designing reliable data systems.

### Key Points
- Consistency ensures data correctness across the system.
- Distributed systems make consistency harder to achieve.
- Trade-offs exist between consistency and availability.
- Business requirements determine the consistency model.
- Consistency affects user experience and correctness.

### Example
A user updates their profile information in an application.  
Some users see the update immediately, while others see it later.  
The system synchronizes data across nodes over time.  
Eventually, all users see the same information.  
This behavior depends on the chosen consistency model.

---

## Topic 2.4: Strong Consistency

### About the Topic
Strong consistency guarantees that all users see the latest data immediately after a write. There are no stale reads in this model. It prioritizes correctness over performance and availability. Strong consistency is essential for systems where accuracy is critical. It is commonly used in financial and transactional systems.

### Key Points
- Guarantees immediate data accuracy.
- No stale or outdated reads allowed.
- Prioritizes correctness over performance.
- Harder to scale in distributed systems.
- Common in banking and payment systems.

### Example
A bank customer transfers money between accounts.  
The account balance is updated instantly across the system.  
Any user checking the balance sees the same updated value.  
No transaction is allowed with outdated data.  
This example shows why strong consistency is critical in banking.

---

## Topic 2.5: Eventual Consistency

### About the Topic
Eventual consistency allows data to be temporarily inconsistent across systems. Over time, all nodes converge to the same state. This model improves performance and scalability. Temporary delays in data updates are acceptable. Eventual consistency is widely used in large distributed systems.

### Key Points
- Allows temporary data inconsistency.
- Data converges over time.
- Improves scalability and performance.
- Suitable for non-critical data.
- Common in social media and feeds.

### Example
A user posts a message on a social network.  
The post appears immediately for the author.  
Followers see the post after a short delay.  
All systems eventually synchronize the data.  
This example illustrates eventual consistency in action.

---

## Topic 2.6: Fault Tolerance

### About the Topic
Fault tolerance is the ability of a system to continue operating when components fail. Failures are inevitable in real-world systems. Fault-tolerant design ensures graceful degradation instead of complete failure. Redundancy and isolation are key techniques. This quality improves system reliability.

### Key Points
- Systems expect and handle failures.
- Redundancy improves fault tolerance.
- Prevents complete system outages.
- Enables graceful degradation.
- Essential for distributed systems.

### Example
One backend server crashes during high traffic.  
Other servers continue handling requests.  
The failed server is replaced automatically.  
Users do not notice the failure.  
This example demonstrates fault-tolerant design.

---

## Topic 2.7: Single Point of Failure (SPOF)

### About the Topic
A single point of failure is a component whose failure can bring down the entire system. SPOFs reduce reliability and availability. Identifying and eliminating SPOFs is a major design goal. Redundancy is the primary solution. Modern systems aim to avoid SPOFs completely.

### Key Points
- One failure can cause total outage.
- Common in early system designs.
- Reduces reliability and availability.
- Eliminated through redundancy.
- Critical consideration in system design.

### Example
A system relies on a single database instance.  
When the database crashes, the entire application goes down.  
Engineers add a replica database.  
The system continues operating during failures.  
This example shows how removing SPOFs improves reliability.

---

## Topic 2.8: IP Address

### About the Topic
An IP address uniquely identifies a device on a network. It enables devices to locate and communicate with each other. IP addresses are fundamental to internet communication. Both IPv4 and IPv6 formats are used today. Servers rely on IP addresses to receive requests.

### Key Points
- Unique identifier for network devices.
- Required for routing traffic.
- IPv4 and IPv6 standards exist.
- Used by all internet services.
- Essential for client–server communication.

### Example
A server is assigned a public IP address.  
Clients send requests to that IP.  
Routers use the IP to route traffic correctly.  
The server receives and processes requests.  
This example shows how IP addressing enables communication.

---

## Topic 2.9: DNS (Domain Name System)

### About the Topic
DNS translates human-readable domain names into IP addresses. It allows users to access services using simple names instead of numbers. DNS is a globally distributed system. Caching improves DNS performance. It is a critical component of the internet.

### Key Points
- Maps domain names to IP addresses.
- Improves usability of the internet.
- Distributed and fault-tolerant system.
- Uses caching for performance.
- Essential for web access.

### Example
A user types a website URL into a browser.  
DNS resolves the domain name to an IP address.  
The browser connects to the server using that IP.  
The server responds with the requested content.  
This example shows how DNS enables simple web access.

---

## Topic 2.10: Relationship Between Client and Server

### About the Topic
The relationship between client and server is based on request–response communication. The client initiates requests, and the server responds. Modern systems often keep this interaction stateless. This design improves scalability and reliability. Understanding this relationship is key to designing APIs.

### Key Points
- Client initiates communication.
- Server processes and responds.
- Often stateless in modern systems.
- Improves scalability.
- Forms the basis of web APIs.

### Example
A client sends an HTTP request for data.  
The server processes the request independently.  
A response is returned to the client.  
The connection is closed after the response.  
This example demonstrates the basic client–server interaction.


# Section 3: Communication Protocols & Proxies

## Topic 3.1: TCP (Transmission Control Protocol)

### About the Topic
TCP is a connection-oriented communication protocol designed to provide reliable data transfer. It ensures that data packets are delivered in order and without loss. TCP handles retransmission of lost packets and manages congestion control automatically. This protocol prioritizes correctness and reliability over speed. TCP is widely used in applications where data integrity is critical.

### Key Points
- Provides reliable and ordered data delivery.
- Retransmits lost packets automatically.
- Uses connection-oriented communication.
- Slower compared to lightweight protocols like UDP.
- Commonly used for web traffic, file transfers, and databases.

### Example
A user downloads a file from a server over the internet.  
The file is broken into packets and sent using TCP.  
If some packets are lost, TCP detects and retransmits them.  
Packets are reassembled in the correct order at the destination.  
This ensures the downloaded file is complete and accurate.

---

## Topic 3.2: UDP (User Datagram Protocol)

### About the Topic
UDP is a connectionless communication protocol focused on speed and low latency. It does not guarantee delivery, ordering, or retransmission of packets. UDP is suitable for real-time applications where speed matters more than accuracy. Because of its simplicity, it has lower overhead than TCP. Some data loss is acceptable in UDP-based systems.

### Key Points
- Connectionless protocol.
- No guarantee of delivery or order.
- Very low latency.
- Minimal protocol overhead.
- Ideal for real-time communication.

### Example
A live video streaming service uses UDP to send video data.  
Packets are sent continuously without waiting for acknowledgments.  
Some packets may be lost during transmission.  
The video continues playing smoothly despite minor losses.  
This shows why UDP is preferred for real-time streaming.

---

## Topic 3.3: HTTP (Hypertext Transfer Protocol)

### About the Topic
HTTP is a stateless, request–response protocol used for web communication. Each HTTP request is independent of others. The protocol is simple and widely supported across platforms. HTTP forms the foundation of REST APIs. Its stateless nature makes systems easier to scale.

### Key Points
- Stateless protocol.
- Uses request–response model.
- Widely supported on the web.
- Foundation for REST APIs.
- Simple and extensible.

### Example
A browser sends an HTTP GET request for a webpage.  
The server processes the request and returns HTML content.  
The connection is closed after the response.  
A new request is required for the next resource.  
This demonstrates HTTP’s stateless behavior.

---

## Topic 3.4: WebSocket

### About the Topic
WebSocket is a protocol that enables persistent, bidirectional communication between client and server. Unlike HTTP, the connection remains open. This allows real-time data exchange with low latency. WebSockets are ideal for applications requiring instant updates. They reduce the overhead of repeated HTTP requests.

### Key Points
- Persistent connection.
- Bidirectional communication.
- Low latency.
- Real-time data exchange.
- Efficient for frequent updates.

### Example
A chat application establishes a WebSocket connection.  
The connection remains open between client and server.  
Messages are pushed instantly to connected users.  
No repeated HTTP polling is required.  
This enables real-time chat functionality.

---

## Topic 3.5: Forward Proxy

### About the Topic
A forward proxy sits between clients and external servers. It hides client identities from the internet. Forward proxies control outbound traffic and enforce policies. They are commonly used in corporate environments. Forward proxies can also cache responses to improve performance.

### Key Points
- Acts on behalf of clients.
- Hides client identity.
- Controls outbound traffic.
- Improves security and compliance.
- Can cache responses.

### Example
An employee accesses a website from a company network.  
The request goes through a forward proxy.  
The proxy checks access policies before forwarding the request.  
The response is returned through the proxy.  
This shows how forward proxies control client traffic.

---

## Topic 3.6: Reverse Proxy

### About the Topic
A reverse proxy sits between clients and backend servers. It hides server details from clients. Reverse proxies handle incoming traffic and distribute it to servers. They improve security, performance, and scalability. Reverse proxies are widely used with load balancers.

### Key Points
- Acts on behalf of servers.
- Hides backend infrastructure.
- Handles inbound traffic.
- Improves security and performance.
- Often combined with load balancing.

### Example
A user accesses a website through a reverse proxy.  
The proxy receives the request first.  
It forwards the request to an appropriate backend server.  
The server response is sent back through the proxy.  
This protects backend servers from direct exposure.


# Section 4: Communication Patterns & APIs

## Topic 4.1: REST API

### About the Topic
REST (Representational State Transfer) is an architectural style for designing networked applications using HTTP. It models the system as resources identified by URLs and manipulated using standard HTTP methods. REST emphasizes stateless communication, which improves scalability and simplicity. Because it builds on HTTP, REST is easy to integrate across platforms and languages. Understanding REST is essential since it is the most widely used API style today.

### Key Points
- Uses HTTP methods such as GET, POST, PUT, and DELETE.
- Treats data as resources identified by URLs.
- Enforces stateless request–response communication.
- Simple to understand and widely supported.
- Scales well due to its stateless nature.

### Example
A client sends a GET request to `/users/123` to fetch user data.  
The server processes the request independently of previous calls.  
It retrieves the user record and returns JSON data.  
The connection is closed after the response is sent.  
This example shows how REST handles resources in a stateless way.

---

## Topic 4.2: GraphQL

### About the Topic
GraphQL is a query language and runtime for APIs that allows clients to request exactly the data they need. Unlike REST, it uses a single endpoint for all queries. This design avoids over-fetching and under-fetching of data. GraphQL shifts control of the response shape to the client. It is especially useful for complex frontends with varying data needs.

### Key Points
- Client specifies exactly what data it wants.
- Uses a single endpoint for all operations.
- Avoids over-fetching and under-fetching.
- Strongly typed schema defines available data.
- Well suited for complex frontend applications.

### Example
A mobile app needs only a user’s name and email.  
The client sends a GraphQL query specifying those fields.  
The server returns only the requested data.  
No unnecessary fields are included in the response.  
This example shows how GraphQL optimizes data transfer.

---

## Topic 4.3: gRPC

### About the Topic
gRPC is a high-performance remote procedure call framework developed by Google. It uses HTTP/2 and binary serialization for efficiency. gRPC is strongly typed and relies on interface definitions. It is designed for service-to-service communication. gRPC is commonly used in microservices architectures.

### Key Points
- Uses HTTP/2 for transport.
- Employs binary serialization for speed.
- Strongly typed API contracts.
- Ideal for internal service communication.
- Lower latency than text-based APIs.

### Example
Service A needs data from Service B.  
A gRPC client stub is generated from a shared definition.  
Service A calls a method as if it were local.  
Data is sent efficiently in binary form.  
This example highlights gRPC’s performance benefits.

---

## Topic 4.4: Message Queues (As a Communication Pattern)

### About the Topic
Message queues enable asynchronous communication between systems. Producers send messages to a queue without waiting for processing. Consumers process messages independently and at their own pace. This pattern decouples services and improves resilience. It is widely used in event-driven architectures.

### Key Points
- Enables asynchronous communication.
- Decouples producers and consumers.
- Improves system resilience.
- Handles traffic spikes effectively.
- Supports event-driven workflows.

### Example
An order service publishes an order-created event to a queue.  
The user receives an immediate response confirming the order.  
An email service consumes the message later.  
Order processing is not blocked by email delivery.  
This example shows how queues improve responsiveness.


# Section 5: Database & Storage Systems

## Topic 5.1: Relational (SQL) Database

### About the Topic
Relational databases store data in structured tables with predefined schemas. They enforce relationships between tables using keys and constraints. SQL databases support ACID transactions to guarantee correctness. They are well suited for systems where data integrity is critical. Understanding SQL databases is essential for designing reliable transactional systems.

### Key Points
- Data is stored in tables with fixed schemas.
- Supports ACID transactions for reliability.
- Enforces relationships using primary and foreign keys.
- Strong consistency is guaranteed.
- Commonly used in financial and transactional systems.

### Example
A banking system stores accounts and transactions in tables.  
Each transaction updates balances atomically.  
If an error occurs, the transaction is rolled back.  
Data remains consistent across all tables.  
This example shows why SQL databases are trusted for financial systems.

---

## Topic 5.2: Non-Relational (NoSQL) Database

### About the Topic
NoSQL databases are designed for flexibility and scalability. They allow schema-less or semi-structured data models. These databases scale horizontally with ease. They often trade strong consistency for availability and performance. NoSQL systems are widely used in large distributed applications.

### Key Points
- Flexible or schema-less data models.
- Designed for horizontal scaling.
- High performance for large datasets.
- Often uses eventual consistency.
- Suitable for distributed systems.

### Example
A social media platform stores posts in a NoSQL database.  
Each post can have different attributes.  
The database scales across many servers.  
Temporary data inconsistency is acceptable.  
This example highlights NoSQL’s flexibility and scalability.

---

## Topic 5.3: SQL vs NoSQL

### About the Topic
SQL and NoSQL databases solve different problems. SQL focuses on strong consistency and structured data. NoSQL emphasizes scalability and flexibility. The choice depends on system requirements and access patterns. Understanding the trade-offs helps in selecting the right database.

### Key Points
- SQL provides strong consistency.
- NoSQL prioritizes scalability.
- Schema rigidity vs flexibility.
- Different consistency models.
- Choice depends on use case.

### Example
A payment system uses an SQL database for accuracy.  
A recommendation engine uses a NoSQL database for scale.  
Each system has different requirements.  
Using the wrong database would cause issues.  
This example shows how trade-offs guide decisions.

---

## Topic 5.4: Object Storage

### About the Topic
Object storage is optimized for storing large unstructured data. It stores data as objects rather than rows or blocks. Object storage scales independently from databases. It provides high durability at low cost. Databases usually store references to objects instead of the data itself.

### Key Points
- Stores unstructured data like images and videos.
- Highly scalable and durable.
- Cost-effective for large files.
- Accessed via URLs or object IDs.
- Keeps databases lightweight.

### Example
A video-sharing app stores uploaded videos in object storage.  
The database saves only metadata and file URLs.  
Videos are streamed directly from storage.  
Database load stays minimal.  
This example shows efficient separation of concerns.

---

## Topic 5.5: Cache

### About the Topic
Cache is a fast in-memory storage layer used to reduce latency. It stores frequently accessed data temporarily. Caching reduces repeated database queries. Proper cache design improves performance significantly. Cache invalidation is one of the hardest challenges.

### Key Points
- In-memory data storage.
- Reduces database load.
- Improves response time.
- Data is temporary.
- Requires eviction strategies.

### Example
An application caches user session data.  
Repeated requests read from cache.  
Database load decreases significantly.  
Expired sessions are removed automatically.  
This example shows how cache boosts performance.

---

## Topic 5.6: CDN (Content Delivery Network)

### About the Topic
A CDN distributes static content across multiple geographic locations. Content is served from the nearest location to the user. This reduces latency and improves load times. CDNs offload traffic from origin servers. They are critical for global-scale applications.

### Key Points
- Global content distribution.
- Reduced latency for users.
- Serves static assets efficiently.
- Offloads backend servers.
- Improves overall user experience.

### Example
A website hosts images using a CDN.  
Users in different countries access nearby CDN nodes.  
Images load faster regardless of location.  
Origin servers handle fewer requests.  
This example shows how CDNs improve global performance.


# Section 6: Caching Strategies

## Topic 6.1: Types of Caching

### About the Topic
Caching refers to storing data temporarily in a faster storage layer to improve performance. Different types of caching exist based on where the cache is placed in the system. Common types include client-side caching, server-side caching, and distributed caching. Each type serves a specific purpose and has different trade-offs. Understanding types of caching helps in choosing the right strategy for a given system.

### Key Points
- Caching can exist at client, server, or network level.
- Different cache types serve different performance goals.
- Placement of cache affects latency and consistency.
- Multiple cache layers can coexist in one system.
- Choosing the wrong cache type can cause stale data issues.

### Example
A browser caches static assets like images and CSS files.  
The backend server also caches frequently requested API responses.  
A distributed cache like Redis is used across services.  
Each cache layer reduces load on the layer below it.  
This example shows how multiple caching types work together.

---

## Topic 6.2: Read-Aside Cache Strategy

### About the Topic
Read-aside caching is one of the most commonly used cache strategies. In this approach, the application checks the cache before querying the database. If the data is not found in the cache, the application retrieves it from the database and updates the cache. This strategy is simple and flexible. It gives the application full control over cache behavior.

### Key Points
- Application checks cache first.
- Database is queried only on cache miss.
- Cache is populated lazily.
- Simple to implement and manage.
- Risk of stale data if not invalidated properly.

### Example
A request for product details arrives at the application.  
The application checks the cache for the product data.  
If the cache is empty, the database is queried.  
The retrieved data is stored in the cache.  
Future requests are served quickly from the cache.

---

## Topic 6.3: Write-Through Cache Strategy

### About the Topic
Write-through caching updates the cache and database simultaneously on write operations. This ensures that the cache always contains the latest data. It simplifies read logic because reads can always rely on cache. However, write operations become slower due to multiple writes. This strategy favors consistency over write performance.

### Key Points
- Writes go to cache and database together.
- Cache always stays consistent with database.
- Simplifies read operations.
- Slower write performance.
- Suitable for read-heavy workloads.

### Example
A user updates their profile information.  
The application writes the update to the cache first.  
The same update is written to the database.  
Subsequent reads fetch updated data from cache.  
This example shows how write-through ensures consistency.

---

## Topic 6.4: Read & Write Cache Strategies (Combined)

### About the Topic
Real-world systems often combine multiple caching strategies. Read-heavy systems favor read-aside caching. Write-heavy systems may use write-through or write-behind strategies. The choice depends on access patterns and consistency needs. Combining strategies allows systems to balance performance and correctness.

### Key Points
- Strategy choice depends on read/write patterns.
- Read-heavy systems prefer read-aside.
- Write-heavy systems need careful write handling.
- Consistency requirements influence strategy.
- Hybrid approaches are common in production.

### Example
A product catalog service uses read-aside caching for reads.  
User profile updates use write-through caching.  
Different access patterns are optimized separately.  
Cache performance improves overall system speed.  
This example shows practical hybrid caching usage.


# Section 7: Utilities & Advanced Concepts

## Topic 7.1: Cloud Computing

### About the Topic
Cloud computing provides on-demand access to computing resources such as servers, storage, and networking over the internet. It allows teams to provision and release resources quickly without managing physical hardware. Cloud platforms support scalability, high availability, and global distribution by design. Pay-as-you-go pricing helps control costs as usage changes. Understanding cloud fundamentals is essential for building modern, scalable systems.

### Key Points
- Resources are provisioned on demand over the internet.
- Supports rapid scaling without hardware management.
- Enables high availability through managed services.
- Pay-as-you-go pricing optimizes cost.
- Forms the foundation of modern system architectures.

### Example
A startup deploys its application on a cloud platform.  
Traffic increases during a marketing campaign.  
Auto-scaling provisions additional servers automatically.  
When traffic drops, extra servers are released.  
This example shows how cloud computing enables elastic scaling.

---

## Topic 7.2: Logging & Monitoring

### About the Topic
Logging and monitoring provide visibility into system behavior and health. Logs record events, errors, and important actions in the system. Monitoring tracks metrics such as latency, error rates, and resource usage. Together, they help detect issues early and diagnose failures. These practices are critical for operating reliable production systems.

### Key Points
- Logs capture detailed system events.
- Monitoring tracks system metrics and trends.
- Alerts notify teams of abnormal behavior.
- Essential for debugging and root cause analysis.
- Improves reliability and operational confidence.

### Example
An application starts returning errors for some users.  
Error logs capture stack traces and request details.  
Monitoring dashboards show a spike in latency.  
Alerts notify engineers immediately.  
This example shows how logging and monitoring speed up recovery.

---

## Topic 7.3: Hashing

### About the Topic
Hashing converts input data into a fixed-size output using a hash function. The same input always produces the same hash, but the original input cannot be easily recovered. Hashing is widely used for fast lookups, data integrity checks, and security. It plays a key role in password storage and distributed systems. Understanding hashing helps in designing efficient and secure systems.

### Key Points
- Converts data into fixed-size values.
- Deterministic but one-way.
- Enables fast data lookup.
- Used for security and integrity checks.
- Fundamental in distributed system design.

### Example
A user creates an account with a password.  
The system hashes the password before storing it.  
The original password is never saved directly.  
During login, the entered password is hashed again.  
This example shows how hashing improves security.

---

## Topic 7.4: Consistent Hashing

### About the Topic
Consistent hashing is a technique used to distribute data across nodes in a scalable way. It minimizes data movement when nodes are added or removed. This property is especially useful for distributed caches and load balancing. Consistent hashing improves stability during scaling events. It is a core concept in distributed system design.

### Key Points
- Distributes keys evenly across nodes.
- Minimizes data reassignment during scaling.
- Improves cache efficiency.
- Commonly used in distributed systems.
- Supports dynamic addition and removal of nodes.

### Example
A distributed cache uses consistent hashing to assign keys.  
A new cache node is added to the cluster.  
Only a small subset of keys is reassigned.  
Most cached data remains in place.  
This example shows why consistent hashing scales well.

---

## Topic 7.5: CAP Theorem

### About the Topic
The CAP theorem states that a distributed system can guarantee only two of consistency, availability, and partition tolerance at the same time. Network partitions are unavoidable in distributed systems. Designers must choose between consistency and availability during partitions. CAP helps explain trade-offs in real-world system behavior. It is a guiding principle for distributed architecture decisions.

### Key Points
- Only two of consistency, availability, and partition tolerance are possible.
- Partition tolerance is mandatory in distributed systems.
- Trade-offs are unavoidable.
- Guides database and system design.
- Explains real-world system behavior.

### Example
A banking system prioritizes consistency over availability.  
During a network partition, some requests are rejected.  
Data correctness is always maintained.  
A social feed system chooses availability instead.  
This example illustrates CAP trade-offs.

---

## Topic 7.6: Database Indexing

### About the Topic
Database indexing improves query performance by creating optimized lookup structures. Indexes allow databases to find data faster without scanning entire tables. While indexes speed up reads, they add overhead to write operations. Choosing the right indexes is crucial for performance. Indexing is a key optimization technique in database design.

### Key Points
- Speeds up read queries.
- Uses additional storage.
- Adds overhead to writes.
- Requires careful selection.
- Critical for database performance.

### Example
A user table stores millions of records.  
Queries frequently filter by email address.  
An index is added on the email column.  
Query response time improves dramatically.  
This example shows how indexing optimizes performance.

---

## Topic 7.7: Capacity Estimation

### About the Topic
Capacity estimation predicts the resources needed to support expected load. It considers traffic volume, data size, and growth patterns. Proper estimation prevents under-provisioning and outages. It also helps control infrastructure costs. Capacity planning is an essential step in system design and interviews.

### Key Points
- Predicts traffic and resource needs.
- Prevents system overload.
- Helps control costs.
- Performed during design phase.
- Critical for scalable systems.

### Example
Engineers estimate daily active users for a new service.  
They calculate expected requests per second.  
Servers are provisioned based on this estimate.  
The system handles launch traffic smoothly.  
This example shows the value of capacity planning.

---

## Topic 7.8: Event Streaming

### About the Topic
Event streaming processes continuous streams of events in real time. Producers emit events as they occur. Consumers process events independently and asynchronously. This model supports real-time analytics and reactive systems. Event streaming is common in modern data-driven architectures.

### Key Points:
- Processes events in real time.
- Supports asynchronous communication.
- Enables reactive system design.
- Scales well with high event volume.
- Commonly used for analytics and monitoring.

### Example:
User actions are published as events to a stream.  
Analytics services consume these events in real time.  
Dashboards update instantly with new data.  
Other services react to the same events independently.  
This example shows how event streaming enables real-time insights.
