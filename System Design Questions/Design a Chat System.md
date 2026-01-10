# 📩 System Design: Chat System (WhatsApp / Messenger-like)

### 1️⃣ Clarify Requirements (Always start here)

**Functional Requirements**
- One-to-one messaging
- Group chats
- Real-time message delivery
- Message status: sent, delivered, read
- Online / offline users
- Message history
- Push notifications

**Non-Functional Requirements**
- Low latency (< 200ms)
- High availability (99.99%)
- Horizontal scalability (millions of users)
- Fault tolerance
- Message ordering
- Data durability

**Out of Scope (Mention briefly)**
- End-to-end encryption (can be added later)
- Voice/video calls
- Message search (optional)

### 2️⃣ High - Level Architecture

<img width="944" height="722" alt="image" src="https://github.com/user-attachments/assets/fd398d66-ab09-47b0-ac96-70b7a13639d8" />

### 3️⃣ Client Side

- Web / Mobile App
- Uses WebSocket for real-time communication
- Falls back to HTTP if WebSocket unavailable
- Maintains local cache of messages

### 4️⃣ Communication Protocol

**Why WebSocket?**
- Full-duplex communication
- Low latency
- Persistent connection
- Ideal for real-time chat

**Alternative (Explain briefly)**
- Long polling → inefficient
- Server-Sent Events → one-way only

### 5️⃣ Chat Gateway (WebSocket Servers)

**Responsibilities**
- Maintain active connections
- Authenticate users (JWT / OAuth)
- Route messages to correct recipients
- Heartbeat to detect disconnected users

**Scaling Strategy**
- Stateless servers
- Use consistent hashing or Redis to map:

```nginx
userId → socketServerId
```

### 6️⃣ Message Flow (Step-by-Step)

**Sending a Message**

1. User A sends message via WebSocket
2. Chat Gateway forwards message to Message Service
3. Message is stored in DB
4. Message pushed to Kafka
5. If User B is online → deliver instantly
6. If offline → store and trigger push notification

### 7️⃣ Message Queue (Kafka / RabbitMQ)

**Why needed?
- Decouples message sending from delivery
- Handles traffic spikes
- Ensures durability

**Guarantees**
- At-least-once delivery
- Message ordering per chat (use partition key = chatId)

### 8️⃣ Database Design

**Why NoSQL?**
- High write throughput
- Easy horizontal scaling

**Suggested DB**
- Cassandra / DynamoDB / MongoDB

**Message Table**

```
Message {
  messageId (PK)
  chatId
  senderId
  content
  timestamp
  status
}
```

**Chat Table**

```
Chat {
  chatId
  participants[]
  lastMessage
}
```

### 9️⃣ Presence Service (Online / Offline)

- Tracks user status
- Uses Redis
- Updated on:
   - WebSocket connect
   - WebSocket disconnect

```
userId → online/offline
```

### 10️⃣ Notification Service

- Triggered when recipient is offline
- Uses:
   - Firebase Cloud Messaging (Android)
   - APNs (iOS)
- Sends push notifications

### 11️⃣ Message Status (Ticks)

- Sent → stored in DB
- Delivered → received by recipient server
- Read → client sends read receipt

Handled asynchronously via message queue.

### 12️⃣ Group Chat Handling

- Maintain groupId → userIds
- Fan-out strategy:
   - Small groups → push to each user
   - Large groups → write once, users pull messages
 
### 13️⃣ Scaling Strategy

**Horizontal Scaling**
- WebSocket servers auto-scale
- Partition Kafka by chatId
- Shard DB by chatId

**Caching**
- Redis for:
  - Recent messages
  - Presence
  - User-socket mapping
 
### 14️⃣ Fault Tolerance

- Message stored before delivery
- Retry on failure
- Kafka replication
- DB replication

### 15️⃣ Bottlenecks & Solutions

| Problem            | Solution           |
| ------------------ | ------------------ |
| WebSocket overload | Auto-scaling       |
| Message loss       | Persistent queue   |
| Hot partitions     | Better sharding    |
| Offline users      | Push notifications |

### 16️⃣ Security

- TLS encryption
- JWT authentication
- Rate limiting
- Spam detection

### 17️⃣ Optional Improvements (Mention at End)

- End-to-end encryption
- Message search using Elasticsearch
- Media storage via S3 + CDN
- Typing indicators

> Designed a real-time chat system using WebSockets for low latency, Kafka for reliable message delivery, Redis for presence tracking, and NoSQL databases for scalability. The system supports offline messaging, message ordering, and horizontal scaling while ensuring high availability and fault tolerance.
