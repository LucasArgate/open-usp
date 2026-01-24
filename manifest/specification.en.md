# USP Specification - Technical Specification

**Language**: [🇧🇷 Português](./specification.md) | [🇺🇸 English](#1-introduction)

## 1. Introduction

This specification defines the **Universal Service Protocol (USP)**, a universal communication protocol designed to bridge the gap between AI agents and the real world of services. While protocols like Google's UCP (Universal Commerce Protocol) effectively solve the problem for Retail (static products, standard shipping, fixed prices), there is a massive vacuum in the Service Economy.

USP fills this gap, enabling AI agents to discover, understand, and interact with real-world services in a standardized way.

### 1.1 Scope

USP defines:
- Message formats
- Service contracts
- Discovery mechanisms
- Authentication and authorization protocols
- Data formats and serialization
- Error handling
- AI agent-friendly interfaces

### 1.2 Version

**Current Version**: 1.0.0-draft

## 2. Architecture

### 2.1 Main Components

#### 2.1.1 Service
A service is an entity that exposes functionalities through the USP protocol. Each service:
- Has a unique identifier (Service ID)
- Exposes one or more contracts
- Can be discovered through discovery mechanisms
- Implements endpoints for message processing
- Provides AI-readable descriptions and schemas

#### 2.1.2 AI Agent / Client
An AI agent or client is an entity that consumes services through the USP protocol. AI agents:
- Discover available services through discovery mechanisms
- Understand service contracts through standardized schemas
- Establish connections with services
- Send requests and process responses
- Can subscribe to events
- Interpret service responses naturally

#### 2.1.3 Broker (Optional)
A broker facilitates service discovery and routing:
- Maintains registry of available services
- Routes messages between agents and services
- Provides discovery mechanisms

#### 2.1.4 Gateway (Optional)
Gateways allow integration with external protocols:
- Translate messages between USP and other protocols
- Act as adapters for legacy systems

### 2.2 Communication Model

USP supports two main communication models:

1. **Request-Response**: Synchronous communication where the agent sends a request and waits for a response
2. **Event-Driven**: Asynchronous communication based on events and pub/sub

## 3. Identifiers

### 3.1 Service ID

Each service has a unique identifier in the format:

```
<namespace>:<service-name>@<version>
```

Example: `com.example:user-service@1.0.0`

### 3.2 Endpoint ID

Endpoints within a service are identified by:

```
<service-id>/<endpoint-path>
```

Example: `com.example:user-service@1.0.0/users`

## 4. Contracts

### 4.1 Contract Definition

A contract defines:
- Name and version
- Available methods
- Published events
- Data types
- Authentication requirements
- AI-readable descriptions

### 4.2 Contract Structure

```yaml
contract:
  name: string
  version: string
  namespace: string
  description: string  # AI-readable description
  methods:
    - name: string
      description: string  # AI-readable method description
      input: Schema
      output: Schema
      errors: ErrorSchema[]
  events:
    - name: string
      description: string
      payload: Schema
  types:
    - name: string
      definition: Schema
```

## 5. Messages

### 5.1 Message Structure

All USP messages follow the structure:

```json
{
  "header": {
    "id": "message-id",
    "type": "request|response|event|error",
    "timestamp": "ISO8601",
    "service": "service-id",
    "endpoint": "endpoint-path",
    "correlationId": "correlation-id"
  },
  "payload": {},
  "metadata": {}
}
```

### 5.2 Message Types

#### 5.2.1 Request
Request message sent by an agent to a service.

#### 5.2.2 Response
Response message sent by a service to an agent.

#### 5.2.3 Event
Event message published by a service.

#### 5.2.4 Error
Error message indicating processing failure.

## 6. Serialization

USP supports multiple serialization formats:
- JSON (default)
- MessagePack
- Protocol Buffers
- Avro

The format is negotiated during the initial handshake.

## 7. Transport

USP is transport-agnostic and can be implemented over:
- HTTP/HTTPS
- WebSocket
- gRPC
- MQTT
- AMQP
- Custom TCP/UDP

## 8. Service Discovery

### 8.1 Discovery Mechanisms

1. **Service Registry**: Centralized service registry
2. **DNS-based**: Discovery through DNS
3. **Multicast**: Local network discovery
4. **Configuration**: Static configuration

### 8.2 Discovery Format

```json
{
  "service": "service-id",
  "endpoints": ["endpoint1", "endpoint2"],
  "contracts": ["contract1", "contract2"],
  "location": "transport://address",
  "metadata": {},
  "description": "AI-readable service description"
}
```

## 9. Authentication and Authorization

### 9.1 Authentication

USP supports multiple authentication methods:
- API Keys
- OAuth 2.0
- JWT (JSON Web Tokens)
- mTLS (Mutual TLS)
- Custom authentication

### 9.2 Authorization

Authorization is based on:
- Roles and Permissions
- Resource-based access control
- Policy-based access control

## 10. Error Handling

### 10.1 Error Codes

Errors follow a structured code:

```
<namespace>:<error-code>
```

Example: `com.example:INVALID_INPUT`

### 10.2 Error Structure

```json
{
  "code": "error-code",
  "message": "Human readable message",
  "details": {},
  "retryable": false
}
```

## 11. Versioning

### 11.1 Semantic Versioning

USP uses [Semantic Versioning](https://semver.org/):
- **MAJOR**: Incompatible changes
- **MINOR**: Compatible new features
- **PATCH**: Compatible bug fixes

### 11.2 Compatibility

- Services must maintain compatibility with previous versions within the same major version
- New versions must document breaking changes

## 12. Extensions

USP allows extensions through:
- Custom message types
- Additional metadata
- Domain-specific contracts
- Custom serialization formats

---

**Status**: Draft  
**Last Revision**: January 2026
