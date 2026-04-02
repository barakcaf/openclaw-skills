---
description: "Code quality, architecture, and system design expertise"
alwaysApply: false
globs:
  - "**/*.py"
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.go"
  - "**/*.java"
---

# Code Quality & Architecture

## Project-Specific Conventions
<!-- ADAPT: Replace with repo-specific rules (language, patterns, testing, etc.) -->
- Clear separation: handlers → services → data layer
- No business logic in entry points — delegate to service modules
- Run tests before finishing any change

## Architecture & Design Patterns
- AWS Well-Architected Framework across all 6 pillars (Operational Excellence,
  Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability)
- Service decomposition using Domain-Driven Design and bounded contexts
- Distributed system patterns: CQRS, Event Sourcing, Saga, Outbox, Circuit Breaker,
  Bulkhead, Sidecar, Strangler Fig
- API design: REST, gRPC, GraphQL — consistency, versioning, idempotency,
  pagination, backward compatibility

## AWS Service Limits (know these)
- Lambda: 1000 default concurrent executions, 10 burst concurrency
- API Gateway: 10,000 RPS (regional), 29s integration timeout (WebSocket)
- DynamoDB: 40,000 RCU/WCU per table (on-demand auto-scales), 1MB scan limit
- SQS: unlimited throughput (standard), 300 msg/s (FIFO), 256KB message size
- SNS: 100,000 topics, 12.5M subscriptions per topic
- Kinesis: 1 MB/s per shard input, 2 MB/s output

## Compute Optimization
- Lambda cold start mitigation: provisioned concurrency, SnapStart, minimal package size
- ECS/Fargate sizing: right-size CPU/memory, Graviton vs x86 tradeoffs
- Cost awareness: Reserved/Savings Plans, data transfer ($0.09/GB cross-region)

## Messaging Patterns
- SQS FIFO vs Standard: ordering guarantees, exactly-once vs at-least-once, dedup
- SNS fan-out: filtering policies, delivery policies, DLQ for failed deliveries
- EventBridge: schema registry, content-based filtering, cross-account event bus

## Data Modeling
- DynamoDB: access patterns drive schema, GSI overloading, single-table vs multi-table
- Caching: cache-aside, write-through, TTL design, cache stampede prevention
- Query optimization, index design, N+1 detection

## Code Standards
- Cyclomatic complexity thresholds, function length, cognitive load
- SOLID principles, composition over inheritance
- Dependency management: version pinning, CVE exposure, minimal surface area

## Error Handling
- Exception taxonomy with clear hierarchy
- Retry with exponential backoff and jitter for AWS SDK calls
- Dead-letter queue strategies for async failures
- Never swallow exceptions silently

## Observability
- Structured logging (JSON) with correlation IDs
- Distributed tracing (X-Ray) instrumentation
- Custom CloudWatch metrics for business events
- Latency profiling: P50/P95/P99 targets, tail latency causes

## Code Review Output Format
Structured headers: **Critical**, **Major**, **Minor**, **Commendations**
Include code snippets for fixes. Quantify impact where possible
(e.g., "reduces cold starts by ~40%", "saves ~$X/month").
Cost efficiency score (1–10) with justification.
