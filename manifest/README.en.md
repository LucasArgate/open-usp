# USP Manifest - Universal Service Protocol

**Language**: [🇧🇷 Português](./README.md) | [🇺🇸 English](#-overview)

## 🌐 Overview

The **Universal Service Protocol (USP)** is a universal communication protocol designed to bridge the gap between AI agents and the real world of services. While protocols like Google's UCP (Universal Commerce Protocol) effectively solve the problem for Retail (static products, standard shipping, fixed prices), there is a massive vacuum in the Service Economy.

This manifest defines the complete protocol specification, including its architecture, contracts, messages, and communication mechanisms, specifically designed to enable AI agents to discover, understand, and interact with real-world services.

## 🎯 Fundamental Principles

### 1. AI Agent Focus
USP is designed with AI agents as first-class citizens, enabling them to understand and interact with services naturally.

### 2. Universality
USP is designed to be agnostic to technologies, programming languages, and platforms, allowing any service to communicate with any other service.

### 3. Simplicity
The specification prioritizes simplicity and clarity, facilitating implementation and adoption of the protocol.

### 4. Extensibility
The protocol supports domain-specific extensions and customizations without breaking compatibility with base implementations.

### 5. Security
Security is a fundamental concern, with native support for authentication, authorization, and encryption.

## 📚 Documentation

**English:**
- [Technical Specification](./specification.en.md) - Complete technical details of the protocol
- [Service Contracts](./contracts.en.md) - Contract and interface definitions
- [Messages and Payloads](./messages.en.md) - Message structure and data formats
- [Security](./security.en.md) - Security and authentication specifications
- [Extensions](./extensions.en.md) - How to create and use protocol extensions

**Português:**
- [Especificação Técnica](./specification.md) - Detalhes técnicos completos do protocolo
- [Contratos de Serviço](./contracts.md) - Definição de contratos e interfaces
- [Mensagens e Payloads](./messages.md) - Estrutura de mensagens e formatos de dados
- [Segurança](./security.md) - Especificações de segurança e autenticação
- [Extensões](./extensions.md) - Como criar e usar extensões do protocolo

## 🏗️ Architecture

USP is based on a distributed services architecture where:

- **Services** expose functionalities through standardized interfaces
- **AI Agents** consume services through defined contracts
- **Brokers** (optional) facilitate service discovery and routing
- **Gateways** (optional) allow integration with external protocols

## 🔄 Communication Flow

1. **Discovery**: AI agent discovers available services
2. **Negotiation**: Agent and service negotiate capabilities and contracts
3. **Authentication**: Credential establishment and authorization
4. **Communication**: Message exchange following the protocol
5. **Monitoring**: Health checks and metrics (optional)

## 📦 Main Components

### Services
Entities that expose functionalities through the USP protocol.

### Contracts
Formal definitions of interfaces, methods, events, and data types.

### Messages
Basic units of communication between services and agents.

### Schemas
Data type definitions and validation.

## 🚀 Next Steps

- Read the [Technical Specification](./specification.en.md) to understand implementation details
- Explore [Examples](../examples/) to see practical implementations
- Consult [Contracts](./contracts.en.md) to understand how to define interfaces

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
