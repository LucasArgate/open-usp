# Universal Services Protocol (USP)

### *Bridging the Gap Between AI Agents and the Real World of Services*

**Language**: [🇧🇷 Português](./ABSTRACT.md) | [🇺🇸 English](#1-abstract)

## 1. Abstract

We are entering the **Agentic Era** of the internet. Large Language Models (LLMs) are evolving from text generators into autonomous agents capable of executing tasks. While protocols like Google's UCP (Universal Commerce Protocol) are effectively solving the problem for **Retail** (static products, standard shipping, fixed prices), there is a massive vacuum in the **Service Economy**.

Services are not products. You cannot "add to cart" a leaking pipe repair, a medical consultation, or a custom safety net installation without context. Services depend on **Time**, **Geography**, and **Dynamic Logic**.

The **Universal Services Protocol (USP)** is an open standard proposal to define how AI Agents discover, negotiate, quote, and book services in the real world. It provides the structured "handshake" necessary for a legacy SQL database from 1991 to transact natively with a futuristic AI Agent in 2026.

---

## 2. The "Retail Fallacy" in AI

Current commerce protocols operate under what we call the "Retail Fallacy." They assume:

1. **Price is Static:** An item costs $X.
2. **Inventory is Quantitative:** There are Y units in the warehouse.
3. **Logistics is Generic:** Shipping costs are calculated *after* the purchase based on weight.

**In the Service Industry, these assumptions fail:**

1. **Price is a Function:** The cost of a service is rarely fixed. It is a function of complexity, urgency, materials, and context. (e.g., *Installing a network on the 1st floor is cheaper than on the 10th floor*).
2. **Inventory is Temporal:** A service provider's inventory is their **Calendar**. "Sold out" doesn't mean empty shelves; it means "No available slots."
3. **Geography is a Constraint:** A plumber in New York is useless to a client in São Paulo. Serviceability is binary based on geofencing, not just shipping costs.

The lack of a standardized protocol for these variables forces AI Agents today to rely on "brute force" methods: scraping websites, making phone calls (via voice synthesis), or hallucinations. **USP aims to solve this by making Service Logic machine-readable.**

---

## 3. Core Philosophy: The 4 Pillars of USP

To successfully digitize the service economy for AI, USP is built upon four non-negotiable pillars. Any implementation of USP must respect these primitives.

### I. Geography as a Constraint (Serviceability Layer)

In retail, if a user wants a product, the question is "How much is shipping?". In services, the question is "Can you even do it?".

USP defines **Serviceability** as the first gate. Before any quote is generated, the Agent must validate the `UserLocation` against the Provider's `ServicePolygon`.

* *Legacy Compatibility:* This allows providers to map complex logic (e.g., "I serve Campinas, but not the rural area") into a simple boolean response for the AI.

### II. Price as a Function (Dynamic Quoting Layer)

USP rejects the notion of a "Price List" for complex services. Instead, it standardizes the **Quoting Interface**.

The Provider exposes a schema of `RequiredInputs` (e.g., square meters, wall type, floor number). The Agent collects these inputs and submits them to the Provider's endpoint. The logic—whether it's a simple multiplication or a query to a 30-year-old SQL stored procedure—happens on the Provider's side (The Black Box), returning a deterministic quote.

### III. Inventory is Time (Availability Layer)

Services are perishable. An unsold hour today cannot be sold tomorrow.

USP treats **Time Slots** as SKUs. The protocol defines a standard way to query availability windows (`GetSlots`), hold them (`SoftReserve`), and commit them (`Book`). This eliminates the "double-booking" problem common in asynchronous AI negotiations.

### IV. Contextual Idempotency

Because service negotiations can be long and multi-turn, USP requires transactions to be stateful and idempotent. If an Agent requests a quote for the same parameters twice, it should receive the same `QuoteID`, ensuring that price fluctuations do not occur mid-negotiation unless explicitly expired.

---

## 4. Architecture & Protocol Specification

The USP is designed to be transport-agnostic (it can run over HTTP/REST, gRPC, or even WebSockets), but the reference implementation focuses on **JSON over HTTP**.

The interaction flow follows the **D-Q-B Cycle**: *Discovery -> Quoting -> Booking*.

### 4.1. Discovery & Serviceability (`/check`)

The entry point. The Agent asks: "Do you perform Service X at Location Y?"

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

**Response (Success):**

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

**Response (Failure):**

```json
{
  "status": "OUT_OF_AREA",
  "message": "We currently do not serve the requested location."
}
```

### 4.2. The Metadata Handshake (Requirements)

Before quoting, the Provider tells the Agent what it *needs* to know. This is crucial for LLMs, as it provides the "System Prompt" instructions for data collection.

**Endpoint:** `GET /usp/v1/requirements/{service_category}`

**Response:**

```json
{
  "required_fields": [
    {
      "key": "building_type",
      "type": "enum",
      "options": ["house", "apartment"],
      "description": "Is it a house or an apartment building?"
    },
    {
      "key": "protection_area",
      "type": "string",
      "format": "dimensions",
      "description": "Approximate dimensions (e.g., 3x2m) or 'unknown'"
    },
    {
      "key": "photo_url",
      "type": "file",
      "optional": true,
      "description": "Photo of the window/balcony"
    }
  ]
}
```

### 4.3. Dynamic Quoting (`/quote`)

The heart of USP. This is where legacy systems shine. The Agent submits the data, and the Provider's backend (C#, Java, Python) runs the proprietary math.

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

**Response (Scenario A: Definitive Quote):**

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

**Response (Scenario B: Estimation / Lead Gen):**
*Used when the legacy system doesn't have enough data or the logic is too complex for auto-quoting.*

```json
{
  "quote_id": "lead_7722",
  "type": "ESTIMATE",
  "price_range": {
    "min": 700.00,
    "max": 900.00,
    "currency": "BRL"
  },
  "message": "Based on historical data for this building, prices vary. A technical visit is required.",
  "next_action": "BOOK_VISIT" // Instead of BOOK_SERVICE
}
```

### 4.4. Inventory & Booking (`/slots` & `/book`)

Standardized calendar management.

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

## 5. Use Case: The "Legacy Wrapper" Pattern

A major goal of USP is to enable established businesses (Service SMBs) to participate in the AI economy without rewriting their core software.

Consider a company, **"Example Services"**, running a custom ERP built in C# (MVC 4) with a SQL Server database containing data from 1991.

**Without USP:**
An AI Agent (like Gemini or ChatGPT) cannot help a user hire Example Services. It can only summarize the website text. It cannot know if the technician is available or calculate the price based on the building's history.

**With USP:**
Example Services implements a lightweight "Sidecar API" (The USP Adapter) that translates JSON requests into SQL queries.

1. **Agent:** "Quote for Building Solar, Campinas."
2. **USP Adapter:** Receives request.
3. **Legacy Logic:** Executes `SELECT * FROM Predios WHERE Nome = 'Solar'`. Finds historical measurements (Varanda: 12m²).
4. **USP Adapter:** Returns exact price JSON based on pre-stored measurements.
5. **Result:** The User gets an instant, bookable price via the AI interface, bypassing the need for a phone call or manual measurement.

This pattern turns **Data Gravity** (years of accumulated business knowledge) into a competitive moat in the age of AI.

---

## 6. Security & Trust (Security-by-Design)

Opening service quoting to autonomous agents requires robust safeguards. USP implements a "Defense-in-Depth" layer:

1.  **Capabilities Negotiation (The Handshake):** Permissions are dynamically negotiated. Untrusted agents operate in limited scopes.
2.  **Draft Mode (Dry Runs):** Native support for "SIMULATION" execution mode, preventing real-world side effects during planning.
3.  **Semantic Rate Limiting (Risk Budget):** Limiting based on *Business Risk* (Currency value) rather than just request volume.
4.  **Verifiable Identity:** Agents must sign requests with Identity Certificates (`Souls.md`) to access sensitive contexts.
5.  **Kill Switch:** Standardized `SIGTERM` protocol for immediate Agent revocation.

For the full security specification, see [SECURITY.md](../SECURITY.md).

---

## 7. Why Open Source? Why Brazil?

### The Brazilian Thesis

Brazil is the perfect sandbox for USP. It is a country with:

* **High Complexity:** Logistics, taxes, and service variability are extreme.
* **Conversational Culture:** Brazil is the world leader in WhatsApp commerce. Users are already trained to buy services via chat.
* **Legacy Infrastructure:** Thousands of robust SMBs operate on custom legacy software that works perfectly but is disconnected from the modern AI stack.

By developing USP as an Open Source standard in Brazil, we create a robust protocol tested in "Hard Mode." If it works for a chaotic service environment in São Paulo, it works anywhere.

### Contributing

We are looking for:

* **Architects:** To refine the JSON-LD schemas and OpenAPI specs.
* **Service Providers:** To test the protocol on real-world businesses (Plumbers, Electricians, Installers, Doctors).
* **AI Engineers:** To build the "Client Side" (Custom GPTs, LangChain tools) that consume USP APIs.

---

## 8. Roadmap

* **v0.1 (Current):** Definition of the Manifesto and Core Primitives.
* **v0.2:** Release of the OpenAPI (Swagger) Reference Specification.
* **v0.3:** Release of the "USP Adapter" Reference Implementation (Node.js & .NET Core).
* **v1.0:** First live production transaction between a major LLM and a local service provider.

---

## 9. License & Governance

USP is released under the **MIT License**. It is designed to be a community-driven standard, not a product of a single corporation.

* **Maintainer:** [Your GitHub Profile / Organization]
* **Initial Contributor:** Open Source Community Tech Teams

---

### *A Note to Developers*

*We are not building a marketplace. We are building the language that allows marketplaces and agents to talk to the real world. Join us in building the API for the Service Economy.*

---

## 10. Example: Full Transaction Flow

*Below is a simplified example of a full conversation between an Agent and a USP-compliant API.*

**User:** "I need a safety net for my balcony. I live at Rua Ary Monteiro, 246."

**1. Agent checks Serviceability:**
`POST /usp/check { "address": "Rua Ary Monteiro 246..." }`
`<= 200 OK { "status": "OK", "zone": "Campinas_North" }`

**2. Agent asks for Quote:**
`POST /usp/quote { "address": "...", "service": "net_install" }`
`<= 200 OK { "price": 450.00, "currency": "BRL", "valid_id": "q_123" }`

**3. Agent lists Availability:**
`GET /usp/slots?date=2026-02-01`
`<= 200 OK { "slots": ["10:00", "15:00"] }`

**4. User selects 10:00. Agent Books:**
`POST /usp/book { "quote_id": "q_123", "slot": "10:00", "customer": {...} }`
`<= 201 Created { "order_id": "ORD-999", "status": "CONFIRMED" }`

**User Result:** "Done. Example Services will be there on Feb 1st at 10:00 AM. The cost is R$ 450.00."

---

**Version**: 1.0.0-draft  
**Last Update**: January 2026
