# Cursor Enablement: Deploy Demo Scenarios to Your Salesforce Org

This guide walks **brand-new Cursor users** through using [Demo-Lab](https://github.com/sfdc-brendan/Demo-Lab/tree/main/Demo%20Scenarios) scenarios to build complete Salesforce solutions with Cursor and deploy them to your org. No prior Cursor or AI-assisted development experience required.

---

## What You’ll Learn

- How **Cursor** (AI-powered IDE) and **Cursor Agent** work with Salesforce projects
- How to run **Step 00** in the terminal to create an SFDX project and connect your org
- How to run **Steps 01–07** in Cursor Agent so the AI generates objects, data, flows, LWCs, and deployment steps for you
- End-to-end flow: pick a scenario → scaffold project → open in Cursor → paste prompts → deploy to Salesforce

---

## Table of Contents

1. [What is Cursor and why use it for Salesforce demos?](#1-what-is-cursor-and-why-use-it-for-salesforce-demos)
2. [Prerequisites](#2-prerequisites)
3. [One-time setup](#3-one-time-setup)
4. [Pick a scenario and run Step 00](#4-pick-a-scenario-and-run-step-00)
5. [Open the project in Cursor](#5-open-the-project-in-cursor)
6. [Run Steps 01–07 in Cursor Agent](#6-run-steps-0107-in-cursor-agent)
7. [Verify in Salesforce](#7-verify-in-salesforce)
8. [Quick reference](#8-quick-reference)
9. [Troubleshooting](#9-troubleshooting)

---

## 1. What is Cursor and why use it for Salesforce demos?

**Cursor** is a code editor (based on VS Code) that uses AI to help you write and modify code. For Demo-Lab scenarios:

- You **paste a prompt** (e.g. “Create four custom objects…”) into **Cursor Agent**.
- The Agent **generates** the right metadata (objects, fields, flows, LWCs, etc.) in your project.
- You **deploy** that metadata to your Salesforce org using Salesforce CLI (`sf`).

So you’re not hand-building every object and flow—you’re guiding the AI with the scenario prompts and then deploying what it creates. That’s ideal for building demo-ready Salesforce apps quickly and learning how Cursor + Salesforce fit together.

---

## 2. Prerequisites

Before starting, have these in place:

| Requirement | What you need |
|-------------|----------------|
| **Cursor** | [Cursor](https://cursor.com) installed on your machine. Sign up or sign in as needed. |
| **Salesforce CLI** | [Salesforce CLI (`sf`)](https://developer.salesforce.com/docs/atlas-en-us/sfdx_setup_install_cli/sfdx_setup_install_cli_install.htm) installed. We use `sf` (v2), not the legacy `sfdx` wrapper. |
| **Salesforce org** | A **scratch org** or **sandbox** to deploy to. Demo-Lab is for demos/learning—do **not** use production. |
| **Terminal** | Your OS terminal (macOS Terminal, Windows Terminal, etc.) for running CLI commands. |

### Check your setup

```bash
# Should show a version (e.g. 2.x)
sf --version

# Should list any orgs you’ve already authenticated
sf org list
```

If `sf` isn’t found, install Salesforce CLI from the link above, then run the commands again.

---

## 3. One-time setup

### 3.1 Install and sign in to Cursor

1. Download Cursor from [cursor.com](https://cursor.com) and install it.
2. Open Cursor and sign in (or create an account).
3. You can accept default settings for now. Cursor Agent is available once the app is open.

### 3.2 Authenticate Salesforce CLI to your org

If you don’t have an org set up yet:

**Option A — Scratch org (recommended for demos)**

```bash
# Create a scratch org from your project’s config (after you’ve run Step 00 and have a project)
sf org create scratch --definition-file config/project-scratch-def.json --alias my-demo-org --set-default
```

**Option B — Sandbox or dev org**

```bash
# Opens browser to log in; replace MySandbox with an alias you choose
sf org login web --alias MySandbox --instance-url https://test.salesforce.com
```

Then set the default org:

```bash
sf config set target-org MySandbox
```

Confirm:

```bash
sf org list
```

You should see your org with a checkmark or “(default)”.

---

## 4. Pick a scenario and run Step 00

### 4.1 Choose a scenario

Open the Demo-Lab repo and go to **Demo Scenarios**:

**[Demo-Lab → Demo Scenarios](https://github.com/sfdc-brendan/Demo-Lab/tree/main/Demo%20Scenarios)**

In the main [Demo Scenarios README](https://github.com/sfdc-brendan/Demo-Lab/blob/main/Demo%20Scenarios/README.md) you’ll see a table of scenarios by industry (Energy, Biotechnology, Aviation, Agriculture, etc.). Each row links to a scenario folder that contains a **README.md** with:

- The Challenge / What we’re building
- Custom objects
- **Step 00** (terminal commands)
- **Steps 01–07** (prompts to paste into Cursor Agent)

For your first time, a **Beginner** scenario is easiest, e.g.:

- **[PawPrint Rescue Network](https://github.com/sfdc-brendan/Demo-Lab/tree/main/Demo%20Scenarios/Nonprofit%20%26%20Rescue/PawPrint%20Rescue%20Network)** — Animal rescue (Nonprofit & Rescue, ~35 min)

Open that scenario’s README and keep it open in a browser tab—you’ll copy from it in the next sections.

### 4.2 Run Step 00 in your terminal

**Step 00** creates a new SFDX project and sets the target org. The exact commands are in each scenario’s README; the pattern looks like this (example for PawPrint):

```bash
# Replace 'pawprint-rescue' and project name with the scenario’s Step 00 if different
sf project generate --name pawprint-rescue
cd pawprint-rescue
sf org list
sf config set target-org <your-org-alias>
```

Do this:

1. Open your **terminal** (not Cursor yet).
2. `cd` to the folder where you want the project (e.g. `Documents`, `Demo Lab`, etc.).
3. Copy **Step 00** from the scenario README and run it.
4. When it says “open the `<project-name>` folder in Cursor,” you’re ready for the next section.

Important: use the **exact** project name and commands from the scenario you chose (e.g. Helion Forge uses a different project name than PawPrint). Each scenario’s README has its own **Step 00** block.

---

## 5. Open the project in Cursor

1. In Cursor: **File → Open Folder** (or **Open…** on Mac).
2. Select the **project folder** you just created (e.g. `pawprint-rescue`).
3. Confirm the left sidebar shows the project (e.g. `force-app`, `config`, etc.).

You’ll run all of Steps 01–07 inside this project so the Agent has the right context.

---

## 6. Run Steps 01–07 in Cursor Agent

Each scenario has **Steps 01–07** in its README. Each step is a **single prompt** you paste into **Cursor Agent**. Run them **in order**; later steps depend on earlier ones.

### 6.1 Open Cursor Agent

- Use **Cmd+Shift+I** (Mac) or **Ctrl+Shift+I** (Windows/Linux), or click the **Agent** icon in the Cursor UI (e.g. in the sidebar or top bar).
- Make sure you’re in **Agent** mode (not only Chat), so the AI can create and edit files and run commands.

### 6.2 Paste one full prompt per step

For each step (01 through 07):

1. In the scenario README, find the **Step 0X** section and the **entire** prompt (the block of instructions in quotes or a code block).
2. **Copy the full prompt** (include all the text the scenario gives you).
3. **Paste** it into the Cursor Agent input box.
4. Send the message and **wait for the Agent to finish** (creating/editing files, and sometimes running terminal commands).
5. **Skim the changes** in the file tree or open a file or two to see what was created.
6. Then move on to the **next** step.

Do **not** skip steps or reorder them. If a step fails, see [Troubleshooting](#9-troubleshooting) before continuing.

### 6.3 What each step usually does

| Step | Typical purpose |
|------|------------------|
| **01** | Create custom objects and fields (metadata in `force-app/main/default/objects/...`). |
| **02** | Create sample data (e.g. plan + CSV in `data/` or Apex/scripts). |
| **03** | Create permission sets so users can access the new objects. |
| **04** | Create flows (e.g. screen flows, record-triggered flows). |
| **05** | Deploy to org: `sf project deploy start`, assign permission set, import data. |
| **06** | Create a Lightning Web Component (LWC) for the “wow” moment. |
| **07** | Initialize Git, add `.gitignore`, commit, and push to GitHub. |

Step 05 often runs `sf` commands for you; if the Agent doesn’t, run the deploy/assign/import commands from the scenario README yourself.

---

## 7. Verify in Salesforce

After Step 05 (and again after 06 if you redeploy):

1. In terminal, from the project folder, run:
   ```bash
   sf org open
   ```
2. In the org:
   - Check **App Launcher** or **Setup** for the new custom objects and tabs.
   - Run the **Screen Flow** from the scenario (e.g. Quick Adoption Application) if applicable.
   - Open the **record page** or **app** where you added the LWC and confirm the component appears and works.

If something’s missing, re-run the deploy step (Step 05) or the specific step that creates that artifact.

---

## 8. Quick reference

| Phase | Where | Action |
|-------|--------|--------|
| Get scenarios | [Demo-Lab → Demo Scenarios](https://github.com/sfdc-brendan/Demo-Lab/tree/main/Demo%20Scenarios) | Open README, pick a scenario, open that scenario’s README. |
| Create project | Terminal | Run **Step 00** from the scenario README. |
| Open in Cursor | Cursor | File → Open Folder → select project folder. |
| Build solution | Cursor Agent | Paste **Step 01** → wait → paste **Step 02** → … → **Step 07**. |
| Deploy / open org | Terminal or Agent | `sf project deploy start`, assign permset, data import; `sf org open`. |

---

## 9. Troubleshooting

### “I don’t see Cursor Agent”

- Confirm you’re in **Cursor** (not VS Code). Agent is a Cursor feature.
- Try **Cmd+Shift+I** (Mac) or **Ctrl+Shift+I** (Windows/Linux), or look for “Agent” in the sidebar or command palette (**Cmd/Ctrl+Shift+P** → “Agent”).

### “Step 00 fails: `sf project generate` not found”

- Install [Salesforce CLI](https://developer.salesforce.com/docs/atlas-en-us/sfdx_setup_install_cli/sfdx_setup_install_cli_install.htm) and ensure `sf` is on your PATH. Restart the terminal and run `sf --version`.

### “Step 05 fails: target org not set”

- Run:
  ```bash
  sf org list
  sf config set target-org <your-org-alias>
  ```
- Then re-run the deploy part of Step 05 (or the full step).

### “Permission set or object not found after deploy”

- Deploy again:
  ```bash
  sf project deploy start --target-org <your-org-alias>
  ```
- Assign the permission set (use the exact name from the scenario):
  ```bash
  sf org assign permset --name <PermissionSetName> --target-org <your-org-alias>
  ```

### “Agent created files in the wrong place”

- Make sure you **opened the project folder** (the one created in Step 00) in Cursor, not a parent or sibling folder. Re-open the correct folder and, if needed, re-run the step that went wrong.

### “Data import fails in Step 05”

- Ensure Step 02 ran successfully and that `data/plan.json` (and any CSV files) exist. If the scenario uses a different data approach (e.g. Apex), follow the scenario’s README. You can ask the Agent to “run the data import for this project as described in the scenario.”

### “I want to try another scenario”

- Create a **new** project: run **Step 00** from the new scenario’s README in a new folder, then open that new folder in Cursor and run Steps 01–07 for that scenario. Don’t mix two scenarios in one project.

---

## Where to go next

- **All scenarios:** [Demo-Lab → Demo Scenarios](https://github.com/sfdc-brendan/Demo-Lab/tree/main/Demo%20Scenarios)
- **Scenario README (what each step contains):** [Demo Scenarios README](https://github.com/sfdc-brendan/Demo-Lab/blob/main/Demo%20Scenarios/README.md)
- **Salesforce CLI:** [Salesforce CLI (sf) Setup](https://developer.salesforce.com/docs/atlas-en-us/sfdx_setup_install_cli/sfdx_setup_install_cli_install.htm)
- **Cursor:** [Cursor documentation](https://docs.cursor.com)

---

*This enablement guide is for demonstration and learning. Do not deploy Demo-Lab scenarios to production. Use a scratch org or sandbox only.*
