# AI Case Generator

A Lightning Web Component for the **Utility Bar** that uses Einstein LLM to generate realistic Service Cloud cases, optionally create a Knowledge article, and produce a **demo scenario script** — a cohesive narrative a presenter can read during a live demo.

---

## Features

| Feature | Description |
|---------|-------------|
| **AI Case Generation** | Generates 1–10 realistic cases tailored to a chosen company and industry using Einstein Prompt Templates (GPT-4.1). Falls back to industry-specific mock templates when Einstein is unavailable. |
| **Knowledge Article** | Optionally creates a rich HTML Knowledge article (`Knowledge__kav`) with troubleshooting steps and prevention tips. Only appears if Knowledge is enabled in the org. |
| **Demo Scenario Script** | After generation, Einstein produces a Markdown-formatted demo story tying the cases, contact, and knowledge article into a coherent narrative with talking points. Includes a **Copy Script** button. |
| **Random Contact Assignment** | Picks a random existing Contact in the org and assigns all generated cases to them — great for demoing the 360-degree customer view. |
| **Industry Auto-Detection** | If no industry is selected, the controller attempts to detect it from the company name (e.g. "Kaiser" → Healthcare). |

---

## Prerequisites

- **Salesforce org** with Service Cloud
- **Einstein Generative AI** enabled (Einstein for Sales/Service or Einstein 1 license) — the component works without it using mock data, but the AI features require it
- **Knowledge** enabled (optional — only needed for the Knowledge article feature)
- **Salesforce CLI v2** (`sf`) installed locally
- At least one **Contact** record in the org

---

## What Gets Deployed

```
force-app/main/default/
├── lwc/aiCaseGenerator/                    LWC component (Utility Bar)
│   ├── aiCaseGenerator.html
│   ├── aiCaseGenerator.js
│   ├── aiCaseGenerator.css
│   └── aiCaseGenerator.js-meta.xml
├── classes/
│   ├── AICaseGeneratorController.cls       Apex controller
│   └── AICaseGeneratorController.cls-meta.xml
└── genAiPromptTemplates/
    ├── Case_Generator.genAiPromptTemplate-meta.xml
    ├── Knowledge_Article_Generation_Template.genAiPromptTemplate-meta.xml
    └── Demo_Scenario_Story_Generator.genAiPromptTemplate-meta.xml
```

---

## Deploy

```bash
# Authenticate to your org (if not already)
sf org login web --alias my-demo-org

# Deploy everything
sf project deploy start --source-dir force-app --target-org my-demo-org
```

---

## Post-Deploy Setup

1. **Add to Utility Bar** — Open **Setup → App Manager**, edit the app you demo with (e.g. Service Console), go to **Utility Items**, and add **AI Case Generator**.
2. **Verify Prompt Templates** — Go to **Setup → Einstein → Prompt Builder** and confirm the three templates (`Case_Generator`, `Knowledge_Article_Generation_Template`, `Demo_Scenario_Story_Generator`) are active. If they show as Draft, activate them.
3. **Test It** — Open the Utility Bar panel, enter a company name and industry, and click **Generate Cases**.

---

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│  LWC: aiCaseGenerator (Utility Bar)                     │
│  ┌──────────────┐    ┌────────────────────────────────┐ │
│  │  Form Input   │───▶│  Apex: AICaseGeneratorController│ │
│  │  - Company    │    │                                │ │
│  │  - Industry   │    │  1. Validate inputs            │ │
│  │  - # Cases    │    │  2. Find random Contact        │ │
│  │  - Knowledge? │    │  3. Call Einstein LLM          │ │
│  └──────────────┘    │     → Case_Generator template   │ │
│                       │  4. Create Case records (DML)  │ │
│  ┌──────────────┐    │  5. Create Knowledge (optional) │ │
│  │  Results      │◀──│     → Knowledge template        │ │
│  │  - Contact    │    │  6. Generate Demo Scenario     │ │
│  │  - Cases      │    │     → Scenario template        │ │
│  │  - Knowledge  │    │  7. Return response            │ │
│  │  - Scenario   │    └────────────────────────────────┘ │
│  └──────────────┘                                        │
└─────────────────────────────────────────────────────────┘
```

---

## Einstein Prompt Templates

| Template | Purpose | Inputs |
|----------|---------|--------|
| `Case_Generator` | Generates realistic case JSON (subject, description, priority, type, origin) | customerName, company, industry, caseCount, additionalDetails |
| `Knowledge_Article_Generation_Template` | Produces an HTML knowledge article with troubleshooting steps | customerName, industry, additionalDetails, casesSummary |
| `Demo_Scenario_Story_Generator` | Creates a Markdown demo script tying cases + contact + knowledge into a story | customerName, industry, contactName, casesSummary, knowledgeTitle |

All templates use `sfdc_ai__DefaultGPT41` and fall back to template-based generation if Einstein is unavailable.

---

*Made by Sheridan Labs — 2025*
