# Elastic Architecture Diagram Generator
### For Elastic Solutions Architects

Generate polished, on-brand customer architecture diagrams in minutes using Claude Code.
The output is a 1920x1080 SVG — drop it directly into PowerPoint.

---

## How to Use

### 1. Clone this repo
```bash
git clone <repo-url>
cd "Architecture Diagrams"
```

### 2. Drop your customer notes into `customer-input/`

The more detail the better. Accepted formats: `.txt`, `.md`, `.docx`, `.pdf`, plain text copy-paste.

Include anything you have:
- Customer name and agency/company
- Their current process (as-is workflow)
- Existing systems and data sources (MMIS, Salesforce, custom DBs, etc.)
- Document types they work with (PDFs, EHR notes, invoices, etc.)
- Pain points and manual bottlenecks
- Investigation or use case types (fraud, compliance, ops monitoring, etc.)
- Any recovery or output processes (reports, referrals, invoices, etc.)

> You don't need all of this — even a rough set of notes or a discovery call summary works.

### 3. Open Claude Code in this folder
```bash
claude
```

### 4. Say: `generate architecture diagram`

Claude will read your customer notes and produce a branded SVG in `output/`.

---

## Output

Your diagram will be saved to:
```
output/<CustomerName>_Architecture.svg
```

To use in PowerPoint:
- Insert → Pictures → This Device → select the SVG
- It scales perfectly to any slide size

To add logos (Elastic, AWS, etc.) after generation:
- Pre-built logos are in `elastic-assets/` — drag them onto the slide in PowerPoint

---

## Folder Structure

```
Architecture Diagrams/
│
├── README.md               ← You are here
├── CLAUDE.md               ← Instructions Claude reads automatically
│
├── customer-input/         ← DROP YOUR CUSTOMER NOTES HERE
│
├── output/                 ← Generated diagrams appear here
│
├── elastic-assets/         ← Elastic PPTs, logos, brand reference
│   ├── Architecture Diagrams Quick Guide - 2025.pptx
│   ├── Architecture Graphs.pptx
│   ├── elastic-logo.svg
│   └── aws-logo.svg
│
└── examples/
    └── MDH/                ← Full worked example (MDH & OIG fraud investigation)
        ├── MDH_Proposed_Architecture.svg
        ├── MDH IG Pilot Processes .docx
        └── MDH_Instructions.md
```

---

## What the Diagram Always Includes

The generator follows a standard left-to-right layout:

| Column | Content |
|--------|---------|
| **Data Sources** | Customer's existing systems (gray dashed, non-Elastic) |
| **OCR & Ingestion** | AWS Textract for PDFs + Elastic Ingest Pipelines |
| **Elasticsearch** | ML Anomaly Detection, Doc Review, Rules & Alerts, Kibana |
| **Intelligence & Action** | Elastic Agent Builder (AI agent) + Workflows (automation) |
| **Bottom** | IDE / Python scripting via elasticsearch-py |

Elastic components use the official brand colors and styling from the Architecture Diagrams Quick Guide.

---

## Tips for Better Output

- **Name specific systems** — "MMIS" is better than "claims database"
- **Call out manual bottlenecks** — Claude will highlight where Elastic automates them
- **List document types** — this shapes the OCR and ingest pipeline sections
- **Mention multiple use cases** — they become labeled badges in the diagram
- **Paste discovery call notes** — raw, unformatted notes work fine

---

## Example Prompt Variations

```
generate architecture diagram
```
```
generate a diagram for [Customer Name]
```
```
generate and focus the automation section on their verification process
```

---

## Questions / Contributions
Built by the Elastic SA team. Add new examples to `examples/` and open a PR.
