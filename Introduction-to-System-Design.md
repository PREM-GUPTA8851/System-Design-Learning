# Introduction to System Design

> **Focus:** System Design fundamentals, architecture components, scaling, and application types

---

## 1. Introduction to System Design

### What is System Design?

System Design is the process of defining the architecture, components, and interactions of a system to satisfy specified requirements.

A software system consists of multiple components that work together to achieve a common goal.

**Core Concept:**

`System = Components + Common Goal`

For example, an online banking application contains a client application, backend server, database, and other components that work together to process transactions.

### Why Do We Need System Design?

System Design helps us build applications that can:

- Handle increasing numbers of users.
- Process requests efficiently.
- Store and retrieve data reliably.
- Maintain good performance as demand grows.
- Handle failures and distribute workloads.
- Remain maintainable as the application becomes more complex.

---

## 2. Understanding System Design Through a Banking Example

Consider a fictional bank called **AlienBank** with a cash counter.

### Components of the System

- Customer
- Cash counter
- Cashier
- Receipt system
- Database

### Customer Flow

1. A customer arrives at the counter.
2. The customer requests a withdrawal or deposit.
3. The cashier processes the request.
4. The receipt system generates a receipt.
5. The transaction is completed.

This simple example helps explain how System Design concepts solve real-world problems.

### Problem 1: Slow Processing

**Problem:** Each customer takes approximately 10 minutes because of manual work.

**Possible solutions:**

- Improve the cashier's workflow.
- Use better tools.
- Reduce unnecessary steps.

**System Design mapping:**

- Code optimization
- Better algorithms and data structures
- Low-Level Design (LLD)

Improving code quality and choosing efficient algorithms can reduce processing time. LLD focuses on the internal design of software components, classes, and their interactions.

### Problem 2: Increasing Customers

**Problem:** A single counter cannot handle increasing demand efficiently.

**Solution:** Upgrade the resources available to the existing counter.

**System Design mapping: Vertical Scaling**

Vertical scaling means increasing the resources of a single server, such as CPU, RAM, or storage.

### Problem 3: High Waiting Time

**Problem:** A single counter becomes a bottleneck during peak hours.

**Solution:** Add more counters and cashiers.

**System Design mapping: Horizontal Scaling**

Horizontal scaling means adding more servers or instances to distribute the workload.

### Problem 4: Data Inconsistency

**Problem:** Different counters maintain separate records, resulting in discrepancies.

**Solution:** Maintain a centralized database as a shared source of truth.

A centralized database can help different application servers access shared data. However, correct transaction handling and concurrency control are also important for consistency.

### Problem 5: Uneven Workload

**Problem:** Some counters are overloaded while others remain idle.

**Solution:** Introduce a system that distributes customers among available counters.

**System Design mapping: Load Balancer**

A load balancer distributes incoming requests across multiple servers and can use health checks to avoid sending traffic to unhealthy servers.

### Key Takeaways

- Code optimization improves internal processing efficiency.
- Vertical scaling upgrades a single server.
- Horizontal scaling adds more servers.
- A centralized database provides shared data storage.
- A load balancer distributes incoming traffic.

---

## 3. Core Components of System Design

Modern applications such as WhatsApp, YouTube, LinkedIn, and Instagram store, process, and deliver different types of data.

A typical application may use the following components.

### 3.1 Database

A database is the persistent storage layer of an application.

It stores and retrieves information such as user details, messages, orders, and transactions.

**Major database categories:**

**SQL — Relational Databases**

- Organize data primarily into tables containing rows and columns.
- Generally use structured schemas and relationships.
- Examples: MySQL and PostgreSQL.

**NoSQL — Non-relational Databases**

- Support different data models, including documents, key-value pairs, and graphs.
- Their data models and schema flexibility depend on the database.
- Examples: MongoDB and Cassandra.

**Important:** The appropriate database depends on the application's data model, query patterns, consistency requirements, and scalability needs.

### 3.2 Application Code — Backend / Server

The backend contains the application logic and acts as an intermediary between client applications and databases.

Its responsibilities include:

- Processing requests.
- Applying business rules.
- Validating user input.
- Reading and writing database records.
- Returning responses through APIs.
- Hiding database implementation details from clients.

Backend applications commonly perform CRUD operations:

| Operation | Meaning                |
| --------- | ---------------------- |
| Create    | Add new data           |
| Read      | Retrieve existing data |
| Update    | Modify existing data   |
| Delete    | Remove data            |

### APIs and Endpoints

An API (Application Programming Interface) defines how software components communicate.

An endpoint is a specific API address through which a client accesses a particular operation.

Examples:

| HTTP Method | Example Endpoint | Purpose                |
| ----------- | ---------------- | ---------------------- |
| GET         | `/users/123`     | Retrieve a user's data |
| POST        | `/orders`        | Create an order        |
| PUT         | `/users/123`     | Update a user resource |
| DELETE      | `/posts/123`     | Delete a post          |

The HTTP method and endpoint together help define the requested operation. Actual API behavior depends on its implementation.

### JSON Data Format

JSON (JavaScript Object Notation) is a lightweight format commonly used to exchange data between clients and servers.

Example:

```json
{
  "id": 123,
  "name": "Prem",
  "role": "Student"
}
```

### How Does the Backend Communicate With Other Components?

Backend services communicate with databases and other services over networks using suitable protocols and database drivers.

Depending on the connection, technologies can include TCP, HTTP/HTTPS, and database-specific protocols.

---

### 3.3 Client Application — Frontend

The client application is the interface through which users interact with the system.

**Examples:**

- Web applications
- Mobile applications
- Desktop applications

**Responsibilities:**

- Display information to users.
- Collect user input.
- Send requests to backend APIs.
- Receive and display server responses.

For example, when a user opens a profile page, the frontend can request profile information from the backend and display the returned data.

### Basic Request-Response Flow

1. The user performs an action in the client application.
2. The client sends a request to an API.
3. The backend processes the request.
4. The backend may retrieve or update data in a database.
5. The backend sends a response.
6. The client displays the result.

---

### 3.4 Cache

A cache stores frequently accessed data in a faster-access storage layer.

**Why is caching useful?**

- Reduces repeated database queries.
- Can improve response time.
- Reduces the workload on the database.
- Helps applications handle repeated requests efficiently.

**Example:**

Suppose thousands of users request the same homepage feed. If suitable data is cached, the application may serve repeated requests from the cache rather than querying the database every time.

**Example technology:** Redis.

Caching requires appropriate expiration and invalidation strategies because cached data can become outdated.

---

### 3.5 Load Balancer

A load balancer distributes incoming requests across multiple backend servers.

**Responsibilities:**

- Distribute traffic.
- Prevent a single server from receiving all requests.
- Perform health checks, depending on its configuration.
- Stop routing traffic to servers identified as unhealthy.
- Improve scalability and availability when properly configured.

**Example:**

If three healthy servers are available, a load balancer can distribute incoming requests among them according to its routing algorithm and configuration.

A load balancer does not necessarily divide every request equally; its behavior depends on the chosen algorithm and server health.

---

### 3.6 Message Queue

A message queue enables asynchronous communication between components or services.

It stores messages until consumers process them.

**Why do we need a message queue?**

- Separate tasks that do not need to finish during the initial request.
- Handle background processing.
- Buffer workloads during traffic spikes.
- Support retries and failure handling when configured appropriately.

**Example: Order Processing**

When a customer places an order, the application may need to:

- Update inventory.
- Notify the delivery service.
- Send an email or SMS confirmation.

If all these tasks are performed sequentially during the initial request, the response may become slower.

Instead, the application can publish a notification message to a queue. A separate consumer processes that message and sends the notification.

**How it works:**

1. The order service creates an order.
2. It publishes a notification message to the queue.
3. The queue stores the message.
4. A notification worker consumes the message.
5. The worker sends the email or SMS.
6. If processing fails, retries or a dead-letter queue may be used, depending on the implementation.

The queue does not send the SMS itself; the consumer or notification service performs that task.

### Synchronous vs. Asynchronous Communication

| Synchronous                                              | Asynchronous                                                               |
| -------------------------------------------------------- | -------------------------------------------------------------------------- |
| The caller generally waits for the operation's response. | The caller can continue without waiting for the background task to finish. |
| Useful when the result is immediately required.          | Useful for background work and independent tasks.                          |
| Example: Waiting for payment authorization.              | Example: Sending a confirmation email after an order is accepted.          |

---

### 3.7 Monitoring and Logs

Monitoring and logging help developers understand the health and behavior of an application.

**Monitoring** tracks metrics such as:

- CPU and memory usage.
- Request rate.
- Response time and latency.
- Error rate.
- Server availability.

**Logs** record events and diagnostic details that help explain what happened inside the application.

**Why are they important?**

- Detect application errors.
- Investigate unexpected behavior.
- Identify performance bottlenecks.
- Check whether servers are healthy.
- Understand failures after deployments.
- Help reproduce and debug issues.

Logs, metrics, alerts, and stack traces can work together to make troubleshooting easier.

---

## 4. Scaling in System Design

Scaling means increasing a system's ability to handle a growing workload.

### 4.1 Vertical Scaling

Vertical scaling increases the resources of an existing server.

Examples:

- Increasing CPU capacity.
- Adding more RAM.
- Upgrading storage.

**Advantages:**

- Often simpler to implement.
- May require fewer architectural changes.

**Limitations:**

- Hardware has practical limits.
- Upgrades may require downtime.
- A single server can remain a bottleneck or point of failure.

### 4.2 Horizontal Scaling

Horizontal scaling increases capacity by adding more server instances.

Examples:

- Running multiple backend servers.
- Distributing requests using a load balancer.
- Adding more application instances as demand grows.

**Advantages:**

- Can support larger workloads.
- Provides flexibility to add or remove instances.
- Can improve availability when redundancy and failover are configured.

**Challenges:**

- Requires traffic distribution.
- Shared state and sessions need careful handling.
- Database consistency and coordination can become more complex.

### Vertical vs. Horizontal Scaling

| Aspect        | Vertical Scaling           | Horizontal Scaling                       |
| ------------- | -------------------------- | ---------------------------------------- |
| Approach      | Upgrade an existing server | Add more servers                         |
| Main resource | CPU, RAM, storage          | Additional instances                     |
| Complexity    | Often simpler initially    | Often more distributed-system complexity |
| Capacity      | Limited by the machine     | Can grow across multiple machines        |
| Example       | Upgrade server RAM         | Run three backend instances              |

---

## 5. Centralized Database and Multiple Servers

When an application runs on multiple backend servers, those servers may connect to a shared centralized database.

**Example architecture:**

- Multiple application servers handle requests.
- A load balancer distributes traffic.
- All application servers connect to the same database.

This arrangement allows servers to work with shared application data.

**Benefits:**

- A common source of data.
- Easier sharing of records between application servers.
- Less need to maintain separate independent copies of the same application data.

**Limitations:**

- The database can become a bottleneck.
- The database may become a single point of failure if no redundancy is configured.
- High traffic may require indexing, caching, replication, or partitioning.

A centralized database does not automatically guarantee consistency in every situation; transactions and concurrency control are also important.

---

## 6. Types of Applications

Applications can be classified according to their primary performance bottlenecks.

Two important categories are data-intensive and compute-intensive applications.

### What Is Latency?

Latency is the time taken for a system to respond to a request or for data to travel between two points, depending on the context.

**Examples:**

- Opening an Instagram feed.
- Loading a YouTube video.
- Sending a WhatsApp message.

The delay between an action and its response contributes to the user-perceived latency.

Latency may come from network communication, database queries, application processing, or other parts of the system.

---

### 6.1 Data-Intensive Applications

Data-intensive applications primarily deal with storing, retrieving, transferring, updating, and serving large volumes of data.

**Examples:**

- Instagram: storing and serving posts, images, and feeds.
- WhatsApp: storing and delivering messages.
- Banking systems: processing transactional reads and writes.
- Analytics dashboards: retrieving and aggregating datasets.
- Log processing systems: storing and querying large log volumes.

#### Key Challenges

- Slow database queries.
- Large amounts of data moving over networks.
- High numbers of simultaneous requests.
- Storage capacity and throughput limitations.
- Maintaining data consistency and availability.
- Handling server or network failures.

#### Important Questions

- How quickly can data be retrieved?
- How safely can data be stored?
- How many users can access the data simultaneously?
- What happens if a server fails?
- What happens if network communication is interrupted?

#### Common Solutions

**Caching:** Reduce repeated database access by serving frequently requested data from a faster layer.

**Database indexing:** Improve the efficiency of suitable queries.

**Sharding:** Split data across multiple database partitions or servers to distribute storage and workload.

**Replication:** Maintain copies of data to support availability, redundancy, and read scaling, depending on the design.

**Database optimization:** Improve query performance and select appropriate data models.

**Consistency models:** Define how and when changes become visible across replicas.

---

### 6.2 Compute-Intensive Applications

Compute-intensive applications are primarily limited by processing requirements, such as CPU or GPU calculations.

**Examples:**

- Image processing and transformations.
- Video rendering and encoding.
- Machine learning training.
- Scientific simulations.
- Complex cryptographic calculations.

#### Key Concerns

- How quickly can computations be performed?
- Can the workload be divided into parallel tasks?
- Can algorithmic complexity be reduced?
- Would GPU acceleration help?
- Can processing be distributed across multiple machines?

#### Common Solutions

**Algorithm optimization:** Reduce unnecessary operations and improve time complexity.

**Parallel processing:** Execute independent tasks simultaneously.

**GPU acceleration:** Use GPUs for workloads that benefit from highly parallel computation.

**Distributed computing:** Spread suitable computational workloads across multiple machines.

---

### 6.3 Data-Intensive vs. Compute-Intensive

| Aspect                   | Data-Intensive                           | Compute-Intensive                                     |
| ------------------------ | ---------------------------------------- | ----------------------------------------------------- |
| Main bottleneck          | Data storage, retrieval, or movement     | Processing and calculations                           |
| Main focus               | Efficient data access and delivery       | Faster computation                                    |
| Common resource concerns | Storage, memory, network, disk I/O       | CPU and GPU                                           |
| Common solutions         | Caching, indexing, sharding, replication | Algorithm optimization, parallelism, GPU acceleration |
| Example                  | Serving a large social media feed        | Rendering video frames                                |

### Modern Applications Use Both

**YouTube**

- Data-intensive: storing, retrieving, and streaming videos.
- Compute-intensive: video encoding and recommendation calculations.

**Instagram**

- Data-intensive: storing posts and serving feeds.
- Compute-intensive: image processing and recommendation calculations.

### Rule of Thumb

- If the main bottleneck is storing, retrieving, or moving data, investigate data-intensive optimizations.
- If the main bottleneck is calculations and processing, investigate compute-intensive optimizations.

Real applications often combine both workloads, so identifying the actual bottleneck is essential before choosing a solution.

---

## 7. Key Takeaways

- System Design defines how components work together to satisfy requirements.
- Databases provide persistent data storage.
- Backend services process application logic and expose APIs.
- Client applications collect user input and display responses.
- Caches reduce repeated access to slower storage layers.
- Load balancers distribute traffic across available servers.
- Message queues support asynchronous background processing.
- Monitoring and logs help identify failures and performance problems.
- Vertical scaling upgrades an existing server.
- Horizontal scaling adds more server instances.
- Data-intensive applications focus on data access, storage, and movement.
- Compute-intensive applications focus on processing and calculations.
- Choosing the right design requires understanding the application's bottlenecks and trade-offs.

---

## 8. Topics to Revisit

- [ ] Understand the difference between LLD and System Design.
- [ ] Understand how a client request reaches the backend.
- [ ] Understand why multiple servers may need a load balancer.
- [ ] Understand how a centralized database is shared across servers.
- [ ] Understand when caching is useful.
- [ ] Understand synchronous and asynchronous communication.
- [ ] Practice identifying data-intensive and compute-intensive workloads.

---

*These notes will evolve as I continue learning System Design. Architecture diagrams, examples, and deeper explanations will be added alongside future topics.*
