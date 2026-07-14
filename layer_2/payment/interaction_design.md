# Payment Interaction Design

## Document Information

| Property | Value |
|----------|-------|
| Layer | Layer 2 – Guided Journey |
| Module | Payment |
| Document Type | Interaction Design |
| Status | Production |
| Owner | Journey Team |

---

# Purpose

This document describes the interaction patterns between users, Layer 2 (Journey Orchestrator), Layer 4 (Payment Service), external payment gateways, and supporting services during the Payment stage.

The objective is to ensure consistent workflow behavior, asynchronous communication, and resilient payment orchestration.

---

# Interaction Principles

- Layer 2 orchestrates interactions.
- Layer 4 executes payments.
- External gateways process financial transactions.
- Communication is asynchronous wherever possible.
- Journey state is persisted before external interactions.
- Payment completion is event-driven.

---

# High-Level Interaction

```mermaid
flowchart LR

    U[User]

    J["Layer 2<br/>Journey Orchestrator"]

    P["Layer 4<br/>Payment Service"]

    G["Payment Gateway"]

    K["Kafka / Event Bus"]

    U --> J
    J --> P
    P --> G

    G --> P
    P --> K
    K --> J
    J --> U
```

---

# User Interaction Flow

```mermaid
sequenceDiagram

    participant User
    participant Journey
    participant Payment

    User->>Journey: Click "Proceed to Payment"
    Journey->>Payment: Create Payment Session
    Payment-->>Journey: Payment URL
    Journey-->>User: Redirect to Payment
```

---

# Successful Payment Flow

```mermaid
sequenceDiagram

    participant User
    participant Journey
    participant Payment
    participant Gateway
    participant Kafka

    User->>Journey: Confirm Payment

    Journey->>Payment: InitiatePayment

    Payment->>Gateway: Process Payment

    Gateway-->>Payment: Success

    Payment->>Kafka: PaymentSucceeded

    Kafka-->>Journey: PaymentSucceeded

    Journey-->>User: Continue Journey
```

---

# Failed Payment Flow

```mermaid
sequenceDiagram

    participant Journey
    participant Payment
    participant Gateway
    participant Kafka

    Journey->>Payment: InitiatePayment

    Payment->>Gateway: Process Payment

    Gateway-->>Payment: Failure

    Payment->>Kafka: PaymentFailed

    Kafka-->>Journey: PaymentFailed

    Journey->>Journey: Pause Workflow
```

---

# Retry Interaction

```mermaid
sequenceDiagram

    participant Journey
    participant Scheduler
    participant Payment

    Journey->>Scheduler: Schedule Retry

    Scheduler->>Payment: RetryPayment

    Payment-->>Journey: PaymentSucceeded
```

---

# Layer Interaction Matrix

| Source | Target | Interaction |
|---------|--------|-------------|
| User | Journey | Payment request |
| Journey | Payment Service | Initiate payment |
| Payment Service | Gateway | Financial transaction |
| Gateway | Payment Service | Transaction result |
| Payment Service | Kafka | Publish payment event |
| Kafka | Journey | Resume workflow |

---

# Communication Pattern

| Interaction | Type |
|-------------|------|
| Journey → Payment | REST / gRPC |
| Payment → Gateway | HTTPS |
| Payment → Kafka | Event |
| Kafka → Journey | Event |

---

# Interaction States

```mermaid
stateDiagram-v2

    [*] --> WaitingForPayment

    WaitingForPayment --> Processing

    Processing --> Pending

    Pending --> Success
    Pending --> Failure
    Pending --> Cancelled

    Success --> Execution

    Failure --> Retry

    Retry --> Processing

    Cancelled --> JourneyPaused

    Execution --> [*]
```

---

# Error Interaction

```mermaid
flowchart TD

    A[Payment Error]

    B{Recoverable?}

    C[Retry Payment]

    D[Pause Journey]

    E[Manual Review]

    A --> B

    B -->|Yes| C

    B -->|No| D

    D --> E
```

---

# UX Considerations

The user should always receive clear feedback during payment processing:

- Payment initiated
- Payment in progress
- Payment successful
- Payment failed
- Retry available
- Journey resumed

---

# Design Principles

- Event-driven interaction
- Loose coupling
- Idempotent communication
- Asynchronous workflow
- Graceful error handling
- Observable interactions
- Secure communication

---

# Related Documents

- API_CONTRACT.md
- API_USAGE.md
- EVENTS.md
- STATE_MACHINE.md
- FAILURE_STRATEGY.md
- IMPLEMENTATION_MANIFEST.md
