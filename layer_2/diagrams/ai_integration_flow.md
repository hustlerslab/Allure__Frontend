# AI Integration Flow

## Purpose

This document describes how the Guided Journey (Layer 2) interacts with the AI Operating System (Layer 3).

Layer 2 never performs AI inference directly.

Instead, it sends contextual requests to Layer 3 and continues workflow execution using the returned recommendations.

---

# Design Principles

- AI is optional.
- Journey remains deterministic.
- AI provides recommendations only.
- Business rules always take precedence.
- Journey can continue if AI is unavailable.
- AI services remain independently deployable.

---

# High-Level Architecture

```mermaid
flowchart LR

    USER["User"]

    L1["Layer 1<br/>Experience"]

    L2["Layer 2<br/>Journey Orchestrator"]

    L3["Layer 3<br/>AI Operating System"]

    L4["Layer 4<br/>Core Services"]

    L5["Layer 5<br/>Trust Ecosystem"]

    USER --> L1

    L1 --> L2

    L2 --> L3

    L2 --> L5

    L2 --> L4
```

---

# AI Integration Flow

```mermaid
sequenceDiagram

    participant Journey
    participant AI
    participant Core

    Journey->>AI: Request Recommendation

    AI-->>Journey: AI Decision

    Journey->>Core: Execute Business Action

    Core-->>Journey: Result
```

---

# Payment Example

```mermaid
flowchart TD

    A[Payment Started]

    B[Journey Orchestrator]

    C[AI Recommendation]

    D{Recommendation Available?}

    E[Apply Recommendation]

    F[Use Default Workflow]

    G[Continue Payment]

    A --> B

    B --> C

    C --> D

    D -->|Yes| E
    D -->|No| F

    E --> G
    F --> G
```

---

# AI Capabilities

Layer 3 may provide

- Designer recommendation
- Product recommendation
- Dynamic pricing insights
- Budget estimation
- Timeline prediction
- Risk assessment
- Journey personalization

---

# Failure Handling

If AI is unavailable

↓

Log warning

↓

Continue Journey

↓

Use default business rules

AI failure must never block customer workflows.

---

# Communication Pattern

Journey → AI

- REST
- gRPC

AI → Journey

- Recommendation Response

No direct database sharing.

---

# Security

- JWT Authentication
- mTLS
- Correlation IDs
- Trace IDs

---

# Observability

Monitor

- AI latency
- Recommendation accuracy
- AI availability
- Timeout rate
- Error rate

---

# Related Documents

- layer_3/overview.md
- layer_2/payment/overview.md
- layer_2/payment/journey_rules.md
