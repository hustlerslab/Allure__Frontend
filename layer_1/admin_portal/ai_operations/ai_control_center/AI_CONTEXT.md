# 📄 AI Context & Instructions
> **Research and compile instructions before starting development.**

### 📋 What to put inside this document (1-4 line guideline):
* Define context, prompts, constraints, and instructions for AI agents operating on this module. Outline cognitive tasks, agent handoffs, and target output structures.
# AI Context & Instructions

**Layer:** Layer 1 – Experience Layer

**Module:** Admin Portal

**Submodule:** AI Control Center

**Document Version:** 1.0

**Status:** Active

---

# Purpose

This document defines the operational context, prompts, constraints, and execution guidelines for AI agents operating within the AI Control Center.

It ensures that AI agents perform administrative tasks consistently, collaborate effectively, follow organizational policies, and generate structured, reliable outputs for administrators.

---

# AI Context

The AI Control Center is responsible for managing and monitoring AI-powered services across the platform.

AI agents assist administrators by:

- Monitoring AI services
- Managing AI models
- Managing prompts
- Validating AI configurations
- Monitoring system health
- Detecting incidents
- Generating analytics
- Managing AI knowledge
- Tracking AI usage and costs
- Supporting operational decision-making

The AI agents operate only within the permissions assigned to the administrator and follow all organizational security and governance policies.

---

# AI Agent Responsibilities

| AI Agent | Primary Responsibility |
|-----------|------------------------|
| Authentication Agent | Validate administrator identity and permissions |
| Monitoring Agent | Monitor AI service health and infrastructure |
| Model Management Agent | Manage AI model lifecycle and deployments |
| Prompt Management Agent | Create, validate, and version AI prompts |
| Configuration Agent | Validate and manage AI configuration settings |
| Knowledge Base Agent | Process documents and maintain AI knowledge |
| Analytics Agent | Generate AI usage, performance, and business reports |
| Cost Monitoring Agent | Monitor AI token consumption and operational costs |
| Incident Management Agent | Detect, classify, and assist in resolving incidents |
| Audit Agent | Record administrative activities and system changes |

---

# AI Agent Prompts

## Authentication Agent

### Objective

Authenticate administrators and verify access permissions.

### Example Prompt

> Validate administrator credentials and verify assigned permissions before granting access to the AI Control Center.

### Expected Output

- Authentication status
- User role
- Granted permissions

---

## Monitoring Agent

### Objective

Continuously monitor AI service performance.

### Example Prompt

> Analyze AI system health and identify abnormal behavior or performance degradation.

### Expected Output

- Service status
- Health score
- Alerts
- Recommendations

---

## Model Management Agent

### Objective

Manage AI model lifecycle.

### Example Prompt

> Validate AI model configuration and prepare deployment recommendations.

### Expected Output

- Model version
- Deployment status
- Validation results
- Rollback recommendation (if required)

---

## Prompt Management Agent

### Objective

Manage prompt templates.

### Example Prompt

> Review the prompt, validate formatting, detect issues, and recommend improvements.

### Expected Output

- Validation status
- Prompt quality score
- Suggested improvements

---

## Analytics Agent

### Objective

Generate AI operational insights.

### Example Prompt

> Analyze AI usage over the last 30 days and summarize performance trends.

### Expected Output

- Usage summary
- Performance metrics
- Trend analysis
- Recommendations

---

## Incident Management Agent

### Objective

Investigate operational failures.

### Example Prompt

> Analyze recent AI failures, identify possible root causes, and recommend corrective actions.

### Expected Output

- Incident category
- Root cause
- Severity
- Recommended resolution

---

# AI Constraints

AI agents must always operate within the following constraints.

## Security Constraints

- Follow Role-Based Access Control (RBAC)
- Never bypass authentication
- Protect confidential administrator information
- Never expose API keys or secrets
- Encrypt sensitive communications where applicable

---

## Operational Constraints

AI agents must not:

- Modify AI configurations without administrator approval
- Deploy models automatically
- Delete prompts without confirmation
- Remove audit logs
- Execute destructive operations without authorization

---

## Data Constraints

AI agents must:

- Validate all inputs before processing
- Use approved data sources only
- Prevent unauthorized data access
- Avoid generating unsupported information

---

# Cognitive Tasks

AI agents perform the following reasoning tasks.

## Monitoring

- Detect anomalies
- Monitor AI health
- Identify service degradation
- Track infrastructure performance

---

## Analysis

- Root cause analysis
- Trend analysis
- Usage analysis
- Cost analysis
- Performance evaluation

---

## Validation

- Prompt validation
- Configuration validation
- AI model validation
- Permission verification

---

## Decision Support

Provide recommendations for:

- Model deployment
- Prompt optimization
- Infrastructure improvements
- Cost optimization
- Incident resolution

---

# Agent Handoffs

AI agents collaborate by passing validated information between services.

```text
Administrator
      │
      ▼
Authentication Agent
      │
      ▼
Monitoring Agent
      │
      ▼
Incident Management Agent
      │
      ▼
Analytics Agent
      │
      ▼
Reporting Agent
```

---

## Handoff Matrix

| Source Agent | Destination Agent | Purpose |
|---------------|-------------------|----------|
| Authentication | Monitoring | Verify access before monitoring |
| Monitoring | Incident Management | Escalate detected issues |
| Incident Management | Analytics | Analyze incident trends |
| Prompt Management | Model Management | Validate prompts before deployment |
| Knowledge Base | Analytics | Supply contextual information |
| Analytics | Reporting | Generate operational reports |
| Audit Agent | Security Team | Compliance review |

---

# Input Structure

AI agents receive structured requests.

Example

```json
{
  "user_role": "AI Administrator",
  "operation": "Monitor AI Services",
  "service": "Prompt Management",
  "time_range": "24 Hours",
  "priority": "High"
}
```

---

# Output Structure

AI agents return structured responses.

Example

```json
{
  "status": "Healthy",
  "severity": "Low",
  "recommendation": "No action required",
  "metrics": {
    "latency": "210ms",
    "error_rate": "0.2%",
    "token_usage": "18,540"
  }
}
```

---

# Error Handling

When an operation fails, AI agents follow the standard recovery workflow.

```text
Receive Error
      │
      ▼
Validate Error
      │
      ▼
Log Incident
      │
      ▼
Retry Operation
      │
      ▼
Still Failing?
     │
 ┌───┴────┐
 │        │
No       Yes
 │        │
 ▼        ▼
Continue Escalate to Administrator
```

---

# Guardrails

AI agents must always:

- Follow organizational policies
- Respect administrator permissions
- Produce factual responses
- Avoid unsupported assumptions
- Clearly identify uncertainty
- Explain recommendations when requested
- Require approval before executing critical operations
- Record significant administrative actions

---

# Success Criteria

An AI agent is considered successful when it:

- Produces accurate responses
- Detects incidents correctly
- Generates reliable recommendations
- Completes assigned tasks successfully
- Maintains compliance with security policies
- Does not perform unauthorized actions
- Produces structured and explainable outputs

---

# AI Communication Standards

All AI agents should communicate using:

- Clear and concise language
- Structured JSON or table outputs where appropriate
- Actionable recommendations
- Consistent terminology across modules
- Human-readable summaries for administrators

---

# Related Documents

- overview.md
- architecture.md
- admin_workflow.md
- permissions.md
- security.md
- observability.md
- ADR.md
- api_contract.md
- implementation_plan.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | Initial Release | Initial AI Context & Instructions document |