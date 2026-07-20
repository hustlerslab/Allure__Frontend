# Dependency Graph

## Overview

This document illustrates the dependency relationships between the major components of Layer 2.

Layer 2 follows a layered dependency model where the Journey Orchestrator coordinates business modules and integrates with external platform services.

---

# Internal Dependency Graph

```mermaid
flowchart TD

    Journey[Journey Orchestrator]

    Discovery[Discovery]
    Template[Template Selection]
    Customization[Customization]
    AI[AI Integration]
    Designer[Designer Matching]
    Quotation[Quotation]
    Approval[Approval]
    Payment[Payment]
    Execution[Execution]
    Handover[Project Handover]

    Journey --> Discovery
    Journey --> Template
    Journey --> Customization
    Journey --> AI
    Journey --> Designer
    Journey --> Quotation
    Journey --> Approval
    Journey --> Payment
    Journey --> Execution
    Journey --> Handover
```

---

# External Dependencies

```mermaid
flowchart LR

    Journey[Layer 2 Journey]

    Layer1[Layer 1 Experience]
    Layer3[Layer 3 AI]
    Layer4[Layer 4 Core Services]
    Layer5[Layer 5 Trust]

    Kafka[Kafka]
    Redis[Redis]
    Database[Journey Database]

    Layer1 --> Journey

    Journey --> Layer3
    Journey --> Layer4
    Journey --> Layer5

    Journey --> Kafka
    Journey --> Redis
    Journey --> Database
```

---

# Payment Module Dependencies

```mermaid
flowchart LR

    Payment[Payment Module]

    API[API Layer]
    Rules[Business Rules]
    Events[Event Handler]
    State[State Machine]
    Security[Security]
    Cache[Cache]
    Monitor[Observability]

    Payment --> API
    Payment --> Rules
    Payment --> Events
    Payment --> State
    Payment --> Security
    Payment --> Cache
    Payment --> Monitor
```

---

# Event Dependencies

```mermaid
flowchart LR

    Journey[Journey]

    Kafka[Kafka]

    Payment[Payment]

    AI[AI]

    Trust[Trust]

    Journey --> Kafka

    Kafka --> Payment
    Kafka --> AI
    Kafka --> Trust

    Payment --> Kafka
    AI --> Kafka
    Trust --> Kafka
```

---

# Dependency Rules

- Components communicate through defined interfaces.
- External services are accessed only through integration modules.
- Business modules remain loosely coupled.
- Events are preferred over direct service calls for asynchronous workflows.
- Circular dependencies are not permitted.

---

# Related Documents

- architecture.md
- README.md
- ADR.md
- diagrams/component_hierarchy.md
- diagrams/state_diagram.md
- payment/overview.md
