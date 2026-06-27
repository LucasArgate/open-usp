# Universal Services Protocol (USP)

### *Bridging the Gap Between AI Agents and the Real World of Services*

**Language**: [🇧🇷 Português](#1-abstract) | [🇺🇸 English](./ABSTRACT.en.md)

## 1. Abstract

Estamos entrando na **Era Agêntica** da internet. Large Language Models (LLMs) estão evoluindo de geradores de texto para agentes autônomos capazes de executar tarefas. Enquanto protocolos como o UCP (Universal Commerce Protocol) do Google estão resolvendo efetivamente o problema para o **Varejo** (produtos estáticos, envio padrão, preços fixos), há um grande vácuo na **Economia de Serviços**.

Serviços não são produtos. Você não pode "adicionar ao carrinho" um reparo de tubulação com vazamento, uma consulta médica ou uma instalação de rede de segurança personalizada sem contexto. Serviços dependem de **Tempo**, **Geografia** e **Lógica Dinâmica**.

O **Universal Services Protocol (USP)** é uma proposta de padrão aberto para definir como Agentes de IA descobrem, negociam, cotam e reservam serviços no mundo real. Ele fornece o "handshake" estruturado necessário para que um banco de dados SQL legado de 1991 transacione nativamente com um Agente de IA futurista em 2026.

> **Posição na holarquia universal `u.*`.** O USP é a **esfera de serviços** (`u.service`) — um hólon autossuficiente e, ao mesmo tempo, o **tecido de execução/auditoria** que as demais esferas (UHP, UDP, UGP, UWP, UEP) reutilizam. Como "tudo vem de serviços", o USP é primordial; mas isso **não** o torna a raiz do namespace: ele é um par entre esferas. O campo comum a todas (erros, observabilidade, Ledger) vive em `u.core`. Ver o [mapa de protocolos](https://github.com/LucasArgate/uhp/tree/main/docs/protocolos/mapa-protocolos.md).

---

## 2. A "Falácia do Varejo" na IA

Os protocolos de comércio atuais operam sob o que chamamos de "Falácia do Varejo". Eles assumem:

1. **Preço é Estático:** Um item custa $X.
2. **Estoque é Quantitativo:** Existem Y unidades no estoque.
3. **Logística é Genérica:** Custos de envio são calculados *após* a compra com base no peso.

**Na Indústria de Serviços, essas suposições falham:**

1. **Preço é uma Função:** O custo de um serviço raramente é fixo. É uma função de complexidade, urgência, materiais e contexto. (ex: *Instalar uma rede no 1º andar é mais barato que no 10º andar*).
2. **Estoque é Temporal:** O estoque de um provedor de serviços é seu **Calendário**. "Esgotado" não significa prateleiras vazias; significa "Sem horários disponíveis".
3. **Geografia é uma Restrição:** Um encanador em Nova York é inútil para um cliente em São Paulo. A capacidade de serviço é binária com base em geofencing, não apenas custos de envio.

A falta de um protocolo padronizado para essas variáveis força os Agentes de IA hoje a depender de métodos de "força bruta": scraping de websites, fazer ligações telefônicas (via síntese de voz) ou alucinações. **O USP visa resolver isso tornando a Lógica de Serviços legível por máquina.**

---

## 3. Filosofia Central: Os 4 Pilares do USP

Para digitalizar com sucesso a economia de serviços para IA, o USP é construído sobre quatro pilares não negociáveis. Qualquer implementação do USP deve respeitar esses primitivos.

### I. Geografia como Restrição (Camada de Capacidade de Serviço)

No varejo, se um usuário quer um produto, a pergunta é "Quanto custa o envio?". Em serviços, a pergunta é "Você consegue fazer isso?".

O USP define **Capacidade de Serviço (Serviceability)** como o primeiro portão. Antes de qualquer cotação ser gerada, o Agente deve validar a `UserLocation` contra o `ServicePolygon` do Provedor.

* *Compatibilidade com Legados:* Isso permite que provedores mapeiem lógica complexa (ex: "Atendo Campinas, mas não a área rural") em uma resposta booleana simples para a IA.

### II. Preço como Função (Camada de Cotação Dinâmica)

O USP rejeita a noção de uma "Lista de Preços" para serviços complexos. Em vez disso, padroniza a **Interface de Cotação**.

O Provedor expõe um schema de `RequiredInputs` (ex: metros quadrados, tipo de parede, número do andar). O Agente coleta essas entradas e as submete ao endpoint do Provedor. A lógica—seja uma multiplicação simples ou uma consulta a um stored procedure SQL de 30 anos—acontece no lado do Provedor (A Caixa Preta), retornando uma cotação determinística.

### III. Estoque é Tempo (Camada de Disponibilidade)

Serviços são perecíveis. Uma hora não vendida hoje não pode ser vendida amanhã.

O USP trata **Horários (Time Slots)** como SKUs. O protocolo define uma forma padrão de consultar janelas de disponibilidade (`GetSlots`), reservá-las (`SoftReserve`), e confirmá-las (`Book`). Isso elimina o problema de "dupla reserva" comum em negociações assíncronas de IA.

### IV. Idempotência Contextual

Como negociações de serviços podem ser longas e multi-turno, o USP exige que transações sejam stateful e idempotentes. Se um Agente solicita uma cotação para os mesmos parâmetros duas vezes, deve receber o mesmo `QuoteID`, garantindo que flutuações de preço não ocorram no meio da negociação, a menos que explicitamente expiradas.

---

## 4. Arquitetura & Especificação do Protocolo

O USP é projetado para ser agnóstico ao transporte (pode rodar sobre HTTP/REST, gRPC ou até WebSockets), mas a implementação de referência foca em **JSON sobre HTTP**.

O fluxo de interação segue o **Ciclo D-Q-B**: *Discovery -> Quoting -> Booking*.

### 4.1. Descoberta & Capacidade de Serviço (`/check`)

O ponto de entrada. O Agente pergunta: "Você realiza o Serviço X no Local Y?"

**Request:**

```json
POST /usp/v1/serviceability
{
  "service_category": "safety_net_installation",
  "location": {
    "latitude": -22.9099,
    "longitude": -47.0626,
    "address_components": {
      "city": "Campinas",
      "state": "SP",
      "country": "BR"
    }
  }
}
```

**Response (Sucesso):**

```json
{
  "status": "SERVICABLE",
  "service_area_id": "campinas_metro",
  "travel_fee": {
    "amount": 45.00,
    "currency": "BRL",
    "reason": "Distance surcharge (>10km)"
  },
  "lead_time_days": 2
}
```

**Response (Falha):**

```json
{
  "status": "OUT_OF_AREA",
  "message": "Atualmente não atendemos o local solicitado."
}
```

### 4.2. O Handshake de Metadados (Requisitos)

Antes de cotar, o Provedor diz ao Agente o que ele *precisa* saber. Isso é crucial para LLMs, pois fornece as instruções do "System Prompt" para coleta de dados.

**Endpoint:** `GET /usp/v1/requirements/{service_category}`

**Response:**

```json
{
  "required_fields": [
    {
      "key": "building_type",
      "type": "enum",
      "options": ["house", "apartment"],
      "description": "É uma casa ou um prédio de apartamentos?"
    },
    {
      "key": "protection_area",
      "type": "string",
      "format": "dimensions",
      "description": "Dimensões aproximadas (ex: 3x2m) ou 'desconhecido'"
    },
    {
      "key": "photo_url",
      "type": "file",
      "optional": true,
      "description": "Foto da janela/varanda"
    }
  ]
}
```

### 4.3. Cotação Dinâmica (`/quote`)

O coração do USP. É aqui que sistemas legados brilham. O Agente submete os dados, e o backend do Provedor (C#, Java, Python) executa a matemática proprietária.

**Request:**

```json
POST /usp/v1/quote
{
  "user_id": "agent_session_8829",
  "location_context": { ... },
  "inputs": {
    "building_type": "apartment",
    "protection_area": "12m2",
    "floor_level": 5
  }
}
```

**Response (Cenário A: Cotação Definitiva):**

```json
{
  "quote_id": "q_998811",
  "valid_until": "2026-02-01T23:59:59Z",
  "price_breakdown": {
    "base_service": 600.00,
    "materials": 150.00,
    "logistics": 45.00,
    "total": 795.00,
    "currency": "BRL"
  },
  "next_action": "SELECT_SLOT"
}
```

**Response (Cenário B: Estimativa / Geração de Lead):**
*Usado quando o sistema legado não tem dados suficientes ou a lógica é muito complexa para cotação automática.*

```json
{
  "quote_id": "lead_7722",
  "type": "ESTIMATE",
  "price_range": {
    "min": 700.00,
    "max": 900.00,
    "currency": "BRL"
  },
  "message": "Com base em dados históricos para este prédio, os preços variam. Uma visita técnica é necessária.",
  "next_action": "BOOK_VISIT" // Em vez de BOOK_SERVICE
}
```

### 4.4. Estoque & Reserva (`/slots` & `/book`)

Gerenciamento de calendário padronizado.

**Request:** `GET /usp/v1/slots?date=2026-02-10&duration_minutes=120`

**Response:**

```json
{
  "available_slots": [
    { "start_time": "09:00", "end_time": "11:00", "slot_id": "s_101" },
    { "start_time": "14:00", "end_time": "16:00", "slot_id": "s_102" }
  ]
}
```

---

## 5. Caso de Uso: O Padrão "Legacy Wrapper"

Um objetivo principal do USP é permitir que empresas estabelecidas (SMBs de Serviços) participem da economia de IA sem reescrever seu software principal.

Considere uma empresa, **"Exemplo Serviços"**, rodando um ERP customizado construído em C# (MVC 4) com um banco de dados SQL Server contendo dados de 1991.

**Sem USP:**
Um Agente de IA (como Gemini ou ChatGPT) não pode ajudar um usuário a contratar a Exemplo Serviços. Ele só pode resumir o texto do website. Ele não pode saber se o técnico está disponível ou calcular o preço com base no histórico do prédio.

**Com USP:**
A Exemplo Serviços implementa uma leve "Sidecar API" (O Adaptador USP) que traduz requisições JSON em consultas SQL.

1. **Agente:** "Cotação para Prédio Solar, Campinas."
2. **Adaptador USP:** Recebe a requisição.
3. **Lógica Legada:** Executa `SELECT * FROM Predios WHERE Nome = 'Solar'`. Encontra medições históricas (Varanda: 12m²).
4. **Adaptador USP:** Retorna JSON de preço exato baseado em medições pré-armazenadas.
5. **Resultado:** O Usuário obtém um preço instantâneo e reservável via interface de IA, contornando a necessidade de uma ligação telefônica ou medição manual.

Este padrão transforma **Gravidade de Dados** (anos de conhecimento de negócios acumulado) em uma vantagem competitiva na era da IA.

---

## 6. Segurança & Confiança (Security-by-Design)

Abrir cotação de serviços para agentes autônomos requer salvaguardas robustas. O USP implementa uma camada de "Defesa em Profundidade":

1.  **Handshake de Permissões (Capabilities Negotiation):** Permissões são negociadas dinamicamente entre Agente e Servidor. Agentes não verificados recebem escopos limitados.
2.  **Modo Rascunho (Draft Mode):** Suporte nativo para execuções simuladas ("Dry Runs") sem efeitos colaterais no banco de dados.
3.  **Rate Limiting Semântico (Risk Budget):** Limites baseados no *Impacto de Negócio* (Risco financeiro) e não apenas volume de requisições.
4.  **Identidade Verificável:** Integração com certificados de identidade para agentes confiáveis.
5.  **Kill Switch:** Padrão para revogação imediata de acesso em emergências.

Para a especificação completa de segurança, consulte [SECURITY.md](../SECURITY.md).

---

## 7. Por que Open Source? Por que Brasil?

### A Tese Brasileira

O Brasil é o sandbox perfeito para o USP. É um país com:

* **Alta Complexidade:** Logística, impostos e variabilidade de serviços são extremos.
* **Cultura Conversacional:** O Brasil é líder mundial em comércio via WhatsApp. Usuários já estão treinados para comprar serviços via chat.
* **Infraestrutura Legada:** Milhares de SMBs robustas operam em software legado customizado que funciona perfeitamente mas está desconectado da stack moderna de IA.

Ao desenvolver o USP como um padrão Open Source no Brasil, criamos um protocolo robusto testado em "Modo Difícil". Se funciona para um ambiente de serviços caótico em São Paulo, funciona em qualquer lugar.

### Contribuindo

Estamos procurando por:

* **Arquitetos:** Para refinar os schemas JSON-LD e especificações OpenAPI.
* **Provedores de Serviços:** Para testar o protocolo em negócios do mundo real (Encanadores, Eletricistas, Instaladores, Médicos).
* **Engenheiros de IA:** Para construir o "Lado Cliente" (Custom GPTs, ferramentas LangChain) que consomem APIs USP.

---

## 8. Roadmap

* **v0.1 (Atual):** Definição do Manifesto e Primitivos Centrais.
* **v0.2:** Lançamento da Especificação de Referência OpenAPI (Swagger).
* **v0.3:** Lançamento da Implementação de Referência "USP Adapter" (Node.js & .NET Core).
* **v1.0:** Primeira transação em produção ao vivo entre um LLM major e um provedor de serviços local.

---

## 9. Licença & Governança

O USP é lançado sob a **Licença MIT**. É projetado para ser um padrão dirigido pela comunidade, não um produto de uma única corporação.

* **Mantenedor:** [Seu Perfil GitHub / Organização]
* **Contribuidor Inicial:** Equipes de Tecnologia da Comunidade Open Source

---

### *Uma Nota para Desenvolvedores*

*Não estamos construindo um marketplace. Estamos construindo a linguagem que permite que marketplaces e agentes conversem com o mundo real. Junte-se a nós na construção da API para a Economia de Serviços.*

---

## 10. Exemplo: Fluxo Completo de Transação

*Abaixo está um exemplo simplificado de uma conversa completa entre um Agente e uma API compatível com USP.*

**Usuário:** "Preciso de uma rede de segurança para minha varanda. Moro na Rua Ary Monteiro, 246."

**1. Agente verifica Capacidade de Serviço:**
`POST /usp/check { "address": "Rua Ary Monteiro 246..." }`
`<= 200 OK { "status": "OK", "zone": "Campinas_North" }`

**2. Agente solicita Cotação:**
`POST /usp/quote { "address": "...", "service": "net_install" }`
`<= 200 OK { "price": 450.00, "currency": "BRL", "valid_id": "q_123" }`

**3. Agente lista Disponibilidade:**
`GET /usp/slots?date=2026-02-01`
`<= 200 OK { "slots": ["10:00", "15:00"] }`

**4. Usuário seleciona 10:00. Agente Reserva:**
`POST /usp/book { "quote_id": "q_123", "slot": "10:00", "customer": {...} }`
`<= 201 Created { "order_id": "ORD-999", "status": "CONFIRMED" }`

**Resultado para o Usuário:** "Pronto. A Exemplo Serviços estará lá em 1º de fevereiro às 10:00. O custo é R$ 450,00."

---

**Versão**: 1.0.0-draft  
**Última Atualização**: Janeiro 2026

---

**English Version**: [Abstract](./ABSTRACT.en.md)
