# USP Messages - Mensagens e Payloads

**Language**: [🇧🇷 Português](#1-introdução) | [🇺🇸 English](./messages.en.md)

## 1. Introdução

Este documento define a estrutura de mensagens no Universal Service Protocol (USP), incluindo formatos, tipos e processamento.

## 2. Estrutura de Mensagem

### 2.1 Formato Base

Todas as mensagens USP seguem uma estrutura comum:

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

O header contém informações de controle da mensagem:

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| `id` | string (UUID) | Sim | Identificador único da mensagem |
| `type` | enum | Sim | Tipo da mensagem (request, response, event, error) |
| `timestamp` | string (ISO8601) | Sim | Timestamp de criação da mensagem |
| `service` | string | Sim | ID do serviço destinatário |
| `endpoint` | string | Sim | Endpoint dentro do serviço |
| `correlationId` | string (UUID) | Não | ID para correlacionar request/response |
| `version` | string | Sim | Versão do protocolo USP |

### 2.3 Payload

O payload contém os dados específicos da mensagem, formatados conforme o contrato do serviço.

### 2.4 Metadata

Metadata contém informações adicionais sobre a mensagem:

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `source` | string | Identificador da origem |
| `destination` | string | Identificador do destino |
| `encoding` | string | Formato de serialização (json, msgpack, protobuf) |
| `compression` | string | Algoritmo de compressão (gzip, brotli, null) |
| `ttl` | number | Time-to-live em segundos |
| `priority` | number | Prioridade da mensagem (0-100) |

## 3. Tipos de Mensagem

### 3.1 Request

Mensagem de requisição enviada por um cliente para um serviço.

**Estrutura**:
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

**Campos Adicionais no Header**:
- `method`: Nome do método a ser invocado
- `timeout`: Timeout em milissegundos (opcional)

### 3.2 Response

Mensagem de resposta enviada por um serviço para um cliente.

**Estrutura**:
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

**Status Possíveis**:
- `success`: Requisição processada com sucesso
- `error`: Erro no processamento (ver mensagem de erro)

### 3.3 Event

Mensagem de evento publicada por um serviço.

**Estrutura**:
```json
{
  "header": {
    "type": "event",
    "event": "userCreated",
    "source": "com.example:user-service@1.0.0"
  },
  "payload": {
    "userId": "user-123",
    "timestamp": "2026-01-24T10:30:00Z"
  }
}
```

**Campos Adicionais no Header**:
- `event`: Nome do evento
- `source`: ID do serviço que publicou o evento

### 3.4 Error

Mensagem de erro indicando falha no processamento.

**Estrutura**:
```json
{
  "header": {
    "type": "error",
    "correlationId": "550e8400-e29b-41d4-a716-446655440001",
    "status": "error"
  },
  "payload": {
    "code": "com.example:INVALID_INPUT",
    "message": "Email inválido",
    "details": {
      "field": "email",
      "reason": "Formato inválido"
    },
    "retryable": false
  }
}
```

**Campos do Payload de Erro**:
- `code`: Código de erro estruturado
- `message`: Mensagem legível para humanos
- `details`: Detalhes adicionais do erro
- `retryable`: Indica se a operação pode ser tentada novamente

## 4. Códigos de Erro

### 4.1 Formato

Códigos de erro seguem o formato:
```
<namespace>:<error-code>
```

### 4.2 Códigos Padrão

| Código | Descrição |
|--------|-----------|
| `u.core:INVALID_MESSAGE` | Mensagem inválida ou malformada |
| `u.core:UNAUTHORIZED` | Falha de autenticação |
| `u.core:FORBIDDEN` | Falha de autorização |
| `u.core:NOT_FOUND` | Recurso não encontrado |
| `u.core:TIMEOUT` | Timeout na requisição |
| `u.core:INTERNAL_ERROR` | Erro interno do servidor |
| `u.core:SERVICE_UNAVAILABLE` | Serviço indisponível |
| `u.core:RATE_LIMIT_EXCEEDED` | Limite de taxa excedido |

## 5. Serialização

### 5.1 Formatos Suportados

- **JSON**: Formato padrão, legível e amplamente suportado
- **MessagePack**: Formato binário compacto
- **Protocol Buffers**: Formato binário eficiente e tipado
- **Avro**: Formato binário com schema

### 5.2 Negociação de Formato

O formato é negociado durante o handshake inicial ou pode ser especificado no metadata da mensagem.

## 6. Compressão

### 6.1 Algoritmos Suportados

- **gzip**: Compressão padrão
- **brotli**: Compressão mais eficiente
- **deflate**: Compressão alternativa
- **null**: Sem compressão

### 6.2 Uso

Compressão é aplicada ao payload completo após serialização. O algoritmo é especificado no metadata.

## 7. Correlação

### 7.1 Correlation ID

O `correlationId` permite correlacionar requisições e respostas:

- Cliente gera um `correlationId` único para cada requisição
- Serviço copia o `correlationId` na resposta correspondente
- Permite processamento assíncrono e rastreamento

### 7.2 Rastreamento

Mensagens podem incluir headers de rastreamento para distributed tracing:
- `traceId`: ID do trace distribuído
- `spanId`: ID do span atual
- `parentSpanId`: ID do span pai

## 8. Timeouts e Retries

### 8.1 Timeouts

- Timeout pode ser especificado no header da requisição
- Timeout padrão é definido pelo serviço
- Timeout deve ser respeitado pelo transporte

### 8.2 Retries

- Retries devem ser baseados no campo `retryable` do erro
- Implementações devem usar exponential backoff
- Idempotência deve ser considerada para operações que podem ser retentadas

## 9. Validação

### 9.1 Validação de Mensagem

Todas as mensagens devem ser validadas:
- Estrutura do header
- Tipos de dados
- Campos obrigatórios
- Formatos (UUID, ISO8601, etc.)

### 9.2 Validação de Payload

Payloads devem ser validados contra os schemas definidos nos contratos.

## 10. Exemplos

### 10.1 Request/Response Completo

**Request**:
```json
{
  "header": {
    "id": "req-001",
    "type": "request",
    "timestamp": "2026-01-24T10:30:00Z",
    "service": "com.example:user-service@1.0.0",
    "endpoint": "/users",
    "method": "createUser",
    "correlationId": "corr-001",
    "version": "1.0.0"
  },
  "payload": {
    "name": "John Doe",
    "email": "john@example.com"
  },
  "metadata": {
    "source": "client-123",
    "encoding": "json"
  }
}
```

**Response**:
```json
{
  "header": {
    "id": "resp-001",
    "type": "response",
    "timestamp": "2026-01-24T10:30:01Z",
    "status": "success",
    "correlationId": "corr-001",
    "version": "1.0.0"
  },
  "payload": {
    "id": "user-123",
    "name": "John Doe",
    "email": "john@example.com",
    "createdAt": "2026-01-24T10:30:01Z"
  }
}
```

---

**Versão**: 1.0.0-draft  
**Última Atualização**: Janeiro 2026

---

**English Version**: [Messages and Payloads](./messages.en.md)
