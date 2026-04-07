# Elastic Architecture Diagram Generator

You are helping an Elastic Solutions Architect generate a customer-facing architecture diagram showing how Elastic fits into a customer's existing workflow for fraud investigation, data analysis, or similar use cases.

## Your Job

When the user says "generate" or "create diagram", do the following:

1. Read every file in `customer-input/` to understand the customer's current process, pain points, data sources, and systems.
2. Generate a single SVG file saved to `output/<CustomerName>_Architecture.svg` following the format and styling rules below.
3. Keep it clean and presentation-ready — this goes directly into a PowerPoint slide deck.

---

## SVG Styling Rules (Elastic Brand Guidelines)

- **Canvas size:** 1920x1080 (16:9, matches PowerPoint full-bleed slide)
- **Font:** Inter, Segoe UI, Arial (fallback chain)
- **Elastic components:** Blue solid border `#0B64DD`, 2px, rounded corners (rx=8–14)
- **Non-Elastic / external systems:** Gray dashed border `#BCBCBC`, stroke-dasharray="7,4", square corners (rx=0)
- **AWS components:** Orange border `#FF9900`, square corners
- **Section label text:** `#535966`, font-weight 600, letter-spacing 1.5, ALL CAPS, font-size 12
- **Arrow color (Elastic-to-Elastic):** `#0B64DD`
- **Arrow color (external-to-Elastic):** `#9CA3AF`
- **Arrow color (Elastic orchestration):** `#153385`
- **Drop shadow filter:** `feDropShadow dx=0 dy=3 stdDeviation=6 flood-color=#000 flood-opacity=0.1`
- **Glow filter (ES core box):** `feDropShadow dx=0 dy=3 stdDeviation=10 flood-color=#0B64DD flood-opacity=0.18`
- **Title:** font-size 30, font-weight 700, centered at x=960 y=52
- **Subtitle:** font-size 14, fill #535966, centered at x=960 y=78
- **Bottom bar:** `#0B64DD`, height 25, y=1055, white text: "ELASTIC  |  SEARCH  |  OBSERVE  |  PROTECT"
- **Label tags on arrows** (API, QUERY, TRIGGER): small colored rect + white text, positioned BELOW or BESIDE the arrow line — never on top of it
- **Divider lines** inside boxes: `#E5E7EB`, stroke-width 1 — always placed BELOW the last line of text in a section, not cutting through it

---

## Standard Diagram Layout (Left to Right, 5 columns)

### Column 1 — RAW DATA SOURCES (x: 60–320)
List the customer's existing data sources as individual gray dashed boxes. Each box should have:
- A simple icon area (small colored square with initial letter)
- System name in bold (font-size 14, font-weight 700)
- One line description (font-size 11, fill #535966)

### Column 2 — OCR & INGESTION (x: 420–720)
Two boxes stacked:
1. **AWS Amazon Textract** (orange border) — for any raw PDF/document inputs
2. **Elastic Ingest Pipelines** (blue border) — list the specific pipelines relevant to the customer:
   - Name each pipeline based on what's in the customer notes
   - Include the key transformation logic (zero-out, enrichment, field extraction, etc.)

Both connect to Elasticsearch via API. Source systems connect via API where applicable.

### Column 3 — ELASTICSEARCH (x: 800–1180)
The core blue box with glow filter. Always include these internal sections as sub-boxes:
1. **Anomaly Detection Jobs** — list ML jobs specific to the customer's fraud/analysis use cases
2. **Documentation Review & Evidence Management** — searchable doc index, automated checklist validation, cross-validation
3. **Rules & Alerts** — threshold rules, custom query rules, ES|QL rules
4. **Search & Query** — pill tags: ES|QL, Full-Text, Vector, API
5. **Connectors** — pill tags: Email, Slack, Webhook, and any customer-specific systems
6. **Kibana** — Dashboards, Discover, Canvas, Lens, Maps

Add **use case badges** above the Anomaly Detection section for each investigation type (amber for one, blue for another).

The italic callout "Eliminates need for [X] manual [process]" should appear inside Anomaly Detection wherever the customer has a documented bottleneck.

### Column 4 — INTELLIGENCE & ACTION (x: 1260–1860)
Two boxes stacked:
1. **Elastic Agent Builder** — Conversational Investigation Agent
   - Natural language queries across all case data
   - Auto-verify services rendered via doc cross-reference
   - Document comparison & similarity analysis
   - Investigator copilot — summarize & suggest next steps
   - Automated case triage & prioritization
   - Include an example query bubble (pill shape, light blue) with a realistic query from the customer's domain
2. **Workflows** — Case Lifecycle Orchestration
   - Pipeline: Intake → Triage → Review → Recovery (or equivalent steps for the customer)
   - Automated Actions as colored cards: flag, reports, letters, assign, route, refer

### Bottom — IDE & PYTHON (from center of Elasticsearch)
Bidirectional arrows (read/write) dropping from the bottom of Elasticsearch down to an IDE box. Always include:
- Visual Studio Code
- Jupyter Notebooks
- elasticsearch-py client

---

## Elastic Product Notes (Always Accurate)
- **Elasticsearch** — central store, search, and analytics engine
- **Kibana** — visualization: Dashboards, Discover, Canvas, Lens, Maps, ML UI
- **Elastic Ingest Pipelines** — data transformation: attachment, script, enrich, set processors
- **Elastic Agent Builder** — build AI agents with natural language interfaces over Elasticsearch data
- **Workflows** — orchestration engine for multi-step automated actions triggered by rules/alerts
- **Anomaly Detection Jobs** — unsupervised ML jobs that detect unusual patterns in time-series data
- **Rules & Alerts** — threshold, custom query, and ES|QL-based rules with action connectors
- **Enrich Processor** — cross-reference incoming documents against existing Elasticsearch indices
- **ES|QL** — Elasticsearch Query Language, pipe-based SQL-like syntax for analytics

---

## What to Extract from Customer Notes

When reading customer-input files, extract:
- **Customer name / agency** → use in title and output filename
- **Investigation or use case types** → become the use case badges and workflow steps
- **Existing data sources and systems** → Column 1 boxes
- **Current pain points / manual steps** → become callouts inside Elastic boxes ("eliminates need for X")
- **Document types they work with** → inform Textract and Document Pipeline
- **Any specific systems to connect to** (e.g. MMIS, PICTS, Salesforce) → inform Connectors and Ingest Pipelines
- **Recovery or output processes** → become Workflow action cards

---

## Reference Example
See `examples/MDH/MDH_Proposed_Architecture.svg` for a complete working example.
The customer notes that generated it are in `examples/MDH/MDH IG Pilot Processes .docx`.
The Instructions file at `examples/MDH/MDH_Instructions.md` contains the full slide-by-slide breakdown for reference.

## Elastic Asset Reference
The `elastic-assets/` folder contains:
- `Architecture Diagrams Quick Guide - 2025.pptx` — official Elastic branding rules
- `Architecture Graphs.pptx` — reference architectures and icon library
- `elastic-logo.svg` — Elastic wordmark logo (can be embedded or placed separately)
- `aws-logo.svg` — AWS wordmark logo (can be embedded or placed separately)
