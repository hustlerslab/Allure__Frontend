# Payment Failure Strategy

## Document Information

| Property | Value |
|----------|-------|
| Layer | Layer 2 – Guided Journey |
| Module | Payment |
| Document Type | Failure Strategy |
| Status | Production |

---

# Purpose

This document defines how failures occurring during the Payment stage are detected, classified, recovered, and monitored.

The objective is to ensure that payment failures never corrupt workflow state while maintaining high availability and resilience.

Layer 2 orchestrates failure recovery.

Layer 4 owns payment execution.

---

# Failure Principles

- Never lose workflow state.
- Never process duplicate payments.
- Never leave workflows in an inconsistent state.
- Recover automatically whenever possible.
- Escalate only when recovery fails.
- Use eventual consistency.

---

# Failure Classification

| Category | Examples | Recoverable |
|-----------|----------|-------------|
| Network | Connection timeout | Yes |
| Infrastructure | Kafka unavailable | Yes |
| Gateway | Temporary gateway outage | Yes |
| Validation | Invalid quotation | No |
| Authentication | JWT expired | No |
| Authorization | Access denied | No |
| Fraud | Trust verification failed | No |
| Internal Service | Payment service unavailable | Yes |

---

# Failure Lifecycle

```mermaid
flowchart TD

    A[Failure Detected]

    B{Recoverable?}

    C[Retry]

    D[Compensation]

    E[Manual Review]

    F[Resume Journey]

    G[Pause Journey]

    A --> B

    B -->|Yes| C
    B -->|No| D

    C --> F

    D --> E

    E --> G
```

---

# Retry Strategy

Recoverable failures

- Gateway timeout
- Temporary network interruption
- Kafka unavailable
- Service unavailable

Retry Policy

- Exponential Backoff
- Jitter
- Maximum 5 attempts

---

# Retry Flow

```mermaid
flowchart TD

    A[Payment Failed]

    B{Retry Allowed?}

    C[Schedule Retry]

    D[Retry Payment]

    E[Payment Success]

    F[Pause Journey]

    A --> B

    B -->|Yes| C
    C --> D
    D --> E

    B -->|No| F
```

---

# Timeout Strategy

Timeouts occur when

- Gateway does not respond
- Callback never arrives
- Workflow exceeds configured duration

Action

- Mark payment as expired
- Schedule retry
- Notify Journey

---

# Timeout Flow

```mermaid
flowchart TD

    A[Payment Processing]

    B[Timeout]

    C[Payment Expired]

    D[Retry]

    E[Journey Paused]

    A --> B
    B --> C
    C --> D
    C --> E
```

---

# Circuit Breaker

Protect external payment gateways.

States

```mermaid
stateDiagram-v2

    [*] --> Closed

    Closed --> Open : Failure Threshold Reached

    Open --> Half_Open : Recovery Timeout

    Half_Open --> Closed : Success

    Half_Open --> Open : Failure
```

---

# Saga Compensation

Payment is one Saga step.

Compensation is triggered when payment cannot complete.

Example

```mermaid
flowchart TD

    A[Payment Failed]

    B[Cancel Payment Session]

    C[Pause Journey]

    D[Notify Customer]

    A --> B
    B --> C
    C --> D
```

---

# Dead Letter Queue

Events failing repeatedly are moved to a DLQ.

```mermaid
flowchart TD

    A["Payment Event"]
    B["Kafka"]
    C["Consumer"]
    D{"Processing Successful?"}
    E["Retry"]
    F["Dead Letter Queue<br/>DLQ"]
    G["Continue Processing"]

    A --> B
    B --> C
    C --> D

    D -->|Yes| G
    D -->|No| E

    E --> D
    E -->|Retry Limit Exceeded| F
```
---

# Event Replay

DLQ events may be replayed after issue resolution.

Replay requirements

- Preserve ordering
- Preserve correlation ID
- Preserve event version

---

# Idempotency

Every retry must reuse the original:

- Payment ID
- Idempotency Key
- Correlation ID

Duplicate payment execution is prohibited.

---

# Cache Failure

```mermaid
flowchart TD

    A[Redis Unavailable]
    B[Read Database]
    C[Continue Journey]

    A --> B
    B --> C
```

Cache failure must never block payment processing.

---

# Database Failure

If database unavailable

- Pause workflow
- Retry persistence
- Prevent payment execution until checkpoint is stored

---

# Gateway Failure

```mermaid
flowchart TD

    A[Payment Failure]

    B{Failure Type?}

    C[Temporary Failure]
    D[Permanent Failure]

    E[Retry Payment]

    F[Pause Journey]

    G[Manual Review]

    A --> B

    B -->|Temporary| C
    B -->|Permanent| D

    C --> E

    D --> F
    F --> G
```

---

# Observability

Monitor

- Retry count
- Failure rate
- Timeout rate
- DLQ size
- Gateway latency
- Circuit breaker state

---

# Alerts

Generate alerts when

- Retry limit exceeded
- DLQ growing
- Gateway unavailable
- High payment failure rate
- Circuit breaker open

---

# Failure Matrix

| Failure | Action |
|----------|--------|
| Network Timeout | Retry |
| Gateway Timeout | Retry |
| Kafka Down | Retry Event |
| Redis Down | Read Database |
| Payment Failed | Pause Journey |
| Fraud | Reject Payment |
| Invalid Request | Reject Immediately |
| Duplicate Request | Return Existing Result |

---

# Design Principles

- Fail Fast
- Recover Automatically
- Retry Only Recoverable Errors
- Never Duplicate Payments
- Event-Driven Recovery
- Saga Compensation
- High Observability
- Graceful Degradation

---

# Related Documents

- ADR.md
- IMPLEMENTATION_MANIFEST.md
- API_CONTRACT.md
- EVENTS.md
- STATE_MACHINE.md
- OBSERVABILITY.md
- SECURITY.md
