# 🔗 System Design – URL Shortener (Interview Ready)

This document provides a **complete end-to-end system design** for a URL Shortener (Bitly-like). You can reuse this format directly in **system design interviews**.

---

## 1. Problem Statement

Design a system that:

* Accepts a long URL
* Generates a short URL
* Redirects users from short URL to the original URL

---

## 2. Clarifying Questions

Before starting the design, ask:

* How many URLs are created per day?
* Is the system read-heavy or write-heavy?
* Should URLs support expiry?
* Is analytics required?
* Is the system global or single-region?

### Assumptions

* 100M URLs/year
* Read-heavy system (5:1 read/write ratio)
* Low-latency redirects
* High availability required

---

## 3. Functional Requirements (FR)

* Generate a short URL for a given long URL
* Redirect short URL to original URL
* Support optional expiry time
* Ensure uniqueness of short URLs

---

## 4. Non-Functional Requirements (NFR)

* Availability: 99.99%
* Latency: <100 ms for redirects
* Scalability: millions of URLs
* Consistency: strong consistency for URL mapping
* Security: rate limiting and validation

---

## 5. Capacity Estimation

### Write Traffic

* 100M URLs/year ≈ 3 URLs/sec

### Read Traffic

* Average 5 redirects per URL
* ~15 RPS average
* Peak traffic ~1–2k RPS

### Storage

* ~500 bytes per record
* 100M × 500B ≈ 50 GB/year

---

## 6. High-Level Design (HLD)

### Components

```
Client
  |
Load Balancer
  |
URL Service (Stateless)
  |
Redis Cache
  |
NoSQL Database
```

---

## 7. Request Flow

### Write Flow (Create Short URL)

1. Client submits long URL
2. Load balancer routes request
3. URL service generates short code
4. Mapping stored in database
5. Short URL returned to client

### Read Flow (Redirect)

1. User accesses short URL
2. Service checks Redis cache
3. Cache hit → immediate redirect
4. Cache miss → DB lookup → cache update → redirect

---

## 8. Database Design

### Choice

**NoSQL (DynamoDB / Cassandra)**

### Schema

```text
URL_Table
- short_code (Primary Key)
- long_url
- created_at
- expiry_time
```

### Reasoning

* Simple key-value access
* Easy horizontal scaling
* Low-latency reads

---

## 9. Short URL Generation (LLD)

### Options Considered

* Random string → collision risk
* UUID → long URLs
* **Base62 encoding with ID generator (Chosen)**

### Why Base62?

* No collisions
* Short and readable URLs
* High performance

---

## 10. API Design

### Create Short URL

```http
POST /api/v1/shorten
```

```json
{
  "longUrl": "https://example.com/very/long/url"
}
```

```json
{
  "shortUrl": "https://sho.rt/aZ3f"
}
```

### Redirect

```http
GET /{shortCode}
→ HTTP 302 Redirect
```

---

## 11. Caching Strategy

* Cache mapping: `shortCode → longUrl`
* Redis cache
* Cache-aside pattern
* TTL based on expiry

### Why Cache?

* Redirects are highly read-heavy
* Reduces database load significantly

---

## 12. Scalability

* Stateless application servers
* Horizontal scaling
* Load balancer
* Redis cluster
* Database sharding on `short_code`

---

## 13. Consistency & CAP Theorem

* Writes require strong consistency
* Reads can tolerate eventual consistency

**CAP Choice:**

* CP for writes
* AP for reads

---

## 14. Reliability & Fault Tolerance

* Database replication
* Cache replication
* Retry with exponential backoff
* Health checks and monitoring

---

## 15. Security Considerations

* HTTPS enforced
* Rate limiting per IP/user
* Input validation
* Protection against malicious redirects

---

## 16. Bottlenecks & Optimizations

| Bottleneck        | Solution               |
| ----------------- | ---------------------- |
| Hot short URLs    | Redis cache            |
| Cache miss storm  | Request coalescing     |
| Database overload | Increase cache TTL     |
| ID generation     | Distributed ID service |

---

## 17. Final Summary (Interview Closing)

This URL shortener design is:

* Cache-first and read-optimized
* Highly scalable using stateless services
* Reliable with replication and failover
* Simple yet production-ready

> This design balances performance, scalability, and reliability for a real-world URL shortening service.

---
