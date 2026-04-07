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

### 3. Open this folder in VS Code

- Open **Visual Studio Code**
- Go to **File → Open Folder** and select the `Architecture Diagrams` folder

### 4. Open the Claude Code interface in VS Code

- Open the Claude panel from the sidebar or press the Claude icon in VS Code
- Make sure the working directory shown is `Architecture Diagrams/`

### 5. Type: `generate architecture diagram`

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
├── elastic-assets/         ← Elastic PPTs and brand reference
│   ├── Architecture Diagrams Quick Guide - 2025.pptx
│   └── Architecture Graphs.pptx
│
└── examples/
    └── MDH/                ← Full worked example (MDH & OIG fraud investigation)
        ├── MDH_Proposed_Architecture.svg
        ├── MDH IG Pilot Processes .docx
        └── MDH_Instructions.md
```

---

## What the Diagram Includes

The layout adapts to the customer's use case — it is not a fixed template. Claude detects whether the customer is a **Search**, **Observability**, **Security/SIEM**, or **Fraud/Investigation** use case and designs the flow accordingly. A security SOC diagram looks very different from an e-commerce search diagram.

All diagrams follow Elastic brand colors and styling from the official Architecture Diagrams Quick Guide.

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
