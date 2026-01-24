# USP Specification - Especificação Técnica

**Language**: [🇧🇷 Português](#1-introdução) | [🇺🇸 English](#1-introduction)

## 1. Introdução

Esta especificação define o **Universal Service Protocol (USP)**, um protocolo de comunicação universal projetado para conectar agentes de IA ao mundo real de serviços. O USP preenche o vácuo na Economia de Serviços, permitindo que agentes de IA descubram, entendam e interajam com serviços de forma padronizada.

### 1.1 Escopo

O USP define:
- Formato de mensagens
- Contratos de serviço
- Mecanismos de descoberta
- Protocolos de autenticação e autorização
- Formatos de dados e serialização
- Tratamento de erros

### 1.2 Versão

**Versão Atual**: 1.0.0-draft

## 2. Arquitetura

### 2.1 Componentes Principais

#### 2.1.1 Serviço (Service)
Um serviço é uma entidade que expõe funcionalidades através do protocolo USP. Cada serviço:
- Possui um identificador único (Service ID)
- Expõe um ou mais contratos
- Pode ser descoberto através de mecanismos de descoberta
- Implementa endpoints para processamento de mensagens

#### 2.1.2 Agente de IA / Cliente (AI Agent / Client)
Um agente de IA ou cliente é uma entidade que consome serviços através do protocolo USP. Agentes de IA:
- Descobrem serviços disponíveis através de mecanismos de descoberta
- Entendem contratos de serviço através de schemas padronizados
- Estabelecem conexões com serviços
- Enviam requisições e processam respostas
- Podem assinar eventos
- Interpretam respostas de serviços de forma natural

#### 2.1.3 Broker (Opcional)
Um broker facilita a descoberta e roteamento de serviços:
- Mantém registro de serviços disponíveis
- Roteia mensagens entre clientes e serviços
- Fornece mecanismos de descoberta

#### 2.1.4 Gateway (Opcional)
Gateways permitem integração com protocolos externos:
- Traduzem mensagens entre USP e outros protocolos
- Atuam como adaptadores para sistemas legados

### 2.2 Modelo de Comunicação

O USP suporta dois modelos principais de comunicação:

1. **Request-Response**: Comunicação síncrona onde o cliente envia uma requisição e aguarda uma resposta
2. **Event-Driven**: Comunicação assíncrona baseada em eventos e pub/sub

## 3. Identificadores

### 3.1 Service ID

Cada serviço possui um identificador único no formato:

```
<namespace>:<service-name>@<version>
```

Exemplo: `com.example:user-service@1.0.0`

### 3.2 Endpoint ID

Endpoints dentro de um serviço são identificados por:

```
<service-id>/<endpoint-path>
```

Exemplo: `com.example:user-service@1.0.0/users`

## 4. Contratos

### 4.1 Definição de Contrato

Um contrato define:
- Nome e versão
- Métodos disponíveis
- Eventos publicados
- Tipos de dados
- Requisitos de autenticação
- Descrições legíveis por IA

### 4.2 Estrutura de Contrato

```yaml
contract:
  name: string
  version: string
  namespace: string
  description: string  # Descrição legível por IA
  methods:
    - name: string
      description: string  # Descrição do método legível por IA
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

## 5. Mensagens

### 5.1 Estrutura de Mensagem

Todas as mensagens USP seguem a estrutura:

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

### 5.2 Tipos de Mensagem

#### 5.2.1 Request
Mensagem de requisição enviada por um cliente para um serviço.

#### 5.2.2 Response
Mensagem de resposta enviada por um serviço para um cliente.

#### 5.2.3 Event
Mensagem de evento publicada por um serviço.

#### 5.2.4 Error
Mensagem de erro indicando falha no processamento.

## 6. Serialização

O USP suporta múltiplos formatos de serialização:
- JSON (padrão)
- MessagePack
- Protocol Buffers
- Avro

O formato é negociado durante o handshake inicial.

## 7. Transporte

O USP é agnóstico ao transporte e pode ser implementado sobre:
- HTTP/HTTPS
- WebSocket
- gRPC
- MQTT
- AMQP
- TCP/UDP customizado

## 8. Descoberta de Serviços

### 8.1 Mecanismos de Descoberta

1. **Service Registry**: Registro centralizado de serviços
2. **DNS-based**: Descoberta através de DNS
3. **Multicast**: Descoberta em rede local
4. **Configuration**: Configuração estática

### 8.2 Formato de Descoberta

```json
{
  "service": "service-id",
  "endpoints": ["endpoint1", "endpoint2"],
  "contracts": ["contract1", "contract2"],
  "location": "transport://address",
  "metadata": {},
  "description": "Descrição do serviço legível por IA"
}
```

## 9. Autenticação e Autorização

### 9.1 Autenticação

O USP suporta múltiplos métodos de autenticação:
- API Keys
- OAuth 2.0
- JWT (JSON Web Tokens)
- mTLS (Mutual TLS)
- Custom authentication

### 9.2 Autorização

Autorização é baseada em:
- Roles e Permissions
- Resource-based access control
- Policy-based access control

## 10. Tratamento de Erros

### 10.1 Códigos de Erro

Erros seguem um código estruturado:

```
<namespace>:<error-code>
```

Exemplo: `com.example:INVALID_INPUT`

### 10.2 Estrutura de Erro

```json
{
  "code": "error-code",
  "message": "Human readable message",
  "details": {},
  "retryable": false
}
```

## 11. Versionamento

### 11.1 Versionamento Semântico

O USP utiliza [Semantic Versioning](https://semver.org/):
- **MAJOR**: Mudanças incompatíveis
- **MINOR**: Novas funcionalidades compatíveis
- **PATCH**: Correções de bugs compatíveis

### 11.2 Compatibilidade

- Serviços devem manter compatibilidade com versões anteriores dentro da mesma major version
- Novas versões devem documentar breaking changes

## 12. Extensões

O USP permite extensões através de:
- Custom message types
- Additional metadata
- Domain-specific contracts
- Custom serialization formats

---

**Status**: Draft  
**Última Revisão**: Janeiro 2026

---

**English Version**: [Technical Specification](./specification.en.md)
