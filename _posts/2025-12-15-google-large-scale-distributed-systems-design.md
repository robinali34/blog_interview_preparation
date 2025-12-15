---
layout: post
title: "Google Large-Scale Distributed Systems Design Interview Guide"
date: 2025-12-15 14:00:00 -0000
categories: interview-preparation system-design google distributed-systems
tags: google system-design distributed-systems large-scale scalability consistency availability
excerpt: "Comprehensive guide for Google large-scale distributed systems design interviews covering distributed systems concepts, CAP theorem, consistency models, and Google-scale architecture patterns."
---

# Google Large-Scale Distributed Systems Design Interview Guide

A focused guide for preparing for Google large-scale distributed systems design interviews. This guide covers distributed systems fundamentals, Google-scale architecture patterns, and advanced concepts.

## Interview Format

- **Duration**: 45 minutes total
- **Design Exercise**: 35-40 minutes (remaining time for intro/outro/questions)
- **Format**: Video call with shared whiteboard/drawing tool
- **Focus**: Distributed systems, large-scale architecture, consistency, availability
- **Scale**: Billions of users, petabytes of data, global distribution
- **Style**: Deep technical discussion, emphasis on distributed systems concepts

## What Google Evaluates

- ✅ **Distributed Systems Knowledge**: CAP theorem, consistency models, consensus
- ✅ **Scalability**: Design for billions of users and petabytes of data
- ✅ **Consistency**: Understanding of consistency models and trade-offs
- ✅ **Availability**: High availability, fault tolerance, graceful degradation
- ✅ **Performance**: Low latency, high throughput at scale
- ✅ **Trade-offs**: Deep understanding of distributed systems trade-offs

## Core Distributed Systems Concepts

### CAP Theorem

**CAP Theorem** states that in a distributed system, you can only guarantee 2 out of 3:

- **Consistency**: All nodes see the same data simultaneously
- **Availability**: System remains operational
- **Partition Tolerance**: System continues despite network partitions

**Google's Approach:**
- Most systems prioritize **Availability** and **Partition Tolerance** (AP)
- Some systems need **Consistency** and **Partition Tolerance** (CP)
- Choose based on use case requirements

**Examples:**
- **AP Systems**: DNS, web caching (eventual consistency OK)
- **CP Systems**: Distributed locks, leader election (consistency critical)
- **CA Systems**: Not possible in distributed systems (partition always possible)

---

### Consistency Models

#### Strong Consistency
- All reads return the most recent write
- Linearizability, sequential consistency
- **Use When**: Financial transactions, leader election
- **Trade-off**: Higher latency, lower availability

#### Eventual Consistency
- System will become consistent eventually
- Reads may return stale data temporarily
- **Use When**: Social feeds, DNS, caching
- **Trade-off**: Lower latency, higher availability, but stale reads possible

#### Weak Consistency
- No guarantees about when consistency will be achieved
- **Use When**: Real-time analytics, metrics
- **Trade-off**: Fastest, but least guarantees

---

### Distributed Systems Patterns

#### Replication Strategies

**Master-Slave (Primary-Replica):**
- One master handles writes
- Multiple replicas handle reads
- **Use When**: Read-heavy workloads
- **Trade-off**: Simple but master is bottleneck

**Master-Master (Multi-Master):**
- Multiple masters handle writes
- **Use When**: Write-heavy, multi-region
- **Trade-off**: More complex, conflict resolution needed

**Quorum-Based:**
- Write to majority of nodes
- Read from majority of nodes
- **Use When**: Need strong consistency
- **Trade-off**: Higher latency, but strong guarantees

---

#### Sharding Strategies

**Horizontal Sharding:**
- Partition data across multiple servers
- **Strategies**:
  - **Range-based**: Partition by key range
  - **Hash-based**: Partition by hash of key
  - **Directory-based**: Lookup table for shard location

**Vertical Sharding:**
- Split by feature/service
- **Use When**: Different access patterns

**Consistent Hashing:**
- Distributes data evenly
- Handles node additions/removals gracefully
- **Use When**: Dynamic cluster, need rebalancing

---

## Large-Scale Architecture Patterns

### Pattern 1: Distributed Storage Systems

#### Design Considerations:
- **Data Partitioning**: How to split data across nodes
- **Replication**: How many replicas, where to place them
- **Consistency**: Strong vs eventual consistency
- **Durability**: How to ensure data isn't lost
- **Fault Tolerance**: How to handle node failures

#### Example: Design a Distributed Key-Value Store

**Requirements:**
- Store billions of key-value pairs
- Support millions of QPS
- High availability (99.99%)
- Eventual consistency acceptable

**Design:**

**Architecture:**
```
[Client] → [Load Balancer] → [Coordinator Nodes] → [Storage Nodes]
                ↓                      ↓                    ↓
            [Cache]              [Metadata]          [Replicas]
```

**Key Components:**

1. **Storage Nodes**: Store actual data
   - Sharded by consistent hashing
   - Each shard replicated 3x
   - Data stored in SSTables (sorted string tables)

2. **Coordinator Nodes**: Handle requests
   - Route requests to correct storage node
   - Handle replication
   - Maintain metadata

3. **Consistent Hashing**:
   - Hash key to determine storage node
   - Virtual nodes for even distribution
   - Easy to add/remove nodes

4. **Replication**:
   - 3 replicas per shard
   - Quorum writes (write to 2/3)
   - Quorum reads (read from 2/3)

5. **Consistency**:
   - Eventual consistency (AP system)
   - Vector clocks for conflict resolution
   - Read repair for consistency

**Scaling:**
- Add more storage nodes
- Rebalance using consistent hashing
- Add coordinator nodes for more throughput

---

### Pattern 2: Distributed Consensus

#### Consensus Algorithms

**Raft:**
- Leader election
- Log replication
- **Use When**: Need strong consistency, simpler than Paxos

**Paxos:**
- Classic consensus algorithm
- More complex than Raft
- **Use When**: Need proven consensus

**ZAB (Zookeeper):**
- Used by Apache ZooKeeper
- Similar to Raft
- **Use When**: Coordination services

#### Example: Design a Distributed Lock Service

**Requirements:**
- Distributed locking across multiple nodes
- High availability
- Low latency
- Deadlock prevention

**Design:**

**Architecture:**
```
[Client] → [Lock Service] → [Consensus Layer (Raft)] → [Storage]
```

**Key Components:**

1. **Lock Service Nodes**: Handle lock requests
   - Multiple nodes for availability
   - Leader handles writes (Raft)

2. **Consensus Layer (Raft)**:
   - Leader election
   - Log replication
   - Ensures consistency

3. **Lock Storage**:
   - Store lock metadata
   - Lock expiration (TTL)
   - Lock ownership

**Lock Operations:**

**Acquire Lock:**
- Client requests lock
- Leader checks if available
- If available, grant with TTL
- Replicate to followers
- Return success

**Release Lock:**
- Client releases lock
- Leader removes lock
- Replicate to followers

**Lock Expiration:**
- Heartbeat mechanism
- Auto-release expired locks
- Prevent deadlocks

---

### Pattern 3: Distributed Data Processing

#### MapReduce Pattern

**Map Phase:**
- Split data into chunks
- Process chunks in parallel
- Output key-value pairs

**Shuffle Phase:**
- Group values by key
- Transfer to reduce nodes

**Reduce Phase:**
- Aggregate values per key
- Output final results

#### Example: Design a Distributed Log Processing System

**Requirements:**
- Process terabytes of logs per day
- Real-time and batch processing
- Query logs efficiently

**Design:**

**Architecture:**
```
[Log Sources] → [Kafka] → [Stream Processors] → [Storage]
                                    ↓
                              [Batch Processors] → [Data Warehouse]
```

**Key Components:**

1. **Log Ingestion (Kafka)**:
   - Distributed message queue
   - Partition logs by source/time
   - High throughput

2. **Stream Processing**:
   - Real-time processing
   - Aggregations, filtering
   - Low latency queries

3. **Batch Processing**:
   - Process historical data
   - Complex aggregations
   - Data warehouse queries

4. **Storage**:
   - **Hot Storage**: Recent logs (Elasticsearch)
   - **Cold Storage**: Older logs (S3, BigQuery)

**Scaling:**
- Partition Kafka topics
- Scale stream processors horizontally
- Partition storage by time/source

---

### Pattern 4: Distributed Caching

#### Cache Consistency Strategies

**Cache-Aside (Lazy Loading):**
- App checks cache, fetches from DB if miss
- **Pros**: Simple, cache failures don't affect DB
- **Cons**: Cache miss penalty, possible stale data

**Write-Through:**
- Write to cache and DB simultaneously
- **Pros**: Cache always consistent
- **Cons**: Higher write latency

**Write-Behind (Write-Back):**
- Write to cache, async write to DB
- **Pros**: Fast writes
- **Cons**: Risk of data loss, complexity

**Refresh-Ahead:**
- Proactively refresh before expiration
- **Pros**: Lower cache miss rate
- **Cons**: Wastes resources if not accessed

#### Example: Design a Distributed Cache

**Requirements:**
- Cache terabytes of data
- Millions of QPS
- High availability
- Eventual consistency acceptable

**Design:**

**Architecture:**
```
[Client] → [Cache Proxy] → [Cache Cluster] → [Database]
```

**Key Components:**

1. **Cache Cluster**:
   - Sharded by consistent hashing
   - Each shard replicated 3x
   - In-memory storage (Redis)

2. **Cache Proxy**:
   - Route requests to correct node
   - Handle failover
   - Load balancing

3. **Consistent Hashing**:
   - Distribute keys evenly
   - Handle node failures gracefully
   - Virtual nodes for balance

**Consistency:**
- Eventual consistency (AP system)
- Read repair for consistency
- TTL-based expiration

**Scaling:**
- Add more cache nodes
- Rebalance using consistent hashing
- Increase replication factor

---

## Google-Specific Distributed Systems

### Google File System (GFS)

**Key Concepts:**
- **Chunk Servers**: Store file chunks (64MB each)
- **Master Node**: Metadata, chunk location
- **Replication**: 3 replicas per chunk
- **Consistency**: Relaxed consistency model

**Design Principles:**
- Optimize for large files
- Handle failures gracefully
- High throughput over low latency

---

### Bigtable

**Key Concepts:**
- **Tablets**: Rows partitioned into tablets
- **SSTables**: Sorted string tables for storage
- **Compression**: Compress SSTables
- **Bloom Filters**: Fast non-existence checks

**Design Principles:**
- Wide-column store
- Sparse data support
- High write throughput

---

### Spanner

**Key Concepts:**
- **Global Distribution**: Multi-region, globally distributed
- **Strong Consistency**: External consistency across regions
- **TrueTime**: Hardware clocks for ordering
- **Paxos**: Consensus for replication

**Design Principles:**
- Strong consistency globally
- Low latency despite global distribution
- High availability

---

## Common Large-Scale Distributed Systems Questions

### Very High Frequency ⭐⭐⭐

1. **Design a Distributed Key-Value Store**
   - Sharding, replication, consistency
   - Scale: Billions of keys, millions of QPS

2. **Design a Distributed File System**
   - Chunk storage, replication, metadata
   - Scale: Petabytes of data, millions of files

3. **Design a Distributed Database**
   - Sharding, replication, transactions
   - Scale: Petabytes of data, millions of QPS

4. **Design a Distributed Cache**
   - Sharding, consistency, eviction
   - Scale: Terabytes of cache, millions of QPS

5. **Design a Distributed Lock Service**
   - Consensus, leader election, fault tolerance
   - Scale: Millions of locks, high concurrency

### High Frequency ⭐⭐

6. **Design a Distributed Counter**
   - Sharding, aggregation, consistency
   - Scale: Millions of increments per second

7. **Design a Distributed Task Queue**
   - Task distribution, fault tolerance, scheduling
   - Scale: Millions of tasks per day

8. **Design a Distributed Search System**
   - Index sharding, query routing, ranking
   - Scale: Billions of documents, millions of QPS

9. **Design a Distributed Log System**
   - Log aggregation, storage, querying
   - Scale: Terabytes of logs per day

10. **Design a Distributed Configuration System**
    - Configuration storage, updates, distribution
    - Scale: Millions of services, real-time updates

---

## Step-by-Step Framework for Distributed Systems

### 1. Clarify Requirements (4-5 minutes)

**Key Questions:**
- What's the scale? (users, QPS, data size)
- What are the consistency requirements? (strong, eventual)
- What are the availability requirements? (99.9%, 99.99%)
- What are the latency requirements? (p50, p95, p99)
- What are the geographic requirements? (single region, multi-region)
- What are the durability requirements? (data retention, backup)

---

### 2. High-Level Architecture (7-8 minutes)

**Draw:**
- Client layer
- API/Service layer
- Data layer (sharded, replicated)
- Consensus layer (if needed)
- Monitoring/observability

**Discuss:**
- Overall architecture
- Key components
- Data flow
- High-level trade-offs

---

### 3. Deep Dive - Distributed Systems (15-18 minutes)

**For Each Component:**

#### Data Partitioning
- **Sharding Strategy**: How to partition data
- **Consistent Hashing**: For dynamic clusters
- **Replication Factor**: How many replicas
- **Replica Placement**: Where to place replicas

#### Consistency & Consensus
- **Consistency Model**: Strong vs eventual
- **Consensus Algorithm**: Raft, Paxos (if needed)
- **Conflict Resolution**: How to handle conflicts
- **Vector Clocks**: For causality tracking

#### Fault Tolerance
- **Failure Detection**: How to detect failures
- **Failover**: How to handle failures
- **Data Recovery**: How to recover lost data
- **Split-Brain Prevention**: How to prevent partitions

#### Performance
- **Caching Strategy**: What to cache, where
- **Load Balancing**: How to distribute load
- **Connection Pooling**: Manage connections
- **Batching**: Batch operations for efficiency

---

### 4. Scale the Design (6-7 minutes)

**Scaling Strategies:**
- **Horizontal Scaling**: Add more nodes
- **Vertical Scaling**: Increase node capacity (limited)
- **Geographic Distribution**: Multi-region deployment
- **Data Partitioning**: Shard data across nodes
- **Read Replicas**: Scale reads independently
- **Caching**: Reduce load on storage

**Handle Failures:**
- **Replication**: Multiple replicas for availability
- **Quorum**: Majority-based operations
- **Circuit Breakers**: Fail fast when downstream is down
- **Graceful Degradation**: Serve reduced functionality

---

### 5. Trade-offs & Edge Cases (3-4 minutes)

**Key Trade-offs:**
- **Consistency vs Availability**: CAP theorem
- **Latency vs Consistency**: Strong consistency = higher latency
- **Cost vs Performance**: More nodes vs optimization
- **Complexity vs Scalability**: Simple vs scalable

**Edge Cases:**
- **Network Partitions**: Split-brain scenarios
- **Node Failures**: How to handle gracefully
- **Data Corruption**: How to detect and recover
- **Clock Skew**: How to handle time differences

---

## Detailed Example: Design a Distributed Key-Value Store

### Step 1: Clarify Requirements

**Functional Requirements:**
- Store key-value pairs
- Get value by key
- Put/update value by key
- Delete key-value pair

**Non-Functional Requirements:**
- Scale: Billions of keys, millions of QPS
- Availability: 99.99%
- Consistency: Eventual consistency acceptable
- Latency: <10ms p95
- Durability: Data persisted, not lost

**Clarifying Questions:**
- "What's the key/value size?" → Keys: <1KB, Values: <1MB
- "What's the read/write ratio?" → 80% reads, 20% writes
- "Do we need transactions?" → No, single key operations
- "Geographic distribution?" → Multi-region

---

### Step 2: High-Level Architecture

```
[Client] → [Load Balancer] → [Coordinator] → [Storage Nodes]
                ↓                  ↓              ↓
            [Cache]          [Metadata]      [Replicas]
```

**Components:**
1. **Coordinator Nodes**: Route requests, handle metadata
2. **Storage Nodes**: Store actual data
3. **Metadata Store**: Stores shard locations
4. **Cache Layer**: Cache hot data

---

### Step 3: Deep Dive

#### Data Partitioning

**Sharding Strategy: Consistent Hashing**
- Hash key to determine storage node
- Virtual nodes for even distribution
- Easy to add/remove nodes (only rebalance ~1/N of data)

**Replication:**
- 3 replicas per shard
- Place replicas in different racks/data centers
- Quorum-based operations (read/write from 2/3)

#### Consistency Model

**Eventual Consistency (AP System):**
- Writes go to primary, async replicate to replicas
- Reads can go to any replica (may be stale)
- Read repair: Update stale replicas on read
- Vector clocks for conflict resolution

**Why Eventual Consistency:**
- Higher availability
- Lower latency
- Acceptable for most use cases

#### Storage Format

**SSTables (Sorted String Tables):**
- Immutable sorted files
- Write-optimized (append-only)
- Read-optimized (sorted, can use binary search)
- Compressed for storage efficiency

**Write Path:**
- Write to memtable (in-memory)
- When memtable full, flush to SSTable
- Background compaction merges SSTables

**Read Path:**
- Check memtable first
- Then check SSTables (newest first)
- Merge results from multiple SSTables

#### Fault Tolerance

**Node Failures:**
- Detect via heartbeat
- Promote replica to primary
- Replicate data to new replica

**Network Partitions:**
- Quorum-based: Continue with majority
- Minority partition: Read-only mode
- Merge when partition heals

---

### Step 4: Scale the Design

#### Handle High Write Volume
- **Sharding**: Partition writes across nodes
- **Batching**: Batch writes for efficiency
- **Write Buffering**: Buffer writes, flush in batches

#### Handle High Read Volume
- **Read Replicas**: Scale reads independently
- **Caching**: Cache hot data (80% of reads from cache)
- **Read Distribution**: Distribute reads across replicas

#### Handle Large Data
- **Compression**: Compress SSTables
- **Compaction**: Merge and compact SSTables
- **Tiered Storage**: Hot data in memory, cold data on disk

#### Geographic Distribution
- **Multi-Region**: Deploy in multiple regions
- **Replication**: Replicate data across regions
- **Route to Nearest**: Route requests to nearest region

---

### Step 5: Trade-offs & Edge Cases

**Trade-offs:**
- **Consistency vs Availability**: Chose availability (AP)
- **Latency vs Consistency**: Lower latency, accept stale reads
- **Storage vs Performance**: Compression vs faster reads

**Edge Cases:**
- **Split-Brain**: Quorum prevents split-brain
- **Clock Skew**: Use logical clocks (vector clocks)
- **Data Corruption**: Checksums, replication for recovery
- **Hot Keys**: Single key getting too many requests → Cache heavily

---

## Quick Reference: Distributed Systems Concepts

### Consensus Algorithms

**Raft:**
- Leader election
- Log replication
- Simpler than Paxos
- **Use When**: Need consensus, want simplicity

**Paxos:**
- Classic consensus algorithm
- More complex
- **Use When**: Need proven consensus

**PBFT (Practical Byzantine Fault Tolerance):**
- Handles Byzantine failures
- **Use When**: Need to handle malicious nodes

---

### Consistency Models

**Strong Consistency:**
- Linearizability
- Sequential consistency
- **Use When**: Financial transactions, critical data

**Eventual Consistency:**
- Will become consistent eventually
- **Use When**: Social feeds, caching, DNS

**Causal Consistency:**
- Preserves causality
- **Use When**: Need causality but not full consistency

---

### Replication Strategies

**Synchronous Replication:**
- Wait for all replicas
- **Pros**: Strong consistency
- **Cons**: Higher latency

**Asynchronous Replication:**
- Don't wait for replicas
- **Pros**: Lower latency
- **Cons**: Risk of data loss

**Semi-Synchronous:**
- Wait for majority
- **Pros**: Balance of consistency and latency
- **Cons**: More complex

---

## Last-Minute Preparation Checklist

### Tonight (Before Interview)

- [ ] **Review Distributed Systems Concepts**: CAP theorem, consistency models, consensus
- [ ] **Practice Drawing**: Distributed architecture diagrams
- [ ] **Review Google Systems**: GFS, Bigtable, Spanner concepts
- [ ] **Practice One Full Design**: Time yourself (35-40 minutes)
- [ ] **Review Common Questions**: Distributed storage, consensus, caching
- [ ] **Prepare Questions**: Questions to ask interviewer

### Key Concepts to Review

- [ ] **CAP Theorem**: Consistency, Availability, Partition tolerance
- [ ] **Consistency Models**: Strong, eventual, causal
- [ ] **Consensus Algorithms**: Raft, Paxos
- [ ] **Sharding**: Consistent hashing, range-based, hash-based
- [ ] **Replication**: Master-slave, master-master, quorum
- [ ] **Fault Tolerance**: Failure detection, failover, recovery
- [ ] **Vector Clocks**: Causality tracking
- [ ] **SSTables**: Storage format for distributed systems

### Google-Specific to Review

- [ ] **GFS**: Chunk servers, master node, replication
- [ ] **Bigtable**: Tablets, SSTables, compression
- [ ] **Spanner**: Global distribution, TrueTime, Paxos
- [ ] **MapReduce**: Distributed data processing
- [ ] **Scale**: Billions of users, petabytes of data

---

## Interview Day Tips

### Before the Interview (30 minutes before)

- [ ] **Review Framework**: Step-by-step approach
- [ ] **Practice Drawing**: Distributed architecture diagrams
- [ ] **Review Key Concepts**: CAP theorem, consistency models
- [ ] **Prepare Questions**: 2-3 thoughtful questions
- [ ] **Test Tech**: Whiteboard tool, video, audio

### During the Interview

1. **Start with Clarification**
   - Ask about consistency requirements
   - Understand scale and constraints
   - Clarify availability vs consistency needs

2. **Think Out Loud**
   - Explain distributed systems concepts
   - Discuss trade-offs explicitly
   - Show your reasoning

3. **Draw Clearly**
   - Show node topology
   - Show data flow
   - Show replication and sharding

4. **Discuss Trade-offs**
   - CAP theorem trade-offs
   - Consistency vs availability
   - Latency vs consistency
   - Cost vs performance

5. **Address Failures**
   - How to handle node failures
   - How to handle network partitions
   - How to prevent split-brain

6. **Handle Feedback**
   - Listen to interviewer's suggestions
   - Incorporate feedback
   - Show you can collaborate

### Common Mistakes to Avoid

1. ❌ **Ignoring CAP Theorem**: Always discuss consistency vs availability
2. ❌ **Not Addressing Failures**: Always discuss fault tolerance
3. ❌ **Over-Complicating**: Start simple, add complexity as needed
4. ❌ **Not Discussing Trade-offs**: Every decision has trade-offs
5. ❌ **Ignoring Scale**: Always consider Google scale (billions, petabytes)
6. ❌ **Not Drawing**: Visuals help explain distributed systems

---

## Questions to Ask Interviewer

### About Requirements
- "What are the consistency requirements? (strong vs eventual)"
- "What's the expected scale? (users, QPS, data)"
- "What are the availability requirements?"
- "Do we need global distribution?"

### About Constraints
- "Are there any technology constraints?"
- "What's the acceptable latency?"
- "What's the data retention policy?"

### About the Role
- "What kind of distributed systems does the team work on?"
- "What are the biggest technical challenges?"
- "How does the team approach distributed systems design?"

---

## Final Checklist - Right Before Interview

### 5 Minutes Before
- [ ] Take deep breaths
- [ ] Review CAP theorem mentally
- [ ] Review consistency models
- [ ] Have whiteboard tool ready
- [ ] Smile and be confident

### During Interview
- [ ] Clarify consistency requirements first
- [ ] Think out loud
- [ ] Draw clearly (show nodes, replication, sharding)
- [ ] Discuss CAP theorem trade-offs
- [ ] Address failures explicitly
- [ ] Handle feedback gracefully

### After Interview
- [ ] Send thank you note
- [ ] Reflect on what went well
- [ ] Note areas to improve
- [ ] Follow up as needed

---

## Conclusion

Google large-scale distributed systems interviews focus on:
- **Distributed Systems Fundamentals**: CAP theorem, consistency, consensus
- **Scale**: Design for billions of users and petabytes of data
- **Fault Tolerance**: Handle failures gracefully
- **Trade-offs**: Deep understanding of distributed systems trade-offs
- **Google Systems**: Understanding of GFS, Bigtable, Spanner concepts

**Key Success Factors:**
1. ✅ Understand CAP theorem deeply
2. ✅ Know consistency models and when to use each
3. ✅ Understand consensus algorithms (Raft, Paxos)
4. ✅ Address failures explicitly
5. ✅ Discuss trade-offs clearly
6. ✅ Consider Google scale (billions, petabytes)

**Remember**: Google values deep understanding of distributed systems. Show your knowledge of CAP theorem, consistency models, and fault tolerance. Be ready to discuss trade-offs and handle failures.

Good luck with your interview! You've got this! 🚀

---

**Related Posts:**
- [Google System Design Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-system-design-interview-preparation %})
- [Google Coding Interview Preparation]({{ site.baseurl }}{% post_url 2025-12-11-google-coding-interview-preparation %})
- [Google Behavioral Interview Guide]({{ site.baseurl }}{% post_url 2025-11-13-google-behavioral-interviews %})

