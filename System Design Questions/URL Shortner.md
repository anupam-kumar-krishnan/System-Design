### 1. Problem Statement

Design a URL Shortener (like bit.ly) that:

- Converts a long URL into a short URL
- Redirects the short URL to the original long URL
- Scales to millions of users
- Is highly available and low latency

```
Example:

Input  → https://www.amazon.in/product/iphone-15-pro-max
Output → https://sho.rt/aZ3xP9
```

### 2️. Requirements Clarification

Functional Requirements
- Generate a unique short URL
- Redirect short URL → original URL
- Handle duplicate URLs efficiently (optional)
- Support URL expiration (optional)
- Basic analytics (click count, optional)

Non-Functional Requirements
- Low latency redirection (< 50ms)
- High availability
- Scalable (100M+ URLs)
- Durability (no data loss)
- Collision-free short URLs

### 3. High-Level Architecture

<img width="624" height="588" alt="image" src="https://github.com/user-attachments/assets/70df1b13-6a9b-4fca-ae05-0058dce45832" />


### 4️. API Design

1. Create Short URL

POST /api/shorten

```js
Body:
{
  "longUrl": "https://example.com"
}
```

```js
Response:

{
  "shortUrl": "https://sho.rt/aZ3xP9"
}
```

2. Redirect

GET /aZ3xP9 → HTTP 302 Redirect to long URL

### 5️. URL Shortening Strategy (IMPORTANT)

Option 1: Base62 Encoding (Most Common)

- Use characters: a-z A-Z 0-9 (62 chars)
- Generate a unique ID
- Convert ID → Base62

```
Example:

ID = 1256789
Base62 → aZ3xP9
```

✔ Short
✔ URL-safe
✔ No collisions if ID is unique

**How to generate unique ID?**

- Use Auto-Increment ID from DB
- OR Snowflake ID generator (preferred at scale)

### 6️. Database Design

```
Table: url_mapping

Field	       Type
-----------------
id	         BIGINT (PK)
short_code	 VARCHAR
long_url	   TEXT
created_at	 TIMESTAMP
expires_at	 TIMESTAMP (optional)
```

📌 Use NoSQL (DynamoDB / Cassandra) for scalability<br>
📌 Primary key → short_code

### 7️. Redirection Flow (Step-by-Step)

1. User hits sho.rt/aZ3xP9
2. Load Balancer forwards request
3. Check Redis Cache
   - If found → redirect immediately
4. If not in cache:
   - Fetch from DB
   - Store in Redis
   - Redirect user

👉 99% traffic served from cache

### 8️. Caching Strategy

- Redis / Memcached
- Key: short_code
- Value: long_url
- TTL: configurable (e.g., 24 hrs)

**Benefits:**
- Extremely low latency
- Reduces DB load

### 9️. Handling Scale

**Read Heavy System**
- Redirections ≫ URL creation

**Scaling Techniques**
- Horizontal scaling of services
- Database sharding by short_code
- CDN for popular URLs
- Redis cluster

### 10. Collision Handling

- Base62 + Unique ID → No collision
- If random generation used:
   - Retry on collision
   - Maintain unique index on short_code

### 1️1. Fault Tolerance & Reliability

- Replicated databases
- Redis with persistence
- Graceful fallback to DB if cache fails
- Health checks + auto scaling

### 1️2. Security Considerations

- URL validation
- Rate limiting (prevent abuse)
- Blacklist malicious URLs
- HTTPS only

### 1️3. Optional Enhancements (Mention if Time Allows)

- Custom short URLs
- Analytics (click count, geo, device)
- URL expiration
- QR code generation


> Designed a scalable URL shortener using Base62 encoding, Redis caching, and a NoSQL database to ensure low-latency redirection, high availability, and collision-free short URLs.
