# USP Security & Governance Model (Draft v0.1)

The Universal Services Protocol (USP) adopts a "Security-by-Design" approach to enable safe interactions between independent AI Agents and real-world Service Providers.

Unlike traditional APIs where the client is a trusted developer, USP assumes the client is a probabilistic AI Agent. Therefore, security controls must be **Semantic** and **Negotiated**, not just cryptographic.

## 1. Capabilities Negotiation ("The Handshake")

In traditional APIs, permissions (Scopes) are static. In USP, permissions are dynamically negotiated based on trust levels.

### Concept
When an Agent connects, it proposes a "Manifest of Intent". The Service Provider challenges this intent and grants a provisional or full capability set.

### Flow
1.  **Agent Hello**: "I am a Scheduling Agent. I request `read:calendar` and `write:appointment`."
2.  **Service Challenge**: checks Agent reputation/signature.
3.  **Service Response**: 
    - *Trusted Agent*: "Granted `read:calendar`, `write:appointment`."
    - *Untrusted Agent*: "Granted `read:calendar`. `write:appointment` granted in `PROVISIONAL` mode (requires human approval)."

## 2. Native "Draft Mode" (Dry Runs)

Agents often need to "think" or "plan" without causing real-world side effects. USP mandates a standard way to execute "Dry Runs".

### Specification
All state-changing verbs (`POST`, `PUT`, `PATCH`) MUST support a standard headers or flag (e.g., `X-USP-Execution-Mode: Dry-Run`).

### Behavior
- **Input**: Agent sends a request to book a service.
- **Process**: Server validates data, checks availability, calculates prices, and runs business logic.
- **Output**: Server returns `200 OK` (or `202 Accepted`) with the *would-be* result, but **does not commit** the transaction to the database.
- **Benefit**: Allows agents to validate complex plans (e.g., "Can I book this + order parts?") without risk.

## 3. Semantic Rate Limiting (Risk Budgeting)

Traditional Rate Limiting (Requests Per Second) is insufficient for AI. An AI can destroy a business with 1 request per minute if each request triggers a high-cost action.

### The "Risk Budget"
USP introduces rate limiting based on **Business Impact**, not just HTTP traffic.

| Action | HTTP Cost | **Risk Cost** |
| :--- | :--- | :--- |
| `GET /slots` | 1 | 1 |
| `POST /quote` | 1 | 5 |
| `POST /book` | 1 | **50** |
| `DELETE /book` | 1 | **100** |

**Mechanism**:
- A Service Provider sets a "Daily Risk Budget" (e.g., 500 points) for an unverified agent.
- If the agent cancels 5 bookings (5 * 100 = 500), it is blocked, even if it only made 5 HTTP requests.

## 4. Verifiable Identity (`Souls.md` Pattern)

Agents must claim and prove their identity to access sensitive contexts.

### Mechanism
- Agents present a signed "Manifest" or "Identity Certificate" (conceptually similar to `souls.md` in agentic frameworks).
- This certificate contains the Agent's Owner, Purpose, and Public Key.
- **Example**: "I am the Personal Shopper Agent for User [Lucas Argate], signed by [AuthAuthority]."
- **Result**: Provider verifies the signature and upgrades the session from "Anonymous" to "Personalized".

## 5. The Kill Switch (Standardized Termination)

To ensure human control, USP specifies a standard termination protocol.

### Requirement
- All USP-compliant sessions must accept a revocation signal.
- If a Service Provider (or the User) triggers a "Panic Button", the server sends a standardized `SIGTERM` signal (via WebSocket or Webhook).
- The Agent MUST immediately cease all operations and discard all tokens for that session.
