# USP Security - Security

**Language**: [🇧🇷 Português](./security.md) | [🇺🇸 English](#1-introduction)

## 1. Introduction

This document defines the security requirements and mechanisms of the Universal Service Protocol (USP), including authentication, authorization, encryption, and best practices, with special consideration for AI agent interactions.

## 2. Security-by-Design for Autonomous Agents

Unlike traditional APIs where the client is a trusted developer, USP assumes the client is a probabilistic AI Agent (like **Moldbot** or others). Therefore, security controls must be **Semantic** and **Negotiated**.

### 2.1 Capabilities Negotiation ("The Handshake")

In traditional APIs, permissions (Scopes) are static. In USP, permissions are dynamically negotiated based on trust levels.

**Flow:**
1.  **Agent Hello**: "I am a Scheduling Agent. I request `read:calendar` and `write:appointment`."
2.  **Service Challenge**: checks Agent reputation/signature.
3.  **Service Response**: 
    - *Trusted Agent*: "Granted `read:calendar`, `write:appointment`."
    - *Untrusted Agent*: "Granted `read:calendar`. `write:appointment` granted in `PROVISIONAL` mode (requires human approval)."

### 2.2 Native "Draft Mode" (Dry Runs)

Agents often need to "think" or "plan" without causing real-world side effects.

**Specification:**
All state-changing verbs (`POST`, `PUT`, `PATCH`) MUST support a standard headers or flag (e.g., `X-USP-Execution-Mode: Dry-Run`). The server processes business logic, validates inventory/pricing, but **does not commit** the transaction, returning the would-be result (`200 OK` or `202 Accepted`).

### 2.3 Semantic Rate Limiting (Risk Budgeting)

Traditional Rate Limiting (Requests Per Second) is insufficient for AI. An AI can destroy a business with 1 request per minute if each request triggers a high-cost action.

**The "Risk Budget":**
USP introduces rate limiting based on **Business Impact**, not just HTTP traffic.

| Action | HTTP Cost | **Risk Cost** |
| :--- | :--- | :--- |
| `GET /slots` | 1 | 1 |
| `POST /quote` | 1 | 5 |
| `POST /book` | 1 | **50** |
| `DELETE /book` | 1 | **100** |

If an agent burns its daily "Risk Budget" (e.g. canceling 5 bookings), it is blocked, even if HTTP volume is low.

### 2.4 Verifiable Identity (`Souls.md` Pattern)

Agents must claim and prove their identity to access sensitive contexts.
Agents present a signed "Manifest" or "Identity Certificate" (conceptually similar to `souls.md`), containing Owner, Purpose, and Public Key. The Provider verifies the signature and upgrades the session from "Anonymous" to "Personalized".

### 2.5 The Kill Switch (Standardized Termination)

To ensure human control, USP specifies a standard termination protocol.
If a Service Provider triggers a "Panic Button", the server sends a standardized `SIGTERM` signal. The Agent MUST immediately cease all operations.

---

## 3. Technical Security Implementation (Defense in Depth)

In addition to the agentic principles above, standard security layers are mandatory:


## 3. Authentication

### 3.1 Authentication Methods

USP supports multiple authentication methods:

#### 3.1.1 API Keys
- Simple keys for basic authentication
- Sent in header `Authorization: Bearer <api-key>`
- Suitable for service-to-service communication

#### 3.1.2 OAuth 2.0
- Industry standard for authorization
- Supports different flows (client credentials, authorization code, etc.)
- Access tokens with expiration

#### 3.1.3 JWT (JSON Web Tokens)
- Self-contained tokens with claims
- Digital signature for verification
- Supports refresh tokens

#### 3.1.4 mTLS (Mutual TLS)
- Mutual authentication based on certificates
- High security for service-to-service communication
- Requires PKI infrastructure

## 4. Authorization

### 4.1 Authorization Models

#### 4.1.1 Role-Based Access Control (RBAC)
- Users/services have roles
- Roles have associated permissions
- Access based on user roles

#### 4.1.2 Permission-Based
- Granular permissions per resource/action
- More flexible than RBAC
- Allows fine-grained control

## 5. Encryption

### 5.1 Encryption in Transit

**TLS/SSL mandatory in production**:
- TLS 1.2 minimum (TLS 1.3 recommended)
- Secure cipher suites
- Valid and trusted certificates
- Certificate validation

### 5.2 Encryption at Rest

Sensitive data must be encrypted when stored:
- Encryption keys managed securely
- Strong algorithms (AES-256, etc.)
- Key rotation

## 6. Input Validation

### 6.1 Sanitization

All input must be sanitized:
- Remove dangerous characters
- Validate data types
- Limit field sizes
- Prevent injection attacks

### 6.2 Schema Validation

Payloads must be validated against schemas:
- Correct data types
- Required fields present
- Valid formats (email, UUID, etc.)
- Constraints respected

## 7. Rate Limiting

### 7.1 Abuse Protection

Rate limiting protects against:
- DDoS attacks
- API abuse
- Resource exhaustion

### 7.2 Implementation

- Limits per client/service
- Limits per endpoint
- Global limits
- Rate limit headers (X-RateLimit-*)

## 8. Logging and Auditing

### 8.1 Security Logging

Record security events:
- Authentication attempts (success/failure)
- Denied accesses
- Sensitive operations
- Security errors

### 8.2 Sensitive Information

**DO NOT** log:
- Passwords
- Complete tokens
- Sensitive personal data
- Encryption keys

## 9. Best Practices

### 9.1 Development

- Never commit credentials in code
- Use environment variables for secrets
- Validate all inputs
- Use prepared statements/parameterized queries
- Update dependencies regularly

### 9.2 Operations

- Rotate credentials regularly
- Monitor access attempts
- Implement security alerts
- Make secure backups
- Have an incident response plan

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
