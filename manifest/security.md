# USP Security - Segurança

**Language**: [🇧🇷 Português](#1-introdução) | [🇺🇸 English](./security.en.md)

## 1. Introdução

Este documento define os requisitos e mecanismos de segurança do Universal Service Protocol (USP), incluindo autenticação, autorização, criptografia e boas práticas.

## 2. Princípios de Segurança

### 2.1 Security by Design

Segurança é uma preocupação fundamental desde o design do protocolo:
- Todas as comunicações devem ser criptografadas em produção
- Autenticação é obrigatória para operações sensíveis
- Autorização baseada em princípios de menor privilégio
- Auditoria e logging de operações críticas

### 2.2 Defense in Depth

Múltiplas camadas de segurança:
- Transporte seguro (TLS/SSL)
- Autenticação de mensagens
- Autorização baseada em roles/permissions
- Validação de entrada
- Rate limiting

## 3. Autenticação

### 3.1 Métodos de Autenticação

O USP suporta múltiplos métodos de autenticação:

#### 3.1.1 API Keys
- Chaves simples para autenticação básica
- Enviadas no header `Authorization: Bearer <api-key>`
- Adequadas para comunicação serviço-a-serviço

#### 3.1.2 OAuth 2.0
- Padrão da indústria para autorização
- Suporta diferentes flows (client credentials, authorization code, etc.)
- Tokens de acesso com expiração

#### 3.1.3 JWT (JSON Web Tokens)
- Tokens auto-contidos com claims
- Assinatura digital para verificação
- Suporta refresh tokens

#### 3.1.4 mTLS (Mutual TLS)
- Autenticação mútua baseada em certificados
- Alta segurança para comunicação serviço-a-serviço
- Requer infraestrutura de PKI

#### 3.1.5 Custom Authentication
- Permite implementações customizadas
- Deve seguir padrões de segurança estabelecidos

### 3.2 Header de Autenticação

Credenciais são enviadas no header da mensagem:

```json
{
  "header": {
    "authorization": {
      "type": "Bearer",
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
    }
  }
}
```

Ou no header HTTP (quando aplicável):
```
Authorization: Bearer <token>
```

### 3.3 Validação de Token

- Tokens devem ser validados antes do processamento
- Tokens expirados devem ser rejeitados
- Tokens inválidos devem retornar erro `usp.core:UNAUTHORIZED`

## 4. Autorização

### 4.1 Modelos de Autorização

#### 4.1.1 Role-Based Access Control (RBAC)
- Usuários/serviços têm roles
- Roles têm permissões associadas
- Acesso baseado em roles do usuário

#### 4.1.2 Permission-Based
- Permissões granulares por recurso/ação
- Mais flexível que RBAC
- Permite controle fino

#### 4.1.3 Policy-Based
- Políticas customizadas
- Permite lógica complexa de autorização
- Baseado em atributos e contexto

### 4.2 Verificação de Autorização

Autorização é verificada após autenticação:
1. Extrair claims/permissões do token
2. Verificar se o usuário/serviço tem permissão para a operação
3. Rejeitar com `usp.core:FORBIDDEN` se não autorizado

## 5. Criptografia

### 5.1 Criptografia em Trânsito

**TLS/SSL obrigatório em produção**:
- TLS 1.2 mínimo (TLS 1.3 recomendado)
- Cipher suites seguros
- Certificados válidos e confiáveis
- Validação de certificados

### 5.2 Criptografia em Repouso

Dados sensíveis devem ser criptografados quando armazenados:
- Chaves de criptografia gerenciadas de forma segura
- Algoritmos fortes (AES-256, etc.)
- Rotação de chaves

### 5.3 Assinatura Digital

Mensagens críticas podem ser assinadas digitalmente:
- Garante integridade
- Garante autenticidade
- Previne tampering

## 6. Validação de Entrada

### 6.1 Sanitização

Toda entrada deve ser sanitizada:
- Remover caracteres perigosos
- Validar tipos de dados
- Limitar tamanho de campos
- Prevenir injection attacks

### 6.2 Schema Validation

Payloads devem ser validados contra schemas:
- Tipos de dados corretos
- Campos obrigatórios presentes
- Formatos válidos (email, UUID, etc.)
- Constraints respeitados

## 7. Rate Limiting

### 7.1 Proteção contra Abuso

Rate limiting protege contra:
- DDoS attacks
- Abuse de API
- Resource exhaustion

### 7.2 Implementação

- Limites por cliente/serviço
- Limites por endpoint
- Limites globais
- Headers de rate limit (X-RateLimit-*)

## 8. Logging e Auditoria

### 8.1 Logging de Segurança

Registrar eventos de segurança:
- Tentativas de autenticação (sucesso/falha)
- Acessos negados
- Operações sensíveis
- Erros de segurança

### 8.2 Auditoria

Manter logs de auditoria para:
- Compliance
- Investigação de incidentes
- Análise de comportamento

### 8.3 Informações Sensíveis

**NÃO** logar:
- Senhas
- Tokens completos
- Dados pessoais sensíveis
- Chaves de criptografia

## 9. Headers de Segurança

### 9.1 Headers HTTP

Quando usando HTTP, incluir headers de segurança:
- `Strict-Transport-Security`: Força HTTPS
- `X-Content-Type-Options`: Previne MIME sniffing
- `X-Frame-Options`: Previne clickjacking
- `Content-Security-Policy`: Política de segurança de conteúdo

## 10. Boas Práticas

### 10.1 Desenvolvimento

- Nunca commitar credenciais no código
- Usar variáveis de ambiente para secrets
- Validar todas as entradas
- Usar prepared statements/queries parametrizadas
- Atualizar dependências regularmente

### 10.2 Operações

- Rotacionar credenciais regularmente
- Monitorar tentativas de acesso
- Implementar alertas de segurança
- Fazer backup seguro de dados
- Ter plano de resposta a incidentes

### 10.3 Certificados

- Usar certificados válidos
- Monitorar expiração de certificados
- Implementar renovação automática
- Validar cadeia de certificados

## 11. Vulnerabilidades Comuns

### 11.1 OWASP Top 10

Proteger contra vulnerabilidades comuns:
- Injection
- Broken Authentication
- Sensitive Data Exposure
- XML External Entities (XXE)
- Broken Access Control
- Security Misconfiguration
- XSS (Cross-Site Scripting)
- Insecure Deserialization
- Using Components with Known Vulnerabilities
- Insufficient Logging & Monitoring

## 12. Compliance

### 12.1 Regulamentações

Considerar compliance com:
- GDPR (General Data Protection Regulation)
- LGPD (Lei Geral de Proteção de Dados)
- HIPAA (para saúde)
- PCI-DSS (para pagamentos)

### 12.2 Privacy by Design

- Minimizar coleta de dados
- Anonimizar quando possível
- Permitir exclusão de dados
- Transparência sobre uso de dados

---

**Versão**: 1.0.0-draft  
**Última Atualização**: Janeiro 2026

---

**English Version**: [Security](./security.en.md)
