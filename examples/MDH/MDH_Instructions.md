# MDH Program Integrity - Elastic Architecture Slide Template

## Overview
These instructions describe how to build architecture slides for the MDH (Minnesota Dept. of Health) Program Integrity pilot, showing how Elastic replaces the current fragmented toolset as the unified data query layer.

Use the **Architecture Diagrams Quick Guide - 2025.pptx** for styling rules (strokes, fonts, colors) and the **Architecture Graphs.pptx** for Elastic product logos, icons, and reference layouts.

---

## Slide 1: Current State ("As-Is")

**Title:** MDH Program Integrity - Current Architecture

Show the existing fragmented data flow across both investigation types:

```
Data Sources                    Processing               Analysis/Output
-----------                    ----------               ---------------
Cognito Forms (Complaints) --> PICTS (manual entry) --> Excel Spreadsheets
MMIS (Claims Data)         --> Excel (manual extract)    |
JSURS / IBM System         --> Python Scripts             |
CLEAR Reports              --> Manual Phone Calls         v
Provider EHR/PDFs          --> Network Folders        Manual Review & Calc
```

**Icons to use from the PPT:**
- Generic: "document", "government", "stethoscope", "user"
- Feature: "discover", "console"
- Gray dashed borders (Hex #BCBCBC) for all non-Elastic tools

**Key callouts (red/orange text boxes):**
- "Manual data cleaning & zero-ing out logic in Excel"
- "~100 calls to get 10 verified interviews"
- "No unified Case Management System"
- "Unstructured PDFs with no searchable index"
- "Manual Google searches for unit conversions (DME)"

---

## Slide 2: Proposed Architecture ("To-Be") - High Level

**Title:** MDH Program Integrity - Elastic as Unified Data Query Layer

This is the main architecture diagram. Use blue solid borders (Hex #0B64DD) for all Elastic components and gray dashed borders for external systems.

### Left Side: Data Sources & Ingestion

```
MEDICAL PRACTICES                    AWS                         ELASTIC INGEST
+------------------+          +----------------+          +-------------------+
| Raw Documents    |          | Amazon         |          | Ingest Pipeline   |
| (PDFs, EHR Notes,| -------> | Textract       | -------> | - PDF/OCR text    |
|  Invoices,       |          | (OCR Service)  |          | - Field extraction|
|  Shipping Logs)  |          +----------------+          | - Enrich processor|
+------------------+                                      +-------------------+
                                                                   |
EXISTING STATE SYSTEMS                                             |
+------------------+          +-------------------+                |
| MMIS             | -------> | Ingest Pipeline   | ----+          |
| (Claims Data)    |   API    | - Claims parsing  |    |          |
+------------------+          | - Zero-out logic  |    |          |
                              | - Reversal netting|    |          |
+------------------+          +-------------------+    |          |
| JSURS / IBM      | -------> | Ingest Pipeline   | ---+          |
| (Reporting)      |   API    | - Red flag rules  |    |          |
+------------------+          +-------------------+    |          |
                                                       v          v
+------------------+          +-------------------+
| Cognito Forms    | -------> | Ingest Pipeline   | ------------>  ES
| (Complaints)     |   API    | - Form mapping    |
+------------------+          +-------------------+

+------------------+
| CLEAR Reports    | -------> Enrich Processor (contact enrichment)
| (Contact Data)   |
+------------------+
```

### Center: Elasticsearch Cluster

```
+============================================+
|           ELASTICSEARCH                     |
|  +-------------+  +--------------------+   |
|  | HOT DATA    |  | INGEST NODES       |   |
|  | Active cases|  | - OCR text parsing  |   |
|  | Recent claims|  | - Claims netting   |   |
|  +-------------+  | - Enrich (CLEAR)   |   |
|  | WARM DATA   |  +--------------------+   |
|  | Historical  |  +--------------------+   |
|  | claims      |  | MACHINE LEARNING   |   |
|  +-------------+  | - Anomaly detection |   |
|  | COORDINATING|  | - Clone detection   |   |
|  | NODES       |  | - Concurrency flags |   |
|  +-------------+  +--------------------+   |
+============================================+
```

### Right Side: Intelligence, Automation & Query Layer

```
ELASTICSEARCH ----> ANOMALY DETECTION JOBS (ML)
                    | - Claims pattern anomaly detection
                    | - Billing concurrency analysis
                    | - Clone/GenAI note detection
                    | - Provider behavior baseline deviations

ELASTICSEARCH ----> RULES & ALERTS
                    | - Threshold rules (billing caps, overage flags)
                    | - Custom query rules (concurrency, duplicate text)
                    | - ES|QL rules (complex multi-condition logic)
                    | - Action connectors: email, Slack, webhook, PICTS

ELASTICSEARCH ----> ELASTIC AGENT BUILDER (AI ASSISTANT)
                    | - Natural language queries over case data
                    | - Investigator copilot (summarize case, suggest next steps)
                    | - Automated case triage recommendations
                    | - Document comparison & similarity analysis

ELASTICSEARCH ----> WORKFLOWS (EXECUTION ENGINE)
                    | - Automated case lifecycle orchestration
                    | - Intake -> Triage -> Verify -> Review -> Recovery
                    | - Trigger actions based on rule/alert outputs
                    | - Auto-assign cases, send engagement letters
                    | - Schedule verification call batches

ELASTICSEARCH ----> KIBANA
                    | - Dashboards (case status, recovery amounts)
                    | - Discover (full-text search across all docs)
                    | - Canvas (executive reporting)
                    | - Lens (claims analytics)
                    | - Maps (provider location analysis)

ELASTICSEARCH ----> IDE / PYTHON SCRIPTING
                    | - VS Code + Elasticsearch Python Client
                    | - Jupyter Notebooks
                    | - Custom algorithms (red flag detection)
                    | - Scenario-based data mining scripts
                    | - Three-way match automation (DME)

ELASTICSEARCH ----> ES|QL / API
                    | - Ad-hoc queries replacing Excel
                    | - SQL-like syntax for analysts
                    | - REST API for integrations
```

**Icons to use from the PPT:**
- Product logos: Elasticsearch, Kibana, Logstash (if used), Beats
- Feature icons: "machine learning", "discover", "dashboards", "alerting", "console", "dev tools", "notebook", "apm"
- Generic icons: "document", "government", "stethoscope", "script", "source code"
- Industry icons: "government", "stethoscope" (healthcare)
- AWS logo (from OSS/3rd Party logos section, slide 136 area)

**Arrows:**
- Blue solid (Elastic-to-Elastic connections)
- Gray dashed (external-to-Elastic connections)

---

## Slide 3: Behavioral Health Investigation Flow (Detailed)

**Title:** Behavioral Health - Elastic-Powered Investigation Pipeline

Show the 5-stage process with Elastic replacing manual steps:

| Stage | Current State | With Elastic |
|-------|--------------|--------------|
| 1. Intake | Cognito Forms + manual PICTS entry | Cognito Forms API -> Ingest Pipeline -> ES (auto-indexed, searchable) |
| 2. Data Extraction | Manual MMIS Excel exports, manual zero-out | MMIS API -> Ingest Pipeline with scripted processors (automatic reversal netting) |
| 3. Verification | Manual phone + CLEAR lookups | ES Enrich Processor pulls CLEAR data automatically; Kibana dashboard tracks call status |
| 4. Doc Review | Network folders, manual PDF reading | AWS Textract OCR -> ES full-text index; ML anomaly detection for cloned/AI-generated notes; concurrency analysis via ES|QL |
| 5. Recovery | Manual Excel calculations | Kibana dashboards auto-calculate discrepancies; alerting for dispute deadlines |

---

## Slide 4: DME Investigation Flow (Detailed)

**Title:** DME - Elastic-Powered Three-Way Match

Show the automated matching replacing manual Excel/Google work:

```
THE THREE-WAY MATCH (Automated in Elasticsearch)

   CLAIM (MMIS)              INVOICE (OCR'd PDF)         PRESCRIBER ORDER
   Structured Data           Semi-Structured              Authority Doc
   - Procedure Code          - Item Description           - Doctor approval
   - Unit Count (items)      - Unit Count (cases)         - Patient info
   - Medicaid ID             - Patient Name/Address
   - Claim Date              - Invoice Date
        |                          |                           |
        v                          v                           v
   +----------------------------------------------------------+
   |              ELASTICSEARCH MATCHING LOGIC                  |
   |                                                            |
   | 1. Fuzzy Match: Patient Name + Address (no common ID)     |
   | 2. Temporal Match: Invoice Date <-> nearest Claim Date    |
   | 3. Unit Conversion: ES Enrich w/ product lookup table     |
   |    (Invoice Qty * Items_Per_Case = Total vs Claimed)      |
   | 4. Procedure Code Mapping: Semantic search on product     |
   |    description -> procedure code (using ES vector search) |
   +----------------------------------------------------------+
        |
        v
   KIBANA DASHBOARD: Overage flags, match confidence scores
```

---

## Slide 5: AWS OCR + Elastic Ingest Detail

**Title:** Document Digitization Pipeline

```
Medical Practice          AWS Cloud                    Elastic Cloud
+--------------+    +------------------+    +-------------------------+
| Paper/PDF    |--->| S3 Bucket        |--->| Ingest Pipeline         |
| Documents    |    | (raw storage)    |    | - Attachment processor  |
| - EHR Notes  |    +--------|---------+    | - Script processor      |
| - Invoices   |             |              |   (field extraction)    |
| - Shipping   |    +--------v---------+    | - Enrich processor      |
|   Logs       |    | Amazon Textract  |    |   (MMIS cross-ref)     |
| - Prescriber |    | - OCR extraction |    | - Set processor         |
|   Orders     |    | - Table detection|    |   (procedure code map) |
+--------------+    | - Form parsing   |    +-------------------------+
                    +--------|---------+              |
                             |                        v
                    +--------v---------+    +-------------------------+
                    | Lambda Function  |--->| Elasticsearch Index     |
                    | - Format JSON    |    | - Full-text searchable  |
                    | - Call ES API    |    | - Structured fields     |
                    +------------------+    | - ML-ready features     |
                                            +-------------------------+
```

**Icons:** Use AWS logo, S3 icon, Lambda icon from 3rd party logos. Use Elasticsearch logo, Ingest pipeline icon from Elastic product icons.

---

## Slide 6: IDE & Python Integration

**Title:** Developer & Analyst Workbench

```
+------------------+     +-------------------+     +------------------+
| VS Code / IDE    |     | Elasticsearch     |     | Kibana           |
| +==============+ |     | Python Client     |     | Dev Tools        |
| | Python       | |<--->| (elasticsearch-py)| <-->| Console          |
| | Scripts      | |     +-------------------+     +------------------+
| | - Red flag   | |
| |   detection  | |     +-------------------+
| | - Data mining| |     | Jupyter Notebook  |
| | - Scenario   | |<--->| - EDA on claims   |
| |   algorithms | |     | - ML model dev    |
| | - Reports    | |     | - Visualization   |
| +==============+ |     +-------------------+
+------------------+
```

**Key message:** Analysts keep their Python workflows but query Elasticsearch instead of Excel. ES|QL provides SQL-like syntax for those less comfortable with Python.

---

## Styling Reference (from Quick Guide)

- **Elastic components:** Blue solid border, 2px, Hex #0B64DD, rounded corners
- **Non-Elastic components:** Gray solid/dashed border, 2px, Hex #BCBCBC, no rounded corners
- **Section titles (H1):** Inter Bold, 10pt, Hex #000000
- **Sub-section titles (H2):** Inter Bold, 7pt, Hex #000000
- **Labels:** Inter Semi-Bold, All Caps, 4pt, Hex #535966
- **Arrows:** Blue solid for Elastic flows, gray dashed for external connections
- **Logos:** Always use full-color versions for Elastic products

## PPT Asset Locations

From **Architecture Diagrams Quick Guide - 2025.pptx:**
- Slide 1: Construction elements & styling rules
- Slide 2: Beats, Elasticsearch, Kibana, Logstash icons with labels
- Slides 7-8: Marketing & General icons
- Slides 12-14: Industry, Security, General icons
- Slides 20-24: Full feature icon library
- Slides 25-26: Product/Solution logos (Elasticsearch, Kibana, etc.)

From **Architecture Graphs.pptx:**
- Slides 31: Node Organization breakdown (Hot, Warm, Ingest, ML, Coordinating)
- Slides 40-43: Platform marketecture (good starting template)
- Slides 104-114: Elastic & product logos
- Slides 115-134: Complete icon libraries (feature, generic, training, industry)
- Slides 135-137: OSS/3rd party logos (AWS, etc.)

## Key Elastic Products to Highlight

1. **Elasticsearch** - Central data store and query engine (replaces Excel + manual searches)
2. **Kibana** - Dashboards, Discover, Canvas (replaces manual reporting)
3. **Ingest Pipelines** - Automated data transformation (replaces manual zero-out/netting logic)
4. **Enrich Processor** - Cross-reference CLEAR data, product lookups (replaces manual enrichment)
5. **Anomaly Detection Jobs** - ML-based detection for billing anomalies, cloned notes, concurrency patterns
6. **Rules & Alerts** - Threshold, custom query, and ES|QL rules with action connectors for automated notifications
7. **Elastic Agent Builder** - AI-powered investigator assistant for natural language case queries, triage, and document analysis
8. **Workflows** - Automated case lifecycle orchestration (intake through recovery), triggered by rules/alerts
9. **ES|QL** - SQL-like query language for analysts
10. **Elasticsearch Python Client** - IDE integration for custom algorithms
