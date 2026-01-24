# USP Extensions - Extensions

**Language**: [🇧🇷 Português](./extensions.md) | [🇺🇸 English](#1-introduction)

## 1. Introduction

The Universal Service Protocol (USP) is designed to be extensible, allowing implementations to add domain-specific functionalities without breaking compatibility with the base protocol.

## 2. Extensibility Principles

### 2.1 Compatibility

- Extensions must not break base implementations
- Implementations must ignore unknown extensions
- Extensions must be optional

### 2.2 Namespacing

- Extensions must use unique namespaces
- Prevent conflicts between extensions
- Facilitate discovery and documentation

### 2.3 Versioning

- Extensions must be versioned
- Incompatible changes require a new version
- Maintain compatibility when possible

## 3. Types of Extensions

### 3.1 Message Extensions

Add custom fields to header or metadata:

```json
{
  "header": {
    "id": "msg-001",
    "type": "request",
    "extensions": {
      "com.example:custom-field": "value",
      "com.example:priority": 10
    }
  }
}
```

### 3.2 Contract Extensions

Add functionalities to existing contracts:

```yaml
contract:
  name: "UserService"
  extensions:
    - namespace: "com.example"
      name: "audit-logging"
      version: "1.0.0"
      config:
        enabled: true
        level: "detailed"
```

### 3.3 Transport Extensions

Add support for new transport protocols:
- WebRTC
- QUIC
- Custom protocols

## 4. Extension Registry

### 4.1 Registration Format

Extensions must be registered with:

```yaml
extension:
  namespace: "com.example"
  name: "custom-feature"
  version: "1.0.0"
  description: "Extension description"
  author: "Author name"
  contact: "contact@example.com"
  specification: "URL to specification"
  compatibility:
    usp-version: ">=1.0.0"
  dependencies: []
```

## 5. Extension Discovery

### 5.1 Negotiation

During handshake, services can announce supported extensions:

```json
{
  "service": "com.example:user-service@1.0.0",
  "extensions": [
    {
      "namespace": "com.example",
      "name": "custom-feature",
      "version": "1.0.0"
    }
  ]
}
```

## 6. Extension Examples

### 6.1 Response Cache Extension

Add response caching support:

```yaml
extension:
  namespace: "usp.extensions"
  name: "response-cache"
  version: "1.0.0"
  features:
    - cache-control headers
    - cache invalidation
    - cache strategies
```

### 6.2 Observability Extension

Add metrics and tracing:

```yaml
extension:
  namespace: "usp.extensions"
  name: "observability"
  version: "1.0.0"
  features:
    - distributed tracing
    - metrics collection
    - structured logging
```

## 7. Creating Extensions

### 7.1 Process

1. **Define Need**: Identify missing functionality
2. **Design**: Design the extension
3. **Document**: Create complete specification
4. **Implement**: Develop the extension
5. **Register**: Add to registry
6. **Publish**: Make available to community

### 7.2 Best Practices

- Use descriptive namespaces
- Document completely
- Provide examples
- Maintain compatibility
- Version appropriately

## 8. Standard Extensions

### 8.1 Core Extensions

Extensions maintained by the USP project:

- `usp.core:health-check`: Standardized health checks
- `usp.core:metrics`: Basic metrics
- `usp.core:logging`: Structured logging

## 9. Compatibility

### 9.1 Backward Compatibility

- Extensions should be added, not removed
- Optional fields can be added
- New methods/events are compatible

### 9.2 Forward Compatibility

- Implementations must ignore unknown extensions
- Do not fail on unrecognized fields
- Allow graceful degradation

## 10. Extension Versioning

### 10.1 Semantic Versioning

Extensions follow semantic versioning:
- **MAJOR**: Breaking changes
- **MINOR**: Compatible new features
- **PATCH**: Bug fixes

### 10.2 Deprecation

- Deprecate before removing
- Provide transition period
- Document migration

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
