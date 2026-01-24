# USP Contracts - Service Contracts

**Language**: [🇧🇷 Português](./contracts.md) | [🇺🇸 English](#1-introduction)

## 1. Introduction

Contracts define the formal interfaces between services and clients (AI agents) in USP. A contract specifies available methods, published events, data types, and security requirements, with AI-readable descriptions to enable natural interaction.

## 2. Contract Structure

### 2.1 Base Format

A USP contract is defined in YAML or JSON format:

```yaml
contract:
  name: "UserService"
  version: "1.0.0"
  namespace: "com.example"
  description: "User management service"  # AI-readable description
  
  methods:
    - name: "createUser"
      description: "Creates a new user"  # AI-readable method description
      input:
        type: "object"
        properties:
          name:
            type: "string"
            required: true
          email:
            type: "string"
            format: "email"
            required: true
      output:
        type: "object"
        properties:
          id:
            type: "string"
          name:
            type: "string"
          email:
            type: "string"
      errors:
        - code: "INVALID_EMAIL"
          message: "Invalid email"
        - code: "DUPLICATE_USER"
          message: "User already exists"
      authentication: required
      authorization:
        roles: ["admin", "user-manager"]
```

## 3. Methods

### 3.1 Method Definition

A method defines an operation that can be invoked on a service:

- **name**: Unique method name
- **description**: AI-readable description of what the method does
- **input**: Input schema (parameters)
- **output**: Output schema (return value)
- **errors**: List of possible errors
- **authentication**: Authentication requirements
- **authorization**: Authorization requirements

### 3.2 Method Types

#### 3.2.1 Query
Methods that only read data (idempotent):
- Do not modify state
- Can be cached
- Should be safe for retry

#### 3.2.2 Command
Methods that modify state:
- May have side effects
- Should be handled carefully in retries
- May require idempotency

## 4. Events

### 4.1 Event Definition

Events represent occurrences in the system that can be published and consumed:

- **name**: Unique event name
- **description**: AI-readable event description
- **payload**: Event payload schema
- **version**: Event version (for evolution)

## 5. Data Types

### 5.1 Schemas

Data types are defined using schemas (JSON Schema, Avro, Protocol Buffers, etc.):

- Primitive types: string, number, boolean, null
- Complex types: object, array
- Custom types: references to other types

## 6. Authentication and Authorization

### 6.1 Authentication Levels

- **none**: No authentication
- **optional**: Optional authentication
- **required**: Required authentication

### 6.2 Authorization

Authorization can be specified through:
- **roles**: List of allowed roles
- **permissions**: List of specific permissions
- **policies**: Custom policies

## 7. Contract Versioning

### 7.1 Compatibility

Contracts must maintain compatibility:
- Adding new methods is compatible
- Adding new optional fields is compatible
- Removing fields or methods is incompatible
- Changing field types is incompatible

## 8. Standard Contracts

### 8.1 Health Check

Every service should implement a basic health check contract:

```yaml
contract:
  name: "HealthCheck"
  version: "1.0.0"
  namespace: "usp.core"
  methods:
    - name: "ping"
      input: {}
      output:
        type: "object"
        properties:
          status: "string"
          timestamp: "string"
    
    - name: "health"
      input: {}
      output:
        type: "object"
        properties:
          status: "string"
          version: "string"
          uptime: "number"
          dependencies: "array"
```

## 9. Best Practices

### 9.1 Contract Design

- Use descriptive and consistent names
- Document all methods and events with AI-readable descriptions
- Provide usage examples
- Clearly define possible errors
- Specify authentication/authorization requirements

### 9.2 Versioning

- Use semantic versioning
- Document breaking changes
- Maintain compatibility when possible
- Provide migration paths

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
