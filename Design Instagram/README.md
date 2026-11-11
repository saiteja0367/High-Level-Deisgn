# Design Instagram — High-Level System Design

## Problem Statement

Design a scalable, highly available social media platform similar to Instagram, where users can upload photos and videos, follow other users, interact with posts, and browse a personalized home feed.

The system should support millions of active users, handle high volumes of media uploads and feed requests, and deliver content with low latency.

The primary focus is designing the **media upload pipeline, personalized home feed, and scalable data storage architecture**.

---

## 1. Functional Requirements

The system should support the following features:

1. **Create Posts**
   - Users can upload photos and videos.
   - Users can add captions to their posts.
   - Users can view their previously published posts.

2. **Home Feed**
   - Users can view posts published by accounts they follow.
   - Posts should initially be displayed in reverse chronological order.
   - The feed should support infinite scrolling and pagination.

3. **Follow and Unfollow**
   - Users can follow and unfollow other users.
   - Users can view their followers and following lists.

4. **Likes and Comments**
   - Users can like and unlike posts.
   - Users can comment on posts.
   - Users can view like counts and comments.

5. **User Profiles**
   - Users can view profiles and published posts.
   - Users can search for other users.

### Out of Scope

The following features are excluded from the initial design:

- Instagram Stories
- Reels recommendation algorithms
- Direct messaging
- Live streaming
- Advertisements
- Advanced machine-learning feed ranking
- Push notifications

---

## 2. Non-Functional Requirements

### High Availability
The platform should remain available during server failures and infrastructure disruptions.

### Low Latency
Home feeds and user profiles should load quickly, even during peak traffic.

### Scalability
The system should support millions of concurrent users and scale horizontally as traffic increases.

### High Durability
Successfully uploaded photos, videos, and post metadata should not be lost.

### Reliability
The system should handle failed uploads, retries, and temporary service outages gracefully.

### Consistency
Eventual consistency is acceptable for home feed updates and engagement counts. User relationships and access-control decisions require stronger consistency guarantees where appropriate.

---

## 3. Capacity Estimation

Use the following assumptions for the system design exercise.

| Metric | Assumption |
|---|---|
| Daily Active Users (DAU) | 100 million |
| Daily uploaded posts | 10 million |
| Average feed views per user per day | 50 |
| Average photo size | 2 MB |
| Average feed page size | 20 posts |
| Peak traffic multiplier | 5× |
| Data retention | Long-term |

### Estimation Tasks

Calculate the following:

1. Average and peak post-upload requests per second.
2. Average and peak home-feed requests per second.
3. Daily and yearly raw photo storage requirements, assuming all posts are photos.
4. Estimated metadata storage requirements.
5. Approximate media-delivery bandwidth.
6. Expected read-to-write traffic ratio.

Clearly state any additional assumptions.

---

## 4. API Design

Design REST APIs for the following operations:

| Operation | Suggested Endpoint |
|---|---|
| Initiate media upload | `POST /api/v1/media/upload-url` |
| Complete media upload | `POST /api/v1/media/complete` |
| Create post | `POST /api/v1/posts` |
| Get user posts | `GET /api/v1/users/{userId}/posts` |
| Get home feed | `GET /api/v1/feed` |
| Follow user | `POST /api/v1/users/{userId}/follow` |
| Unfollow user | `DELETE /api/v1/users/{userId}/follow` |
| Like post | `POST /api/v1/posts/{postId}/likes` |
| Unlike post | `DELETE /api/v1/posts/{postId}/likes` |
| Comment on post | `POST /api/v1/posts/{postId}/comments` |

Define the request/response structure, authentication requirements, pagination, and error handling.

---

## 5. Core Design Challenges

### Challenge 1: Media Upload

Design an efficient image and video upload system.

Consider:

- How clients upload large media files.
- Where original media should be stored.
- How images are resized and optimized.
- How videos are transcoded.
- How failed uploads and processing failures are handled.
- How media is delivered globally.

### Challenge 2: Personalized Home Feed

Design a home feed that retrieves recent posts from accounts followed by a user.

Consider:

- How follower relationships are stored.
- How recent posts are retrieved.
- How feed latency can be reduced.
- How new posts become visible to followers.
- How feed ordering and pagination work.

### Challenge 3: Feed Generation at Scale

Compare different feed-generation approaches.

Consider:

- Fan-out on Write (Push Model)
- Fan-out on Read (Pull Model)
- Hybrid feed generation
- Accounts with millions of followers
- Read amplification versus write amplification

Explain the trade-offs and choose an approach appropriate for the workload.

### Challenge 4: Caching

Design a caching strategy for:

- Home feeds
- Post metadata
- Popular posts
- Engagement counts
- Media delivery

Explain cache invalidation, expiration, and recovery after cache failures.

### Challenge 5: Database Scaling

Design storage for:

- User profiles
- Posts and captions
- Follow relationships
- Likes and comments

Consider indexing, replication, partitioning, sharding, and hot partitions.

---

## 6. Reliability and Failure Scenarios

Explain how the system handles:

1. A media upload failing halfway through.
2. An image-processing worker crashing.
3. A feed-generation worker crashing after partially updating followers.
4. Redis becoming unavailable or losing cached feed entries.
5. Duplicate event delivery.
6. A celebrity publishing a post to millions of followers.
7. A database shard becoming overloaded.
8. Sudden spikes in likes or comments.
9. New posts arriving while a user is scrolling.

---

## 7. Interview Follow-Up Questions

1. How would you optimize home-feed retrieval for users following thousands of accounts?
2. How would you handle a celebrity with 50 million followers?
3. Why might you use object storage instead of storing images directly in a relational database?
4. What are the trade-offs between fan-out on write and fan-out on read?
5. How would you prevent duplicate feed entries during retries?
6. How would you scale the Likes Service for a viral post?
7. How would you prevent duplicate or missing posts during infinite scrolling?
8. How would you recover if the Redis feed cache is lost?
9. How would you shard the Posts Database?
10. How would you maintain feed correctness when users unfollow accounts or posts are deleted?

---

## 8. Expected Deliverables

A complete system design should include:

- Functional and non-functional requirements
- Capacity and storage estimates
- API definitions
- High-level architecture diagram
- Database schema and storage decisions
- Media upload and processing flow
- Home feed generation and retrieval flow
- Caching strategy
- Database sharding strategy
- Reliability and failure recovery
- Scalability bottlenecks and trade-offs

---

## 9. Interview Evaluation Criteria

The design will be evaluated on:

- **Requirements clarity:** Identifying core functionality and constraints.
- **Architecture:** Clear service boundaries and data flows.
- **Scalability:** Handling high read/write traffic and celebrity accounts.
- **Performance:** Minimizing feed latency and media-delivery overhead.
- **Reliability:** Recovering from failures without data loss or duplicate processing.
- **Trade-offs:** Explaining why specific technologies and approaches were selected.
- **Communication:** Presenting the design clearly and defending technical decisions.

---

## Goal

Design an Instagram-like platform that can efficiently support **100 million daily active users**, millions of daily media uploads, and personalized home feeds while maintaining low latency, high availability, durability, and horizontal scalability.

**Next document:** `SOLUTION.md` — Complete Instagram High-Level System Design Solution.
