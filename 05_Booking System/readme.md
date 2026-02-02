# Booking System – System Design (Feature Learning)

## Problem Statement
Design a booking system that allows users to search availability, reserve a slot/resource, and confirm a booking while preventing double booking and handling high traffic safely.

---

## Section 1: Functional Requirements

### Core Functional Requirements
- Users should be able to search for available items (tickets, rooms, seats, slots).
- Users should be able to select an item and initiate a booking.
- The system should temporarily hold a selected item to prevent double booking.
- Users should be able to confirm or cancel a booking.
- Users should receive a booking confirmation after successful payment.

### Optional / Nice-to-Have Features
- User authentication and booking history.
- Booking cancellation and refund support.
- Notifications via email/SMS.
- Admin ability to add or remove inventory.
- Support for multiple booking categories (movies, hotels, events).

---

## Section 2: Non-Functional Requirements

### Availability
- The system should be highly available, especially during peak traffic (sales, festivals).
- Booking and availability checks should not go down frequently.

### Scalability
- The system should scale to handle sudden traffic spikes.
- Reads (availability checks) will be much higher than writes.

### Consistency
- Double booking must never happen.
- Strong consistency is required during seat/slot allocation.
- Eventual consistency is acceptable for analytics and notifications.

### Performance
- Availability checks should be fast (low latency).
- Booking confirmation should complete within a few seconds.

### Durability
- Confirmed bookings must never be lost.
- Data must survive system restarts or crashes.

---

## Section 3: Traffic Pattern & Assumptions

- Read-heavy system (searching availability).
- Write operations spike during booking confirmation.
- High traffic during flash sales or popular events.
- Peak traffic is time-bound (limited windows).

---

## Section 4: Consistency & CAP Choice

### Consistency Decision
- Strong consistency is required for booking confirmation.
- Temporary inconsistency is acceptable for search results.

### CAP Theorem Choice
- Partition tolerance is mandatory.
- Availability is prioritized for search.
- Consistency is prioritized for booking confirmation.
- Overall system behaves as a **CP system** during booking.

---

## Section 5: High-Level Design (HLD)

### Core Components
- Client (Web / Mobile App)
- API Gateway
- Load Balancer
- Booking Service
- Inventory Service
- Payment Service
- Cache
- Database
- Message Queue

### High-Level Flow
1. User searches availability.
2. System fetches availability from cache or database.
3. User selects an item.
4. System places a temporary hold on the item.
5. User completes payment.
6. Booking is confirmed and persisted.
7. Notification is sent asynchronously.

---

## Section 6: Low-Level Design (LLD)

### Key APIs
- `GET /availability`
- `POST /booking/hold`
- `POST /booking/confirm`
- `POST /booking/cancel`

### Database Tables
- Users
- Inventory (seats/rooms/slots)
- Bookings
- Payments

### Booking States
- AVAILABLE
- HELD
- CONFIRMED
- CANCELLED
- EXPIRED

---

## Section 7: Concurrency & Double Booking Prevention

### Techniques Used
- Database row-level locking.
- Optimistic locking with version numbers.
- Temporary holds with TTL (time-to-live).
- Atomic transactions during confirmation.

---

## Section 8: Caching Strategy

### What to Cache
- Availability data.
- Event or listing metadata.

### Cache Strategy
- Read-aside cache for availability.
- Cache invalidation on booking confirmation.
- Short TTL to avoid stale data.

---

## Section 9: Failure Handling

### Scenarios
- Payment succeeds but booking fails.
- Booking succeeds but notification fails.
- Cache failure.

### Solutions
- Idempotent booking APIs.
- Retry mechanisms.
- Message queues for async processing.
- Graceful degradation.

---

## Section 10: Security Considerations

- Authentication and authorization.
- Prevent duplicate API calls.
- Rate limiting during peak traffic.
- Secure payment handling.

---

## Section 11: Monitoring & Metrics

- Booking success rate.
- Booking failure rate.
- Availability latency.
- Payment latency.
- Inventory utilization.

---

## Section 12: Future Improvements

- Dynamic pricing.
- Recommendation system.
- Multi-region deployment.
- Fraud detection.
- Auto-scaling based on traffic patterns.

