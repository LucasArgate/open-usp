# USP Manifest - Universal Service Protocol

**Language**: [🇧🇷 Português](#-visão-geral) | [🇺🇸 English](#-overview)

## 📖 Visão Geral

O **Universal Service Protocol (USP)** é um protocolo de comunicação universal projetado para conectar agentes de IA ao mundo real de serviços. Enquanto protocolos como o UCP (Universal Commerce Protocol) do Google resolvem efetivamente o problema para o varejo (produtos estáticos, envio padrão, preços fixos), há um grande vácuo na Economia de Serviços.

Este manifesto define a especificação completa do protocolo, incluindo sua arquitetura, contratos, mensagens e mecanismos de comunicação, especificamente projetado para permitir que agentes de IA descubram, entendam e interajam com serviços do mundo real.

## 🎯 Princípios Fundamentais

### 1. Universalidade
O USP é projetado para ser agnóstico a tecnologias, linguagens de programação e plataformas, permitindo que qualquer serviço possa se comunicar com qualquer outro serviço.

### 2. Simplicidade
A especificação prioriza simplicidade e clareza, facilitando a implementação e adoção do protocolo.

### 3. Extensibilidade
O protocolo suporta extensões e customizações específicas de domínio sem quebrar a compatibilidade com implementações base.

### 4. Segurança
Segurança é uma preocupação fundamental, com suporte nativo para autenticação, autorização e criptografia.

## 📚 Documentação

### Manifesto e Visão

- [Abstract](./ABSTRACT.md) - Manifesto fundador do USP (Português)
- [Abstract](./ABSTRACT.en.md) - Founding manifesto of USP (English)

### Especificações Técnicas

**Português:**
- [Especificação Técnica](./specification.md) - Detalhes técnicos completos do protocolo
- [Contratos de Serviço](./contracts.md) - Definição de contratos e interfaces
- [Mensagens e Payloads](./messages.md) - Estrutura de mensagens e formatos de dados
- [Segurança](./security.md) - Especificações de segurança e autenticação
- [Extensões](./extensions.md) - Como criar e usar extensões do protocolo

**English:**
- [Technical Specification](./specification.en.md) - Complete technical details of the protocol
- [Service Contracts](./contracts.en.md) - Contract and interface definitions
- [Messages and Payloads](./messages.en.md) - Message structure and data formats
- [Security](./security.en.md) - Security and authentication specifications
- [Extensions](./extensions.en.md) - How to create and use protocol extensions

## 🏗️ Arquitetura

O USP é baseado em uma arquitetura de serviços distribuídos onde:

- **Serviços** expõem funcionalidades através de interfaces padronizadas
- **Clientes** consomem serviços através de contratos definidos
- **Brokers** (opcionais) facilitam descoberta e roteamento de serviços
- **Gateways** (opcionais) permitem integração com protocolos externos

## 🔄 Fluxo de Comunicação

1. **Descoberta**: Cliente descobre serviços disponíveis
2. **Negociação**: Cliente e serviço negociam capacidades e contratos
3. **Autenticação**: Estabelecimento de credenciais e autorização
4. **Comunicação**: Troca de mensagens seguindo o protocolo
5. **Monitoramento**: Health checks e métricas (opcional)

## 📦 Componentes Principais

### Serviços
Entidades que expõem funcionalidades através do protocolo USP.

### Contratos
Definições formais de interfaces, métodos, eventos e tipos de dados.

### Mensagens
Unidades básicas de comunicação entre serviços e clientes.

### Schemas
Definições de tipos de dados e validação.

## 🚀 Próximos Passos

- Leia a [Especificação Técnica](./specification.md) para entender os detalhes de implementação
- Explore os [Exemplos](../examples/) para ver implementações práticas
- Consulte os [Contratos](./contracts.md) para entender como definir interfaces

---

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
- [Extensões](./extensions.en.md) - Como criar e usar extensões do protocolo

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
