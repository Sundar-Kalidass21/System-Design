# 🚦 System Design – Rate Limiter (Interview Ready)

This document describes a **distributed rate limiter** design suitable for high-traffic APIs. It follows a **standard system design interview flow**.

---

## 1. Problem Statement

Design a system that:

* Limits the number of requests a user can make
* Rejects requests exceeding the limit
* Works in a distributed environment
* Supports millions of users

Example:

> Allow **100 requests per minute per user**. The 101st request is rejected.

---

## 2. Clarifying Questions

Before designing:

* Is rate limiting per user, IP, or API key?
* Is the limit configurable?
* Should blocked requests be logged?
* What should happen if the rate limiter fails?

### Assumptions

* Rate limit: 100 requests/min/user
* 10 million users
* Peak traffic: 100k RPS
* Latency requirement: <10 ms
* Highly available system

---

## 3. Functional Requirements (FR)

* Allow up to N requests per user per time window
* Reject requests exceeding the limit
* Return HTTP 429 for blocked requests
* Work across multiple servers

---

## 4. Non-Functional Requirements (NFR)

* Latency: <10 ms
* Availability: 99.99%
* Scalability: handle 100k+ RPS
* Accuracy: minimal rate-limit violations
* Fault tolerance

---

## 5. Capacity Estimation

### Traffic

* Peak load: 100k requests/sec
* Active users at peak: ~1M

### Storage

* Store counters per active user
* Few hundred MB (in-memory)

---

## 6. High-Level Design (HLD)

### Components

```
Client
  |
API Gateway / Load Balancer
  |
Rate Limiter Service (Stateless)
  |
Redis (In-Memory Data Store)
```

---

## 7. Algorithm Choice

### Options Considered

| Algorithm        | Drawback          |
| ---------------- | ----------------- |
| Fixed Window     | Burst at boundary |
| Sliding Window   | Complex & costly  |
| Leaky Bucket     | Not flexible      |
| **Token Bucket** | ✅ Balanced        |

---

## 8. Chosen Algorithm – Token Bucket

### How It Works

* Each user has a bucket of tokens
* Tokens refill at a fixed rate
* Each request consumes one token
* If no token is available → reject request

---

## 9. Data Design (Redis)

### Key Structure

```
rate_limit:{user_id}
```

### Value

```json
{
  "tokens": 20,
  "last_refill_timestamp": 1700000000
}
```

### Why Redis?

* In-memory (low latency)
* Atomic operations
* TTL support
* Horizontal scaling

---

## 10. Request Flow

1. Extract user ID / API key
2. Fetch token bucket from Redis
3. Refill tokens based on elapsed time
4. If tokens ≥ 1:

   * Decrement token
   * Allow request
5. Else:

   * Reject request (HTTP 429)

---

## 11. API Design

### Internal Rate Limit Check

```http
POST /internal/rate-limit/check
```

### Response

```json
{
  "allowed": true,
  "remaining": 19
}
```

---

## 12. Caching Strategy

* Redis itself acts as cache
* Active users stay in memory
* TTL cleans up inactive users

---

## 13. Scalability

* Stateless rate limiter service
* Horizontal scaling
* Redis cluster with sharding
* Consistent hashing for keys

---

## 14. Consistency & CAP

* Strong consistency required for counters
* Atomic Redis operations ensure correctness

**CAP Choice:** CP (Consistency + Partition tolerance)

---

## 15. Reliability & Fault Tolerance

* Redis replication
* Health checks
* Retry with backoff
* Fail-open or fail-close strategy (business dependent)

---

## 16. Bottlenecks & Optimizations

| Bottleneck      | Solution               |
| --------------- | ---------------------- |
| Redis hot keys  | Sharding               |
| Redis failure   | Replication + fallback |
| Network latency | Local rate limiting    |
| Burst traffic   | Token bucket smoothing |

---

## 17. Security Considerations

* Rate limit per API key
* HTTPS enforced
* Prevent spoofing
* IP + user-based limits

---

## 18. Final Summary (Interview Closing)

This rate limiter design:

* Uses Token Bucket for flexibility
* Relies on Redis for low-latency distributed coordination
* Scales horizontally to millions of users
* Is production-ready and fault-tolerant


