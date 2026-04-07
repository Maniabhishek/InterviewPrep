1. Describe a backend system you designed or worked on that had to scale significantly. What were the key design decisions and trade-offs?
> One of the most impactful systems I worked on was the customer lifecycle and provisioning system for the Kyndryl Bridge platform. This system handled onboarding, provisioning, de-provisioning and lifecycle management for enterprise customers across multiple cloud providers. As adoption grew, we had to scale from handling a few provisioning requests per hour to thousands of concurrent workflows, each involving multiple dependent steps and external integrations.

### Key Challenges
- High concurrency (multiple onboarding workflows running simultaneously)
- Long-running workflows (minutes to hours)
- Need for reliability (no partial provisioning)
- Observability and debugging failures

### Key Design Decisions
1. Event-Driven Architecture (Decoupling via Kafka) Instead of synchronous APIs, we moved to event-driven workflows using Kafka, Each step (create account, assign resources, configure policies) became an independent consumer

- ✅ Why:
    - Loose coupling
    - Better scalability
    - Retry & failure isolation

- ⚠️ Trade-off:
    - Increased complexity (event ordering, debugging)
    - Eventual consistency instead of immediate consistency

2. Workflow Orchestration vs Choreography
Initially used pure event choreography → became hard to track state
Introduced a workflow orchestrator service to maintain state machine

✅ Why:

Centralized control of workflow state
Easier debugging and retries

⚠️ Trade-off:

Slight coupling introduced
Orchestrator became a critical component (needed HA)

2. Workflow Orchestration vs Choreography
- Initially used pure event choreography → became hard to track state
- Introduced a workflow orchestrator service to maintain state machine
- ✅ Why:
  - Centralized control of workflow state
  - Easier debugging and retries
- ⚠️ Trade-off:
  - Slight coupling introduced
  - Orchestrator became a critical component (needed HA)

3. Idempotency & Retry Mechanism
- Every operation was designed to be idempotent
- Used unique request IDs + state tracking in DB
- ✅ Why:
  - Safe retries in distributed systems
  - Handles duplicate Kafka events
- ⚠️ Trade-off:
  - Additional storage and logic overhead

2. What is Idempotency
- An operations is idempotenct if, performing it multiple times has the same effect as doing it once
- post is idempotent as it can create or have side effect if called multiple times
- where as patch is also conditionally idempotent 
- Example 1 (Non-idempotent):
```
PATCH /user/1
{
  "incrementAge": 1
}
1st call → age = 26
2nd call → age = 27
3rd call → age = 28
```
```
Example 2 (Idempotent PATCH):
PATCH /user/1
{
  "age": 25
}
1 call → age = 25
10 calls → still 25

👉 Same result → idempotent
```
