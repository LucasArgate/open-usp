# USP Extensions - Extensões

**Language**: [🇧🇷 Português](#1-introdução) | [🇺🇸 English](./extensions.en.md)

## 1. Introdução

O Universal Service Protocol (USP) foi projetado para ser extensível, permitindo que implementações adicionem funcionalidades específicas de domínio sem quebrar a compatibilidade com o protocolo base.

## 2. Princípios de Extensibilidade

### 2.1 Compatibilidade

- Extensões não devem quebrar implementações base
- Implementações devem ignorar extensões desconhecidas
- Extensões devem ser opcionais

### 2.2 Namespacing

- Extensões devem usar namespaces únicos
- Prevenir conflitos entre extensões
- Facilitar descoberta e documentação

### 2.3 Versionamento

- Extensões devem ser versionadas
- Mudanças incompatíveis requerem nova versão
- Manter compatibilidade quando possível

## 3. Tipos de Extensões

### 3.1 Extensões de Mensagem

Adicionar campos customizados ao header ou metadata:

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

### 3.2 Extensões de Contrato

Adicionar funcionalidades a contratos existentes:

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

### 3.3 Extensões de Transporte

Adicionar suporte a novos protocolos de transporte:
- WebRTC
- QUIC
- Custom protocols

### 3.4 Extensões de Serialização

Adicionar novos formatos de serialização:
- BSON
- CBOR
- Custom formats

## 4. Registro de Extensões

### 4.1 Formato de Registro

Extensões devem ser registradas com:

```yaml
extension:
  namespace: "com.example"
  name: "custom-feature"
  version: "1.0.0"
  description: "Descrição da extensão"
  author: "Nome do autor"
  contact: "contato@example.com"
  specification: "URL para especificação"
  compatibility:
    usp-version: ">=1.0.0"
  dependencies: []
```

### 4.2 Registry de Extensões

Manter um registry centralizado de extensões:
- Facilita descoberta
- Evita duplicação
- Promove reuso

## 5. Descoberta de Extensões

### 5.1 Negociação

Durante o handshake, serviços podem anunciar extensões suportadas:

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

### 5.2 Capability Discovery

Clientes podem consultar capacidades de extensão:

```json
{
  "header": {
    "type": "request",
    "method": "getCapabilities"
  },
  "payload": {
    "extensions": true
  }
}
```

## 6. Exemplos de Extensões

### 6.1 Extensão de Cache

Adicionar suporte a cache de respostas:

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

### 6.2 Extensão de Observabilidade

Adicionar métricas e tracing:

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

### 6.3 Extensão de Transações

Suporte a transações distribuídas:

```yaml
extension:
  namespace: "usp.extensions"
  name: "transactions"
  version: "1.0.0"
  features:
    - two-phase commit
    - saga pattern
    - compensation
```

## 7. Criação de Extensões

### 7.1 Processo

1. **Definir Necessidade**: Identificar funcionalidade faltante
2. **Design**: Projetar a extensão
3. **Documentar**: Criar especificação completa
4. **Implementar**: Desenvolver a extensão
5. **Registrar**: Adicionar ao registry
6. **Publicar**: Tornar disponível para comunidade

### 7.2 Boas Práticas

- Usar namespaces descritivos
- Documentar completamente
- Fornecer exemplos
- Manter compatibilidade
- Versionar adequadamente

## 8. Extensões Padrão

### 8.1 Extensões Core

Extensões mantidas pelo projeto USP:

- `usp.core:health-check`: Health checks padronizados
- `usp.core:metrics`: Métricas básicas
- `usp.core:logging`: Logging estruturado

### 8.2 Extensões da Comunidade

Extensões desenvolvidas pela comunidade:
- Registradas no registry
- Seguem padrões do projeto
- Mantidas pela comunidade

## 9. Compatibilidade

### 9.1 Backward Compatibility

- Extensões devem ser adicionadas, não removidas
- Campos opcionais podem ser adicionados
- Novos métodos/eventos são compatíveis

### 9.2 Forward Compatibility

- Implementações devem ignorar extensões desconhecidas
- Não falhar ao encontrar campos não reconhecidos
- Permitir graceful degradation

## 10. Versionamento de Extensões

### 10.1 Semantic Versioning

Extensões seguem semantic versioning:
- **MAJOR**: Breaking changes
- **MINOR**: Novas funcionalidades compatíveis
- **PATCH**: Correções de bugs

### 10.2 Deprecação

- Deprecar antes de remover
- Fornecer período de transição
- Documentar migração

---

**Versão**: 1.0.0-draft  
**Última Atualização**: Janeiro 2026

---

**English Version**: [Extensions](./extensions.en.md)
