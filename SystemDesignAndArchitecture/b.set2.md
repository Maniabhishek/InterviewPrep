## Describe some of the most challenging problems that you have solved?
### Problem 1: Handling Duplicate & Inconsistent Provisioning in Distributed Workflows
- One of the most challenging problems I solved was **inconsistent customer provisioning** caused by duplicate and concurrent requests in our lifecycle system.
#### Context:
- The system handled onboarding workflows involving multiple services (account creation, resource provisioning, policy setup) Due to retries, Kafka reprocessing, and concurrent API calls, the same workflow could be triggered multiple times

#### Problem:
- Duplicate requests led to:
- Resources being created multiple times
- Partial provisioning states
- Difficult rollback scenarios

#### Solution
- I redesigned the workflow with idempotency and strong state management:
- Introduced idempotency keys (workflowId / requestId)
- Maintained a state machine in DB
- Enforced unique constraints to prevent duplicate processing

#### Key Design Decisions
- Allowed eventual consistency but ensured state correctness

#### Impact
- Eliminated duplicate provisioning issues
- Improved system reliability significantly
- Reduced production incidents related to inconsistent states

#### Why this was challenging
- Required deep understanding of distributed system failure modes
- Needed balancing consistency vs availability
- Fix had to work without breaking existing workflows

## Problem 2: Debugging & Observability in Distributed Systems
### Another challenging problem was lack of visibility in distributed workflows, which made debugging extremely difficult.

#### Problem
- A single provisioning workflow involved multiple services and async events
- Failures were hard to trace:
- Logs were scattered
- No clear request flow
- High MTTR (mean time to resolution)

#### Solution
- I introduced end-to-end observability:
  - Added correlation IDs per workflow
  - Implemented distributed tracing
  - Standardized structured logging across services
- Built dashboards for:
  - Workflow success/failure rates
  - Latency
  - Retry patterns    

#### Impact
- Reduced debugging time from hours → minutes
- Improved on-call efficiency
- Enabled proactive issue detection

#### Why this was challenging
- Required coordination across multiple services/teams
