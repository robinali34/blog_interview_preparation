---
layout: post
title: "Google System Design Interview Preparation - Top Questions by Category"
date: 2025-12-11 14:00:00 -0000
categories: interview-preparation system-design google
tags: google system-design interview preparation guide scalability distributed-systems search
excerpt: "Comprehensive guide to Google system design interviews covering Google's approach, categorized top questions, step-by-step framework, and interview day tips."
---

# Google System Design Interview Preparation - Top Questions by Category

A comprehensive guide for preparing for Google system design interviews. This guide covers Google's interview format, categorized common questions, step-by-step approach, and last-minute preparation strategies.

## Google System Design Interview Format

### Interview Structure
- **Duration**: 45 minutes total
- **Design Exercise**: 35-40 minutes (remaining time for intro/outro/questions)
- **Format**: Video call with shared whiteboard/drawing tool
- **Focus**: Scalability, performance, reliability, trade-offs, distributed systems
- **Scale**: Design for billions of users (Google scale)
- **Style**: Collaborative discussion, emphasis on scalability and distributed systems

### What Google Evaluates
- ✅ **Scalability**: Can it handle billions of users and petabytes of data?
- ✅ **Performance**: Low latency, high throughput, efficient algorithms
- ✅ **Reliability**: Fault tolerance, high availability (99.99%+)
- ✅ **Distributed Systems**: Understanding of distributed systems concepts
- ✅ **Trade-offs**: Deep understanding of pros/cons of decisions
- ✅ **Communication**: Clear explanation of design and reasoning
- ✅ **Problem-solving**: Breaking down complex problems systematically

## Google's System Design Approach

### Key Principles Google Values
1. **Scale First**: Design for billions of users and petabytes of data
2. **Distributed Systems**: Understanding of CAP theorem, consistency models
3. **Performance**: Low latency is critical (especially for search)
4. **Reliability**: High availability, fault tolerance, graceful degradation
5. **Efficiency**: Cost-effective at scale, resource optimization
6. **Data-Driven**: Use data to make design decisions

### Google-Specific Considerations
- **Search Systems**: Indexing, ranking, crawling, query processing
- **Distributed Storage**: Bigtable, Spanner, distributed databases
- **Cloud Infrastructure**: GCP services, microservices, containers
- **Real-time Systems**: Streaming data, real-time analytics
- **Global Scale**: Worldwide users, multi-region deployment
- **Data Processing**: MapReduce, data pipelines, analytics

## Step-by-Step System Design Framework

**Total Design Time: 35-40 minutes**

### 1. Clarify Requirements (4-5 minutes)

**Ask These Questions:**

**Functional Requirements:**
- What are the core features?
- What are the use cases?
- What are the input/output formats?
- What are the constraints?

**Non-Functional Requirements:**
- What's the scale? (users, QPS, data size, storage)
- What are the read/write patterns? (read-heavy, write-heavy, balanced)
- What are the consistency requirements? (strong, eventual)
- What are the latency requirements? (p50, p95, p99)
- What are the availability requirements? (99.9%, 99.99%, 99.999%)
- What are the durability requirements? (data retention, backup)

**Example Clarification:**
- "Is this read-heavy or write-heavy?"
- "What's the expected QPS (queries per second)?"
- "What's the acceptable latency? (p50, p95, p99)"
- "Do we need strong consistency or eventual consistency?"
- "What's the data retention policy?"
- "What are the geographic requirements? (single region, multi-region)"
- "What's the expected data growth rate?"

---

### 2. High-Level Design (7-8 minutes)

**Draw the Big Picture:**
- Client (mobile app, web, API clients)
- Load balancer / API Gateway
- Application servers (microservices)
- Data stores (databases, caches, object storage)
- CDN (for static content)
- Message queues (for async processing)
- Search/indexing systems (if applicable)
- Analytics/monitoring

**Key Components:**
```
[Client] → [Load Balancer] → [API Gateway] → [Microservices] → [Cache] → [Database]
                                    ↓                              ↓
                              [Message Queue] → [Workers]    [Object Storage]
                                    ↓
                              [CDN] (for static content)
                                    ↓
                              [Search/Index] (if needed)
```

**Discuss:**
- Overall architecture (monolithic vs microservices)
- Main components and their roles
- Data flow (request/response paths)
- Key technologies (mention but don't dive deep yet)
- High-level trade-offs

---

### 3. Deep Dive into Components (15-18 minutes)

**For Each Component, Discuss:**

#### API Layer / Microservices
- REST vs gRPC vs GraphQL
- API Gateway (routing, rate limiting, authentication)
- Service discovery and load balancing
- Circuit breakers and retries
- API versioning

#### Database Design
- **SQL vs NoSQL**: When to use each
  - SQL: ACID transactions, complex queries, relationships
  - NoSQL: High write throughput, flexible schema, horizontal scaling
- **Database Schema**: Tables, indexes, relationships (if SQL)
- **Sharding Strategy**: 
  - Horizontal sharding (by user_id, hash, range)
  - Vertical sharding (by feature)
  - Consistent hashing
- **Replication Strategy**:
  - Master-slave (read replicas)
  - Master-master (multi-master)
  - Quorum-based (distributed consensus)
- **Indexing Strategy**: What to index, composite indexes
- **Partitioning**: Range, hash, list partitioning

#### Caching Strategy
- **What to Cache**: Hot data, frequently accessed data
- **Cache Layers**: 
  - L1: Application cache (in-memory)
  - L2: Distributed cache (Redis, Memcached)
  - L3: CDN (static content)
- **Cache Invalidation**: 
  - Write-through
  - Write-behind
  - Cache-aside
  - TTL-based
- **Cache Eviction Policies**: LRU, LFU, FIFO
- **Cache Consistency**: How to keep cache and DB in sync

#### Load Balancing
- **Algorithms**: Round robin, least connections, IP hash, weighted
- **Health Checks**: Active/passive health checks
- **Session Persistence**: Sticky sessions vs stateless
- **Geographic Load Balancing**: Route to nearest data center

#### Message Queue
- **Why Needed**: Async processing, decoupling, handling spikes
- **What to Queue**: Heavy operations, notifications, analytics
- **Queue Processing**: Workers, consumer groups, parallelism
- **Options**: Kafka, RabbitMQ, Pub/Sub, SQS
- **Reliability**: At-least-once vs exactly-once delivery

#### CDN (Content Delivery Network)
- **Use For**: Static content (images, videos, CSS, JS)
- **Benefits**: Reduce latency, reduce origin server load
- **How It Works**: Cache at edge locations, serve from nearest
- **Cache Invalidation**: On content update

#### Search/Indexing (if applicable)
- **Full-text Search**: Elasticsearch, Solr
- **Indexing Strategy**: What to index, update frequency
- **Ranking Algorithm**: Relevance scoring
- **Distributed Search**: Sharding indexes

---

### 4. Scale the Design (6-7 minutes)

**Discuss Scaling Strategies:**

#### Horizontal Scaling
- Add more servers (stateless services)
- Load balancing across servers
- Auto-scaling based on load

#### Database Scaling
- **Read Scaling**: Read replicas, caching
- **Write Scaling**: Sharding, partitioning
- **Connection Pooling**: Manage database connections
- **Database Federation**: Split by feature/service

#### Caching at Scale
- Multiple cache layers
- Distributed caching (Redis cluster)
- CDN for static content
- Cache warming strategies

#### Handle Traffic Spikes
- **Auto-scaling**: Scale up/down based on load
- **Rate Limiting**: Per user, per IP, per API key
- **Circuit Breakers**: Fail fast when downstream is down
- **Graceful Degradation**: Serve reduced functionality
- **Queue Buffering**: Use queues to handle spikes

#### Geographic Distribution
- **Multi-region Deployment**: Deploy in multiple regions
- **Data Replication**: Replicate data across regions
- **Edge Caching**: Cache at edge locations
- **Route to Nearest**: Route users to nearest data center

#### Handle Large Data
- **Partitioning**: Partition large datasets
- **Compression**: Compress data at rest and in transit
- **Archival**: Move old data to cold storage
- **Data Lifecycle**: Hot → Warm → Cold storage

---

### 5. Address Edge Cases & Trade-offs (3-4 minutes)

**Discuss:**

#### Single Point of Failure
- Identify SPOFs
- Add redundancy
- Failover mechanisms

#### Data Consistency
- **CAP Theorem**: Consistency vs Availability vs Partition tolerance
- **Consistency Models**: 
  - Strong consistency
  - Eventual consistency
  - Weak consistency
- **When to Use Each**: Based on use case

#### Network Partitions
- What happens during partition?
- How to handle split-brain?
- Quorum-based decisions

#### Failure Scenarios
- Server failures
- Database failures
- Network failures
- Data center failures
- How to handle gracefully?

#### Trade-offs
- **Consistency vs Availability**: Choose based on use case
- **Latency vs Consistency**: Can accept stale data?
- **Cost vs Performance**: More servers vs optimization
- **Complexity vs Scalability**: Simple vs scalable

---

## Common Google System Design Questions by Category

### Category 1: Search & Indexing Systems

#### Very High Frequency ⭐⭐⭐

1. **Design Google Search**
   - Web crawling, indexing, ranking, query processing
   - Scale: Billions of web pages, millions of queries per second
   - Key: Distributed crawling, inverted index, PageRank algorithm

2. **Design a Web Crawler**
   - URL discovery, crawling, parsing, storage
   - Scale: Billions of URLs, distributed crawling
   - Key: URL frontier, politeness, deduplication, distributed workers

3. **Design Google Autocomplete**
   - Query suggestions, ranking, low latency
   - Scale: Millions of queries, <100ms latency
   - Key: Trie, caching, ranking by popularity

#### High Frequency ⭐⭐

4. **Design a Search Engine**
   - Indexing, ranking, query processing
   - Scale: Millions of documents, thousands of QPS
   - Key: Inverted index, ranking algorithm, distributed search

5. **Design Google Image Search**
   - Image indexing, reverse image search, similarity matching
   - Scale: Billions of images, millions of queries
   - Key: Image features, similarity search, distributed storage

6. **Design Google News Feed**
   - News aggregation, ranking, personalization
   - Scale: Millions of articles, real-time updates
   - Key: Content aggregation, ranking algorithm, personalization

---

### Category 2: Distributed Storage Systems

#### Very High Frequency ⭐⭐⭐

7. **Design a Distributed File System (like GFS)**
   - File storage, replication, fault tolerance
   - Scale: Petabytes of data, millions of files
   - Key: Chunk servers, master node, replication, consistency

8. **Design a Distributed Database (like Bigtable)**
   - Wide-column store, sharding, replication
   - Scale: Petabytes of data, millions of QPS
   - Key: Tablets, SSTables, compression, distributed consensus

9. **Design a Key-Value Store (like DynamoDB)**
   - Key-value storage, partitioning, replication
   - Scale: Billions of keys, millions of QPS
   - Key: Consistent hashing, vector clocks, eventual consistency

#### High Frequency ⭐⭐

10. **Design a Distributed Cache**
    - Distributed caching, consistency, eviction
    - Scale: Terabytes of cache, millions of QPS
    - Key: Consistent hashing, cache invalidation, replication

11. **Design Google Drive / Dropbox**
    - File storage, sync, versioning, sharing
    - Scale: Billions of files, petabytes of storage
    - Key: Chunking, deduplication, conflict resolution, sync

12. **Design a Time-Series Database**
    - Time-series data storage, compression, queries
    - Scale: Billions of data points, high write rate
    - Key: Time-based partitioning, compression, downsampling

---

### Category 3: Real-Time & Streaming Systems

#### Very High Frequency ⭐⭐⭐

13. **Design Google Analytics**
    - Event tracking, aggregation, real-time analytics
    - Scale: Billions of events per day, real-time queries
    - Key: Event streaming, aggregation pipelines, time windows

14. **Design a Real-Time Chat System (like Google Chat)**
    - Real-time messaging, presence, notifications
    - Scale: Millions of concurrent users, low latency
    - Key: WebSockets, message queues, presence system

15. **Design a Real-Time Notification System**
    - Push notifications, multi-channel delivery
    - Scale: Billions of notifications per day
    - Key: Message queues, batching, delivery guarantees

#### High Frequency ⭐⭐

16. **Design a Real-Time Leaderboard**
    - Real-time ranking, updates, queries
    - Scale: Millions of users, high update rate
    - Key: Sorted sets, incremental updates, caching

17. **Design a Real-Time Collaboration Tool (like Google Docs)**
    - Real-time editing, conflict resolution, presence
    - Scale: Thousands of concurrent editors per document
    - Key: Operational transforms, CRDTs, WebSockets

18. **Design a Stream Processing System**
    - Stream ingestion, processing, aggregation
    - Scale: Millions of events per second
    - Key: Stream processing frameworks, windowing, state management

---

### Category 4: Social & Content Systems

#### Very High Frequency ⭐⭐⭐

19. **Design YouTube**
    - Video upload, storage, streaming, recommendations
    - Scale: Billions of videos, petabytes of storage
    - Key: Video encoding, CDN, recommendation algorithm

20. **Design Google Photos**
    - Photo storage, search, sharing, face recognition
    - Scale: Trillions of photos, petabytes of storage
   - Key: Image storage, metadata indexing, face recognition, deduplication

#### High Frequency ⭐⭐

21. **Design a Social Media Feed**
    - Feed generation, ranking, real-time updates
    - Scale: Billions of posts, millions of users
    - Key: Fan-out, ranking algorithm, caching

22. **Design a Content Delivery Network (CDN)**
    - Content caching, edge servers, routing
    - Scale: Terabytes of content, global distribution
    - Key: Edge locations, cache invalidation, routing

23. **Design a Video Streaming Service**
    - Video encoding, storage, streaming, adaptive bitrate
    - Scale: Millions of concurrent viewers
    - Key: Video encoding, CDN, adaptive streaming

---

### Category 5: Communication Systems

#### Very High Frequency ⭐⭐⭐

24. **Design Gmail**
    - Email storage, search, sending, receiving
    - Scale: Billions of emails, petabytes of storage
    - Key: Email storage, search indexing, spam filtering

25. **Design WhatsApp / Messaging System**
    - Real-time messaging, group chats, media sharing
    - Scale: Billions of messages per day, low latency
    - Key: Message queues, presence, media storage

#### High Frequency ⭐⭐

26. **Design a Video Conferencing System (like Google Meet)**
    - Video/audio streaming, screen sharing, recording
    - Scale: Millions of concurrent participants
    - Key: WebRTC, media servers, bandwidth optimization

27. **Design a Voice Assistant (like Google Assistant)**
    - Speech recognition, NLP, response generation
    - Scale: Millions of queries per day, low latency
    - Key: Speech-to-text, NLP pipeline, response generation

---

### Category 6: E-Commerce & Marketplace

#### High Frequency ⭐⭐

28. **Design Amazon / E-Commerce Platform**
    - Product catalog, search, recommendations, checkout
    - Scale: Millions of products, millions of users
    - Key: Product search, recommendation engine, payment processing

29. **Design Uber / Ride-Sharing Service**
    - Matching drivers and riders, real-time tracking, pricing
    - Scale: Millions of rides per day, real-time matching
    - Key: Geospatial indexing, real-time matching, surge pricing

30. **Design a Food Delivery System**
    - Restaurant listing, ordering, delivery tracking
    - Scale: Millions of orders per day, real-time tracking
    - Key: Order management, delivery optimization, real-time updates

---

### Category 7: Infrastructure & Platform Systems

#### Very High Frequency ⭐⭐⭐

31. **Design a URL Shortener (like bit.ly)**
    - Short URL generation, redirects, analytics
    - Scale: Billions of URLs, high read ratio
    - Key: Base62 encoding, distributed ID generation, caching

32. **Design a Rate Limiter**
    - Rate limiting, throttling, distributed rate limiting
    - Scale: Millions of requests per second
    - Key: Token bucket, sliding window, distributed counters

33. **Design a Distributed Lock Service**
    - Distributed locking, leader election
    - Scale: High concurrency, low latency
    - Key: Consensus algorithms, lease-based locks

#### High Frequency ⭐⭐

34. **Design a Monitoring System**
    - Metrics collection, storage, alerting, visualization
    - Scale: Billions of metrics per day
    - Key: Time-series database, aggregation, alerting rules

35. **Design a Logging System**
    - Log collection, storage, search, aggregation
    - Scale: Terabytes of logs per day
    - Key: Log aggregation, indexing, search

36. **Design a Configuration Management System**
    - Configuration storage, updates, distribution
    - Scale: Millions of services, real-time updates
    - Key: Versioning, change propagation, consistency

---

### Category 8: Data Processing & Analytics

#### High Frequency ⭐⭐

37. **Design a MapReduce System**
    - Distributed data processing, map/reduce phases
    - Scale: Petabytes of data, thousands of workers
    - Key: Data partitioning, fault tolerance, shuffle phase

38. **Design a Data Pipeline**
    - ETL pipeline, data transformation, scheduling
    - Scale: Terabytes of data per day
    - Key: Pipeline stages, fault tolerance, monitoring

39. **Design a Data Warehouse**
    - Data storage, ETL, querying, analytics
    - Scale: Petabytes of data, complex queries
    - Key: Columnar storage, partitioning, indexing

---

### Category 9: Gaming & Entertainment

#### Medium Frequency

40. **Design a Gaming Platform**
    - Game state management, multiplayer, leaderboards
    - Scale: Millions of concurrent players, low latency
    - Key: Game state synchronization, real-time updates

41. **Design a Live Streaming Platform**
    - Video streaming, chat, donations, moderation
    - Scale: Millions of concurrent viewers
    - Key: Video streaming, real-time chat, moderation

---

### Category 10: Specialized Systems

#### High Frequency ⭐⭐

42. **Design a Distributed Counter**
    - Distributed counting, high write rate
    - Scale: Millions of increments per second
    - Key: Sharding, eventual consistency, aggregation

43. **Design a Distributed Task Scheduler**
    - Task scheduling, execution, monitoring
    - Scale: Millions of tasks per day
    - Key: Task queues, worker pools, fault tolerance

44. **Design a Distributed Tracing System**
    - Request tracing across services, visualization
    - Scale: Billions of traces per day
    - Key: Trace collection, storage, querying

---

## Google-Specific High-Frequency Questions

### Must-Know Questions (⭐⭐⭐ Very High Frequency)

These questions appear frequently in Google interviews:

1. **Design Google Search** - Web crawling, indexing, ranking
2. **Design a Web Crawler** - Distributed crawling, URL frontier
3. **Design Google Autocomplete** - Trie, caching, ranking
4. **Design a Distributed File System** - GFS concepts, chunk servers
5. **Design a Distributed Database** - Bigtable concepts, tablets, SSTables
6. **Design a Key-Value Store** - Consistent hashing, replication
7. **Design Google Analytics** - Event streaming, aggregation
8. **Design a Real-Time Chat System** - WebSockets, message queues
9. **Design YouTube** - Video storage, streaming, CDN
10. **Design Gmail** - Email storage, search, spam filtering
11. **Design a URL Shortener** - Base62 encoding, distributed IDs
12. **Design a Rate Limiter** - Token bucket, sliding window

---

## Detailed Example: Design Google Search

### Step 1: Clarify Requirements

**Functional Requirements:**
- Users can search the web
- Return relevant results ranked by relevance
- Support autocomplete/suggestions
- Handle spelling corrections
- Support advanced search (filters, operators)

**Non-Functional Requirements:**
- Scale: Billions of web pages indexed, millions of queries per second
- Latency: <100ms for search results (p95)
- Availability: 99.99%
- Freshness: Index updated regularly (daily/hourly)

**Clarifying Questions:**
- "What's the expected QPS?" → 10M QPS peak
- "How many web pages?" → 50+ billion pages
- "What's acceptable latency?" → <100ms p95
- "How fresh should results be?" → Daily updates acceptable
- "Do we need personalization?" → Yes, but optional

---

### Step 2: High-Level Design

```
[User] → [Load Balancer] → [Query Servers] → [Index Servers] → [Document Servers]
              ↓                    ↓                ↓
         [Autocomplete]      [Ranking]        [Crawlers] → [Web]
              ↓                    ↓                ↓
         [Cache]              [Cache]          [URL Frontier]
```

**Components:**
1. **Crawler**: Crawls web pages
2. **Indexer**: Builds inverted index
3. **Query Server**: Handles user queries
4. **Ranking Server**: Ranks results
5. **Document Server**: Stores document content

---

### Step 3: Deep Dive

#### Web Crawler

**Components:**
- **URL Frontier**: Queue of URLs to crawl
- **Crawler Workers**: Distributed workers that fetch pages
- **Parser**: Extracts links and content
- **Deduplication**: Avoid crawling same URL twice
- **Politeness**: Respect robots.txt, rate limiting

**Scale:**
- Billions of URLs
- Distributed across thousands of workers
- URL Frontier: Priority queue (BFS, importance-based)

#### Indexing System

**Inverted Index:**
- **Term → [doc_id, positions, frequency]**
- Sharded by term (hash of term)
- Distributed across index servers

**Index Structure:**
- **Forward Index**: doc_id → terms (for ranking)
- **Inverted Index**: term → [doc_ids] (for search)
- **Compression**: Compress postings lists

**Scale:**
- Billions of documents
- Trillions of terms
- Petabytes of index data
- Sharded across thousands of index servers

#### Query Processing

**Query Flow:**
1. Parse query (extract terms)
2. Lookup terms in inverted index
3. Intersect postings lists (AND queries)
4. Rank results
5. Return top K results

**Optimization:**
- Cache frequent queries
- Early termination (stop after top K)
- Skip lists for fast intersection

#### Ranking Algorithm

**Factors:**
- **Term Frequency (TF)**: How often term appears in doc
- **Inverse Document Frequency (IDF)**: How rare the term is
- **PageRank**: Link-based importance
- **Freshness**: How recent the page is
- **User Signals**: Click-through rate, dwell time

**Formula (simplified):**
```
Score = TF * IDF * PageRank * Freshness * UserSignals
```

---

### Step 4: Scale the Design

#### Handle High Query Volume
- **Query Servers**: Horizontal scaling, load balancing
- **Caching**: Cache frequent queries (80% hit rate)
- **CDN**: Cache static content, autocomplete suggestions

#### Handle Large Index
- **Sharding**: Shard index by term hash
- **Replication**: Replicate index servers for availability
- **Compression**: Compress postings lists

#### Handle Crawling at Scale
- **Distributed Crawlers**: Thousands of crawler workers
- **URL Frontier**: Distributed priority queue
- **Deduplication**: Bloom filters, distributed hash tables

#### Geographic Distribution
- **Multi-region**: Deploy in multiple regions
- **Route to Nearest**: Route queries to nearest data center
- **Index Replication**: Replicate index across regions

---

### Step 5: Trade-offs & Edge Cases

**Trade-offs:**
- **Freshness vs Cost**: More frequent crawling = higher cost
- **Latency vs Accuracy**: Faster results vs better ranking
- **Storage vs Speed**: Compressed index vs faster queries

**Edge Cases:**
- **New Pages**: Not yet indexed (eventual consistency)
- **Crawler Failures**: Retry mechanism, checkpointing
- **Index Updates**: How to update index without downtime
- **Spam**: Filter spam pages from results

---

## Quick Reference: System Design Components

### Databases

**SQL (PostgreSQL, MySQL, Cloud SQL):**
- Use when: ACID transactions, complex queries, relationships
- Examples: User data, transactions, relational data

**NoSQL:**

**Key-Value (Redis, Memcached, Cloud Memorystore):**
- Use when: Simple lookups, caching, session storage
- Examples: Cache, user sessions, counters

**Document (MongoDB, Firestore):**
- Use when: Flexible schema, document storage
- Examples: User profiles, content, JSON data

**Column (Cassandra, Bigtable):**
- Use when: High write throughput, wide tables
- Examples: Time-series data, logs, analytics

**Graph (Neo4j):**
- Use when: Complex relationships, graph queries
- Examples: Social graph, recommendations

---

### Caching

**Cache Layers:**
1. **Application Cache**: In-memory (local)
2. **Distributed Cache**: Redis, Memcached
3. **CDN**: Cloud CDN, Cloudflare (static content)

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
- **Kafka**: High throughput, event streaming
- **RabbitMQ**: Feature-rich, complex
- **Pub/Sub**: GCP managed, simple
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
- [ ] **Review Google Products**: Search, Gmail, YouTube, Maps, etc.
- [ ] **Practice One Full Design**: Time yourself (35-40 minutes)
- [ ] **Review Common Questions**: Search, storage, real-time systems
- [ ] **Prepare Questions**: Questions to ask interviewer

### Key Concepts to Review

- [ ] **CAP Theorem**: Consistency, Availability, Partition tolerance
- [ ] **ACID vs BASE**: Database consistency models
- [ ] **Sharding Strategies**: Horizontal partitioning
- [ ] **Replication**: Master-slave, master-master, quorum
- [ ] **Caching**: Strategies, eviction policies
- [ ] **Load Balancing**: Algorithms, health checks
- [ ] **Message Queues**: Async processing, decoupling
- [ ] **CDN**: Content delivery, edge caching
- [ ] **Distributed Systems**: Consensus, consistency models

### Google-Specific to Review

- [ ] **Search Systems**: Indexing, ranking, crawling
- [ ] **Distributed Storage**: Bigtable, GFS concepts
- [ ] **Real-time Systems**: Streaming, WebSockets
- [ ] **Scale**: Billions of users, petabytes of data
- [ ] **GCP Services**: Cloud SQL, Bigtable, Pub/Sub, etc.

---

## Interview Day Tips

### Before the Interview (30 minutes before)

- [ ] **Review Framework**: Step-by-step approach
- [ ] **Practice Drawing**: Quick component diagrams
- [ ] **Review Google Products**: Recent features, scale
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
3. ❌ **Ignoring Scale**: Always consider Google scale (billions)
4. ❌ **Not Discussing Trade-offs**: Every decision has trade-offs
5. ❌ **Not Drawing**: Visuals help communication
6. ❌ **Being Rigid**: Be open to feedback and changes
7. ❌ **Forgetting Edge Cases**: Discuss failure scenarios

---

## Quick Reference: Numbers to Remember

### Scale Estimates

**Users:**
- Daily Active Users: 1B - 3B+
- Total Users: 3B - 5B+

**Traffic:**
- Reads: 10M - 100M QPS
- Writes: 1M - 10M QPS

**Data:**
- Web pages indexed: 50B+
- Emails: Billions per day
- Videos: Billions
- Photos: Trillions

**Storage:**
- Web index: Petabytes
- Video storage: Exabytes
- Email storage: Petabytes

**Latency:**
- Search results: <100ms (p95)
- API response: <50ms (p95)
- Database query: <10ms (p95)

**Availability:**
- Target: 99.99% (4 nines)
- Downtime: ~52 minutes per year

---

## Framework Cheat Sheet

### 1. Clarify (4-5 min)
- Functional requirements
- Non-functional requirements
- Scale (users, QPS, data)
- Read/write patterns
- Consistency requirements
- Latency requirements

### 2. High-Level Design (7-8 min)
- Draw main components
- Show data flow
- Discuss overall architecture
- Mention key technologies

### 3. Deep Dive (15-18 min)
- Database design (schema, sharding, indexing)
- Caching strategy (what, where, how)
- API design (endpoints, authentication)
- Load balancing
- Message queues
- CDN
- Search/indexing (if applicable)

### 4. Scale (6-7 min)
- Horizontal scaling
- Database scaling (replicas, sharding)
- Caching at scale
- Handle traffic spikes
- Geographic distribution

### 5. Trade-offs & Edge Cases (3-4 min)
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
1. **Google Search**: Web crawling, indexing, ranking
2. **Gmail**: Email storage, search
3. **YouTube**: Video storage, streaming

### Hard
1. **Distributed File System**: GFS concepts
2. **Distributed Database**: Bigtable concepts
3. **Web Crawler**: Distributed crawling

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

Google system design interviews focus on:
- **Scale**: Design for billions of users and petabytes of data
- **Distributed Systems**: Deep understanding of distributed systems
- **Performance**: Low latency, high throughput
- **Reliability**: High availability, fault tolerance
- **Trade-offs**: Deep understanding of pros/cons
- **Communication**: Clear explanation

**Key Success Factors:**
1. ✅ Clarify requirements first
2. ✅ Start simple, scale gradually
3. ✅ Think out loud
4. ✅ Discuss trade-offs
5. ✅ Handle feedback gracefully
6. ✅ Consider Google scale (billions, petabytes)

**Remember**: It's a collaborative discussion, not a test. Show your thought process, discuss trade-offs, and be open to feedback.

Good luck with your interview! You've got this! 🚀

---

**Related Posts:**
- [Google Coding Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-coding-interview-preparation %})
- [Google Behavioral Interview Guide]({{ site.baseurl }}{% post_url 2025-11-13-google-behavioral-interviews %})
- [Behavioral Interview Preparation Guide]({{ site.baseurl }}{% post_url 2025-11-13-behavioral-interview-preparation-guide %})

