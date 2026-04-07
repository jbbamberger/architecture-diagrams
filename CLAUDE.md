# Elastic Architecture Diagram Generator

You are helping an Elastic Solutions Architect generate a customer-facing architecture diagram showing how Elastic fits into a customer's existing workflow.

## Your Job

When the user says "generate" or "create diagram":

1. **Read every file in `customer-input/`** thoroughly — understand the customer's industry, use case, existing systems, pain points, and goals.
2. **Determine what kind of Elastic customer this is** — see Use Case Detection below.
3. **Design a flow that makes sense for their specific situation** — do not default to a fixed layout. The diagram should feel like it was built for this customer, not templated.
4. **Generate a single SVG** saved to `output/<CustomerName>_Architecture.svg`.

---

## Use Case Detection

Read the customer notes and identify the primary Elastic use case(s). A customer may have more than one.

### Search
**Signals:** e-commerce, site search, app search, product catalog, knowledge base, semantic search, RAG, recommendations, document retrieval, intranet search.
**Elastic products to emphasize:** Elasticsearch (vector + full-text), Elastic Agent Builder (search assistant/chatbot), Semantic Search, ELSER, App Search, Crawler, Workplace Search, Kibana Discover.
**Typical flow:** Data sources → Ingestion/indexing → Elasticsearch (with vector search + semantic ranking) → Search UI / App / Chatbot → End users.

### Observability
**Signals:** logs, metrics, traces, APM, infrastructure monitoring, uptime, alerting on system health, SLAs, DevOps, SRE, cloud monitoring, OpenTelemetry.
**Elastic products to emphasize:** Elastic Agent, Fleet, APM, Beats (Filebeat, Metricbeat, etc.), Logstash, Elasticsearch, Kibana (Logs, Metrics, APM, Uptime, Dashboards, Alerting), Machine Learning anomaly detection.
**Typical flow:** Infrastructure/apps → Elastic Agent / Beats / OTel → Logstash (if needed) → Elasticsearch → Kibana (Logs, Metrics, Dashboards, Alerts) → On-call / DevOps teams.

### Security / SIEM
**Signals:** threat detection, SIEM, SOC, EDR, XDR, SOAR, compliance, vulnerability management, incident response, malware, network traffic analysis, threat intelligence.
**Elastic products to emphasize:** Elastic Security, SIEM, Endpoint Security (EDR), Fleet, Elastic Agent, Detection Rules, ML anomaly detection, Cases, Kibana Security dashboards.
**Typical flow:** Endpoints/network/cloud → Elastic Agent (EDR) / log shippers → Elasticsearch → Detection Engine (rules + ML) → Alerts → Cases → Analyst response / SOAR.

### Fraud & Investigations (Government / Financial)
**Signals:** fraud detection, program integrity, claims analysis, case management, document review, investigation lifecycle, manual verification bottlenecks.
**Elastic products to emphasize:** Elasticsearch (full-text + vector), Ingest Pipelines, ML Anomaly Detection, Rules & Alerts, Agent Builder (investigative assistant), Workflows, Kibana.
**Typical flow:** Data sources (structured + unstructured) → OCR/ETL → Ingest Pipelines → Elasticsearch → ML + Rules → Agent Builder + Workflows → Actions/output.

### Other / Hybrid
If the customer doesn't fit cleanly into one bucket, combine elements. Design the flow around what they actually do — don't force a category.

---

## Diagram Design Principles

**Design the flow first, then draw it.**

Before placing any boxes, ask:
- What data does this customer have and where does it come from?
- What is Elastic replacing or augmenting?
- What is the output / end state the customer cares about?
- Where are the manual bottlenecks Elastic can automate?
- Who are the end users of this system (analysts, developers, customers, SOC teams)?

**The flow should read like a story** — left to right (or top to bottom if that makes more sense), starting with raw inputs and ending with outcomes the customer cares about.

**Common layout patterns:**

- **Left-to-right linear:** Sources → Ingest → Elastic → Output. Good for most use cases.
- **Hub and spoke:** Elasticsearch in the center with data sources feeding in and multiple output surfaces radiating out. Good for observability or search powering many apps.
- **Layered (top to bottom):** Data collection layer → Processing layer → Analytics/detection layer → Action layer. Good for security/SIEM.

**Do not default to the MDH fraud diagram layout unless the customer is doing fraud/investigations.**

---

## What to Extract from Customer Notes

| What to look for | How it shapes the diagram |
|-----------------|--------------------------|
| Customer name / agency | Title, output filename |
| Industry & use case | Determines layout pattern and Elastic products |
| Existing systems & tools | Column 1 / input side boxes |
| Document or data types | Ingest section — pipelines, OCR, ETL |
| Pain points / manual steps | Callouts inside Elastic boxes ("eliminates X") |
| Who uses the output | Right side / output boxes (analysts, devs, customers) |
| Any specific integrations | Connectors, APIs, third-party tools |
| Recovery / output processes | Action cards, alerts, reports, dashboards |

---

## SVG Styling Rules (Elastic Brand Guidelines)

- **Canvas:** 1920x1080 (16:9, PowerPoint full-bleed)
- **Font:** Inter, Segoe UI, Arial (fallback chain)
- **Elastic components:** Blue solid border `#0B64DD`, 2px, rounded corners (rx=8–14)
- **Non-Elastic / external systems:** Gray dashed border `#BCBCBC`, stroke-dasharray="7,4", square corners
- **AWS components:** Orange solid border `#FF9900`, square corners
- **GCP components:** Blue/multi-color treatment, square corners
- **Azure components:** Azure blue `#0078D4`, square corners
- **Section labels:** `#535966`, font-weight 600, letter-spacing 1.5, ALL CAPS, font-size 12
- **Arrow color (Elastic-to-Elastic):** `#0B64DD`
- **Arrow color (external-to-Elastic):** `#9CA3AF`
- **Arrow color (Elastic orchestration/trigger):** `#153385`
- **Drop shadow:** `feDropShadow dx=0 dy=3 stdDeviation=6 flood-color=#000 flood-opacity=0.1`
- **Glow (ES core):** `feDropShadow dx=0 dy=3 stdDeviation=10 flood-color=#0B64DD flood-opacity=0.18`
- **Title:** font-size 30, font-weight 700, centered x=960 y=52
- **Subtitle:** font-size 14, fill #535966, centered x=960 y=78
- **Bottom bar:** `#0B64DD` rect at y=1055 h=25, white text: "ELASTIC  |  SEARCH  |  OBSERVE  |  PROTECT"
- **Arrow labels** (API, QUERY, TRIGGER etc.): small colored rect + white text, placed BELOW or BESIDE the arrow — never overlapping it
- **Divider lines** inside boxes: `#E5E7EB` stroke-width 1 — always below the last line of text in a section

---

## Elastic Product Reference (Always Accurate)

| Product | What it does |
|---------|-------------|
| **Elasticsearch** | Distributed search and analytics engine — full-text, vector, structured |
| **Kibana** | Visualization layer — Dashboards, Discover, Canvas, Lens, Maps, ML UI, Alerting |
| **Elastic Agent** | Unified data shipper — replaces Beats, manages Fleet |
| **Fleet** | Centralized agent management |
| **Beats** | Lightweight shippers (Filebeat, Metricbeat, Packetbeat, Winlogbeat, etc.) |
| **Logstash** | Data processing pipeline — parse, transform, route |
| **Ingest Pipelines** | Server-side processing — attachment, script, enrich, set processors |
| **Elastic Security** | SIEM, EDR, XDR — threat detection, cases, response |
| **Detection Engine** | Rule-based and ML-based alert generation |
| **Elastic Agent Builder** | Build AI agents with natural language interfaces over Elasticsearch |
| **Workflows** | Orchestration engine for multi-step automated actions |
| **Anomaly Detection Jobs** | Unsupervised ML on time-series data |
| **Rules & Alerts** | Threshold, custom query, ES|QL rules with action connectors |
| **Enrich Processor** | Cross-reference documents against existing ES indices |
| **ELSER** | Elastic Learned Sparse EncodeR — semantic search model |
| **ES|QL** | Pipe-based query language for analytics |
| **Crawler** | Web crawler for indexing web content |
| **App Search / Workplace Search** | Managed search experiences |

---

## Reference Example

See `examples/MDH/MDH_Proposed_Architecture.svg` — a fraud investigation use case.
This is one example of one use case. **Do not treat it as the template for all diagrams.**

## Elastic Asset Reference
- `elastic-assets/Architecture Diagrams Quick Guide - 2025.pptx` — official branding rules
- `elastic-assets/Architecture Graphs.pptx` — reference architectures and icon library

Logos should be added manually in PowerPoint after the SVG is generated.
