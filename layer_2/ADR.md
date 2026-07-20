# Architecture Decision Record (ADR)

## Document Information

| Property | Value |
|----------|-------|
| Layer | Layer 2 – Guided Journey |
| Document | Architecture Decision Record |
| Status | Accepted |
| Version | 1.0 |

---

# Purpose

This document records the key architectural decisions made for Layer 2 (Guided Journey). These decisions explain why specific technologies, patterns, and architectural approaches were selected.

---

# ADR-001: Event-Driven Architecture

## Status

Accepted

### Decision

Layer 2 communicates asynchronously using an event-driven architecture.

### Reason

- Loose coupling
- Scalability
- Fault isolation
- Asynchronous workflows

### Consequences

**Advantages**

- Better scalability
- Independent services
- Easier recovery

**Trade-offs**

- Eventual consistency
- More operational complexity

---

# ADR-002: Journey Orchestrator

## Status

Accepted

### Decision

A centralized Journey Orchestrator controls the complete workflow.

### Reason

- Single workflow owner
- Deterministic execution
- Simplified business logic

### Consequences

**Advantages**

- Easier monitoring
- Consistent workflow
- Centralized state management

---

# ADR-003: Saga Pattern

## Status

Accepted

### Decision

Distributed transactions are coordinated using the Saga Pattern.

### Reason

- Long-running workflows
- No distributed database transactions
- Service autonomy

### Consequences

**Advantages**

- High availability
- Service independence

**Trade-offs**

- Compensation logic required

---

# ADR-004: Outbox Pattern

## Status

Accepted

### Decision

Business events are published using the Transactional Outbox Pattern.

### Reason

- Prevent lost events
- Reliable event publishing
- Database consistency

### Consequences

- Reliable messaging
- Simplified recovery

---

# ADR-005: Idempotent APIs

## Status

Accepted

### Decision

All payment APIs must support idempotency.

### Reason

- Prevent duplicate payments
- Safe client retries

### Consequences

- Duplicate requests return the original response
- Improved reliability

---

# ADR-006: State Machine

## Status

Accepted

### Decision

Journey progression is managed using a finite state machine.

### Reason

- Predictable workflow
- Controlled transitions
- Easier validation

### Consequences

- Invalid transitions are rejected
- Easier debugging

---

# ADR-007: AI Integration

## Status

Accepted

### Decision

AI provides recommendations only.

### Reason

- Keep business workflows deterministic
- Prevent AI from directly controlling execution

### Consequences

- AI failures never block the journey
- Business rules remain authoritative

---

# ADR-008: Trust Verification

## Status

Accepted

### Decision

Trust verification is delegated to Layer 5.

### Reason

- Separation of concerns
- Reusable trust services

### Consequences

- Layer 2 consumes trust results
- Verification logic remains centralized

---

# ADR-009: Observability

## Status

Accepted

### Decision

All workflow activities must produce logs, metrics, and traces.

### Reason

- Faster troubleshooting
- Operational visibility

### Consequences

- Improved monitoring
- Easier incident response

---

# ADR-010: Cache Strategy

## Status

Accepted

### Decision

Redis is used as a cache only.

### Reason

- Reduce database load
- Improve response time

### Consequences

- Cache failures never stop journey execution
- Database remains the source of truth

---

# ADR-011: Security

## Status

Accepted

### Decision

All service-to-service communication requires authentication.

### Reason

- Protect internal APIs
- Prevent unauthorized access

### Consequences

- JWT authentication
- TLS encryption
- Role-based authorization

---

# Decision Summary

| ADR | Decision |
|------|----------|
| ADR-001 | Event-Driven Architecture |
| ADR-002 | Journey Orchestrator |
| ADR-003 | Saga Pattern |
| ADR-004 | Transactional Outbox |
| ADR-005 | Idempotent APIs |
| ADR-006 | State Machine |
| ADR-007 | AI Recommendations Only |
| ADR-008 | Trust Verification Delegation |
| ADR-009 | Full Observability |
| ADR-010 | Cache-Aside Strategy |
| ADR-011 | Secure Service Communication |

---

# Related Documents

- overview.md
- requirements.md
- research.md
- business_rules.md
- journey_rules.md
- events.md
- state_machine.md
- failure_strategy.md
- security.md
- observability.md
