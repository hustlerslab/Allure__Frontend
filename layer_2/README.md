# Layer 2 – Guided Journey

## Overview

Layer 2 is responsible for orchestrating the complete customer journey across the platform.

It coordinates every major business workflow from design discovery to project completion while interacting with AI services, payment systems, trust verification, and execution services.

Layer 2 does not contain heavy business logic of downstream services. Instead, it coordinates workflows, maintains state, enforces business rules, and reacts to events.

---

# Responsibilities

- Customer journey orchestration
- Workflow management
- Business rule enforcement
- AI integration
- Payment orchestration
- Trust verification
- Event handling
- State management
- Recovery and retry
- Observability

---

# High-Level Architecture

```mermaid
flowchart LR

    User[Customer]

    Layer1[Layer 1 Experience]

    Layer2[Layer 2 Guided Journey]

    Layer3[Layer 3 AI]

    Layer4[Layer 4 Core Services]

    Layer5[Layer 5 Trust]

    User --> Layer1
    Layer1 --> Layer2

    Layer2 --> Layer3
    Layer2 --> Layer4
    Layer2 --> Layer5
```

---

# Guided Journey Flow

```mermaid
flowchart LR

    Discovery --> Template

    Template --> Customization

    Customization --> AI

    AI --> Designer

    Designer --> Quotation

    Quotation --> Approval

    Approval --> Payment

    Payment --> Execution

    Execution --> Handover

    Handover --> Completed
```

---

# State Lifecycle

```mermaid
stateDiagram-v2

    [*] --> Discovery

    Discovery --> TemplateSelection

    TemplateSelection --> Customization

    Customization --> DesignerMatching

    DesignerMatching --> Quotation

    Quotation --> Approval

    Approval --> Payment

    Payment --> Execution

    Execution --> Handover

    Handover --> Completed

    Completed --> [*]
```

---

# Component Hierarchy

```mermaid
flowchart TD

    Layer2[Layer 2]

    Layer2 --> Discovery

    Layer2 --> Template

    Layer2 --> AI

    Layer2 --> Designer

    Layer2 --> Quotation

    Layer2 --> Approval

    Layer2 --> Payment

    Layer2 --> Execution

    Layer2 --> Handover
```

---

# Directory Structure

```text
layer_2/
│
├── README.md
├── ADR.md
├── AI_CONTEXT.md
├── AI_EXECUTION_RULES.md
│
├── diagrams/
│   ├── component_hierarchy.md
│   ├── dependency_graph.md
│   ├── error_flow.md
│   ├── journey_flow.md
│   ├── routing_tree.md
│   ├── state_diagram.md
│   └── rendering_architecture.md
│
└── payment/
    ├── overview.md
    ├── requirements.md
    ├── api_contract.md
    ├── api_usage.md
    ├── events.md
    ├── state_machine.md
    ├── testing.md
    ├── security.md
    ├── performance.md
    └── ...
```

---

# Design Principles

- Event-driven architecture
- Deterministic workflows
- Saga orchestration
- State machine execution
- AI-assisted decisions
- Business rule enforcement
- Idempotent operations
- Fault tolerance
- Horizontal scalability

---

# External Dependencies

Layer 2 communicates with:

- Layer 1 Experience
- Layer 3 AI Operating System
- Layer 4 Core Services
- Layer 5 Trust Ecosystem
- Kafka Event Bus
- Redis Cache
- Journey Database

---

# Related Documents

## Core

- ADR.md
- AI_CONTEXT.md
- AI_EXECUTION_RULES.md

## Diagrams

- diagrams/component_hierarchy.md
- diagrams/dependency_graph.md
- diagrams/error_flow.md
- diagrams/journey_flow.md
- diagrams/state_diagram.md

## Payment Module

- payment/overview.md
- payment/state_machine.md
- payment/events.md
- payment/security.md
- payment/testing.md
