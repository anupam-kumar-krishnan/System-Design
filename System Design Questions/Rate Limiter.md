# Rate Limiter

### 1️⃣ First 20 seconds — Clarify & Set Assumptions

> “I’ll assume we need to limit API requests per user. For example: 100 requests per minute per user, enforced strictly, for a single-region service.”


### 2️⃣ Requirements (Very Short)

**Functional**
- Allow requests within limit
- Reject extra requests with 429

**Non-Functional**
- Low latency
- Scalable
- Reasonably accurate

### 3️⃣ Where the Rate Limiter Sits

> “The rate limiter will sit at the API gateway or middleware layer, before hitting backend services.”

***Simple flow:**
- Client → Rate Limiter → Backend


### 4️⃣ Algorithms — Mention, Then Choose One

**Algorithims**
- Fixed Window
- Sliding Window
- Token Bucket
- Leaky Bucket

**What I’ll choose:**

> “I’ll use Token Bucket, since it allows bursts and is commonly used in real systems.”

### 5️⃣ Token Bucket — One Clear Explanation

> “Each user has a bucket with fixed capacity, say 100 tokens. Tokens refill at a constant rate. Each request consumes one token. If no token is available, the request is rejected.”


### 6️⃣ Storage Decision (Very Important)

> “If this were a single server, in-memory storage would work. But for multiple instances, I’d use Redis.”

**Why Redis:**
- In-memory, fast
- Atomic operations

**Key structure:**

```css
rate_limit:{userId}
```

**Store:**
- remainingTokens
- lastRefillTimestamp

### 7️⃣ Request Flow (Step-by-Step)

1. Identify user (userId / IP)
2. Fetch token bucket from Redis
3. Refill tokens based on elapsed time
4. If tokens > 0:
    - Decrement
    - Allow request
5. Else:
    - Reject with 429

> This slow and structured.

### 8️⃣ Concurrency (Must Mention Once)

> “Since multiple requests may come at the same time, I’d use Redis atomic operations or a Lua script so token check and update happen atomically.”


### 9️⃣ Edge Cases (Pick Any 3)
- First request → initialize bucket
- Token refill should not exceed max capacity
- Redis down → fail open or fail closed (config decision)



### 🔟 Scaling Answer (Short & Safe)

>“The system scales horizontally by running multiple app instances using a shared Redis cluster. Rate limiting can also be enforced at the API gateway.”


### 1️⃣1️⃣ Response to Client

> “If rate limit is exceeded, return HTTP 429 Too Many Requests, optionally with Retry-After header.”


### 1️⃣2️⃣ Final 1-Line Summary (Very Important)

> “I designed a Redis-backed rate limiter using the Token Bucket algorithm that supports burst traffic, scales horizontally, and enforces per-user request limits with low latency.”
