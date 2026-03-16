# Demo Tools

Ready-to-deploy utilities that help Salesforce Solutions Engineers **generate realistic demo data and content** in their orgs. Unlike the step-by-step scenarios in [Demo Scenarios](../Demo%20Scenarios/), these are complete, self-contained SFDX packages you deploy once and use repeatedly.

---

## Available Tools

| Tool | What It Does |
|------|-------------|
| **[AI Case Generator](./AI%20Case%20Generator/)** | A Utility Bar LWC that uses Einstein LLM to generate realistic Service Cloud cases, knowledge articles, and a presenter-ready demo scenario script — all tailored to a chosen company and industry. |

---

## How to Use

1. Pick a tool folder below.
2. Read its README for prerequisites and setup.
3. Deploy to your demo org:
   ```bash
   sf project deploy start --source-dir force-app --target-org <your-org-alias>
   ```
4. Follow any post-deploy configuration steps in the tool's README.

---

*For demonstration and educational use only. Do not deploy to production.*
