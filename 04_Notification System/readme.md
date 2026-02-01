# 📢 System Design – Notification System (Interview Ready)

This document presents a **scalable, reliable notification system** supporting multiple channels such as **Push, Email, and SMS**. The design follows a **standard system design interview structure**.

---

## 1. Problem Statement

Design a system that can:

* Send notifications to users
* Support multiple channels (Push, Email, SMS)
* Handle millions of users
* Be reliable and scalable

---

## 2. Clarifying Questions

Before designing:

* Are notifications real-time or async?
* Is delivery guaranteed or best-effort?
* Should notifications be retried on failure?
* Do users have channel preferences?

### Assumptions

* 5 million users
* 10 million notifications per day
* Peak: 50k notifications/min
* Mostly async notifications
* Best-effort delivery

---

## 3. Functional Requirements (FR)

* Send notifications to users
* Support Push, Email, and SMS channels
* Respect user notification preferences
* Retry failed notifications
* Allow future channels

---

## 4. Non-Functional Requirements (NFR)

* Availability: 99.9%
* Latency: Near real-time for push
* Scalability: millions of notifications
* Reliability: retry on failure
* Extensibility: add new channels easily

---

## 5. Capacity Estimation

### Traffic

* 10M notifications/day ≈ 115 notifications/sec avg
* Peak: ~830 notifications/sec

### Storage

* Notification metadata only
* Few GBs/month

---

## 6. High-Level Design (HLD)

### Components

```
Client / Service
     |
Notification API
     |
Message Queue (Kafka / SQS)
     |
----------------------------------
| Push Worker | Email Worker | SMS Worker |
----------------------------------
     |
External Providers (FCM / SMTP / Twilio)
```

---

## 7. Core Components & Responsibilities

### Notification API

* Accepts notification requests
* Validates payload
* Publishes message to queue

### Message Queue

* Buffers traffic spikes
* Decouples producers & consumers
* Ensures durability

### Channel Workers

* Consume messages
* Format channel-specific payload
* Send via external providers

---

## 8. Database Design

### User Preferences Table

```text
User_Preferences
- user_id (PK)
- push_enabled
- email_enabled
- sms_enabled
```

### Notification Log Table

```text
Notification_Log
- notification_id (PK)
- user_id
- channel
- status
- created_at
```

---

## 9. Message Queue Design

### Queue Structure

* Topic per channel OR
* Single topic with channel attribute

### Why Queue?

* Async processing
* Traffic smoothing
* Retry support

---

## 10. API Design

### Send Notification

```http
POST /api/v1/notifications/send
```

```json
{
  "userId": "123",
  "type": "ORDER_STATUS",
  "message": "Your order is shipped"
}
```

---

## 11. Retry & Failure Handling

* Retry with exponential backoff
* Max retry count
* Dead Letter Queue (DLQ)
* Manual reprocessing for failures

---

## 12. Scalability

* Stateless API servers
* Horizontally scalable workers
* Partitioned queues
* Independent scaling per channel

---

## 13. Consistency & Delivery Guarantees

* At-least-once delivery
* Duplicate handling at consumer
* Best-effort delivery model

---

## 14. Bottlenecks & Optimizations

| Bottleneck               | Solution                 |
| ------------------------ | ------------------------ |
| External provider limits | Rate limiting & batching |
| Queue lag                | Scale consumers          |
| Retry storms             | Exponential backoff      |

---

## 15. Security Considerations

* Authentication for API
* Rate limiting
* Secure credentials for providers
* Data encryption in transit

---

## 16. Monitoring & Observability

* Metrics: sent, failed, retried
* Logs per notification
* Alerts on failure spikes
* Dashboard per channel

---

## 17. Final Summary (Interview Closing)

This notification system:

* Is asynchronous and highly scalable
* Uses queues for reliability
* Supports multiple channels
* Is extensible and production-ready

