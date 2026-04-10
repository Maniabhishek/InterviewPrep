### p99 ↑, p95 slightly ↑, p50 normal
-  Interpretation:
- Only a small % of requests are slow → tail latency issue
- 🔍 Likely causes:
- Thread pool contention
- DB locks
- GC pauses
- External API spikes
- 🎯 How to confirm:
- Check thread pool / connection pool usage
- Look for slow queries or lock waits
- Check GC logs
### 2. p95 ↑, p99 ↑, p50 normal
- Interpretation:
- More users affected, but not all
- Problem is spreading
- 🔍 Likely causes:
- Resource contention starting (CPU / DB)
- Cache misses increasing
- Dependency slowing down
- 🎯 Action:
- Check CPU trends
- DB latency
- Cache hit ratio
### 3. p50 ↑, p95 ↑, p99 ↑ (all bad)
- Interpretation:
- Entire system is slow
- 🔥 Likely causes:
- CPU saturation
- Network issues
- Infra problem
- Noisy neighbor
- 🎯 Action:
- Check CPU, memory, I/O
- Check if other services are impacted
- Scale system

### 4. p99 ↑, CPU ↑
- Interpretation:
- Requests waiting for CPU → contention
- 🔥 Likely causes:
- High load
- Noisy neighbor (shared env)
- Inefficient code
- 🎯 Action:
- Check CPU throttling
- Scale instances
- Profile CPU usage

### 5. p99 ↑, CPU normal
- Interpretation:
- Not CPU → something is blocking
- 🔥 Likely causes:
- DB locks
- Thread pool exhaustion
- I/O wait
- External API latency
- 🎯 Action:
- Check DB query time
- Check thread states (blocked/waiting)
- Look at dependency latency

### 6. Latency ↑ + Error rate ↑ (timeouts)
- 🧠 Interpretation:
- System is overloaded
- 🔥 Likely causes:
- Thread/connection pool exhaustion
- Downstream dependency failing
- 🎯 Action:
- Check pool limits
- Add rate limiting / circuit breaker

### 7. Throughput ↓ but latency ↑
- 🧠 Interpretation:
- System is choking
- 🔥 Likely causes:
- Resource exhaustion
- Deadlocks
- Queue buildup

### 8. Sudden spike (everything was fine before)
- 🧠 Interpretation:
- External trigger
- 🔥 Likely causes:
- Deployment
- Traffic spike
- Noisy neighbor
- Dependency degradation
