# Verdant Genome Labs — Agricultural Biotechnology & Gene Editing

> Engineering climate-resilient crops through precision CRISPR technology for the world's largest agricultural operations.

| | |
|---|---|
| **Industry** | Biotechnology |
| **Difficulty** | Intermediate |
| **Est. Time** | ~45 min |
| **Key Products** | Scheduled Flows, LWC (lightning-map), Master-Detail, Geolocation |

---

## The Challenge

Commercial farms face massive crop losses due to unpredictable drought conditions, and tracking which gene-edited seed variants perform best across different soil types and climate zones is a nightmare. The sales team needs to match specific crop variants to farm conditions, track field trial performance data, and manage the complex regulatory approval pipeline for each genetic modification.

## What We're Building

A system to track gene-edited crop variants, link them to active field trials across farms, monitor real-time performance metrics, and manage the regulatory approval workflow.

## Custom Objects

| Object | Purpose |
|--------|---------|
| `Crop_Variant__c` | Gene-edited seeds — technique, drought tolerance, yield targets, stage |
| `Field_Trial__c` | Active trials — GPS location, soil type, rainfall, yield tracking |
| `Farm_Partner__c` | Partner farms — acreage, crops grown, partnership tier |
| `Regulatory_Submission__c` | Approval pipeline — USDA/EPA/EU status, comment periods, risk assessments |

## LWC Wow Moment

An interactive map-based LWC showing active field trials across the globe with color-coded performance indicators, drought tolerance scores, and yield predictions — clicking a trial reveals the specific gene edits and real-time soil moisture data.

---

## Prerequisites

- A Salesforce dev org, SDO (Simple Demo Org), or sandbox
- Salesforce CLI (`sf`) installed and authenticated
- Cursor IDE

---

## Step 00 — Project Scaffold (run in terminal before opening Cursor)

**What this step does:** Creates a new SFDX project folder and sets your target org so Cursor and the Salesforce CLI know where to deploy. Run these commands in your terminal (not in Cursor yet).

Run these commands in your terminal to create the SFDX project and set your target org:

```bash
sf project generate --name verdant-genome
cd verdant-genome
sf org list
sf config set target-org <your-org-alias>
```

Then open the `verdant-genome` folder in Cursor and proceed with Step 01.

---

## Step 01 — Custom Objects

**What this prompt does:** Creates four custom objects—**Crop_Variant__c**, **Field_Trial__c**, **Farm_Partner__c**, and **Regulatory_Submission__c**—with fields for variant name, gene-edit technique, drought tolerance, yield, development stage; trial name, dates, acres, soil type, yield, GPS (master-detail to Crop_Variant, lookup to Farm_Partner); farm contact, acres, crops, country, partnership tier; and submission body, status, risk assessment, comment period.

Open Cursor Agent and paste:

```
Create four custom Salesforce objects for Verdant Genome Labs, an agricultural gene-editing company:

1. Crop_Variant__c with fields: Variant_Name__c (text), Target_Crop__c (picklist: Wheat, Corn, Soybean, Rice, Cotton), Gene_Edit_Technique__c (picklist: CRISPR-Cas9, Base Editing, Prime Editing), Drought_Tolerance_Score__c (number 0-100), Expected_Yield_Increase__c (percent), Development_Stage__c (picklist: Research, Greenhouse Testing, Field Trial, Commercial), Lead_Scientist__c (lookup to User), Description__c (long text area)

2. Field_Trial__c with fields: Trial_Name__c (text), Crop_Variant__c (master-detail to Crop_Variant__c), Farm_Partner__c (lookup to Farm_Partner__c), Trial_Start_Date__c (date), Trial_End_Date__c (date), Acres_Planted__c (number), Soil_Type__c (picklist: Clay, Sandy, Loam, Silt, Peat), Average_Rainfall_mm__c (number), Current_Yield_Per_Acre__c (number), Status__c (picklist: Planning, Active, Harvesting, Complete, Cancelled), GPS_Coordinates__c (geolocation), Notes__c (long text area)

3. Farm_Partner__c with fields: Farm_Name__c (text), Primary_Contact__c (text), Contact_Email__c (email), Contact_Phone__c (phone), Total_Acres__c (number), Primary_Crops__c (multi-select picklist: Wheat, Corn, Soybean, Rice, Cotton, Barley), Country__c (text), State_Province__c (text), Partnership_Tier__c (picklist: Prospect, Research Partner, Commercial Partner, Strategic Alliance), Billing_Address__c (text area)

4. Regulatory_Submission__c with fields: Submission_Name__c (text), Crop_Variant__c (lookup to Crop_Variant__c), Regulatory_Body__c (picklist: USDA, EPA, FDA, EU Commission, FSANZ, Health Canada), Submission_Date__c (date), Expected_Approval_Date__c (date), Current_Status__c (picklist: In Preparation, Submitted, Under Review, Additional Info Requested, Approved, Rejected), Risk_Assessment_Complete__c (checkbox), Public_Comment_Period_End__c (date), Approval_Notes__c (long text area)

Ensure all objects have appropriate relationships and standard Salesforce object features like Name fields and audit fields.
```

---

## Step 02 — Sample Data

**What this prompt does:** Generates sample data: **5 crop variants** (e.g. VG-Wheat-DR-447, VG-Corn-XD-892 with drought tolerance and yield), **6 farm partners** (acres, crops, tiers), **8 field trials** (linked to variants and farms, varied statuses), and **4 regulatory submissions** (USDA, EPA, etc.).

```
Create realistic sample data for Verdant Genome Labs with these specific records:

Crop_Variant__c (5 variants):
- "VG-Wheat-DR-447" targeting Wheat, CRISPR-Cas9, 87 drought tolerance, 23% yield increase, Field Trial stage
- "VG-Corn-XD-892" targeting Corn, Base Editing, 94 drought tolerance, 31% yield increase, Commercial stage
- "VG-Soybean-AR-223" targeting Soybean, Prime Editing, 78 drought tolerance, 18% yield increase, Field Trial stage
- "VG-Rice-DT-651" targeting Rice, CRISPR-Cas9, 82 drought tolerance, 26% yield increase, Greenhouse Testing stage
- "VG-Cotton-HY-334" targeting Cotton, Base Editing, 91 drought tolerance, 29% yield increase, Field Trial stage

Farm_Partner__c (6 farms):
- "Cascade Valley Farms" in Washington State, 12,500 acres, primarily Wheat and Barley, Research Partner tier
- "Heartland Agricultural Group" in Nebraska, 28,000 acres, Corn and Soybean, Strategic Alliance tier
- "Red River Cooperative" in Texas, 19,200 acres, Cotton and Corn, Commercial Partner tier
- "Prairie Sky Farms" in Saskatchewan Canada, 15,800 acres, Wheat and Canola, Research Partner tier
- "Sunbelt Agri-Systems" in California, 8,400 acres, Rice and Vegetables, Prospect tier
- "Great Plains Collective" in Kansas, 31,000 acres, Wheat, Corn, Soybean, Strategic Alliance tier

Field_Trial__c (8 trials with varied statuses and realistic data):
- Link trials to the crop variants and farms created above
- Vary trial start dates across 2024-2025
- Use realistic soil types, rainfall amounts (200-800mm), and yield data
- Include GPS coordinates for different US states and Canada
- Mix of Active, Complete, and Planning statuses

Regulatory_Submission__c (4 submissions):
- Link to appropriate crop variants
- Mix of USDA, EPA, and EU Commission submissions
- Various stages: Under Review, Approved, In Preparation, Additional Info Requested
- Realistic submission and expected approval dates

Make all data scientifically plausible and include detailed notes fields with realistic technical information about gene edits, trial conditions, and regulatory requirements.
```

---

## Step 03 — Permission Sets

**What this prompt does:** Creates the **Verdant_Genome_Admin** permission set with full CRUD, View All/Modify All, and field access on Crop_Variant__c, Field_Trial__c, Farm_Partner__c, and Regulatory_Submission__c.

```
Create a Salesforce permission set called 'Verdant_Genome_Admin' that grants the following permissions:

- Read, Create, Edit, Delete access to all four custom objects: Crop_Variant__c, Field_Trial__c, Farm_Partner__c, and Regulatory_Submission__c
- View All and Modify All permissions on these objects
- Read and Edit access to all custom fields on these objects
- Assign the permission set the label 'Verdant Genome Labs - Full Admin Access'
- Include a description: 'Complete access to all Verdant Genome Labs custom objects and fields for lab administrators, lead scientists, and field trial managers'

Generate the permission set XML metadata file in the correct Salesforce DX format under force-app/main/default/permissionsets/
```

---

## Step 04 — Flows & Automations

**What this prompt does:** Builds **Field_Trial_Performance_Alert** (scheduled daily—evaluates trial performance) and **Regulatory_Submission_Reminder** (record-triggered) flows so trial and regulatory data are automated.

```
Create a Salesforce Screen Flow called 'Field_Trial_Performance_Alert' that automates trial monitoring for Verdant Genome Labs:

Flow trigger: Scheduled to run daily

Flow logic:
1. Get Records: Query all Field_Trial__c records where Status__c = 'Active' AND Current_Yield_Per_Acre__c is not null
2. Decision element: Check if Current_Yield_Per_Acre__c is 15% below the expected yield (calculated from the related Crop_Variant__c Expected_Yield_Increase__c field)
3. If below threshold:
   - Create a Task assigned to the Farm_Partner__c Primary_Contact with subject 'Field Trial Performance Review Required' and details about which trial is underperforming
   - Send an email alert to the Lead_Scientist__c on the Crop_Variant__c with trial performance data
   - Update the Field_Trial__c Notes__c field with a timestamped alert: 'Automated Alert: Yield tracking below expected threshold as of [TODAY]'
4. Also create a second flow called 'Regulatory_Submission_Reminder' that triggers when Regulatory_Submission__c Expected_Approval_Date__c is within 30 days and Status__c is still 'Submitted' or 'Under Review', creating a high-priority task for the submission owner

Generate the Flow metadata XML files in the correct Salesforce DX structure under force-app/main/default/flows/
```

---

## Step 05 — Deploy to Org

**What this prompt does:** Runs deploy, assigns **Verdant_Genome_Admin**, and verifies deployment so the four objects and flows are available in the org.

```
Deploy all Verdant Genome Labs metadata to your Salesforce dev org or SDO using SFDX:

1. First, verify the project structure is correct with: sf project deploy validate or sfdx force:source:status
2. Validate that all custom objects, fields, permission sets, and flows are present in force-app/main/default/
3. Run a pre-deployment validation: sf project deploy validate --target-org [your-org-alias] (or sfdx force:source:deploy --checkonly)
4. If validation passes, execute the full deployment: sf project deploy start --target-org [your-org-alias]
5. After deployment, assign the Verdant_Genome_Admin permission set to your user: sf org assign permset --name Verdant_Genome_Admin
6. Verify deployment by opening the org: sf org open
7. If any errors occur, capture the deployment log and display specific failure details

Provide clear console output for each step showing success/failure status.
```

---

## Step 06 — Lightning Web Component

**What this prompt does:** Builds the **trialPerformanceMap** LWC: lightning-map of Field_Trial__c by GPS, color-coded by yield (green/yellow/red), popups with trial and variant details, filters (crop, status, performance), and summary stats (active trials, drought score, acres, top variant).

```
Build a Lightning Web Component called 'trialPerformanceMap' for Verdant Genome Labs that displays an interactive global map of field trials:

Component features:
1. Use lightning-map component to plot all active Field_Trial__c records using their GPS_Coordinates__c field
2. Color-code map markers based on trial performance:
   - Green: Yield meeting or exceeding expectations (>=100% of target)
   - Yellow: Yield 85-99% of target
   - Red: Yield <85% of target
3. Map marker popups should display:
   - Trial name and farm partner name
   - Crop variant being tested
   - Current yield per acre vs expected yield
   - Drought tolerance score of the variant
   - Soil type and average rainfall
   - Days remaining until trial end date
4. Include filter controls above the map:
   - Filter by crop type (Wheat, Corn, Soybean, Rice, Cotton)
   - Filter by trial status
   - Filter by performance threshold
5. Add a summary statistics card showing:
   - Total active trials
   - Average drought tolerance score across trials
   - Total acres under trial
   - Top performing variant name
6. Use Salesforce Lightning Design System (SLDS) styling
7. Query Field_Trial__c with related Crop_Variant__c and Farm_Partner__c data using @wire adapter
8. Make the component available for Lightning App Builder (targets: lightning__AppPage, lightning__RecordPage, lightning__HomePage)
9. Include proper error handling and loading states
10. Add a refresh button to re-query data

Generate complete LWC bundle: HTML template, JavaScript controller with SOQL query, XML metadata config, and CSS for custom styling. Place in force-app/main/default/lwc/trialPerformanceMap/
```

---

## Step 07 — Push to GitHub (Optional)

This step is optional. Use it only if you want to version-control your project or share it on GitHub. You can skip it and still have a complete, deployed solution after Step 06.

**What this prompt does:** Initializes Git, adds .gitignore, creates README (Verdant Genome Labs - Field Trial Management, dev org/SDO deployment), and pushes to a repo such as verdant-genome-salesforce.

```
Initialize a Git repository for the Verdant Genome Labs Salesforce project and push to GitHub:

1. Initialize git in the project directory: git init
2. Create a comprehensive .gitignore file that excludes: .sfdx/, .vscode/, node_modules/, .DS_Store, *.log, .localdevserver/, .sf/, coverage/, .eslintcache
3. Create a README.md file with: project title 'Verdant Genome Labs - Salesforce Field Trial Management System', description, installation instructions for dev org or SDO deployment, object schema overview, LWC usage instructions, and contributing guidelines
4. Stage all files: git add .
5. Create initial commit: git commit -m 'Initial commit: Verdant Genome Labs Salesforce DX project with custom objects, flows, LWC trial map, and sample data'
6. Create a new GitHub repository called 'verdant-genome-salesforce'
7. Add the GitHub remote: git remote add origin [GitHub-repo-URL]
8. Push to GitHub: git push -u origin main
9. Verify the push was successful and display the repository URL

Provide the exact git commands to execute in sequence and confirm each step completes successfully.
```

---

## Architecture Overview

```
Step 00: sf project generate          →  SFDX project scaffold
Step 01: Custom Objects               →  Crop_Variant__c, Field_Trial__c, Farm_Partner__c, Regulatory_Submission__c
Step 02: Sample Data                  →  5 crop variants, 6 farm partners, 8 field trials, 4 regulatory submissions
Step 03: Permission Sets              →  Verdant_Genome_Admin
Step 04: Flows                        →  Field_Trial_Performance_Alert (scheduled), Regulatory_Submission_Reminder (record-triggered)
Step 05: Deploy                       →  Validate + deploy + permissions + verification
Step 06: LWC                          →  trialPerformanceMap (global map with yield performance markers)
Step 07: Git (optional)             →  Push to GitHub
```

---

> **Disclaimer:** This scenario is for demonstration and educational purposes only. Do not deploy to production environments. Code is provided "as-is" without warranty.
