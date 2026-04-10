## there is an application which is working completely fine , this application is hosted in shared environment where some other application is also being hosted by some other teams, so what happened my application was running totally fine , but all of a sudden my application started facing some issue application become way too slow and it getting timed out , tell me step by step procedurre i should follow to debug it , from senior softeare developer POV

- In such a scenario i follow a structured approach:
  - Detect → Scope → Isolate → Validate → Fix → Prevent”

### Confirm & Quantify the Problem
- Check:
    - Latency increase (p95, p99)
    - Error rates (timeouts, 5xx)
    - Traffic patterns (sudden spike?)
- 👉 Goal:
    - Is it real degradation or just perception?
    - Is it global or limited to some users/regions?
    - 🌍 2. Define the Blast Radius

### Identify problem
- Questions I ask:
    - Are all endpoints slow or only specific ones?
    - Are other services also impacted?
    - Did any other team deploy something recently?
- 👉 Goal:
    - Identify if it’s:
    - App-specific
    - Infrastructure-level
    - Shared resource contention

### Check Recent Changes (Fastest Wins First)
- Even if "nothing changed", I always verify Recent:
- Deployments (code/config)
- Infra changes (CPU limits, scaling configs)
- Dependency changes (DB, cache, APIs), Break down latency: 
    - Database:
        - Slow queries?
        - Locks?
        - Connection exhaustion?
    - External APIs:
        - Increased latency?
        - Timeouts?
    - Cache:
        - Cache misses increasing?
- 👉 Reality:
    - 80% of issues come from recent changes

### Resource Contention Analysis (VERY IMPORTANT here)
- Since it's a shared environment, I immediately check:
- CPU usage (is it throttled?)
- Memory (OOM / GC pressure?)
- Disk I/O
- Network latency
- 👉 Especially:
    - Noisy neighbor problem (another app consuming resources)

### Application-Level Investigation
- Now I go deeper into the app:
- Check:
- Thread pool exhaustion
- Connection pool saturation (DB, HTTP)
- Increased queue backlog
- Slow external dependencies
- 👉 Example:
    - DB calls suddenly taking longer
    - Thread pool blocked → cascading latency


### Use Observability (Critical for SDE-3)
- Distributed tracing → find slow spans
- Logs → error patterns
- Metrics → correlation (CPU spike → latency spike)
- 👉 Example:
- “Tracing helps pinpoint exactly which service or DB call is slow”

### 8. Hypothesis-Driven Debugging
- “Another service is hogging CPU”
- “DB connection pool exhausted”
- “Thread pool blocked”
- Then validate:
    - Scale pods → does latency improve?
    - Restart → temporary relief?
    - Isolate instance → compare behavior
### Immediate Mitigation
- Before root cause is fully fixed:
    - Scale horizontally
    - Restart affected instances
    - Throttle traffic / rate limit
    - Increase resource limits
