# USP Contracts - Contratos de Serviço

**Language**: [🇧🇷 Português](#1-introdução) | [🇺🇸 English](./contracts.en.md)

## 1. Introdução

Contratos definem as interfaces formais entre serviços e clientes no USP. Um contrato especifica os métodos disponíveis, eventos publicados, tipos de dados e requisitos de segurança.

## 2. Estrutura de Contrato

### 2.1 Formato Base

Um contrato USP é definido em formato YAML ou JSON:

```yaml
contract:
  name: "UserService"
  version: "1.0.0"
  namespace: "com.example"
  description: "Serviço de gerenciamento de usuários"
  
  methods:
    - name: "createUser"
      description: "Cria um novo usuário"
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
          message: "Email inválido"
        - code: "DUPLICATE_USER"
          message: "Usuário já existe"
      authentication: required
      authorization:
        roles: ["admin", "user-manager"]
    
    - name: "getUser"
      description: "Obtém informações de um usuário"
      input:
        type: "object"
        properties:
          id:
            type: "string"
            required: true
      output:
        type: "object"
        properties:
          id: "string"
          name: "string"
          email: "string"
      errors:
        - code: "USER_NOT_FOUND"
          message: "Usuário não encontrado"
      authentication: required
  
  events:
    - name: "userCreated"
      description: "Evento disparado quando um usuário é criado"
      payload:
        type: "object"
        properties:
          userId: "string"
          timestamp: "string"
          format: "date-time"
    
    - name: "userUpdated"
      description: "Evento disparado quando um usuário é atualizado"
      payload:
        type: "object"
        properties:
          userId: "string"
          changes: "object"
          timestamp: "string"
  
  types:
    - name: "User"
      description: "Representa um usuário no sistema"
      definition:
        type: "object"
        properties:
          id:
            type: "string"
            format: "uuid"
          name:
            type: "string"
          email:
            type: "string"
            format: "email"
          createdAt:
            type: "string"
            format: "date-time"
          updatedAt:
            type: "string"
            format: "date-time"
```

## 3. Métodos

### 3.1 Definição de Método

Um método define uma operação que pode ser invocada em um serviço:

- **name**: Nome único do método
- **description**: Descrição do que o método faz
- **input**: Schema de entrada (parâmetros)
- **output**: Schema de saída (retorno)
- **errors**: Lista de erros possíveis
- **authentication**: Requisitos de autenticação
- **authorization**: Requisitos de autorização

### 3.2 Tipos de Método

#### 3.2.1 Query
Métodos que apenas leem dados (idempotentes):
- Não modificam estado
- Podem ser cacheados
- Devem ser seguros para retry

#### 3.2.2 Command
Métodos que modificam estado:
- Podem ter efeitos colaterais
- Devem ser tratados com cuidado em retries
- Podem requerer idempotência

### 3.3 Validação

Todos os inputs devem ser validados contra o schema definido antes do processamento.

## 4. Eventos

### 4.1 Definição de Evento

Eventos representam ocorrências no sistema que podem ser publicados e consumidos:

- **name**: Nome único do evento
- **description**: Descrição do evento
- **payload**: Schema do payload do evento
- **version**: Versão do evento (para evolução)

### 4.2 Publicação de Eventos

Eventos são publicados de forma assíncrona e podem ter múltiplos subscribers.

### 4.3 Versionamento de Eventos

Eventos devem ser versionados para permitir evolução sem quebrar consumidores existentes.

## 5. Tipos de Dados

### 5.1 Schemas

Tipos de dados são definidos usando schemas (JSON Schema, Avro, Protocol Buffers, etc.):

- Tipos primitivos: string, number, boolean, null
- Tipos complexos: object, array
- Tipos customizados: referências a outros tipos

### 5.2 Validação

Schemas devem incluir:
- Tipos de dados
- Validações (required, format, constraints)
- Documentação
- Exemplos

## 6. Autenticação e Autorização

### 6.1 Níveis de Autenticação

- **none**: Sem autenticação
- **optional**: Autenticação opcional
- **required**: Autenticação obrigatória

### 6.2 Autorização

Autorização pode ser especificada através de:
- **roles**: Lista de roles permitidas
- **permissions**: Lista de permissões específicas
- **policies**: Políticas customizadas

## 7. Versionamento de Contratos

### 7.1 Compatibilidade

Contratos devem manter compatibilidade:
- Adicionar novos métodos é compatível
- Adicionar novos campos opcionais é compatível
- Remover campos ou métodos é incompatível
- Alterar tipos de campos é incompatível

### 7.2 Estratégias de Versionamento

1. **Versionamento de Contrato**: Versão completa do contrato
2. **Versionamento de Método**: Versão individual de métodos
3. **Versionamento de Evento**: Versão individual de eventos

## 8. Contratos Padrão

### 8.1 Health Check

Todo serviço deve implementar um contrato básico de health check:

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

### 8.2 Service Discovery

Contrato para descoberta de serviços:

```yaml
contract:
  name: "ServiceDiscovery"
  version: "1.0.0"
  namespace: "usp.core"
  methods:
    - name: "listServices"
      input:
        type: "object"
        properties:
          filter: "object"
      output:
        type: "array"
        items:
          type: "object"
          properties:
            serviceId: "string"
            contracts: "array"
            endpoints: "array"
```

## 9. Boas Práticas

### 9.1 Design de Contratos

- Use nomes descritivos e consistentes
- Documente todos os métodos e eventos
- Forneça exemplos de uso
- Defina claramente os erros possíveis
- Especifique requisitos de autenticação/autorização

### 9.2 Versionamento

- Use versionamento semântico
- Documente breaking changes
- Mantenha compatibilidade quando possível
- Forneça migração paths

### 9.3 Performance

- Minimize o tamanho de payloads
- Use paginação para listas grandes
- Considere cache para queries frequentes
- Otimize schemas para serialização

---

**Versão**: 1.0.0-draft  
**Última Atualização**: Janeiro 2026

---

**English Version**: [Service Contracts](./contracts.en.md)
