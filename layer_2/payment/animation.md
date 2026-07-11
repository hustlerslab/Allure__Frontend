# Payment Journey Animation

## Purpose

This document visualizes the complete lifecycle of the **Payment Stage** in Layer 2 (Guided Journey Layer).

The diagrams illustrate:

- Journey orchestration
- Layer interactions
- Event flow
- Success path
- Failure path
- Retry flow
- Compensation flow

---

# Animation 1 — Payment Request

```mermaid
sequenceDiagram

    participant U as User
    participant L2 as Journey Orchestrator
    participant L4 as Payment Service

    U->>L2: Click "Proceed to Payment"
    L2->>L4: Initiate Payment
    L4-->>L2: Payment Session Created
```

---

# Animation 2 — Successful Payment

```mermaid
sequenceDiagram

    participant U as User
    participant L2 as Journey
    participant L4 as Payment
    participant PG as Payment Gateway

    U->>L2: Pay
    L2->>L4: Initiate Payment
    L4->>PG: Charge Payment
    PG-->>L4: Success
    L4-->>L2: PaymentSucceeded Event
    L2->>L2: Resume Workflow
    L2->>U: Payment Successful
```

---

# Animation 3 — Payment Failure

```mermaid
sequenceDiagram

    participant L2 as Journey
    participant L4 as Payment
    participant PG as Gateway

    L2->>L4: Initiate Payment

    L4->>PG: Charge

    PG-->>L4: Failure

    L4-->>L2: PaymentFailed

    L2->>L2: Pause Journey
```

---

# Animation 4 — Retry

```mermaid
sequenceDiagram

    participant L2
    participant Scheduler
    participant Payment

    L2->>Scheduler: Schedule Retry

    Scheduler-->>Payment: Retry Payment

    Payment-->>L2: PaymentSucceeded
```

---

# Animation 5 — Timeout

```mermaid
sequenceDiagram

    participant Journey
    participant Payment
    participant Gateway

    Journey->>Payment: Start Payment

    Payment->>Gateway: Charge

    Note over Gateway: No Response

    Gateway-->>Payment: Timeout

    Payment-->>Journey: PaymentExpired
```

---

# Animation 6 — Compensation

```mermaid
sequenceDiagram

    participant Journey
    participant Payment
    participant Workflow

    Journey->>Payment: Payment Request

    Payment-->>Journey: Failure

    Journey->>Workflow: Trigger Compensation

    Workflow-->>Journey: Journey Paused
```

---

# Animation 7 — Layer Interaction

```mermaid
sequenceDiagram

    participant L1 as Experience Layer
    participant L2 as Guided Journey
    participant L3 as AI OS
    participant L4 as Payment
    participant L5 as Trust
    participant L6 as Infrastructure

    L1->>L2: Payment Requested

    L2->>L3: Validate Recommendation

    L3-->>L2: OK

    L2->>L5: Verify Trust

    L5-->>L2: Verified

    L2->>L4: Initiate Payment

    L4->>L6: Gateway Request

    L6-->>L4: Success

    L4-->>L2: PaymentSucceeded

    L2-->>L1: Continue Journey
```

---

# Animation 8 — Complete Lifecycle

```mermaid
stateDiagram-v2

    [*] --> ApprovalCompleted

    ApprovalCompleted --> PaymentRequested

    PaymentRequested --> PaymentProcessing

    PaymentProcessing --> PaymentPending

    PaymentPending --> PaymentSucceeded

    PaymentPending --> PaymentFailed

    PaymentPending --> PaymentExpired

    PaymentFailed --> PaymentRetrying

    PaymentRetrying --> PaymentProcessing

    PaymentExpired --> PaymentRetrying

    PaymentSucceeded --> ExecutionStarted

    ExecutionStarted --> [*]
```

---

# Animation Summary

| Animation | Purpose |
|------------|---------|
| Payment Request | Journey starts payment |
| Success Flow | Normal payment lifecycle |
| Failure Flow | Payment failure handling |
| Retry Flow | Automatic retry |
| Timeout Flow | Gateway timeout |
| Compensation | Saga rollback |
| Layer Interaction | Cross-layer communication |
| Lifecycle | Complete payment state machine |
