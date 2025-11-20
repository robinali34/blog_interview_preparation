---
layout: post
title: "Meta System Design Interview Preparation Guide - Quick Reference"
date: 2025-11-20 14:00:00 -0000
categories: interview-preparation system-design meta facebook
tags: meta system-design interview preparation guide scalability distributed-systems
excerpt: "Comprehensive last-minute preparation guide for Meta system design interviews covering Meta's approach, common questions, step-by-step framework, and interview day tips."
---

# Meta System Design Interview Preparation Guide

A focused, actionable guide for preparing for Meta system design interviews. This guide covers Meta's interview format, common questions, step-by-step approach, and last-minute preparation strategies.

## Meta System Design Interview Format

### Interview Structure
- **Duration**: 45 minutes total
- **Design Exercise**: 35-40 minutes (remaining time for intro/outro/questions)
- **Format**: Video call with shared whiteboard/drawing tool
- **Focus**: Scalability, performance, reliability, trade-offs
- **Scale**: Design for billions of users (Meta scale)
- **Style**: Collaborative discussion, not a test

### What Meta Evaluates
- ✅ **Scalability**: Can it handle billions of users?
- ✅ **Performance**: Low latency, high throughput
- ✅ **Reliability**: Fault tolerance, availability
- ✅ **Trade-offs**: Understanding pros/cons of decisions
- ✅ **Communication**: Clear explanation of design
- ✅ **Problem-solving**: Breaking down complex problems

## Meta's System Design Approach

### Key Principles Meta Values
1. **Scale First**: Design for billions of users from the start
2. **Performance**: Low latency is critical (especially for social products)
3. **Reliability**: High availability (99.99%+)
4. **Efficiency**: Cost-effective at scale
5. **Iteration**: Start simple, evolve based on needs

### Meta-Specific Considerations
- **Social Graph**: Friend connections, social interactions
- **Real-time**: News feed, messaging, notifications
- **Content**: Photos, videos, posts (user-generated content)
- **Global Scale**: Worldwide users, data centers
- **Privacy & Safety**: Content moderation, user privacy

## Step-by-Step System Design Framework

**Total Design Time: 35-40 minutes**

### 1. Clarify Requirements (4-5 minutes)

**Ask These Questions:**
- What are the functional requirements?
- What are the non-functional requirements?
- What's the scale? (users, requests per second, data size)
- What are the read/write patterns?
- What are the consistency requirements?
- What are the latency requirements?
- What are the availability requirements?

**Example Clarification:**
- "Is this read-heavy or write-heavy?"
- "What's the expected QPS (queries per second)?"
- "What's the acceptable latency?"
- "Do we need strong consistency or eventual consistency?"
- "What's the data retention policy?"

---

### 2. High-Level Design (7-8 minutes)

**Draw the Big Picture:**
- Client (mobile app, web)
- Load balancer
- API servers
- Data stores (databases, caches)
- CDN (for static content)
- Message queues (for async processing)

**Key Components:**
```
[Client] → [Load Balancer] → [API Servers] → [Cache] → [Database]
                                    ↓
                              [Message Queue] → [Workers]
                                    ↓
                              [CDN] (for static content)
```

**Discuss:**
- Overall architecture
- Main components and their roles
- Data flow
- Key technologies (mention but don't dive deep yet)

---

### 3. Deep Dive into Components (15-18 minutes)

**For Each Component, Discuss:**

#### API Layer
- REST vs GraphQL
- Rate limiting
- Authentication/authorization
- API versioning

#### Database Design
- SQL vs NoSQL (when to use each)
- Database schema (if SQL)
- Sharding strategy
- Replication strategy
- Indexing strategy

#### Caching Strategy
- What to cache (hot data)
- Cache invalidation
- Cache layers (L1, L2, CDN)
- Cache eviction policies

#### Load Balancing
- Load balancing algorithms
- Health checks
- Session persistence (if needed)

#### Message Queue
- Why needed (async processing)
- What to queue (heavy operations)
- Queue processing (workers)

#### CDN
- What to serve via CDN (static content)
- Edge locations
- Cache invalidation

---

### 4. Scale the Design (6-7 minutes)

**Discuss Scaling Strategies:**

#### Horizontal Scaling
- Add more servers
- Stateless services
- Load balancing

#### Database Scaling
- Read replicas (for read-heavy)
- Sharding (partition data)
- Caching (reduce DB load)

#### Caching at Scale
- Multiple cache layers
- Distributed caching (Redis cluster)
- CDN for static content

#### Handle Traffic Spikes
- Auto-scaling
- Rate limiting
- Circuit breakers
- Graceful degradation

---

### 5. Address Edge Cases & Trade-offs (3-4 minutes)

**Discuss:**
- Single point of failure
- Data consistency
- Network partitions
- Failure scenarios
- Trade-offs (consistency vs availability, latency vs consistency)

---

## Common Meta System Design Questions

### High-Frequency Questions

1. **Design Facebook News Feed**
   - Social graph, ranking algorithm, real-time updates
   - Scale: billions of users, millions of posts per second

2. **Design Facebook Messenger / WhatsApp**
   - Real-time messaging, delivery guarantees, group chats
   - Scale: billions of messages per day

3. **Design Instagram / Photo Sharing**
   - Photo storage, feed generation, image processing
   - Scale: billions of photos, high read/write ratio

4. **Design Facebook Search**
   - Search indexing, ranking, autocomplete
   - Scale: billions of posts, low latency requirements

5. **Design Facebook Events**
   - Event creation, discovery, notifications
   - Scale: millions of events, location-based queries

6. **Design Facebook Groups**
   - Group management, content feed, permissions
   - Scale: millions of groups, varying sizes

7. **Design Facebook Stories**
   - Temporary content, high write rate, expiration
   - Scale: billions of stories per day

8. **Design a URL Shortener (like bit.ly)**
   - Short URL generation, redirects, analytics
   - Scale: billions of URLs, high read ratio

9. **Design a Distributed Cache**
   - Cache consistency, eviction, sharding
   - Scale: millions of requests per second

10. **Design a Notification System**
    - Multi-channel delivery, batching, rate limiting
    - Scale: billions of notifications per day

---

## Detailed Example: Design Facebook News Feed

### Step 1: Clarify Requirements

**Functional Requirements:**
- Users see posts from friends/pages they follow
- Posts appear in reverse chronological order (or ranked)
- Users can like, comment, share
- Real-time updates (new posts appear)

**Non-Functional Requirements:**
- Scale: 2B+ users, 500M+ daily active users
- QPS: ~1M reads/sec, ~100K writes/sec
- Latency: <200ms for feed generation
- Availability: 99.99%

**Clarifying Questions:**
- "Should posts be ranked or chronological?" → Ranked by relevance
- "How many posts per feed?" → 20-30 posts initially
- "Do we need real-time updates?" → Yes, but can be eventual
- "What about ads?" → Include sponsored posts

---

### Step 2: High-Level Design

```
[Client] → [Load Balancer] → [Feed Service] → [Cache] → [Database]
                                    ↓
                              [Ranking Service]
                                    ↓
                              [Graph Service] (friend list)
                                    ↓
                              [Post Service] (post storage)
```

**Components:**
1. **Feed Service**: Generates feed for users
2. **Graph Service**: Manages social graph (friends, follows)
3. **Post Service**: Stores and retrieves posts
4. **Ranking Service**: Ranks posts by relevance
5. **Cache**: Caches feed and hot data
6. **Database**: Stores posts, user data, social graph

---

### Step 3: Deep Dive

#### Database Design

**Posts Table:**
- post_id (PK)
- user_id
- content
- timestamp
- likes_count
- comments_count

**Social Graph:**
- user_id (PK)
- friend_id (PK)
- relationship_type
- created_at

**Sharding Strategy:**
- Shard posts by user_id (posts by same user together)
- Shard social graph by user_id

**Indexing:**
- Index on (user_id, timestamp) for posts
- Index on user_id for social graph

#### Caching Strategy

**Multi-layer Caching:**
1. **Feed Cache**: Cache generated feeds (TTL: 5 minutes)
   - Key: user_id, Value: list of post_ids
2. **Post Cache**: Cache hot posts (TTL: 1 hour)
   - Key: post_id, Value: post data
3. **Graph Cache**: Cache friend lists (TTL: 15 minutes)
   - Key: user_id, Value: list of friend_ids

**Cache Invalidation:**
- Invalidate feed cache when user posts
- Invalidate post cache when post is updated

#### Feed Generation Algorithm

**Pull Model (for small number of friends):**
- Fetch friend list
- Fetch recent posts from friends
- Rank and return

**Push Model (for active users):**
- Pre-compute feed when user posts
- Store in cache
- Serve from cache

**Hybrid Approach:**
- Push for active users (most users)
- Pull for inactive users (rare)

#### Ranking Algorithm

**Factors:**
- Recency (newer posts ranked higher)
- Engagement (likes, comments, shares)
- Relationship strength (close friends vs acquaintances)
- Content type (photos/videos vs text)

**Formula (simplified):**
```
Score = (recency_weight * recency_score) + 
        (engagement_weight * engagement_score) + 
        (relationship_weight * relationship_score)
```

---

### Step 4: Scale the Design

#### Handle High Read Traffic
- **Read Replicas**: Multiple DB replicas for reads
- **Caching**: Aggressive caching (feed cache, post cache)
- **CDN**: For static content (images, videos)

#### Handle High Write Traffic
- **Sharding**: Shard posts by user_id
- **Async Processing**: Queue heavy operations (analytics, notifications)
- **Write-through Cache**: Update cache on write

#### Handle Traffic Spikes
- **Auto-scaling**: Scale servers based on load
- **Rate Limiting**: Limit requests per user
- **Graceful Degradation**: Serve cached feed if ranking service is down

#### Geographic Distribution
- **Multi-region**: Deploy in multiple regions
- **Data Replication**: Replicate data across regions
- **Edge Caching**: Cache at edge locations

---

### Step 5: Trade-offs & Edge Cases

**Trade-offs:**
- **Consistency vs Availability**: Eventual consistency acceptable for feed
- **Latency vs Freshness**: Cache trade-off (stale but fast)
- **Storage vs Compute**: Pre-compute feeds vs compute on-demand

**Edge Cases:**
- **New User**: No friends, show trending/popular posts
- **User with Many Friends**: Use ranking to limit posts
- **Post Deletion**: Invalidate cache, remove from feed
- **Network Partition**: Serve stale feed from cache

---

## Quick Reference: System Design Components

### Databases

**SQL (PostgreSQL, MySQL):**
- Use when: Need ACID, complex queries, relationships
- Examples: User data, transactions, relational data

**NoSQL:**

**Key-Value (Redis, DynamoDB):**
- Use when: Simple lookups, caching, session storage
- Examples: Cache, user sessions

**Document (MongoDB):**
- Use when: Flexible schema, document storage
- Examples: User profiles, posts

**Column (Cassandra):**
- Use when: High write throughput, wide tables
- Examples: Time-series data, logs

**Graph (Neo4j):**
- Use when: Complex relationships, graph queries
- Examples: Social graph, recommendations

---

### Caching

**Cache Layers:**
1. **Application Cache**: In-memory (local)
2. **Distributed Cache**: Redis, Memcached
3. **CDN**: CloudFront, Cloudflare (static content)

**Cache Strategies:**
- **Cache-aside**: App checks cache, fetches from DB if miss
- **Write-through**: Write to cache and DB simultaneously
- **Write-behind**: Write to cache, async write to DB
- **Refresh-ahead**: Proactively refresh before expiration

**Cache Eviction:**
- **LRU**: Least Recently Used
- **LFU**: Least Frequently Used
- **TTL**: Time To Live

---

### Load Balancing

**Algorithms:**
- **Round Robin**: Distribute evenly
- **Least Connections**: Send to server with fewest connections
- **IP Hash**: Consistent hashing (for session persistence)
- **Weighted**: Based on server capacity

**Health Checks:**
- Ping health endpoint
- Remove unhealthy servers
- Add back when healthy

---

### Message Queues

**Use Cases:**
- Async processing (image processing, notifications)
- Decoupling services
- Handling traffic spikes

**Options:**
- **RabbitMQ**: Feature-rich, complex
- **Kafka**: High throughput, event streaming
- **SQS**: AWS managed, simple
- **Redis Pub/Sub**: Simple, fast

---

### CDN (Content Delivery Network)

**Use For:**
- Static content (images, videos, CSS, JS)
- Reduce latency
- Reduce origin server load

**How It Works:**
- Cache content at edge locations
- Serve from nearest location
- Invalidate on update

---

## Last-Minute Preparation Checklist

### Tonight (Before Interview)

- [ ] **Review System Design Basics**: Databases, caching, load balancing
- [ ] **Practice Drawing**: Use whiteboard tool, practice drawing components
- [ ] **Review Meta Products**: News Feed, Messenger, Instagram, etc.
- [ ] **Practice One Full Design**: Time yourself (35-40 minutes for design portion)
- [ ] **Review Common Questions**: News Feed, Messenger, Search, etc.
- [ ] **Prepare Questions**: Questions to ask interviewer

### Key Concepts to Review

- [ ] **CAP Theorem**: Consistency, Availability, Partition tolerance
- [ ] **ACID vs BASE**: Database consistency models
- [ ] **Sharding Strategies**: Horizontal partitioning
- [ ] **Replication**: Master-slave, master-master
- [ ] **Caching**: Strategies, eviction policies
- [ ] **Load Balancing**: Algorithms, health checks
- [ ] **Message Queues**: Async processing, decoupling
- [ ] **CDN**: Content delivery, edge caching

### Meta-Specific to Review

- [ ] **Social Graph**: Friend connections, graph databases
- [ ] **Real-time Systems**: WebSockets, server-sent events
- [ ] **Content Moderation**: Safety, spam detection
- [ ] **Ranking Algorithms**: Relevance, personalization
- [ ] **Scale**: Billions of users, millions of QPS

---

## Interview Day Tips

### Before the Interview (30 minutes before)

- [ ] **Review Framework**: Step-by-step approach
- [ ] **Practice Drawing**: Quick component diagrams
- [ ] **Review Meta Products**: Recent features, scale
- [ ] **Prepare Questions**: 2-3 thoughtful questions
- [ ] **Test Tech**: Whiteboard tool, video, audio

### During the Interview

1. **Start with Clarification**
   - Ask questions about requirements
   - Understand scale and constraints
   - Don't assume - clarify!

2. **Think Out Loud**
   - Explain your thought process
   - Discuss trade-offs
   - Show your reasoning

3. **Draw Clearly**
   - Use clear labels
   - Show data flow
   - Keep it organized

4. **Discuss Trade-offs**
   - Every decision has trade-offs
   - Explain pros and cons
   - Justify your choices

5. **Scale Gradually**
   - Start simple
   - Add complexity as needed
   - Show you can evolve the design

6. **Handle Feedback**
   - Listen to interviewer's suggestions
   - Incorporate feedback
   - Show you can collaborate

### Common Mistakes to Avoid

1. ❌ **Jumping to Solutions**: Clarify requirements first
2. ❌ **Over-engineering**: Start simple, scale as needed
3. ❌ **Ignoring Scale**: Always consider Meta scale (billions)
4. ❌ **Not Discussing Trade-offs**: Every decision has trade-offs
5. ❌ **Not Drawing**: Visuals help communication
6. ❌ **Being Rigid**: Be open to feedback and changes
7. ❌ **Forgetting Edge Cases**: Discuss failure scenarios

---

## Quick Reference: Numbers to Remember

### Scale Estimates

**Users:**
- Daily Active Users: 500M - 2B+
- Total Users: 2B - 3B+

**Traffic:**
- Reads: 1M - 10M QPS
- Writes: 100K - 1M QPS

**Data:**
- Posts per day: 100M - 500M
- Photos per day: 1B+
- Messages per day: 100B+

**Storage:**
- Post size: ~1 KB average
- Photo size: ~200 KB average
- Video size: ~5 MB average

**Latency:**
- Feed generation: <200ms
- API response: <100ms
- Database query: <50ms

**Availability:**
- Target: 99.99% (4 nines)
- Downtime: ~52 minutes per year

---

## Framework Cheat Sheet

### 1. Clarify (5 min)
- Functional requirements
- Non-functional requirements
- Scale (users, QPS, data)
- Read/write patterns
- Consistency requirements
- Latency requirements

### 2. High-Level Design (10 min)
- Draw main components
- Show data flow
- Discuss overall architecture
- Mention key technologies

### 3. Deep Dive (20-25 min)
- Database design (schema, sharding, indexing)
- Caching strategy (what, where, how)
- API design (endpoints, authentication)
- Load balancing
- Message queues
- CDN

### 4. Scale (10 min)
- Horizontal scaling
- Database scaling (replicas, sharding)
- Caching at scale
- Handle traffic spikes
- Geographic distribution

### 5. Trade-offs & Edge Cases (5 min)
- Single point of failure
- Consistency vs availability
- Latency vs freshness
- Failure scenarios

---

## Questions to Ask Interviewer

### About Requirements
- "What's the expected scale?"
- "What are the latency requirements?"
- "Do we need strong consistency or eventual consistency?"
- "What are the read/write patterns?"

### About Constraints
- "Are there any technology constraints?"
- "What's the budget consideration?"
- "What's the timeline?"
- "Are there any compliance requirements?"

### About the Role
- "What kind of systems does the team work on?"
- "What are the biggest technical challenges?"
- "How does the team approach system design?"

---

## Practice Problems (Quick Review)

### Easy
1. **URL Shortener**: Simple key-value, redirects
2. **Rate Limiter**: Token bucket, sliding window
3. **Counter**: Distributed counting

### Medium
1. **News Feed**: Social graph, ranking, caching
2. **Messenger**: Real-time, delivery guarantees
3. **Search**: Indexing, ranking, autocomplete

### Hard
1. **Distributed Cache**: Consistency, sharding, eviction
2. **Video Streaming**: CDN, transcoding, adaptive bitrate
3. **Social Network**: Graph, feed, real-time updates

---

## Final Checklist - Right Before Interview

### 5 Minutes Before
- [ ] Take deep breaths
- [ ] Review framework steps mentally
- [ ] Review key numbers (scale, latency)
- [ ] Have whiteboard tool ready
- [ ] Smile and be confident

### During Interview
- [ ] Clarify requirements first
- [ ] Think out loud
- [ ] Draw clearly
- [ ] Discuss trade-offs
- [ ] Scale gradually
- [ ] Handle feedback gracefully

### After Interview
- [ ] Send thank you note
- [ ] Reflect on what went well
- [ ] Note areas to improve
- [ ] Follow up as needed

---

## Conclusion

Meta system design interviews focus on:
- **Scale**: Design for billions of users
- **Performance**: Low latency, high throughput
- **Reliability**: High availability, fault tolerance
- **Trade-offs**: Understanding pros/cons
- **Communication**: Clear explanation

**Key Success Factors:**
1. ✅ Clarify requirements first
2. ✅ Start simple, scale gradually
3. ✅ Think out loud
4. ✅ Discuss trade-offs
5. ✅ Handle feedback gracefully
6. ✅ Consider Meta scale (billions)

**Remember**: It's a collaborative discussion, not a test. Show your thought process, discuss trade-offs, and be open to feedback.

Good luck with your interview tomorrow! You've got this! 🚀

---

**Related Posts:**
- [Meta Behavioral Interview Preparation Guide]({{ site.baseurl }}{% post_url 2025-11-20-meta-behavioral-interview-preparation-guide %})
- [Meta (Facebook) Behavioral Interview Guide]({{ site.baseurl }}{% post_url 2025-11-13-meta-behavioral-interviews %})
- [Behavioral Interview Preparation Guide]({{ site.baseurl }}{% post_url 2025-11-13-behavioral-interview-preparation-guide %})

