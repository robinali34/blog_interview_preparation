---
layout: post
title: "Distributed System Design - Problem Solving Methodology"
date: 2026-01-06 12:00:00 -0000
categories: interview-preparation system-design distributed-systems methodology
tags: system-design distributed-systems methodology architecture scalability reliability
excerpt: "Comprehensive guide to solving distributed system design problems, covering step-by-step methodology, requirement analysis, component selection, architecture design, and scaling strategies."
---

# Distributed System Design - Problem Solving Methodology

A comprehensive guide to the systematic approach for solving distributed system design problems in technical interviews, covering step-by-step methodology, requirement analysis, component selection, architecture design, and scaling strategies.

## Overview

Designing distributed systems requires a **structured approach** to break down complex problems into manageable components. This guide outlines a proven methodology used by successful system design interviews.

**Key Principles:**
- **Start simple, scale gradually**: Begin with basic design, then add complexity
- **Think in layers**: Application, data, infrastructure layers
- **Consider trade-offs**: Every decision has pros and cons
- **Focus on scale**: Design for millions/billions of users
- **Communicate clearly**: Explain your reasoning

---

## Step-by-Step System Design Process

### Step 1: Understand Requirements & Clarify Scope

**Goal**: Fully understand what needs to be built before designing.

#### What to Do:

1. **Listen carefully** to the problem statement
2. **Ask clarifying questions** (see section below)
3. **Identify functional requirements**
4. **Identify non-functional requirements**
5. **Define scope** (what's in/out of scope)

#### Essential Clarification Questions:

**Functional Requirements:**
- What are the core features?
- What are the use cases?
- What are the input/output formats?
- What are the constraints?

**Non-Functional Requirements:**
- **Scale**: How many users? QPS? Data size?
- **Performance**: Latency requirements? Throughput?
- **Availability**: Uptime requirements? (99.9%, 99.99%)
- **Consistency**: Strong or eventual consistency?
- **Durability**: Data retention? Backup requirements?

**Scope Questions:**
- What's in scope vs out of scope?
- MVP vs full system?
- Single region or multi-region?
- Real-time or batch processing?

#### Example - Requirement Understanding:

**Problem**: "Design a URL shortener like bit.ly"

**Clarifying Questions:**
- How many URLs per day? → 100M URLs/day
- What's the read/write ratio? → 100:1 (read-heavy)
- URL length? → 7 characters
- Redirect latency? → <100ms
- Analytics needed? → Yes, click tracking
- Custom URLs? → Optional feature
- Expiration? → Optional

**Requirements Summary:**
- **Functional**: Shorten URLs, redirect, analytics
- **Scale**: 100M writes/day, 10B reads/day
- **Performance**: <100ms redirect latency
- **Availability**: 99.9%

---

### Step 2: Identify System Type & Patterns

**Goal**: Recognize the system pattern to apply appropriate design patterns.

#### Common System Types:

**1. Data-Intensive Systems:**
- **Examples**: Search engines, analytics platforms
- **Key Components**: Indexing, storage, query processing
- **Patterns**: MapReduce, distributed storage, caching

**2. Compute-Intensive Systems:**
- **Examples**: Video processing, ML training
- **Key Components**: Compute clusters, job queues
- **Patterns**: Distributed computing, task scheduling

**3. Real-Time Systems:**
- **Examples**: Chat, gaming, live streaming
- **Key Components**: WebSockets, message queues, pub/sub
- **Patterns**: Event-driven, streaming

**4. Storage Systems:**
- **Examples**: File storage, databases, CDN
- **Key Components**: Replication, sharding, caching
- **Patterns**: Distributed storage, consistency models

**5. Communication Systems:**
- **Examples**: Messaging, email, notifications
- **Key Components**: Message queues, delivery guarantees
- **Patterns**: At-least-once, exactly-once delivery

**6. Social/Content Systems:**
- **Examples**: Social media, news feeds, recommendations
- **Key Components**: Feed generation, ranking, caching
- **Patterns**: Fan-out, timeline generation

#### Pattern Recognition Examples:

**Example 1: URL Shortener**
- **Type**: Storage + High Read System
- **Pattern**: Key-value store, caching, load balancing
- **Key Insight**: Read-heavy, need fast lookups

**Example 2: Twitter/Feed System**
- **Type**: Social/Content System
- **Pattern**: Fan-out, timeline generation, caching
- **Key Insight**: Write-heavy, read from multiple sources

**Example 3: Video Streaming**
- **Type**: Real-Time + Storage System
- **Pattern**: CDN, adaptive bitrate, chunking
- **Key Insight**: Large files, need low latency delivery

---

### Step 3: Component Selection & Technology Choice

**Goal**: Select appropriate components and technologies for each layer.

#### Component Selection Framework:

**1. Load Balancing:**
- **When**: Multiple servers, high traffic
- **Options**: 
  - Hardware: F5, Citrix
  - Software: Nginx, HAProxy
  - Cloud: AWS ELB, Azure Load Balancer, GCP Load Balancer
- **Algorithm**: Round robin, least connections, IP hash

**2. API Gateway:**
- **When**: Multiple services, need routing/rate limiting
- **Options**: 
  - Kong, AWS API Gateway, Azure API Management
- **Features**: Routing, authentication, rate limiting, logging

**3. Application Servers:**
- **When**: Business logic processing
- **Options**: 
  - Stateless servers (horizontal scaling)
  - Microservices architecture
- **Considerations**: Stateless design, session management

**4. Databases:**
- **SQL**: PostgreSQL, MySQL, Cloud SQL
  - Use when: ACID transactions, complex queries, relationships
- **NoSQL Key-Value**: Redis, DynamoDB
  - Use when: Simple lookups, caching, high throughput
- **NoSQL Document**: MongoDB, CouchDB
  - Use when: Flexible schema, JSON documents
- **NoSQL Column**: Cassandra, HBase, Bigtable
  - Use when: Time-series, wide tables, high write throughput
- **NoSQL Graph**: Neo4j
  - Use when: Complex relationships, graph queries

**5. Caching:**
- **Application Cache**: In-memory (local)
- **Distributed Cache**: Redis, Memcached
- **CDN**: CloudFront, Cloudflare, Fastly
- **Strategy**: Cache-aside, write-through, write-behind

**6. Message Queues:**
- **When**: Async processing, decoupling, handling spikes
- **Options**: 
  - Kafka: High throughput, event streaming
  - RabbitMQ: Feature-rich, complex routing
  - AWS SQS: Managed, simple
  - Google Pub/Sub: Managed, scalable

**7. Search/Indexing:**
- **When**: Full-text search, complex queries
- **Options**: 
  - Elasticsearch, Solr
  - Cloud Search: AWS CloudSearch, Azure Search

**8. Storage:**
- **Object Storage**: S3, Azure Blob, GCS
  - Use when: Files, images, videos
- **Block Storage**: EBS, Azure Disks, Persistent Disks
  - Use when: Databases, file systems
- **File Storage**: EFS, Azure Files, Filestore
  - Use when: Shared file systems

#### Technology Selection Examples:

**Example: High-Traffic Web Application**

```
Load Balancer: AWS ALB (Application Load Balancer)
API Gateway: AWS API Gateway
Application: Stateless microservices (containers)
Database: 
  - Primary: PostgreSQL (RDS) for transactions
  - Cache: Redis for hot data
  - Search: Elasticsearch for search
Message Queue: AWS SQS for async tasks
Storage: S3 for static assets
CDN: CloudFront for content delivery
```

**Example: Real-Time Chat System**

```
Load Balancer: Nginx
WebSocket: Node.js servers
Message Queue: Kafka for message streaming
Database: 
  - Messages: Cassandra (time-series)
  - User data: PostgreSQL
Cache: Redis for presence, recent messages
Storage: S3 for media files
```

---

### Step 4: Architecture Design

**Goal**: Design the overall system architecture.

#### Design Layers:

**1. High-Level Architecture:**
- Draw the big picture
- Show main components
- Show data flow
- Identify key technologies

**2. Detailed Component Design:**
- Database schema
- API design
- Data flow
- Component interactions

**3. Data Model:**
- Entity relationships
- Schema design
- Indexing strategy
- Partitioning strategy

#### Architecture Patterns:

**1. Monolithic vs Microservices:**
- **Monolithic**: Single application, simpler, harder to scale
- **Microservices**: Multiple services, complex, easier to scale

**2. Service-Oriented Architecture (SOA):**
- Services communicate via APIs
- Loose coupling
- Independent deployment

**3. Event-Driven Architecture:**
- Services communicate via events
- Pub/sub pattern
- Decoupled services

**4. Layered Architecture:**
- Presentation layer
- Business logic layer
- Data access layer

#### Example Architecture Design:

**URL Shortener Architecture:**

```
[Client] 
    |
    v
[Load Balancer]
    |
    v
[API Gateway] (Rate limiting, routing)
    |
    +---> [Shortening Service] ---> [Database] (URL mappings)
    |                                    |
    |                                    v
    |                              [Cache] (Redis)
    |
    +---> [Redirect Service] ---> [Cache] (Redis)
                                      |
                                      v
                                  [Database] (fallback)
```

**Components:**
- **Load Balancer**: Distribute traffic
- **API Gateway**: Rate limiting, routing
- **Shortening Service**: Generate short URLs
- **Redirect Service**: Handle redirects
- **Database**: Store URL mappings
- **Cache**: Fast lookups (Redis)

---

### Step 5: Scaling & Optimization

**Goal**: Design for scale and optimize performance.

#### Scaling Strategies:

**1. Horizontal Scaling:**
- Add more servers
- Stateless services
- Load balancing
- **Advantages**: Unlimited scale, fault tolerance
- **Challenges**: Data consistency, coordination

**2. Vertical Scaling:**
- Increase server resources
- **Advantages**: Simpler, no coordination
- **Challenges**: Limited, single point of failure

**3. Database Scaling:**
- **Read Scaling**: Read replicas, caching
- **Write Scaling**: Sharding, partitioning
- **Connection Pooling**: Manage connections
- **Federation**: Split by feature

**4. Caching Strategy:**
- **Multi-layer**: Application, distributed, CDN
- **Cache Patterns**: Cache-aside, write-through
- **Eviction**: LRU, LFU, TTL
- **Invalidation**: Time-based, event-based

**5. CDN:**
- **Purpose**: Reduce latency, offload origin
- **Content**: Static assets, media
- **Strategy**: Cache at edge, invalidate on update

#### Performance Optimization:

**1. Latency Reduction:**
- Caching frequently accessed data
- CDN for static content
- Database query optimization
- Connection pooling
- Async processing

**2. Throughput Increase:**
- Horizontal scaling
- Parallel processing
- Batch operations
- Connection pooling

**3. Resource Optimization:**
- Right-sizing instances
- Auto-scaling
- Resource pooling
- Efficient algorithms

#### Example Scaling Analysis:

**URL Shortener Scaling:**

**Initial Design:**
- 1 server, 1 database
- Handles: 1K QPS

**Scale to 100K QPS:**
1. **Application Layer**:
   - Horizontal scaling: 100 servers
   - Load balancer: Distribute traffic
   - Stateless design: Any server can handle request

2. **Database Layer**:
   - Read replicas: 10 replicas for reads
   - Primary: 1 for writes
   - Connection pooling: Manage connections

3. **Caching Layer**:
   - Redis cluster: Cache hot URLs
   - Cache hit rate: 80% (reduces DB load by 80%)

4. **CDN**:
   - Static assets: Images, CSS, JS
   - Reduces origin server load

**Calculations:**
- Reads: 10B/day = 115K QPS
- With 80% cache hit: 23K QPS to database
- 10 read replicas: 2.3K QPS per replica (manageable)

---

### Step 6: Reliability, Consistency & Trade-offs

**Goal**: Ensure system reliability and handle trade-offs.

#### Reliability Patterns:

**1. Redundancy:**
- **Active-Active**: Multiple active instances
- **Active-Passive**: Standby instances
- **Multi-Region**: Geographic redundancy

**2. Fault Tolerance:**
- **Circuit Breakers**: Fail fast
- **Retries**: Exponential backoff
- **Timeouts**: Prevent hanging
- **Health Checks**: Remove unhealthy instances

**3. Disaster Recovery:**
- **Backup Strategy**: Regular backups
- **Replication**: Multi-region replication
- **RTO**: Recovery Time Objective
- **RPO**: Recovery Point Objective

#### Consistency Models:

**1. Strong Consistency:**
- All nodes see same data immediately
- **Use when**: Financial transactions, critical data
- **Trade-off**: Higher latency, lower availability

**2. Eventual Consistency:**
- Data converges over time
- **Use when**: Social feeds, comments, likes
- **Trade-off**: Lower latency, higher availability, possible stale data

**3. Weak Consistency:**
- No guarantees on when data syncs
- **Use when**: Analytics, logs
- **Trade-off**: Best performance, may lose data

#### CAP Theorem:

**Can only guarantee 2 of 3:**
- **Consistency**: All nodes see same data
- **Availability**: System remains operational
- **Partition Tolerance**: Works despite network partitions

**Choices:**
- **CP**: Strong consistency, partition tolerance (databases)
- **AP**: High availability, partition tolerance (caches)
- **CA**: Not possible in distributed systems

#### Trade-offs Analysis:

**Example Trade-offs:**

**1. Consistency vs Availability:**
- **Strong Consistency**: Lower availability, higher latency
- **Eventual Consistency**: Higher availability, lower latency, possible stale data

**2. Latency vs Freshness:**
- **Caching**: Lower latency, possible stale data
- **No Cache**: Higher latency, fresh data

**3. Cost vs Performance:**
- **More Servers**: Better performance, higher cost
- **Fewer Servers**: Lower cost, lower performance

**4. Complexity vs Scalability:**
- **Simple Design**: Easier to maintain, harder to scale
- **Complex Design**: Harder to maintain, easier to scale

---

## Common Trade-offs in System Design

Understanding and articulating trade-offs is crucial in system design interviews. Every architectural decision involves balancing competing concerns. This section covers the most common trade-offs you'll encounter.

### 1. Consistency vs Availability (CAP Theorem)

**The Fundamental Trade-off:**

**Strong Consistency:**
- **Pros**: All nodes see same data immediately, predictable behavior
- **Cons**: Higher latency (wait for consensus), lower availability (blocks on failures)
- **Use When**: Financial transactions, critical data, inventory systems
- **Examples**: PostgreSQL with synchronous replication, distributed transactions

**Eventual Consistency:**
- **Pros**: Higher availability, lower latency, better performance
- **Cons**: Possible stale data, complex conflict resolution
- **Use When**: Social feeds, comments, likes, analytics
- **Examples**: DynamoDB, Cassandra, DNS

**Real-World Example:**
- **Banking System**: Strong consistency (can't have money appear/disappear)
- **Social Media Feed**: Eventual consistency (slightly stale feed is acceptable)

### 2. Latency vs Freshness

**Caching Strategy:**

**Aggressive Caching:**
- **Pros**: Very low latency, reduced load on backend
- **Cons**: Stale data, cache invalidation complexity
- **Use When**: Read-heavy, data changes infrequently
- **Example**: CDN for static content, Redis for hot data

**No/Minimal Caching:**
- **Pros**: Always fresh data, simpler logic
- **Cons**: Higher latency, more backend load
- **Use When**: Real-time data, frequent updates
- **Example**: Live stock prices, real-time chat

**Hybrid Approach:**
- Cache with TTL (Time To Live)
- Cache invalidation on updates
- Stale-while-revalidate pattern

### 3. Throughput vs Latency

**Batch Processing:**
- **Pros**: High throughput, efficient resource usage
- **Cons**: Higher latency (wait for batch)
- **Use When**: Analytics, reporting, non-real-time processing
- **Example**: ETL pipelines, data warehousing

**Real-Time Processing:**
- **Pros**: Low latency, immediate results
- **Cons**: Lower throughput, more resources
- **Use When**: User-facing features, real-time updates
- **Example**: Real-time recommendations, live dashboards

**Optimization Strategies:**
- Batching small requests
- Async processing for non-critical paths
- Prioritize critical requests

### 4. Cost vs Performance

**Resource Allocation:**

**High Performance:**
- **Pros**: Fast response, good user experience
- **Cons**: Higher infrastructure costs
- **Strategies**: More servers, better hardware, premium services
- **Example**: Dedicated servers, premium CDN, read replicas

**Cost Optimization:**
- **Pros**: Lower operational costs
- **Cons**: May impact performance
- **Strategies**: Right-sizing, reserved instances, spot instances
- **Example**: Auto-scaling, serverless, shared resources

**Balancing Act:**
- Start with cost-effective solution
- Scale up based on actual needs
- Monitor and optimize continuously

### 5. Complexity vs Scalability

**Simple Design:**
- **Pros**: Easier to understand, faster to build, easier to maintain
- **Cons**: Harder to scale, may need redesign later
- **Use When**: MVP, small scale, proof of concept
- **Example**: Monolithic architecture, single database

**Complex Design:**
- **Pros**: Better scalability, fault tolerance, flexibility
- **Cons**: Harder to build, maintain, debug
- **Use When**: Large scale, high availability requirements
- **Example**: Microservices, distributed systems, multi-region

**Evolutionary Approach:**
- Start simple
- Add complexity as scale demands
- Refactor when needed

### 6. Durability vs Performance

**Write Strategy:**

**Synchronous Writes:**
- **Pros**: Data immediately durable, no data loss
- **Cons**: Higher latency, lower throughput
- **Use When**: Critical data, financial transactions
- **Example**: Database with synchronous replication

**Asynchronous Writes:**
- **Pros**: Lower latency, higher throughput
- **Cons**: Risk of data loss on failure
- **Use When**: Logs, analytics, non-critical data
- **Example**: Write-behind cache, async replication

**Write-Ahead Log (WAL):**
- Balance between durability and performance
- Write to log first (fast), then to database
- **Example**: PostgreSQL WAL, Kafka

### 7. Read vs Write Optimization

**Read-Optimized:**
- **Pros**: Fast reads, good for read-heavy workloads
- **Cons**: Slower writes, more storage
- **Strategies**: Denormalization, materialized views, read replicas
- **Example**: Analytics databases, reporting systems

**Write-Optimized:**
- **Pros**: Fast writes, efficient storage
- **Cons**: Slower reads, complex queries
- **Strategies**: Normalization, append-only logs, LSM trees
- **Example**: Time-series databases, event logs

**Balanced:**
- Optimize for both (with trade-offs)
- Use appropriate data structures
- **Example**: B-trees, balanced indexes

### 8. Horizontal vs Vertical Scaling

**Horizontal Scaling (Scale Out):**
- **Pros**: Unlimited scale, fault tolerance, cost-effective
- **Cons**: Complexity, data consistency challenges
- **Use When**: Stateless services, high availability needed
- **Example**: Web servers, stateless APIs

**Vertical Scaling (Scale Up):**
- **Pros**: Simpler, no coordination needed
- **Cons**: Limited scale, single point of failure, expensive
- **Use When**: Small scale, stateful services
- **Example**: Database on single machine, small applications

**Hybrid:**
- Scale vertically first, then horizontally
- Use vertical scaling for databases initially
- Scale horizontally for application layer

### 9. Synchronous vs Asynchronous Communication

**Synchronous:**
- **Pros**: Simple, immediate feedback, easier error handling
- **Cons**: Blocking, tight coupling, cascading failures
- **Use When**: Real-time responses needed, simple workflows
- **Example**: REST APIs, RPC calls

**Asynchronous:**
- **Pros**: Decoupling, better fault tolerance, higher throughput
- **Cons**: Complex error handling, eventual consistency
- **Use When**: Long-running tasks, high throughput needed
- **Example**: Message queues, event-driven architecture

**Hybrid:**
- Synchronous for critical paths
- Asynchronous for background tasks
- **Example**: Request-response with async notifications

### 10. Strong vs Weak Typing

**Strong Typing (Schema):**
- **Pros**: Data validation, type safety, better tooling
- **Cons**: Less flexibility, schema evolution complexity
- **Use When**: Structured data, critical systems
- **Example**: SQL databases, Protocol Buffers

**Weak Typing (Schema-less):**
- **Pros**: Flexibility, easy to evolve, rapid development
- **Cons**: No validation, potential errors, harder to maintain
- **Use When**: Rapid prototyping, flexible requirements
- **Example**: NoSQL document stores, JSON

**Schema Evolution:**
- Version schemas
- Backward compatibility
- Gradual migration

### 11. Centralized vs Distributed

**Centralized:**
- **Pros**: Simpler, easier to manage, consistent
- **Cons**: Single point of failure, scalability limits
- **Use When**: Small scale, simple systems
- **Example**: Single database, monolithic application

**Distributed:**
- **Pros**: Scalability, fault tolerance, geographic distribution
- **Cons**: Complexity, consistency challenges, network issues
- **Use When**: Large scale, high availability, global reach
- **Example**: Distributed databases, microservices, CDN

### 12. Push vs Pull Model

**Push Model (Fan-out on Write):**
- **Pros**: Fast reads, pre-computed results
- **Cons**: Slow writes, storage overhead
- **Use When**: Read-heavy, small number of consumers
- **Example**: Social media feeds (for active users)

**Pull Model (Fan-out on Read):**
- **Pros**: Fast writes, no storage overhead
- **Cons**: Slow reads, computation on demand
- **Use When**: Write-heavy, large number of consumers
- **Example**: Social media feeds (for inactive users)

**Hybrid:**
- Push for active users
- Pull for inactive users
- **Example**: Twitter's hybrid approach

### 13. SQL vs NoSQL

**SQL (Relational):**
- **Pros**: ACID transactions, complex queries, relationships
- **Cons**: Harder to scale, schema rigidity
- **Use When**: Transactions, relationships, complex queries
- **Example**: PostgreSQL, MySQL

**NoSQL:**
- **Pros**: Horizontal scaling, flexible schema, high throughput
- **Cons**: No ACID (usually), limited queries
- **Use When**: High scale, simple queries, flexible schema
- **Example**: MongoDB, Cassandra, DynamoDB

**Polyglot Persistence:**
- Use right database for right use case
- SQL for transactions, NoSQL for scale
- **Example**: PostgreSQL for user data, Redis for cache, Elasticsearch for search

### 14. Monolithic vs Microservices

**Monolithic:**
- **Pros**: Simpler, easier to develop, test, deploy
- **Cons**: Harder to scale, technology lock-in, deployment risk
- **Use When**: Small team, simple system, MVP
- **Example**: Single application, shared database

**Microservices:**
- **Pros**: Independent scaling, technology diversity, fault isolation
- **Cons**: Complexity, network overhead, distributed transactions
- **Use When**: Large team, complex system, different scaling needs
- **Example**: Separate services per domain, service mesh

**Evolution:**
- Start monolithic
- Extract services as needed
- Don't over-engineer

### 15. Stateful vs Stateless

**Stateful:**
- **Pros**: Better performance, simpler logic
- **Cons**: Harder to scale, session management
- **Use When**: Performance critical, session required
- **Example**: Gaming servers, WebSocket connections

**Stateless:**
- **Pros**: Easy to scale, fault tolerant, simple
- **Cons**: Need external storage, more requests
- **Use When**: Web APIs, horizontal scaling needed
- **Example**: REST APIs, stateless microservices

**Hybrid:**
- Stateless application servers
- Stateful storage layer
- **Example**: Stateless web servers + stateful database

### 16. Optimistic vs Pessimistic Locking

**Optimistic Locking:**
- **Pros**: Better concurrency, no blocking
- **Cons**: Retry on conflict, possible wasted work
- **Use When**: Low conflict rate, read-heavy
- **Example**: Version numbers, timestamps

**Pessimistic Locking:**
- **Pros**: Guaranteed consistency, no retries
- **Cons**: Lower concurrency, possible deadlocks
- **Use When**: High conflict rate, critical sections
- **Example**: Database row locks, mutexes

### 17. Eventual vs Strong Consistency

**Strong Consistency:**
- **Pros**: Always correct, predictable
- **Cons**: Higher latency, lower availability
- **Use When**: Financial data, critical operations
- **Example**: Bank transactions, inventory systems

**Eventual Consistency:**
- **Pros**: Higher availability, lower latency
- **Cons**: Possible stale data, conflict resolution
- **Use When**: Social feeds, comments, analytics
- **Example**: DNS, distributed caches

### 18. Replication: Synchronous vs Asynchronous

**Synchronous Replication:**
- **Pros**: No data loss, strong consistency
- **Cons**: Higher latency, lower availability
- **Use When**: Critical data, zero data loss requirement
- **Example**: Financial databases, critical systems

**Asynchronous Replication:**
- **Pros**: Lower latency, higher availability
- **Cons**: Possible data loss, eventual consistency
- **Use When**: High availability, acceptable data loss
- **Example**: Read replicas, geo-replication

### 19. Sharding: Early vs Late

**Early Sharding:**
- **Pros**: Better performance at scale, prepared for growth
- **Cons**: Complexity, operational overhead
- **Use When**: Known high scale, clear sharding key
- **Example**: User-based sharding, geographic sharding

**Late Sharding:**
- **Pros**: Simpler initially, less operational overhead
- **Cons**: May need migration later, harder to add
- **Use When**: Unknown scale, MVP phase
- **Example**: Start with single database, shard when needed

### 20. Development Speed vs Code Quality

**Fast Development:**
- **Pros**: Quick to market, rapid iteration
- **Cons**: Technical debt, harder to maintain
- **Use When**: MVP, time-sensitive features
- **Example**: Prototypes, proof of concepts

**Code Quality:**
- **Pros**: Maintainable, scalable, fewer bugs
- **Cons**: Slower development, more upfront cost
- **Use When**: Production systems, long-term projects
- **Example**: Well-tested, documented code

**Balance:**
- Fast for MVP
- Quality for production
- Continuous refactoring

### Trade-off Decision Framework

When evaluating trade-offs, consider:

1. **Requirements**: What are the non-negotiable requirements?
2. **Scale**: Current and projected scale
3. **Team**: Team size and expertise
4. **Timeline**: Time to market constraints
5. **Budget**: Cost constraints
6. **Risk Tolerance**: How much risk is acceptable?
7. **Future Growth**: How will requirements change?

**Example Decision Process:**

**Problem**: Choose between SQL and NoSQL

**Considerations:**
- **Scale**: 1M users → SQL is fine, 1B users → Consider NoSQL
- **Queries**: Complex joins → SQL, Simple lookups → NoSQL
- **Consistency**: Need ACID → SQL, Eventual OK → NoSQL
- **Team**: SQL expertise → SQL, NoSQL expertise → NoSQL
- **Future**: Unknown → SQL (easier to migrate), Known scale → NoSQL

**Decision**: Start with SQL, migrate to NoSQL if scale demands

---

## Essential Clarification Questions

### Scale Questions:

1. **Users**:
   - "How many daily active users?"
   - "Peak concurrent users?"
   - "User growth rate?"

2. **Traffic**:
   - "What's the expected QPS (queries per second)?"
   - "Read vs write ratio?"
   - "Peak traffic patterns?"

3. **Data**:
   - "How much data do we store?"
   - "Data growth rate?"
   - "Data retention policy?"

### Performance Questions:

1. **Latency**:
   - "What's acceptable latency? (p50, p95, p99)"
   - "Real-time or batch processing?"

2. **Throughput**:
   - "How many requests per second?"
   - "Peak load handling?"

### Availability Questions:

1. **Uptime**:
   - "What's the availability requirement? (99.9%, 99.99%)"
   - "What's acceptable downtime?"

2. **Disaster Recovery**:
   - "RTO (Recovery Time Objective)?"
   - "RPO (Recovery Point Objective)?"

### Consistency Questions:

1. **Data Consistency**:
   - "Strong or eventual consistency?"
   - "What data needs strong consistency?"

2. **Geographic**:
   - "Single region or multi-region?"
   - "Data locality requirements?"

---

## Sample System Design Walkthroughs

### Problem 1: Design a URL Shortener

**Step 1: Understand Requirements**

**Clarifying Questions:**
- Scale: 100M URLs/day, 10B redirects/day
- Read/Write: 100:1 (read-heavy)
- URL length: 7 characters
- Latency: <100ms redirect
- Analytics: Click tracking
- Custom URLs: Optional
- Expiration: Optional

**Requirements:**
- **Functional**: Shorten URLs, redirect, analytics
- **Scale**: 100M writes/day, 10B reads/day
- **Performance**: <100ms redirect
- **Availability**: 99.9%

**Step 2: Identify System Type**

- **Type**: Storage + High Read System
- **Pattern**: Key-value store, caching, load balancing
- **Key Insight**: Read-heavy, need fast lookups

**Step 3: Component Selection**

- **Load Balancer**: AWS ALB
- **API Gateway**: AWS API Gateway (rate limiting)
- **Application**: Stateless microservices
- **Database**: 
  - Primary: PostgreSQL (URL mappings)
  - Cache: Redis (hot URLs)
- **Storage**: S3 (analytics logs)
- **CDN**: CloudFront (static assets)

**Step 4: Architecture Design**

```
[Client]
    |
    v
[Load Balancer]
    |
    v
[API Gateway]
    |
    +---> [Shortening Service] ---> [Database] (PostgreSQL)
    |                                    |
    |                                    v
    |                              [Cache] (Redis)
    |
    +---> [Redirect Service] ---> [Cache] (Redis)
                                      |
                                      v
                                  [Database] (fallback)
```

**Database Schema:**
```sql
CREATE TABLE url_mappings (
    id BIGSERIAL PRIMARY KEY,
    short_url VARCHAR(7) UNIQUE NOT NULL,
    long_url TEXT NOT NULL,
    created_at TIMESTAMP,
    expires_at TIMESTAMP,
    click_count BIGINT DEFAULT 0
);

CREATE INDEX idx_short_url ON url_mappings(short_url);
```

**Step 5: Scaling & Optimization**

**Scaling Strategy:**
1. **Application**: Horizontal scaling (stateless)
2. **Database**: 
   - Read replicas: 10 replicas
   - Sharding: By short_url hash
3. **Caching**: 
   - Redis cluster: 80% cache hit rate
   - Cache hot URLs
4. **CDN**: Static assets

**Calculations:**
- Writes: 100M/day = 1.2K QPS
- Reads: 10B/day = 115K QPS
- With 80% cache: 23K QPS to DB
- 10 replicas: 2.3K QPS per replica

**Step 6: Reliability & Trade-offs**

**Reliability:**
- Multi-AZ deployment
- Database replication
- Cache replication
- Health checks

**Trade-offs:**
- **Consistency**: Eventual (acceptable for URLs)
- **Availability**: 99.9% (multi-AZ)
- **Latency**: <100ms (caching)

---

### Problem 2: Design Twitter Feed System

**Step 1: Understand Requirements**

**Clarifying Questions:**
- Users: 300M DAU
- Tweets: 500M/day
- Feed reads: 23B/day
- Followers: Average 200, max 30M
- Timeline: Home timeline + user timeline
- Real-time: Yes, within seconds

**Requirements:**
- **Functional**: Post tweets, follow users, view feeds
- **Scale**: 500M writes/day, 23B reads/day
- **Performance**: <200ms feed load
- **Real-time**: New tweets appear within seconds

**Step 2: Identify System Type**

- **Type**: Social/Content System
- **Pattern**: Fan-out, timeline generation, caching
- **Key Insight**: Write-heavy, read from multiple sources

**Step 3: Component Selection**

- **Load Balancer**: AWS ALB
- **Application**: Microservices (Tweet, User, Feed)
- **Database**: 
  - Tweets: Cassandra (time-series)
  - Users: PostgreSQL
  - Follows: Graph database or relational
- **Cache**: Redis (user timelines, social graph)
- **Message Queue**: Kafka (tweet distribution)
- **Search**: Elasticsearch (tweet search)

**Step 4: Architecture Design**

```
[Client]
    |
    v
[Load Balancer]
    |
    v
[API Gateway]
    |
    +---> [Tweet Service] ---> [Kafka] ---> [Fan-out Service]
    |                              |
    |                              v
    |                         [Timeline Service]
    |                              |
    |                              v
    +---> [Feed Service] <--- [Cache] (Redis)
                                      |
                                      v
                                  [Database] (Cassandra)
```

**Fan-out Strategies:**
1. **Push Model**: Write to all followers' timelines
   - Pros: Fast reads
   - Cons: Slow writes for popular users
2. **Pull Model**: Read from followed users on read
   - Pros: Fast writes
   - Cons: Slow reads
3. **Hybrid**: Push for active users, pull for inactive

**Step 5: Scaling & Optimization**

**Scaling Strategy:**
1. **Tweet Storage**: 
   - Shard by user_id
   - Time-based partitioning
2. **Timeline Generation**:
   - Hybrid fan-out
   - Pre-compute for active users
   - On-demand for inactive users
3. **Caching**:
   - Cache user timelines
   - Cache social graph
   - Cache trending topics

**Step 6: Reliability & Trade-offs**

**Reliability:**
- Multi-region deployment
- Eventual consistency (acceptable for feeds)
- Retry mechanisms for fan-out

**Trade-offs:**
- **Consistency**: Eventual (feeds can be slightly stale)
- **Latency**: <200ms (caching, pre-computation)
- **Complexity**: High (fan-out logic)

---

### Problem 3: Design a Distributed Cache

**Step 1: Understand Requirements**

**Clarifying Questions:**
- Capacity: 100TB
- QPS: 1M reads, 100K writes
- Latency: <1ms
- Consistency: Eventual acceptable
- Eviction: LRU
- Replication: 3 replicas

**Requirements:**
- **Functional**: Get, Set, Delete operations
- **Scale**: 1M QPS, 100TB capacity
- **Performance**: <1ms latency
- **Availability**: 99.99%

**Step 2: Identify System Type**

- **Type**: Storage System
- **Pattern**: Distributed key-value store, consistent hashing
- **Key Insight**: High throughput, low latency, distributed

**Step 3: Component Selection**

- **Consistent Hashing**: Distribute keys across nodes
- **Replication**: 3 replicas per key
- **Eviction**: LRU per node
- **Gossip Protocol**: Node discovery, failure detection

**Step 4: Architecture Design**

```
[Client]
    |
    v
[Load Balancer]
    |
    v
[Cache Cluster]
    |
    +---> [Node 1] ---> [Replica 1, 2, 3]
    +---> [Node 2] ---> [Replica 1, 2, 3]
    +---> [Node 3] ---> [Replica 1, 2, 3]
    ...
```

**Consistent Hashing:**
- Hash ring: 0 to 2^32-1
- Each node on ring
- Key maps to next node clockwise
- Virtual nodes for better distribution

**Step 5: Scaling & Optimization**

**Scaling Strategy:**
1. **Horizontal Scaling**: Add/remove nodes
2. **Replication**: 3 replicas for availability
3. **Sharding**: Consistent hashing
4. **Caching**: In-memory storage

**Step 6: Reliability & Trade-offs**

**Reliability:**
- Replication: 3 replicas
- Failure detection: Gossip protocol
- Automatic failover

**Trade-offs:**
- **Consistency**: Eventual (acceptable for cache)
- **Availability**: 99.99% (replication)
- **Durability**: In-memory (data can be lost)

---

## Common Mistakes to Avoid

1. **Jumping to Solutions**: Don't start designing without understanding requirements
2. **Not Asking Questions**: Always clarify scale, requirements, constraints
3. **Over-Engineering**: Start simple, add complexity as needed
4. **Ignoring Scale**: Always consider scale (millions/billions)
5. **Not Discussing Trade-offs**: Every decision has trade-offs
6. **Single Point of Failure**: Design for redundancy
7. **Not Considering Consistency**: Understand consistency requirements
8. **Ignoring Cost**: Consider cost implications
9. **Not Testing**: Discuss how to test the system
10. **Poor Communication**: Explain your thinking clearly

---

## Design Checklist

Before finalizing design, ensure you've:
- [ ] Clarified all requirements
- [ ] Identified system type and patterns
- [ ] Selected appropriate components
- [ ] Designed high-level architecture
- [ ] Designed detailed components
- [ ] Planned scaling strategy
- [ ] Discussed reliability and fault tolerance
- [ ] Analyzed trade-offs
- [ ] Considered cost implications
- [ ] Discussed monitoring and observability

---

## Summary

**The 6-Step Process:**
1. **Understand Requirements**: Clarify scope, scale, constraints
2. **Identify System Type**: Recognize patterns, apply appropriate designs
3. **Select Components**: Choose technologies for each layer
4. **Design Architecture**: High-level and detailed design
5. **Scale & Optimize**: Plan for growth, optimize performance
6. **Reliability & Trade-offs**: Ensure reliability, analyze trade-offs

**Key Success Factors:**
- Communication: Explain your thinking
- Systematic approach: Follow the steps
- Scale thinking: Design for millions/billions
- Trade-off awareness: Every decision has pros/cons
- Practical solutions: Balance complexity and requirements

---

## Common System Design Patterns & Methodologies

Understanding design patterns and methodologies is crucial for system design interviews. This section covers Domain-Driven Design (DDD) and other common approaches used in building distributed systems.

### Domain-Driven Design (DDD)

**Domain-Driven Design (DDD)** is a software development approach introduced by Eric Evans that focuses on modeling software based on the business domain and domain logic.

#### Core Concepts

**1. Domain:**
- The sphere of knowledge or activity around which business logic revolves
- **Example**: E-commerce domain includes: products, orders, customers, payments

**2. Domain Model:**
- A system of abstractions that describes selected aspects of a domain
- Represents business concepts, not just data structures
- **Example**: Order entity with business rules (can't cancel shipped order)

**3. Ubiquitous Language:**
- Common vocabulary used by developers and domain experts
- Same terms in code and conversations
- **Example**: "Order", "Cart", "Checkout" used consistently

**4. Bounded Context:**
- Explicit boundary within which a domain model applies
- Different contexts can have different models for same concept
- **Example**: "Customer" in Sales context vs "Customer" in Shipping context

**5. Entities:**
- Objects with unique identity that persists over time
- **Example**: User, Order, Product (identified by ID)

**6. Value Objects:**
- Objects defined by their attributes, not identity
- **Example**: Money (amount + currency), Address (street + city + zip)

**7. Aggregates:**
- Cluster of entities and value objects treated as a single unit
- Aggregate root: Entry point to aggregate
- **Example**: Order (root) contains OrderItems (entities)

**8. Domain Services:**
- Operations that don't naturally belong to entities
- **Example**: Transfer money between accounts (involves multiple entities)

**9. Repositories:**
- Abstraction for accessing aggregates
- Hides persistence details
- **Example**: `OrderRepository.findByCustomerId()`

**10. Domain Events:**
- Something that happened in the domain
- Other parts of system react to events
- **Example**: OrderPlaced, PaymentProcessed, ShipmentDelivered

#### DDD Layers

```
┌─────────────────────────────────┐
│   Presentation Layer            │  (UI, API)
├─────────────────────────────────┤
│   Application Layer             │  (Use Cases, Orchestration)
├─────────────────────────────────┤
│   Domain Layer                  │  (Business Logic, Entities)
├─────────────────────────────────┤
│   Infrastructure Layer          │  (Database, External Services)
└─────────────────────────────────┘
```

**Example - E-commerce System:**

**Domain Model:**
```c
// Entity
class Order {
    private OrderId id;
    private CustomerId customerId;
    private List<OrderItem> items;
    private OrderStatus status;
    
    // Business logic in domain
    void cancel() {
        if (status == OrderStatus.SHIPPED) {
            throw new DomainException("Cannot cancel shipped order");
        }
        this.status = OrderStatus.CANCELLED;
        DomainEvents.raise(new OrderCancelled(this.id));
    }
}

// Value Object
class Money {
    private BigDecimal amount;
    private Currency currency;
    
    Money add(Money other) {
        if (!this.currency.equals(other.currency)) {
            throw new DomainException("Cannot add different currencies");
        }
        return new Money(this.amount + other.amount, this.currency);
    }
}

// Aggregate Root
class Order {  // Aggregate root
    private OrderId id;
    private List<OrderItem> items;  // Entities within aggregate
    // ...
}
```

**Bounded Contexts:**
- **Sales Context**: Order, Customer, Product
- **Shipping Context**: Shipment, Address, Carrier
- **Payment Context**: Payment, Invoice, Refund

**Benefits:**
- **Business Alignment**: Code reflects business domain
- **Maintainability**: Clear structure, easier to understand
- **Testability**: Domain logic isolated from infrastructure
- **Scalability**: Bounded contexts enable microservices

**When to Use:**
- Complex business domains
- Long-lived projects
- Need for business logic clarity
- Multiple teams working on different contexts

---

### Other Common System Design Patterns & Methodologies

#### 1. Microservices Architecture

**Definition:**
- Architecture pattern where application is built as collection of small, independent services
- Each service runs in own process and communicates via APIs

**Characteristics:**
- **Service Independence**: Deploy, scale, update independently
- **Technology Diversity**: Each service can use different tech stack
- **Fault Isolation**: Service failure doesn't crash entire system
- **Team Autonomy**: Different teams own different services

**Example:**
```
┌─────────────┐   ┌─────────────┐   ┌─────────────┐
│ User Service│   │Order Service│   │Payment Service│
└─────────────┘   └─────────────┘   └─────────────┘
      │                 │                 │
      └─────────────────┴─────────────────┘
                    │
            ┌───────┴───────┐
            │  API Gateway  │
            └───────┬───────┘
                    │
            ┌───────┴───────┘
            │    Clients     │
            └────────────────┘
```

**Benefits:**
- Scalability (scale services independently)
- Technology flexibility
- Fault isolation
- Team autonomy

**Challenges:**
- Complexity (distributed system challenges)
- Network overhead
- Data consistency
- Service coordination

**When to Use:**
- Large, complex systems
- Different scaling requirements
- Multiple teams
- Need for technology diversity

---

#### 2. Event-Driven Architecture (EDA)

**Definition:**
- Architecture where services communicate through events
- Services publish events and react to events from other services

**Patterns:**
- **Event Sourcing**: Store all events, reconstruct state from events
- **CQRS (Command Query Responsibility Segregation)**: Separate read/write models
- **Pub/Sub**: Publishers send events, subscribers consume

**Example:**
```
Order Service          Payment Service        Shipping Service
     │                       │                       │
     │  OrderPlaced Event   │                       │
     ├──────────────────────>│                       │
     │                       │  PaymentProcessed    │
     │                       ├──────────────────────>│
     │                       │                       │  ShipmentCreated
     │                       │                       │
```

**Benefits:**
- **Decoupling**: Services don't know about each other
- **Scalability**: Easy to add new subscribers
- **Flexibility**: Services can evolve independently
- **Event Sourcing**: Complete audit trail

**Challenges:**
- Eventual consistency
- Event ordering
- Error handling
- Debugging complexity

**When to Use:**
- Loosely coupled services
- Real-time processing
- Need for audit trail
- Multiple consumers of same data

---

#### 3. CQRS (Command Query Responsibility Segregation)

**Definition:**
- Separate read and write models
- Commands (writes) and Queries (reads) use different models

**Architecture:**
```
Write Side (Commands)          Read Side (Queries)
┌──────────────┐              ┌──────────────┐
│ Command API   │              │  Query API   │
└───────┬───────┘              └───────┬───────┘
        │                               │
        v                               v
┌──────────────┐              ┌──────────────┐
│ Write Model   │              │  Read Model  │
│ (Normalized)  │              │(Denormalized)│
└───────┬───────┘              └───────┬───────┘
        │                               │
        v                               │
┌──────────────┐                       │
│ Event Store   │                       │
└───────┬───────┘                       │
        │                               │
        └───────────> Projection ───────┘
```

**Benefits:**
- **Optimized Reads**: Denormalized read models for fast queries
- **Optimized Writes**: Normalized write models for consistency
- **Scalability**: Scale read/write independently
- **Flexibility**: Different models for different use cases

**Challenges:**
- Complexity (two models to maintain)
- Eventual consistency
- Data synchronization

**When to Use:**
- Read/write workloads differ significantly
- Complex queries on write model
- Need for high read performance
- Event sourcing systems

---

#### 4. Event Sourcing

**Definition:**
- Store all changes as sequence of events
- Current state reconstructed by replaying events

**Example:**
```
Events:
1. OrderCreated(orderId=123, customerId=456)
2. ItemAdded(orderId=123, itemId=789, quantity=2)
3. ItemAdded(orderId=123, itemId=790, quantity=1)
4. OrderPlaced(orderId=123)

Current State (reconstructed):
Order {
    id: 123,
    customerId: 456,
    items: [
        {itemId: 789, quantity: 2},
        {itemId: 790, quantity: 1}
    ],
    status: PLACED
}
```

**Benefits:**
- **Complete Audit Trail**: All changes recorded
- **Time Travel**: Reconstruct state at any point
- **Debugging**: See exactly what happened
- **Event Replay**: Rebuild state from scratch

**Challenges:**
- Storage overhead (all events)
- Event versioning
- Snapshot management
- Query complexity

**When to Use:**
- Need for audit trail
- Complex business logic
- Time-travel queries
- Compliance requirements

---

#### 5. Hexagonal Architecture (Ports & Adapters)

**Definition:**
- Architecture that isolates core business logic from external concerns
- Core logic in center, adapters on outside

**Structure:**
```
        ┌─────────────────────┐
        │   Adapters (Out)    │  (REST, GraphQL, gRPC)
        └──────────┬──────────┘
                   │
        ┌───────────┴───────────┐
        │   Application Core   │
        │  (Business Logic)    │
        └───────────┬──────────┘
                   │
        ┌───────────┴───────────┐
        │   Adapters (In)       │  (Database, External APIs)
        └───────────────────────┘
```

**Benefits:**
- **Testability**: Core logic independent of infrastructure
- **Flexibility**: Easy to swap adapters
- **Independence**: Business logic doesn't depend on frameworks

**When to Use:**
- Need for testability
- Multiple interfaces (REST, GraphQL, etc.)
- Technology flexibility

---

#### 6. Service-Oriented Architecture (SOA)

**Definition:**
- Architecture where services communicate via well-defined interfaces
- Services are reusable, loosely coupled

**Characteristics:**
- **Service Contracts**: Well-defined interfaces (WSDL, OpenAPI)
- **Service Registry**: Discovery mechanism
- **Orchestration**: Coordinate multiple services
- **Enterprise Service Bus (ESB)**: Message routing

**Benefits:**
- Reusability
- Loose coupling
- Interoperability
- Business alignment

**Challenges:**
- Complexity
- Performance overhead
- Service coordination

**When to Use:**
- Enterprise systems
- Multiple systems integration
- Service reuse requirements

---

#### 7. Layered Architecture

**Definition:**
- Organize code into horizontal layers
- Each layer has specific responsibility

**Layers:**
```
┌─────────────────────┐
│  Presentation Layer │  (UI, Controllers)
├─────────────────────┤
│  Business Layer     │  (Business Logic)
├─────────────────────┤
│  Data Access Layer  │  (Database, Repositories)
└─────────────────────┘
```

**Benefits:**
- Clear separation of concerns
- Easy to understand
- Standard structure

**Challenges:**
- Can become anemic
- Layer boundaries can blur

**When to Use:**
- Simple to medium complexity
- Traditional applications
- Team familiarity

---

#### 8. API Gateway Pattern

**Definition:**
- Single entry point for all client requests
- Routes requests to appropriate services

**Functions:**
- **Routing**: Route to correct service
- **Authentication**: Verify credentials
- **Rate Limiting**: Control request rate
- **Load Balancing**: Distribute load
- **Protocol Translation**: Convert protocols
- **Aggregation**: Combine multiple service responses

**Example:**
```
Clients
   │
   v
┌──────────────┐
│ API Gateway  │
└──────┬───────┘
       │
   ┌───┴───┬──────┬──────┐
   │       │      │      │
   v       v      v      v
Service1 Service2 Service3 Service4
```

**Benefits:**
- Single entry point
- Centralized cross-cutting concerns
- Client simplification
- Service decoupling

**When to Use:**
- Microservices architecture
- Multiple clients
- Need for centralized concerns

---

#### 9. Circuit Breaker Pattern

**Definition:**
- Prevents cascading failures by stopping requests to failing service
- Opens circuit when failures exceed threshold

**States:**
- **Closed**: Normal operation
- **Open**: Circuit open, requests fail fast
- **Half-Open**: Testing if service recovered

**Example:**
```
Service A ──> Service B
              │
              ├─> Success: Circuit Closed
              ├─> Failures > Threshold: Circuit Open
              └─> After timeout: Circuit Half-Open
```

**Benefits:**
- Prevents cascading failures
- Fast failure detection
- Automatic recovery

**When to Use:**
- External service calls
- Network calls
- Need for fault tolerance

---

#### 10. Saga Pattern

**Definition:**
- Manages distributed transactions across multiple services
- Uses compensating transactions instead of two-phase commit

**Types:**
- **Choreography**: Services coordinate through events
- **Orchestration**: Central coordinator manages flow

**Example (Order Processing):**
```
1. Create Order
2. Reserve Inventory
3. Process Payment
   ├─> Success: Confirm Order
   └─> Failure: Cancel Inventory, Cancel Order
```

**Benefits:**
- Distributed transaction management
- No distributed locks
- Better scalability

**Challenges:**
- Compensating transaction complexity
- Eventual consistency

**When to Use:**
- Distributed transactions
- Microservices
- Long-running transactions

---

#### 11. Strangler Fig Pattern

**Definition:**
- Gradually replace legacy system by building new system around it
- Gradually migrate functionality

**Process:**
1. Build new system alongside legacy
2. Route new features to new system
3. Gradually migrate existing features
4. Eventually retire legacy system

**Benefits:**
- Low risk migration
- Gradual transition
- No big bang rewrite

**When to Use:**
- Legacy system replacement
- Risk-averse migration
- Continuous operation required

---

#### 12. Bulkhead Pattern

**Definition:**
- Isolate resources to prevent failure in one area from affecting others
- Like ship bulkheads that prevent flooding

**Example:**
```
Service A ──> Thread Pool 1 ──> Database 1
Service B ──> Thread Pool 2 ──> Database 2
Service C ──> Thread Pool 3 ──> Database 3
```

**Benefits:**
- Fault isolation
- Resource isolation
- Prevents cascading failures

**When to Use:**
- Critical services
- Need for isolation
- Resource constraints

---

## Common Design Patterns in System Design

This section lists common design patterns frequently used in distributed system design. These patterns solve recurring problems and provide proven solutions.

### 1. Creational Patterns

#### Singleton Pattern
**Purpose**: Ensure only one instance of a class exists.

**System Design Use Cases:**
- **Configuration Manager**: Single source of configuration
- **Connection Pool**: Single pool manager
- **Cache Manager**: Single cache instance
- **Service Registry**: Single registry instance

**Example:**
```c
// Configuration Manager (Singleton)
class ConfigManager {
private:
    static ConfigManager* instance;
    ConfigManager() {}
    
public:
    static ConfigManager* getInstance() {
        if (instance == nullptr) {
            instance = new ConfigManager();
        }
        return instance;
    }
};
```

**Considerations:**
- Thread safety in multi-threaded environments
- Testing challenges (hard to mock)
- Global state concerns

#### Factory Pattern
**Purpose**: Create objects without specifying exact class.

**System Design Use Cases:**
- **Database Connection Factory**: Create connections for different DB types
- **Message Queue Factory**: Create queues (Kafka, RabbitMQ, SQS)
- **Storage Factory**: Create storage (S3, Azure Blob, GCS)
- **Service Factory**: Create service instances

**Example:**
```c
// Storage Factory
class StorageFactory {
public:
    static Storage* createStorage(string type) {
        if (type == "s3") return new S3Storage();
        if (type == "azure") return new AzureBlobStorage();
        if (type == "gcs") return new GCSStorage();
        throw new InvalidStorageType();
    }
};
```

#### Builder Pattern
**Purpose**: Construct complex objects step by step.

**System Design Use Cases:**
- **Query Builder**: Build complex database queries
- **Request Builder**: Build HTTP requests with headers, body
- **Configuration Builder**: Build system configurations
- **Pipeline Builder**: Build data processing pipelines

**Example:**
```c
// Query Builder
Query query = QueryBuilder()
    .select("id", "name", "email")
    .from("users")
    .where("age > 18")
    .orderBy("name")
    .limit(100)
    .build();
```

### 2. Structural Patterns

#### Adapter Pattern
**Purpose**: Allow incompatible interfaces to work together.

**System Design Use Cases:**
- **Legacy System Integration**: Adapt old APIs to new interfaces
- **Third-Party Service Adapters**: Wrap external services
- **Protocol Adapters**: Convert between protocols (REST to gRPC)
- **Database Adapters**: Abstract different database interfaces

**Example:**
```c
// Legacy Payment System Adapter
class LegacyPaymentAdapter : public PaymentService {
private:
    LegacyPaymentSystem legacy;
    
public:
    void processPayment(PaymentRequest req) {
        LegacyRequest legacyReq = convert(req);
        legacy.process(legacyReq);
    }
};
```

#### Facade Pattern
**Purpose**: Provide simplified interface to complex subsystem.

**System Design Use Cases:**
- **API Gateway**: Simplified interface to multiple services
- **Service Facade**: Hide complexity of multiple service calls
- **Database Facade**: Simplify complex database operations
- **Authentication Facade**: Simplify auth complexity

**Example:**
```c
// Order Service Facade
class OrderFacade {
public:
    OrderResult placeOrder(OrderRequest req) {
        // Hide complexity of multiple service calls
        validateOrder(req);
        reserveInventory(req);
        processPayment(req);
        createShipment(req);
        return result;
    }
};
```

#### Proxy Pattern
**Purpose**: Provide placeholder or surrogate for another object.

**System Design Use Cases:**
- **API Proxy**: Proxy for external APIs (caching, rate limiting)
- **Database Proxy**: Connection pooling, query caching
- **Service Proxy**: Load balancing, failover
- **Security Proxy**: Authentication, authorization

**Example:**
```c
// Caching Proxy
class DatabaseProxy : public Database {
private:
    Database* realDB;
    Cache* cache;
    
public:
    Data query(string sql) {
        if (cache->exists(sql)) {
            return cache->get(sql);
        }
        Data result = realDB->query(sql);
        cache->set(sql, result);
        return result;
    }
};
```

#### Decorator Pattern
**Purpose**: Add behavior to objects dynamically.

**System Design Use Cases:**
- **Request Decorators**: Add logging, metrics, retry logic
- **Service Decorators**: Add caching, rate limiting
- **Message Decorators**: Add encryption, compression
- **Pipeline Decorators**: Add processing steps

**Example:**
```c
// Service with Decorators
Service* service = new BasicService();
service = new LoggingDecorator(service);
service = new MetricsDecorator(service);
service = new RetryDecorator(service);
```

### 3. Behavioral Patterns

#### Observer Pattern
**Purpose**: Notify multiple objects about state changes.

**System Design Use Cases:**
- **Event Notifications**: Notify subscribers of events
- **Cache Invalidation**: Notify caches of data changes
- **UI Updates**: Update multiple UI components
- **Monitoring**: Notify monitors of system events

**Example:**
```c
// Event Publisher
class EventPublisher {
private:
    vector<Observer*> observers;
    
public:
    void subscribe(Observer* obs) {
        observers.push_back(obs);
    }
    
    void notify(Event event) {
        for (auto obs : observers) {
            obs->update(event);
        }
    }
};
```

#### Strategy Pattern
**Purpose**: Define family of algorithms, make them interchangeable.

**System Design Use Cases:**
- **Load Balancing Strategies**: Round-robin, least-connections, IP-hash
- **Caching Strategies**: LRU, LFU, FIFO
- **Retry Strategies**: Exponential backoff, linear, fixed
- **Compression Strategies**: Gzip, Snappy, LZ4

**Example:**
```c
// Load Balancing Strategy
class LoadBalancer {
private:
    LoadBalanceStrategy* strategy;
    
public:
    void setStrategy(LoadBalanceStrategy* s) {
        strategy = s;
    }
    
    Server selectServer(vector<Server> servers) {
        return strategy->select(servers);
    }
};
```

#### Command Pattern
**Purpose**: Encapsulate requests as objects.

**System Design Use Cases:**
- **Request Queues**: Queue commands for async processing
- **Undo/Redo**: Command history for rollback
- **Job Scheduling**: Schedule commands for execution
- **API Requests**: Encapsulate API calls as commands

**Example:**
```c
// Command Interface
class Command {
public:
    virtual void execute() = 0;
    virtual void undo() = 0;
};

// Concrete Commands
class CreateOrderCommand : public Command {
    void execute() { orderService.create(order); }
    void undo() { orderService.delete(orderId); }
};
```

#### Chain of Responsibility Pattern
**Purpose**: Pass requests along chain of handlers.

**System Design Use Cases:**
- **Request Processing Pipeline**: Authentication → Authorization → Validation → Processing
- **Error Handling Chain**: Try handlers in sequence
- **Middleware Chain**: Process through chain
- **Request Validation**: Validate through multiple validators

**Example:**
```c
// Request Handler Chain
class RequestHandler {
protected:
    RequestHandler* next;
    
public:
    void setNext(RequestHandler* handler) {
        next = handler;
    }
    
    virtual void handle(Request req) {
        if (canHandle(req)) {
            process(req);
        } else if (next) {
            next->handle(req);
        }
    }
};

// Chain: AuthHandler -> ValidationHandler -> ProcessingHandler
```

#### State Pattern
**Purpose**: Allow object to alter behavior when internal state changes.

**System Design Use Cases:**
- **Order State Machine**: Order states (Pending, Processing, Shipped, Delivered)
- **Connection States**: Connection lifecycle (Idle, Connecting, Connected, Closed)
- **Job States**: Job processing states
- **Workflow States**: Workflow state management

**Example:**
```c
// Order State
class Order {
private:
    OrderState* state;
    
public:
    void setState(OrderState* s) {
        state = s;
    }
    
    void process() {
        state->process(this);
    }
    
    void cancel() {
        state->cancel(this);
    }
};
```

### 4. System Design Specific Patterns

#### Retry Pattern
**Purpose**: Retry failed operations with exponential backoff.

**Use Cases:**
- Network calls
- Database operations
- External service calls

**Implementation:**
```c
class RetryHandler {
public:
    Result executeWithRetry(function<Result()> operation) {
        int attempts = 0;
        int maxAttempts = 3;
        
        while (attempts < maxAttempts) {
            try {
                return operation();
            } catch (Exception e) {
                attempts++;
                if (attempts >= maxAttempts) throw;
                sleep(exponentialBackoff(attempts));
            }
        }
    }
};
```

#### Timeout Pattern
**Purpose**: Set maximum time for operations.

**Use Cases:**
- API calls
- Database queries
- External service calls

**Implementation:**
```c
class TimeoutHandler {
public:
    Result executeWithTimeout(function<Result()> operation, int timeoutMs) {
        auto future = async(operation);
        if (future.wait_for(timeoutMs) == future_status::timeout) {
            throw TimeoutException();
        }
        return future.get();
    }
};
```

#### Bulkhead Pattern
**Purpose**: Isolate resources to prevent cascading failures.

**Use Cases:**
- Thread pools per service
- Database connections per service
- Resource isolation

**Implementation:**
```c
// Separate thread pools
ThreadPool criticalPool(10);  // For critical services
ThreadPool normalPool(50);     // For normal services
ThreadPool backgroundPool(20); // For background tasks
```

#### Throttling Pattern
**Purpose**: Limit rate of requests.

**Use Cases:**
- API rate limiting
- Request throttling
- Resource protection

**Implementation:**
```c
class RateLimiter {
private:
    int maxRequests;
    int windowSeconds;
    deque<time_t> requests;
    
public:
    bool allowRequest() {
        time_t now = time(nullptr);
        // Remove old requests
        while (!requests.empty() && now - requests.front() > windowSeconds) {
            requests.pop_front();
        }
        
        if (requests.size() >= maxRequests) {
            return false;
        }
        
        requests.push_back(now);
        return true;
    }
};
```

#### Cache-Aside Pattern
**Purpose**: Application manages cache, not cache system.

**Flow:**
1. Check cache
2. If miss, read from database
3. Write to cache
4. Return data

**Implementation:**
```c
class CacheAsideService {
public:
    Data getData(string key) {
        // Check cache
        Data data = cache->get(key);
        if (data != null) {
            return data;
        }
        
        // Cache miss - read from database
        data = database->get(key);
        
        // Write to cache
        cache->set(key, data);
        
        return data;
    }
};
```

#### Write-Through Pattern
**Purpose**: Write to cache and database simultaneously.

**Flow:**
1. Write to cache
2. Write to database
3. Return success

**Use Cases:**
- Critical data
- Need for consistency

#### Write-Behind Pattern
**Purpose**: Write to cache immediately, write to database asynchronously.

**Flow:**
1. Write to cache
2. Return success
3. Write to database asynchronously

**Use Cases:**
- High write throughput
- Acceptable eventual consistency

#### Sharding Pattern
**Purpose**: Partition data across multiple databases.

**Strategies:**
- **Range Sharding**: Partition by range (user_id 0-1000 → DB1)
- **Hash Sharding**: Partition by hash (hash(user_id) % num_shards)
- **Directory Sharding**: Lookup table for shard location

**Example:**
```c
class ShardedDatabase {
private:
    vector<Database> shards;
    
public:
    Database* getShard(string key) {
        int shardId = hash(key) % shards.size();
        return &shards[shardId];
    }
    
    void insert(string key, Data data) {
        Database* shard = getShard(key);
        shard->insert(key, data);
    }
};
```

#### Replication Pattern
**Purpose**: Maintain multiple copies of data.

**Types:**
- **Master-Slave**: One master, multiple read replicas
- **Master-Master**: Multiple masters, bidirectional replication
- **Multi-Master**: Multiple masters, conflict resolution

**Use Cases:**
- Read scaling
- High availability
- Geographic distribution

#### Leader Election Pattern
**Purpose**: Elect leader from group of nodes.

**Use Cases:**
- Distributed coordination
- Master selection
- Service coordination

**Algorithms:**
- **Bully Algorithm**: Highest ID wins
- **Ring Algorithm**: Token passing
- **ZooKeeper**: Using ZooKeeper for election

#### Idempotency Pattern
**Purpose**: Make operations safe to retry.

**Implementation:**
- **Idempotency Keys**: Unique key per operation
- **Check before execute**: Verify if already processed
- **Idempotent operations**: Operations that can be safely repeated

**Example:**
```c
class IdempotentService {
private:
    set<string> processedKeys;
    
public:
    Result processRequest(Request req) {
        string idempotencyKey = req.getIdempotencyKey();
        
        // Check if already processed
        if (processedKeys.contains(idempotencyKey)) {
            return getCachedResult(idempotencyKey);
        }
        
        // Process request
        Result result = doProcess(req);
        
        // Store result
        processedKeys.insert(idempotencyKey);
        cacheResult(idempotencyKey, result);
        
        return result;
    }
};
```

#### Backpressure Pattern
**Purpose**: Control flow when producer is faster than consumer.

**Strategies:**
- **Drop**: Drop excess messages
- **Block**: Block producer until consumer catches up
- **Buffer**: Buffer with size limits
- **Throttle**: Slow down producer

#### Competing Consumers Pattern
**Purpose**: Multiple consumers process messages from queue.

**Use Cases:**
- Parallel processing
- Load distribution
- Scalability

**Example:**
```
Queue: [Msg1, Msg2, Msg3, Msg4, ...]
         │      │      │      │
    Consumer1 Consumer2 Consumer3 Consumer4
```

#### Publisher-Subscriber Pattern
**Purpose**: Decouple publishers and subscribers.

**Components:**
- **Publisher**: Publishes events
- **Subscriber**: Subscribes to events
- **Message Broker**: Routes messages

**Use Cases:**
- Event-driven architecture
- Loose coupling
- Scalability

#### Request-Reply Pattern
**Purpose**: Synchronous request-response communication.

**Use Cases:**
- RPC calls
- API requests
- Service calls

#### Fan-Out Pattern
**Purpose**: Distribute message to multiple consumers.

**Use Cases:**
- Broadcast messages
- Multiple processing paths
- Notification systems

#### Fan-In Pattern
**Purpose**: Aggregate messages from multiple sources.

**Use Cases:**
- Data aggregation
- Result collection
- Merge operations

### 5. Pattern Selection Summary

**By Problem Type:**

| Problem | Pattern |
|---------|---------|
| Object creation | Factory, Builder, Singleton |
| Interface adaptation | Adapter, Facade |
| Behavior extension | Decorator, Strategy |
| Request handling | Chain of Responsibility, Command |
| State management | State, Observer |
| Fault tolerance | Circuit Breaker, Retry, Timeout |
| Performance | Cache-Aside, Write-Through, Sharding |
| Scalability | Replication, Competing Consumers |
| Consistency | Saga, Idempotency |
| Communication | Publisher-Subscriber, Request-Reply |

**Common Pattern Combinations:**
- **Circuit Breaker + Retry**: Fault tolerance
- **Cache-Aside + Write-Through**: Performance + consistency
- **Sharding + Replication**: Scalability + availability
- **Publisher-Subscriber + Competing Consumers**: Event processing
- **Strategy + Factory**: Flexible algorithm selection

---

## Pattern Selection Guide

**Choose Pattern Based On:**

1. **System Complexity**:
   - Simple: Layered Architecture
   - Complex: Microservices, DDD

2. **Team Size**:
   - Small: Monolithic, Layered
   - Large: Microservices, DDD

3. **Scale Requirements**:
   - Low: Layered, Monolithic
   - High: Microservices, Event-Driven

4. **Consistency Requirements**:
   - Strong: Traditional transactions
   - Eventual: Event-Driven, Saga

5. **Technology Constraints**:
   - Flexible: Microservices
   - Constrained: Layered

**Common Combinations:**
- **Microservices + DDD**: Domain-driven microservices
- **Event-Driven + CQRS**: Event sourcing with separate read/write
- **API Gateway + Microservices**: Standard microservices pattern
- **Circuit Breaker + Microservices**: Fault tolerance

---

**Related Posts:**
- [Google System Design Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-system-design-interview-preparation %})
- [Google Coding Interview Problem Solving Methodology]({{ site.baseurl }}{% post_url 2026-01-06-google-coding-interview-problem-solving-methodology %})
- [Zscaler System Reliability & Scalability Interview Preparation]({{ site.baseurl }}{% post_url 2026-01-06-zscaler-system-reliability-scalability-interview-preparation %})

