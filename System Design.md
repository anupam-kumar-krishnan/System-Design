# _System Design_
- System design encompasses the process of transforming user requirements into an architecture that best satisfies those requirements. 
- It involves making decisions about how different components of a system will interact with each other to achieve the desired functionality. 
- System design considers various factors such as performance, scalability, reliability, security, and maintainability to ensure the resulting system meets both technical and business needs.

# Approach to Designing a System
- Reuirement Gathering
- Analysis
- Design
- Implementation
- Testing
- Deployement
- Monitoring and Maintenance

# Key Concepts in System Design
- Modularity
- Scalability
- Reliability
- Performance
- Security

## **What is the Approach to System Design?**

A clear, step-by-step approach is crucial. Here’s a standard interview-ready approach 👇

### 1️⃣ Understand & Clarify Requirements

Never jump to architecture first.

**Ask questions like:**

- Who are the users?
- Read vs write heavy?
- Scale (1K users or 10M users)?
- Latency expectations?
- Platforms (web, mobile)?

**📌 Example:**

> “Is real-time required or eventual consistency is fine?”

### 2️⃣ Define Functional Requirements

**What the system must do.**

Example:
- User can sign up / login
- User can book a seat
- User can upload an image

### 3️⃣ Define Non-Functional Requirements

How well the system should perform.

Examples:

- Scalability (horizontal vs vertical)
- Availability (99.9%?)
- Low latency (<200ms)
- Security
- Consistency

### 4️⃣ High-Level Architecture

Draw big blocks first.

Typical components:

- Client (Web / Mobile)
- Load Balancer
- Backend Services
- Database
- Cache
- Message Queue (if needed)
- Client → Load Balancer → API Server → Database

### 5️⃣ Database Design

Choose the right DB.

Ask:

- SQL or NoSQL?
- Data relationships?
- Read/write patterns?

Example:
- Booking system → SQL
- Feed / timeline → NoSQL
  
Also discuss:
- Indexing
- Partitioning
- Replication

### 6️⃣ Handle Scale & Performance

Add optimizations:
- Caching (Redis)
- CDN
- Rate limiting
- Pagination
- Async processing (queues)

### 7️⃣ Handle Edge Cases & Failures

Shows senior-level thinking:

- What if DB is down?
- Duplicate requests?
- Concurrent users?
- Data consistency?

### 8️⃣ APIs (Optional but Good)

Define key APIs
- POST /book-seat
- GET /available-seats

### 9️⃣ Trade-offs (Very Important)

Every decision has a cost.

Example:

- SQL → strong consistency but less flexible
- NoSQL → scalable but eventual consistency
- Cache → faster reads but stale data risk

📌 Always mention trade-offs.

> **_System design is about structuring scalable, reliable, and efficient software systems while balancing trade-offs._**



