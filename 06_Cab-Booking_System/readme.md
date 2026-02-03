# Cab Booking System – Driver Mapping Focused Design

## Problem Statement
Design a cab booking system that efficiently maps nearby available drivers to a user’s ride request while ensuring low latency, no double assignment, and high availability at scale.

---

## Section 1: Core Problem – Driver Mapping

Driver mapping means:
- Identifying **nearby available drivers**
- Selecting the **best driver** for a ride request
- Ensuring **one driver is assigned to only one ride**
- Completing this process **fast and reliably**

This is the heart of any cab booking system.

---

## Section 2: Functional Requirements (Driver Mapping)

### User Side
- User requests a ride with pickup and drop location.
- User receives driver details once matched.
- User can track driver location in real time.

### Driver Side
- Driver can go online or offline.
- Driver sends live location updates.
- Driver can accept or reject ride requests.

### System Responsibilities
- Find nearby drivers quickly.
- Prevent assigning the same driver to multiple users.
- Reassign driver if request is rejected or times out.

---

## Section 3: Non-Functional Requirements (Mapping-Specific)

### Latency
- Driver matching should complete within 1–3 seconds.
- Slow matching leads to poor user experience.

### Consistency
- Strong consistency is required during driver assignment.
- A driver must never be assigned to two rides.

### Availability
- Even if some components fail, ride requests should still work.

### Scalability
- Must support millions of drivers updating locations frequently.

---

## Section 4: Data Model (Simplified)

### Driver
- driver_id
- current_lat
- current_lng
- status (ONLINE, OFFLINE, BUSY)
- last_updated_at

### Ride
- ride_id
- user_id
- pickup_location
- driver_id
- status (REQUESTED, ASSIGNED, IN_PROGRESS, COMPLETED)

---

## Section 5: Driver Location Management

### How Driver Location Is Stored
- Drivers send GPS updates every few seconds.
- Location updates are written to:
  - In-memory store (Redis)
  - Geo-spatial index (GeoHash)

### Why In-Memory Storage
- Location data changes very frequently.
- Disk-based DB would be too slow.
- Redis allows fast geo-queries.

---

## Section 6: Geo-Spatial Indexing (Driver Mapping Core)

### Why Geo-Spatial Indexing
- Need to find drivers within a radius (e.g., 3–5 km).
- Plain database scans are too slow.

### Common Techniques
- GeoHash
- QuadTree
- H3 (Uber)

### How GeoHash Works (Conceptually)
1. World is divided into grid cells.
2. Each cell has a unique hash.
3. Nearby locations share similar hashes.
4. Search happens only within relevant cells.

---

## Section 7: Driver Mapping Flow (Step-by-Step)

### Step 1: Ride Request
- User sends pickup location to Ride Service.

### Step 2: Find Nearby Drivers
- Pickup location converted into GeoHash.
- System queries Redis for drivers in:
  - Same GeoHash
  - Neighboring GeoHashes
- Filters drivers with status = ONLINE.

### Step 3: Rank Drivers
Drivers are ranked based on:
- Distance from pickup
- Estimated time of arrival (ETA)
- Driver rating
- Recent activity

---

## Section 8: Driver Assignment (Critical Section)

### Strong Consistency Requirement
This step **must be atomic**.

### Assignment Process
1. Pick top-ranked driver.
2. Perform atomic lock on driver:
   - SET driver_id = BUSY (CAS / transaction)
3. Create ride with driver_id.
4. Notify driver with ride request.

### Why Locking Is Needed
- Prevents same driver from being assigned twice.
- Ensures correctness under concurrent requests.

---

## Section 9: Driver Accept / Reject Logic

### Accept Flow
- Driver accepts within timeout (e.g., 15 seconds).
- Ride status → ASSIGNED.
- Driver status → BUSY.

### Reject / Timeout Flow
- Driver rejects or times out.
- Driver status → ONLINE.
- Next best driver is selected.
- Process repeats.

---

## Section 10: Real-Time Tracking After Mapping

### Communication
- Driver sends GPS updates via WebSocket / gRPC.
- Updates stored in Redis.
- User app subscribes to location updates.

### Why Not Polling
- Polling increases latency and server load.
- Real-time streaming is required for UX.

---

## Section 11: Failure Handling in Driver Mapping

### Driver Goes Offline Suddenly
- Heartbeat missing for threshold time.
- Driver marked OFFLINE.
- Ride reassigned if not started.

### Redis Failure
- Fallback to last known location in DB.
- Slightly slower matching but system remains usable.

### Duplicate Requests
- Idempotency keys ensure same request is not processed twice.

---

## Section 12: CAP Consideration (Mapping Perspective)

- Partition tolerance is mandatory.
- During mapping:
  - Consistency > Availability
- Better to reject a request than double-assign a driver.

System behaves as **CP** during assignment.

---

## Section 13: Optimizations & Enhancements

- Dynamic search radius expansion.
- Driver batching for popular areas.
- Heatmaps for demand prediction.
- Pre-positioning drivers in high-demand zones.

