## How did you handle scaling bottlenecks in your system?

### 1. Identifying Bottlenecks
- We used metrics like:
- p95/p99 latency
- Throughput
- Resource utilization (CPU, DB, thread pools)
- 👉 This helped us pinpoint bottlenecks across:
    - Application layer
    - Database
    - External dependencies
### 2. Key Bottlenecks & Solutions
#### A. Synchronous Processing → Moved to Async (Major Win)
- Problem:
    - APIs were tightly coupled and synchronous
    - Long-running provisioning workflows blocked threads
- Solution:
    - Introduced event-driven architecture using Kafka
    - Broke workflows into async steps
- Impact:
    - Improved throughput significantly
    - Reduced request latency at API layer
    - Trade-off:
    - Introduced eventual consistency

#### B. Database Bottlenecks
- Problem:
    - High write contention on workflow/state tables
    - Slow queries under load
- Solution:
    - Added indexes for critical queries
    - Implemented partitioning (by workflow/customer)
    - Used read replicas for read-heavy operations
- Impact:
    - Reduced query latency
    - Improved write scalability

### C. Thread Pool / Connection Pool Exhaustion
- Problem:
    - Threads blocked on long external calls
    - DB connection pool saturation
- Solution:
    - Tuned thread pool sizes
    - Introduced timeouts and retries
    - Used bulkhead pattern to isolate failures
- Impact:
    - Prevented cascading failures
    - Improved system stability

### D. Resource Contention (Shared Environment)
- Problem:
    - CPU spikes caused latency due to shared infrastructure    
- Solution:
    - Added resource limits and autoscaling
    - Improved horizontal scaling of stateless services
- Impact:
    - Reduced tail latency (p99 improvements)

### E. Lack of Backpressure
- Problem:
    - Sudden traffic spikes overwhelmed downstream systems
- Solution:
    - Introduced queue-based buffering (Kafka)
    - Controlled consumption rate
- Impact:
    - Smoothed traffic spikes
    - Avoided system overload

### Key Design Principles I Applied
- Decoupling via async communication
- Horizontal scalability (stateless services)
- Idempotency for safe retries
- Backpressure handling
- Observability-first debugging
