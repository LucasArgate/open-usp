# USP Messages - Messages and Payloads

**Language**: [🇧🇷 Português](./messages.md) | [🇺🇸 English](#1-introduction)

## 1. Introduction

This document defines the message structure in the Universal Service Protocol (USP), including formats, types, and processing, designed to be AI agent-friendly.

## 2. Message Structure

### 2.1 Base Format

All USP messages follow a common structure:

```json
{
  "header": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "type": "request",
    "timestamp": "2026-01-24T10:30:00Z",
    "service": "com.example:user-service@1.0.0",
    "endpoint": "/users",
    "correlationId": "550e8400-e29b-41d4-a716-446655440001",
    "version": "1.0.0"
  },
  "payload": {},
  "metadata": {
    "source": "client-id",
    "destination": "service-id",
    "encoding": "json",
    "compression": null
  }
}
```

### 2.2 Header

The header contains message control information:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string (UUID) | Yes | Unique message identifier |
| `type` | enum | Yes | Message type (request, response, event, error) |
| `timestamp` | string (ISO8601) | Yes | Message creation timestamp |
| `service` | string | Yes | Target service ID |
| `endpoint` | string | Yes | Endpoint within the service |
| `correlationId` | string (UUID) | No | ID to correlate request/response |
| `version` | string | Yes | USP protocol version |

## 3. Message Types

### 3.1 Request

Request message sent by an agent to a service.

**Structure**:
```json
{
  "header": {
    "type": "request",
    "method": "createUser",
    "timeout": 5000
  },
  "payload": {
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### 3.2 Response

Response message sent by a service to an agent.

**Structure**:
```json
{
  "header": {
    "type": "response",
    "status": "success",
    "correlationId": "550e8400-e29b-41d4-a716-446655440001"
  },
  "payload": {
    "id": "user-123",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### 3.3 Event

Event message published by a service.

### 3.4 Error

Error message indicating processing failure.

**Structure**:
```json
{
  "header": {
    "type": "error",
    "correlationId": "550e8400-e29b-41d4-a716-446655440001",
    "status": "error"
  },
  "payload": {
    "code": "com.example:INVALID_INPUT",
    "message": "Invalid email",
    "details": {
      "field": "email",
      "reason": "Invalid format"
    },
    "retryable": false
  }
}
```

## 4. Error Codes

### 4.1 Format

Error codes follow the format:
```
<namespace>:<error-code>
```

### 4.2 Standard Codes

| Code | Description |
|------|-------------|
| `usp.core:INVALID_MESSAGE` | Invalid or malformed message |
| `usp.core:UNAUTHORIZED` | Authentication failure |
| `usp.core:FORBIDDEN` | Authorization failure |
| `usp.core:NOT_FOUND` | Resource not found |
| `usp.core:TIMEOUT` | Request timeout |
| `usp.core:INTERNAL_ERROR` | Internal server error |
| `usp.core:SERVICE_UNAVAILABLE` | Service unavailable |
| `usp.core:RATE_LIMIT_EXCEEDED` | Rate limit exceeded |

## 5. Serialization

### 5.1 Supported Formats

- **JSON**: Default format, readable and widely supported
- **MessagePack**: Compact binary format
- **Protocol Buffers**: Efficient and typed binary format
- **Avro**: Binary format with schema

## 6. Compression

### 6.1 Supported Algorithms

- **gzip**: Default compression
- **brotli**: More efficient compression
- **deflate**: Alternative compression
- **null**: No compression

## 7. Correlation

### 7.1 Correlation ID

The `correlationId` allows correlating requests and responses:

- Agent generates a unique `correlationId` for each request
- Service copies the `correlationId` in the corresponding response
- Enables asynchronous processing and tracking

## 8. Timeouts and Retries

### 8.1 Timeouts

- Timeout can be specified in the request header
- Default timeout is defined by the service
- Timeout must be respected by the transport

### 8.2 Retries

- Retries should be based on the `retryable` field of the error
- Implementations should use exponential backoff
- Idempotency should be considered for operations that can be retried

## 9. Validation

### 9.1 Message Validation

All messages must be validated:
- Header structure
- Data types
- Required fields
- Formats (UUID, ISO8601, etc.)

### 9.2 Payload Validation

Payloads must be validated against schemas defined in contracts.

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
