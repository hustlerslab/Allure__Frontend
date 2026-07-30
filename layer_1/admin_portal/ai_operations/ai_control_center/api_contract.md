# 📄 API Contract
> **Research and compile instructions before starting development.**

### 📋 What to put inside this document (1-4 line guideline):
* Document mock payloads, response schemas, parameter validation rules, and error codes. Specify endpoints, query parameters, and header formats.
# API Contract

**Layer:** Layer 1 – Experience Layer

**Module:** Admin Portal

**Submodule:** AI Control Center

**Document Version:** 1.0

**Status:** Active

---

# Purpose

This document defines the API contract between the Admin Portal and backend services for the AI Control Center. It specifies API endpoints, request and response formats, authentication requirements, parameter validation rules, headers, and standard error responses to ensure consistent communication across the platform.

---

# API Overview

The AI Control Center communicates with backend microservices through a centralized API Gateway using RESTful APIs.

The APIs support:

- Administrator Authentication
- AI Model Management
- Prompt Management
- AI Configuration
- Monitoring & Health Checks
- Analytics & Reporting
- Knowledge Base Management
- Cost Monitoring
- Incident Management
- Audit Logging

---

# Base URL

```text
https://api.allureinteriors.com/api/v1
```

---

# Authentication

All protected endpoints require JWT authentication.

### Authorization Header

```http
Authorization: Bearer <JWT_ACCESS_TOKEN>
```

Authentication Methods

- JWT Bearer Token
- OAuth 2.0 (Optional)
- Multi-Factor Authentication (MFA) for privileged operations

---

# Common Request Headers

| Header | Required | Description |
|---------|----------|-------------|
| Authorization | Yes | JWT Bearer Token |
| Content-Type | Yes | application/json |
| Accept | Yes | application/json |
| X-Request-ID | Optional | Request tracing identifier |
| X-Correlation-ID | Optional | Distributed request tracing |

---

# API Endpoints

## Authentication

| Method | Endpoint | Description |
|----------|----------|-------------|
| POST | `/auth/login` | Administrator login |
| POST | `/auth/logout` | Logout administrator |
| POST | `/auth/refresh-token` | Refresh access token |
| GET | `/auth/profile` | Retrieve administrator profile |

---

## AI Model Management

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/models` | Retrieve AI models |
| GET | `/models/{modelId}` | Retrieve model details |
| POST | `/models` | Create AI model |
| PUT | `/models/{modelId}` | Update AI model |
| DELETE | `/models/{modelId}` | Delete AI model |

---

## Prompt Management

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/prompts` | Retrieve prompts |
| GET | `/prompts/{promptId}` | Retrieve prompt details |
| POST | `/prompts` | Create prompt |
| PUT | `/prompts/{promptId}` | Update prompt |
| DELETE | `/prompts/{promptId}` | Delete prompt |

---

## AI Configuration

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/configurations` | Retrieve AI configuration |
| PUT | `/configurations/{configId}` | Update AI configuration |

---

## Monitoring

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/monitoring/health` | Overall system health |
| GET | `/monitoring/services` | Service status |
| GET | `/monitoring/metrics` | Performance metrics |

---

## Analytics

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/analytics/usage` | AI usage analytics |
| GET | `/analytics/performance` | Performance analytics |
| GET | `/analytics/cost` | AI cost analytics |

---

## Audit Logs

| Method | Endpoint | Description |
|----------|----------|-------------|
| GET | `/audit-logs` | Retrieve audit logs |
| GET | `/audit-logs/{logId}` | Retrieve audit log details |

---

# Request Payloads

## Administrator Login

### Request

```json
{
  "email": "admin@allureinteriors.com",
  "password": "SecurePassword123"
}
```

---

## Create AI Model

### Request

```json
{
  "modelName": "GPT-4",
  "provider": "OpenAI",
  "version": "4.0",
  "status": "Active"
}
```

---

## Create Prompt

### Request

```json
{
  "name": "Interior Design Assistant",
  "category": "Room Design",
  "content": "Generate premium interior design suggestions based on user preferences.",
  "status": "Draft"
}
```

---

# Response Schemas

## Success Response

```json
{
  "success": true,
  "message": "Request processed successfully.",
  "data": {}
}
```

---

## AI Model Response

```json
{
  "success": true,
  "data": {
    "modelId": "mdl_001",
    "modelName": "GPT-4",
    "provider": "OpenAI",
    "version": "4.0",
    "status": "Active"
  }
}
```

---

## Prompt Response

```json
{
  "success": true,
  "data": {
    "promptId": "prm_001",
    "name": "Interior Design Assistant",
    "status": "Published"
  }
}
```

---

# Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | Integer | No | Page number |
| limit | Integer | No | Number of records per page |
| search | String | No | Search keyword |
| status | String | No | Filter by status |
| sort | String | No | Sort field |
| order | String | No | asc / desc |

---

# Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| modelId | UUID | AI Model Identifier |
| promptId | UUID | Prompt Identifier |
| configId | UUID | Configuration Identifier |
| logId | UUID | Audit Log Identifier |

---

# Parameter Validation Rules

| Field | Validation Rule |
|--------|-----------------|
| Email | Required, valid email format |
| Password | Required, minimum 8 characters |
| Model Name | Required, maximum 100 characters |
| Provider | Required |
| Version | Required |
| Prompt Name | Required, maximum 100 characters |
| Prompt Content | Required |
| Status | Must be Active, Inactive, Draft, or Published |
| Page | Integer greater than 0 |
| Limit | Integer between 1 and 100 |

---

# Standard Response Codes

| HTTP Code | Meaning |
|------------|---------|
| 200 | Request Successful |
| 201 | Resource Created |
| 204 | Resource Deleted Successfully |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Resource Not Found |
| 409 | Resource Conflict |
| 422 | Validation Failed |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

---

# Error Response Schema

```json
{
  "success": false,
  "error": {
    "code": 422,
    "message": "Validation failed.",
    "details": [
      {
        "field": "email",
        "issue": "Invalid email format"
      }
    ]
  }
}
```

---

# Pagination

Paginated endpoints return the following metadata.

```json
{
  "page": 1,
  "limit": 20,
  "totalRecords": 250,
  "totalPages": 13
}
```

---

# Rate Limiting

API requests are protected using rate limiting.

| Endpoint Type | Limit |
|---------------|-------|
| Authentication | 10 requests/minute |
| Standard APIs | 100 requests/minute |
| Analytics APIs | 50 requests/minute |

### Response Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 82
X-RateLimit-Reset: 1710000000
```

---

# API Versioning

The current API version is:

```text
/api/v1
```

Future API versions will be introduced without affecting existing integrations.

Example:

```text
/api/v2
```

---

# API Security

The following security controls apply to all APIs.

- HTTPS is mandatory.
- JWT authentication is required for protected endpoints.
- Role-Based Access Control (RBAC) is enforced.
- Input validation is performed on all requests.
- Sensitive operations require Multi-Factor Authentication (MFA).
- Audit logs are generated for administrative actions.
- Request tracing is supported using correlation identifiers.

---

# Related Documents

- overview.md
- architecture.md
- admin_workflow.md
- security.md
- permissions.md
- observability.md
- ADR.md
- ai_context.md

---

# Revision History

| Version | Date | Description |
|----------|------|-------------|
| 1.0 | Initial Release | Initial API Contract specification for the AI Control Center |