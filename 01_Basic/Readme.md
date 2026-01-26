# System Design – Feature Learning (Zero to Hero)

This document is written for deep, feature-level learning of system design.
Each concept follows a strict structure:
- About the Topic (minimum 5 lines)
- Key Points (minimum 5 bullets)
- Example (minimum 5 lines)

---

## SECTION 1 – CORE SYSTEM COMPONENTS

### Client–Server
**About the Topic**  
The client–server model is the foundation of most modern software systems. The client is responsible for user interaction and presentation, while the server handles business logic, validation, security, and database access. This separation improves maintainability by keeping responsibilities clear. It also enhances security by preventing direct access to sensitive resources. Clients and servers can scale independently as usage grows.

**Key Points**
- Client handles UI and user input  
- Server manages business logic and data  
- Clear separation of responsibilities  
- Centralized security and validation  
- Independent scalability  

**Example**  
A mobile app sends a request to fetch user profile data.  
The backend server validates the authentication token.  
It retrieves user data from the database.  
The response is formatted and sent back to the client.  
The database is never accessed directly by the client.

---

### Scaling
**About the Topic**  
Scaling is the ability of a system to handle increased traffic, users, or data without performance degradation. Systems that do not scale properly fail under growth. Scaling must be planned during design, not after failures occur. It directly affects availability and performance. Proper scaling enables long-term system success.

**Key Points**
- Handles growth in users and traffic  
- Prevents performance bottlenecks  
- Planned during design phase  
- Impacts availability  
- Essential for long-term systems  

**Example**  
An application starts with a small number of users.  
As traffic increases, response times slow down.  
Engineers analyze system load.  
Scaling strategies are applied.  
The system continues working smoothly under growth.

---

### Vertical Scaling (Scale Up)
**About the Topic**  
Vertical scaling increases the capacity of a single machine by upgrading CPU, RAM, or storage. It is easy to implement because no architectural changes are required. However, hardware limitations restrict growth. Vertical scaling introduces a single point of failure. It is usually a short-term solution.

**Key Points**
- Upgrades a single server  
- Simple to implement  
- Limited by hardware  
- Expensive at scale  
- Single point of failure  

**Example**  
A backend server runs with 8GB RAM.  
Traffic increases and performance degrades.  
The server is upgraded to 32GB RAM.  
Performance improves temporarily.  
Further growth requires a new approach.

---

### Horizontal Scaling (Scale Out)
**About the Topic**  
Horizontal scaling increases capacity by adding more machines instead of upgrading one. Load is distributed across servers to prevent overload. This improves fault tolerance and availability. Horizontal scaling supports large-scale systems. It is widely used in cloud environments.

**Key Points**
- Adds more servers  
- Improves fault tolerance  
- Handles high traffic  
- Requires load balancing  
- Cloud-friendly approach  

**Example**  
An e-commerce platform adds multiple backend servers.  
A load balancer distributes incoming requests.  
One server fails during peak traffic.  
Other servers continue serving users.  
No downtime is experienced.

---

### Load Balancer
**About the Topic**  
A load balancer distributes incoming requests across backend servers. It prevents any single server from becoming overloaded. Load balancers monitor server health. Unhealthy servers are removed automatically. This improves system reliability and availability.

**Key Points**
- Distributes traffic evenly  
- Prevents server overload  
- Improves availability  
- Enables horizontal scaling  
- Performs health checks  

**Example**  
Thousands of users access a website simultaneously.  
Requests first reach the load balancer.  
Traffic is routed to healthy servers.  
One server becomes unhealthy and is removed.  
Users continue using the site without issues.

---

### Database
**About the Topic**  
A database stores application data persistently. It supports create, read, update, and delete operations. Databases ensure consistency and durability of data. They often become performance bottlenecks at scale. Proper design and scaling are critical.

**Key Points**
- Persistent data storage  
- Supports transactions  
- Ensures consistency  
- Performance sensitive  
- Requires scaling strategies  

**Example**  
An application stores users and orders in a database.  
Traffic increases and queries slow down.  
Indexes and replicas are added.  
Performance improves significantly.  
The system remains reliable.

---

### Database Sharding
**About the Topic**  
Database sharding splits data across multiple databases. Each shard stores a subset of data. This improves write scalability. It reduces load on individual databases. Sharding increases operational complexity.

**Key Points**
- Splits data across shards  
- Improves write scalability  
- Reduces single DB load  
- Requires shard key design  
- Adds complexity  

**Example**  
User data is split by user ID range.  
Writes are distributed across shards.  
No single database is overloaded.  
Queries route to correct shard.  
The system scales efficiently.

---

### Database Replication
**About the Topic**  
Database replication copies data from a primary database to replicas. Writes go to the primary database. Reads are served from replicas. This improves read performance and availability. Replication may introduce slight data lag.

**Key Points**
- Primary handles writes  
- Replicas handle reads  
- Improves availability  
- Supports failover  
- Possible replication lag  

**Example**  
Orders are written to the primary database.  
Product browsing uses replicas.  
Primary database fails temporarily.  
Replicas continue serving reads.  
Users can still browse products.

---

### Object Storage
**About the Topic**  
Object storage stores unstructured data such as images and videos. It is optimized for scalability and durability. Files are accessed using URLs. Databases store only references. This keeps databases efficient.

**Key Points**
- Stores unstructured data  
- Highly scalable  
- Cost-effective  
- URL-based access  
- Database stores references  

**Example**  
A user uploads a profile image.  
The image is stored in object storage.  
The database saves the image URL.  
The app loads the image from storage.  
Database load remains low.

---

### Cache
**About the Topic**  
Cache stores frequently accessed data in memory. It significantly improves response time. Cache reduces database load. Cached data is temporary and may expire. Eviction policies manage memory usage.

**Key Points**
- Fast access  
- Reduces DB load  
- Temporary storage  
- Improves performance  
- Uses eviction policies  

**Example**  
Popular product details are cached.  
Requests are served instantly.  
Cache expires after some time.  
Data reloads from database.  
Cache is refreshed automatically.

---

### CDN
**About the Topic**  
A CDN distributes static content globally. Content is served from locations close to users. This reduces latency and page load time. CDNs offload backend servers. They improve global performance.

**Key Points**
- Global content delivery  
- Reduced latency  
- Static file optimization  
- Backend offloading  
- High availability  

**Example**  
Images are served from nearby CDN nodes.  
Users experience faster load times.  
Backend servers handle APIs only.  
Traffic is reduced on origin servers.  
Performance improves worldwide.

---

### Monolithic Architecture
**About the Topic**  
Monolithic architecture builds all features into a single application. It is easy to start development. Scaling individual components is difficult. Failures affect the entire system. Maintenance becomes harder as complexity grows.

**Key Points**
- Single codebase  
- Easy initial development  
- Tight coupling  
- Hard to scale parts  
- High failure impact  

**Example**  
One application handles login, orders, and payments.  
Payment traffic increases suddenly.  
Entire application must be scaled.  
A payment bug crashes everything.  
Deployments become risky.

---

### Microservices Architecture
**About the Topic**  
Microservices architecture splits an application into independent services. Each service handles a specific business capability. Services can scale independently. Fault isolation improves reliability. Operational complexity increases.

**Key Points**
- Independent services  
- Separate scaling  
- Fault isolation  
- Faster deployments  
- Higher operational overhead  

**Example**  
Order and payment services run separately.  
Payment traffic spikes independently.  
Only payment service scales.  
Order service remains stable.  
System resilience improves.

---

### Message Queue
**About the Topic**  
Message queues enable asynchronous communication between services. Producers send messages without waiting for processing. Consumers process messages independently. This decouples systems. Queues absorb traffic spikes.

**Key Points**
- Asynchronous processing  
- Decouples services  
- Handles spikes  
- Improves reliability  
- Event-driven design  

**Example**  
Order service sends a message to a queue.  
User gets instant confirmation.  
Email service processes later.  
System remains responsive.  
Failures do not cascade.

---

### API Gateway
**About the Topic**  
An API Gateway acts as a single entry point for client requests. It handles authentication and authorization. Rate limiting is enforced centrally. Requests are routed to backend services. Client complexity is reduced.

**Key Points**
- Single entry point  
- Centralized security  
- Rate limiting  
- Request routing  
- Simplifies clients  

**Example**  
Mobile app sends requests to gateway.  
Gateway validates authentication tokens.  
Rate limits are applied.  
Requests are routed correctly.  
Backend services remain protected.

---

## END OF DOCUMENT
