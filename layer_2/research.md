# Layer 2 Guided Journey Research
## Allure Interiors – Production Architecture Overview

---

# 1. Introduction

The **Guided Journey Layer (Layer 2)** is the orchestration backbone of the Allure Interiors platform. It manages the complete customer journey from discovering design inspirations to project handover by coordinating multiple distributed services.

This layer **does not implement business logic**. Instead, it orchestrates AI, Platform, Trust, Payment, and Execution services through APIs, workflow engines, and asynchronous events.

According to the Layer 2 architecture, the journey consists of a structured workflow:
```mermaid
flowchart TB

    Customer

    UI[Experience Layer]

    Journey[Journey Orchestrator]

    Workflow[Workflow Engine]

    State[State Manager]

    EventBus[Kafka Event Bus]

    Customer --> UI

    UI --> Journey

    Journey --> Workflow

    Workflow --> State

    Workflow --> EventBus

    EventBus --> AI

    EventBus --> Platform

    EventBus --> Trust

    EventBus --> Notification

    AI[Layer 3 AI Operating System]

    Platform[Layer 4 Core Platform]

    Trust[Layer 5 Trust Ecosystem]

    Notification[Notification Service]
```

---


Supporting every stage are:

- Request Center
- Approval Center
- Notification Center
- Timeline Center
- Document Center
- Help Center

---

# 2. Architectural Position
```mermaid
flowchart TB

    USER([Customer])

    L1["🖥️ Layer 1<br/>Experience Layer"]

    L2["🎯 Layer 2<br/>Guided Journey Layer<br/><br/>• Journey Controller<br/>• Workflow Engine<br/>• State Manager<br/>• Event Bus"]

    L3["🤖 Layer 3<br/>AI Operating System"]

    L4["⚙️ Layer 4<br/>Core Platform"]

    L5["🛡️ Layer 5<br/>Trust Ecosystem"]

    L6["☁️ Layer 6<br/>Infrastructure"]

    USER --> L1
    L1 --> L2

    L2 --> L3
    L2 --> L4
    L2 --> L5

    L3 --> L6
    L4 --> L6
    L5 --> L6

    L2 -. Coordinates .-> L3
    L2 -. Orchestrates .-> L4
    L2 -. Verifies .-> L5
```

---

Layer 2 acts as the workflow controller between the frontend and downstream services.

---

# 3. Core Responsibilities

Layer 2 is responsible for:

- Journey orchestration
- Workflow execution
- State management
- API orchestration
- Event publishing
- Event subscription
- Session persistence
- Progress tracking
- Retry management
- Compensation triggering
- Workflow recovery
- Journey analytics
- Long-running workflow management

---

# 4. Out of Scope

Layer 2 never performs business logic.

The following remain in their dedicated layers:

| Capability | Responsible Layer |
|------------|-------------------|
| AI Analysis | Layer 3 |
| Recommendation | Layer 3 |
| Spatial Rendering | Layer 3 |
| Verification | Layer 5 |
| Trust Score | Layer 5 |
| Reputation | Layer 5 |
| Payments | Layer 4 |
| Authentication | Layer 6 |
| Notifications | Platform Services |
| Project Execution | Execution Services |

---

# 5. Design Principles

The architecture follows cloud-native distributed system principles.

## Functional Goals

- Complete journey orchestration
- Resumable workflows
- Workflow consistency
- Long-running processes
- Automatic recovery

## Non-Functional Goals

### Scalability

- Stateless APIs
- Horizontal scaling
- Queue-based execution
- Distributed workers

### Reliability

- Durable workflows
- Persistent checkpoints
- Retry mechanism
- Compensation workflow

### Maintainability

- Modular services
- Independent deployment
- API-first architecture
- Clear service boundaries

### Cost Optimization

- Event-driven execution
- Async processing
- Lazy invocation
- Auto scaling

---

# 6. Architecture Principles

The Guided Journey Layer follows:

- Single Responsibility Principle
- Loose Coupling
- Event Driven Architecture
- Stateless Processing
- Durable Workflow Execution
- Eventual Consistency
- API First Design
- Workflow Orchestration
- Hybrid Orchestration + Choreography

---

# 7. Workflow Lifecycle

Every journey follows the same lifecycle.
```mermaid
flowchart TD

    START([Workflow Started])

    S1[Schedule Activity]

    S2[Execute Activity]

    DECISION{Execution Status}

    SUCCESS[Persist Result]

    CHECKPOINT[Save Workflow Checkpoint]

    NEXT[Schedule Next Activity]

    END([Workflow Completed])

    RETRY[Retry Activity]

    RETRY_CHECK{Retry Limit Reached?}

    COMPENSATE[Execute Compensation]

    RECOVER[Recover Workflow State]

    START --> S1
    S1 --> S2
    S2 --> DECISION

    DECISION -- Success --> SUCCESS
    SUCCESS --> CHECKPOINT
    CHECKPOINT --> NEXT
    NEXT --> END

    DECISION -- Failure --> RETRY
    RETRY --> RETRY_CHECK

    RETRY_CHECK -- No --> S2
    RETRY_CHECK -- Yes --> COMPENSATE
    COMPENSATE --> RECOVER
    RECOVER --> NEXT
```


---

# 8. Journey State Machine


```mermaid
flowchart TD

    A([Draft])

    B[Discover Ideas]

    C[Template Selected]

    D[Template Customized]

    E[Spatial Preview]

    F[AI Analysis]

    G[Designer Matching]

    H[Quotation Hub]

    I[Proposal Comparison]

    J[Revision]

    K[Final Walkthrough]

    L{Approval?}

    M[Payment]

    N[Execution]

    O[Handover]

    P([Completed])

    Q([Cancelled])

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L

    L -- Approved --> M
    L -- Revision Required --> J

    M --> N
    N --> O
    O --> P

    A -. Cancel .-> Q
    B -. Cancel .-> Q
    C -. Cancel .-> Q
    D -. Cancel .-> Q
    E -. Cancel .-> Q
    F -. Cancel .-> Q
    G -. Cancel .-> Q
    H -. Cancel .-> Q
    I -. Cancel .-> Q
    J -. Cancel .-> Q
    M -. Payment Failed .-> Q
```

Cancellation is possible from multiple states.

---

# 9. Workflow Checkpoints

After every successful stage, Layer 2 stores:

- Journey ID
- Current State
- Previous State
- Correlation ID
- Execution ID
- Retry Count
- Timestamp
- Metadata
- Last Event

This enables:

- Resume after crash
- Retry
- Rollback
- Audit
- Recovery

---

# 10. Journey Stages Summary

## Stage 1 — Discover Ideas

Purpose

- Collect inspirations
- Load recommendations
- Create design context

Output

- Inspiration selected

---

## Stage 2 — Template Selection

Purpose

- Retrieve templates
- Validate compatibility
- Save selected template

Output

- Template metadata

---

## Stage 3 — Template Customization

Purpose

- Personalize design
- Validate workflow
- Save customization

Output

- Customized template

---

## Stage 4 — Spatial Preview

Purpose

- Generate 3D visualization
- Coordinate rendering service

Output

- Preview metadata

---

## Stage 5 — AI Analysis

Purpose

- Request AI evaluation

AI evaluates:

- Budget
- Space utilization
- Style
- Material compatibility
- Optimization

Output

- AI report

---

## Stage 6 — Designer Matching

Purpose

Match designers using:

- Skills
- Availability
- Budget
- Portfolio
- Trust Score

Output

- Ranked designer list

---

## Stage 7 — Quotation Hub

Purpose

- Send quotation requests
- Collect quotations
- Persist quotation metadata

Output

- Multiple quotations

---

## Stage 8 — Proposal Comparison

Purpose

Compare:

- Budget
- Timeline
- Trust
- Recommendation
- Proposal quality

Output

- Selected proposal

---

## Stage 9 — Revision

Purpose

Manage customer revisions until satisfaction.

Output

- Updated proposal

---

## Stage 10 — Final Walkthrough

Purpose

Present final design.

Customer reviews:

- Design
- Materials
- Timeline
- Assets

Output

- Walkthrough completion

---

## Stage 11 — Approval

Purpose

Capture customer approval.

Output

- Approved proposal

---

## Stage 12 — Payment

Purpose

Coordinate payment engine.

Output

- Payment confirmation

---

## Stage 13 — Execution

Purpose

Transfer approved project to execution.

Output

- Active project

---

## Stage 14 — Handover

Purpose

Deliver:

- Documents
- Assets
- Completion confirmation

Output

- Closed project

---

## Stage 15 — Completed

Purpose

Archive workflow

Publish completion events.

---

# 11. Communication Model

Layer 2 uses Hybrid Communication.

## Synchronous

Used for

- Validation
- Journey retrieval
- Template loading
- Status APIs

REST APIs
Client
↓
Journey API
↓
Service

---

## Asynchronous

Used for

- AI Analysis
- Rendering
- Designer Matching
- Notifications
- Quotations
- Payments
```mermaid
sequenceDiagram

    participant Journey
    participant Kafka
    participant Service

    Journey->>Kafka: Publish Event
    Kafka->>Service: Deliver Event

    Service->>Service: Process Business Logic

    Service-->>Kafka: Publish Completion Event
    Kafka-->>Journey: Notify Completion

    Journey->>Journey: Continue Workflow
```

---

# 12. Workflow Patterns

## Orchestration

Workflow engine controls:

- Order
- Timeout
- Retry
- Compensation

---

## Choreography

Independent services communicate through events.

Used for

- Analytics
- Notifications
- Monitoring

---

## Saga Pattern

```mermaid
flowchart LR

    subgraph Saga["Saga Transaction"]

        A[AI Analysis]

        B[Designer Matching]

        C[Quotation Generation]

        D[Payment Processing]

        E[Project Execution]

        A --> B
        B --> C
        C --> D
        D --> E

    end

    subgraph Compensation["Compensation Path"]

        R1[Release Designer]

        R2[Invalidate Quotation]

        R3[Publish Failure Event]

    end

    D -. Payment Failed .-> R1
    R1 --> R2
    R2 --> R3
    R3 --> C
```

---

# 13. Event Driven Architecture

Core events include:

- JourneyStarted
- IdeaSelected
- TemplateSelected
- TemplateCustomized
- PreviewGenerated
- AIAnalysisCompleted
- DesignerMatched
- QuotationGenerated
- ProposalSelected
- RevisionCompleted
- WalkthroughCompleted
- ProposalApproved
- PaymentCompleted
- ExecutionStarted
- ProjectCompleted

Benefits:

- Loose coupling
- Independent scaling
- Fault tolerance
- Async execution

---

# 14. Data Management

Journey persistence stores:

- Workflow state
- Retry count
- Pending tasks
- Event history
- Metadata
- Correlation IDs

Database includes:

- Journey Table
- Journey Step Table
- Journey Event Table
- Workflow History

---

# 15. Production Patterns

Implemented architectural patterns:

## CQRS

Separate

- Command Model
- Read Model

Benefits

- Faster dashboards
- Independent scaling

---

## CDC

Database changes become events.
```mermaid
sequenceDiagram

    participant DB as Database
    participant CDC as CDC Connector
    participant Kafka
    participant Service

    DB->>CDC: Database Change
    CDC->>Kafka: Publish Event
    Kafka->>Service: Deliver Event
    Service->>Service: Process Event
```
---

## Outbox Pattern

Avoids dual-write problems.

```mermaid
sequenceDiagram

    participant App
    participant DB
    participant Outbox
    participant CDC
    participant Kafka

    App->>DB: Save Business Data
    App->>Outbox: Save Domain Event
    CDC->>Outbox: Read New Events
    CDC->>Kafka: Publish Event
```

---

## Event Store

Stores immutable workflow events for:

- Replay
- Analytics
- Auditing
- Recovery

---

# 16. API Layer

Production APIs include:
POST /journeys

GET /journeys/{id}

POST /resume

POST /cancel

POST /retry

Design principles:

- REST
- Stateless
- JWT
- Correlation IDs
- Idempotency
- Versioning
- Rate limiting

---

# 17. Security

Layer 2 follows Zero Trust.

Authentication

- OAuth2
- JWT
- OIDC

Authorization

- RBAC
- ABAC

Internal Security

- mTLS
- SPIFFE
- API Gateway

Workflow protection

- Tenant isolation
- Ownership validation
- Permission checks

---

# 18. Observability

Production monitoring includes:

Metrics

- Active journeys
- Completion rate
- Workflow duration
- Retry count
- Queue size

Logging

- Journey ID
- Correlation ID
- Event Type
- User ID
- Status

Tracing

OpenTelemetry

Health Checks

- APIs
- Workflow Engine
- Kafka
- Database

---

# 19. Performance

Optimization strategies:

- Async workers
- Connection pooling
- Redis caching
- Lazy loading
- Batch processing
- Pagination
- CDN
- Background execution

---

# 20. Scalability

Supports:

- Horizontal scaling
- Stateless services
- Distributed workers
- Queue processing
- Load balancing
- Auto scaling
- High availability

---

# 21. Reliability

Production resilience includes:

- Durable workflows
- Automatic retries
- Dead Letter Queue
- Event replay
- Circuit breakers
- Timeout handling
- Compensation workflows
- Persistent checkpoints

---

# 22. Cost Optimization

Architecture minimizes cost through:

- Event-driven execution
- Worker auto scaling
- Archive completed journeys
- Async processing
- Storage lifecycle management
- Independent worker scaling

---

# 23. Layer Integration

Layer 2 integrates with:

## Layer 1

Experience Layer

- Public Website
- Homeowner Portal
- Designer Portal
- Admin Portal

## Layer 3

AI Operating System

- AI Analysis
- Spatial Intelligence
- Recommendation Engine

## Layer 4

Core Platform

- Design Library
- Quotation Hub
- Booking
- Payment
- Communication

## Layer 5

Trust Ecosystem

- Verification
- Reputation
- Trust Score
- Safety

## Layer 6

Infrastructure

- PostgreSQL
- Redis
- Kafka
- Workers
- S3
- CDN
- API Gateway

## Layer 7

Observability

- Logging
- Monitoring
- Tracing
- Analytics

---

# 24. Administrator Notes

Layer 2 should remain a **pure orchestration layer**.

It must never implement:

- AI logic
- Trust calculation
- Payment logic
- Business rules
- Rendering
- Recommendation algorithms

Its responsibility is to:

- Coordinate services
- Persist workflow state
- Publish events
- Manage retries
- Execute workflow transitions
- Recover from failures
- Maintain end-to-end customer journey consistency

This separation ensures scalability, maintainability, fault tolerance, and production-grade workflow orchestration across the Allure Interiors platform.

