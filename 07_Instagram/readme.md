# Instagram System Design (Feature Learning)

## Problem Statement
Design a social media platform like Instagram that allows users to upload photos/videos, follow other users, view a personalized feed, like and comment on posts, and receive notifications at massive scale.

---

## Section 1: Functional Requirements

### Core Functional Requirements
- Users should be able to sign up and log in.
- Users should be able to upload photos and videos.
- Users should be able to follow and unfollow other users.
- Users should see a feed of posts from people they follow.
- Users should be able to like, comment, and share posts.
- Users should receive notifications for likes, comments, and follows.

### Optional / Nice-to-Have Features
- Stories with auto-expiry.
- Direct messaging.
- Explore / recommendation feed.
- Hashtags and search.
- Live streaming.

---

## Section 2: Non-Functional Requirements

### Availability
- The feed and post upload features must be highly available.
- Users should always be able to browse content.

### Scalability
- Must support hundreds of millions of users.
- Read traffic is extremely high compared to writes.

### Latency
- Feed loading should be fast (low latency).
- Likes and comments should feel instant.

### Consistency
- Eventual consistency is acceptable for likes and counts.
- Strong consistency is required for authentication and user actions.

### Durability
- Uploaded media must never be lost.
- Posts and user data must persist reliably.

---

## Section 3: Traffic Pattern & Assumptions

- Extremely read-heavy system (feed scrolling).
- Write-heavy for likes, comments, and follows.
- Media uploads are heavy but less frequent.
- Traffic spikes during peak hours and viral events.

---

## Section 4: Consistency & CAP Choice

### Consistency Decision
- Eventual consistency for:
  - Feed updates
  - Like counts
  - Notifications
- Strong consistency for:
  - User authentication
  - Follow/unfollow actions

### CAP Theorem Choice
- Partition tolerance is mandatory.
- Availability is prioritized over strong consistency.
- Overall system behaves as an **AP system**.

---

## Section 5: High-Level Design (HLD)

### Core Components
- Client (Mobile App / Web App)
- API Gateway
- Load Balancer
- User Service
- Post Service
- Feed Service
- Media Service
- Like & Comment Service
- Notification Service
- Cache
- Database
- Object Storage
- Message Queue

---

## Section 6: High-Level Flow

### Upload Post Flow
1. User uploads image/video.
2. Media is stored in object storage.
3. Metadata is stored in database.
4. Feed service is notified asynchronously.

### View Feed Flow
1. User opens feed.
2. Feed service fetches post IDs.
3. Post metadata is fetched from cache/DB.
4. Media URLs are returned to client.
5. Client loads media from CDN.

---

## Section 7: Feed Generation (Core Problem)

### Two Approaches

#### 1. Fan-Out on Write
- When a user posts, push post ID to all followers’ feeds.
- Fast read performance.
- Expensive for users with many followers.

#### 2. Fan-Out on Read
- Generate feed when user opens app.
- Cheaper writes.
- Slower reads.

### Hybrid Approach (Used in Practice)
- Fan-out on write for normal users.
- Fan-out on read for celebrity accounts.

---

## Section 8: Data Model (Simplified)

### User
- user_id
- username
- profile_info
- follower_count
- following_count

### Post
- post_id
- user_id
- media_url
- caption
- created_at

### Follow
- follower_id
- followee_id

### Like
- post_id
- user_id

### Comment
- comment_id
- post_id
- user_id
- text

---

## Section 9: Media Storage & CDN

### Media Handling
- Images/videos stored in object storage.
- Multiple resolutions generated.
- Media served via CDN.

### Why CDN
- Reduces latency globally.
- Offloads backend servers.
- Improves user experience.

---

## Section 10: Caching Strategy

### What to Cache
- User profiles.
- Feed post IDs.
- Post metadata.
- Like counts.

### Cache Strategy
- Read-aside cache.
- Short TTL for dynamic data.
- Cache invalidation on updates.

---

## Section 11: Likes & Comments

### Design Choice
- Likes stored separately from posts.
- Counts may be eventually consistent.
- Asynchronous updates for scalability.

### Why Eventual Consistency Works
- Small delay in like count is acceptable.
- UX impact is minimal.

---

## Section 12: Notifications System

### Notification Flow
1. Like/comment/follow event occurs.
2. Event pushed to message queue.
3. Notification service processes event.
4. Push notification sent to user.

### Why Async
- Prevents blocking user actions.
- Improves performance and reliability.

---

## Section 13: Failure Handling

### Common Failures
- Media upload failure.
- Feed service timeout.
- Cache failure.

### Solutions
- Retry mechanisms.
- Graceful degradation.
- Fallback to database.
- Idempotent APIs.

---

## Section 14: Security & Abuse Prevention

- Authentication and authorization.
- Rate limiting on likes/comments.
- Spam and bot detection.
- Secure media access URLs.

---

## Section 15: Monitoring & Metrics

- Feed load latency.
- Post upload success rate.
- Likes/comments per second.
- Cache hit ratio.
- Notification delivery rate.

---

## Section 16: Future Improvements

- AI-based feed ranking.
- Recommendation system.
- Story prioritization.
- Content moderation automation.
- Multi-region deployment.

---

## Summary:
This Instagram system design focuses on high availability, massive read scalability, efficient feed generation, and reliable media delivery. The architecture balances performance and consistency using caching, asynchronous processing, and hybrid feed generation strategies.
