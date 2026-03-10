# SkyLane Dynamics — Urban Air Mobility & Autonomous Aviation

> Electric vertical takeoff aircraft connecting rooftop skyports across metropolitan areas in under 8 minutes.

| | |
|---|---|
| **Industry** | Aviation |
| **Difficulty** | Intermediate |
| **Est. Time** | ~45 min |
| **Key Products** | Service Cloud, LWC, Flows |

---

## The Challenge

SkyLane operates **47 autonomous air taxis** across **23 rooftop skyports** in Miami. They need to track real-time aircraft health, optimize flight routes based on weather and battery levels, manage skyport landing pad availability, and coordinate maintenance crews when sensors detect anomalies mid-flight.

## What We're Building

A command center system to track air taxi fleet status, skyport capacity, flight manifests, and trigger automated maintenance workflows when aircraft diagnostics flag issues.

## Custom Objects

| Object | Purpose |
|--------|---------|
| `Air_Taxi__c` | Individual aircraft — tail number, battery, status, GPS coordinates |
| `Skyport__c` | Rooftop landing sites — pads, elevation, weather restrictions |
| `Flight_Route__c` | Scheduled and active flights between skyports |
| `Maintenance_Alert__c` | Diagnostic anomalies, severity levels, repair tracking |

## LWC Wow Moment

A real-time dashboard showing a map of Miami with all 47 air taxis in flight, battery levels color-coded (green/yellow/red), skyport occupancy status, and live alerts when aircraft need emergency landing coordination.

---

## Prerequisites

- A Salesforce dev org, SDO (Simple Demo Org), or sandbox with Einstein GenAI enabled (API 60.0+)
- Salesforce CLI (`sf`) installed and authenticated
- Cursor IDE

---

## Step 00 — Project Scaffold (run in terminal before opening Cursor)

**What this step does:** Creates a new SFDX project folder and sets your target org so Cursor and the Salesforce CLI know where to deploy. Run these commands in your terminal (not in Cursor yet).

Run these commands in your terminal to create the SFDX project and set your target org:

```bash
sf project generate --name skylane-dynamics
cd skylane-dynamics
sf org list
sf config set target-org <your-org-alias>
```

Then open the `skylane-dynamics` folder in Cursor and proceed with Step 01.

---

## Step 01 — Custom Objects

**What this prompt does:** Creates four custom objects—**Air_Taxi__c**, **Skyport__c**, **Flight_Route__c**, and **Maintenance_Alert__c**—with fields for tail number, battery, status, GPS, skyport pads/elevation/coordinates, route origin/destination/aircraft/datetimes, and alert type/severity/resolution. Includes master-detail and lookups as specified.

Open Cursor Agent and paste:

```
Create four custom Salesforce objects for an autonomous air taxi network:

1. Air_Taxi__c with fields: Tail_Number__c (text, unique), Model__c (picklist: SkyLane X1, SkyLane X2 Pro), Battery_Level__c (percent 0-100), Current_Status__c (picklist: In Flight, Charging, Maintenance, Available), Last_Diagnostic_Check__c (datetime), Passenger_Capacity__c (number), Total_Flight_Hours__c (number), Current_Location_Latitude__c (number), Current_Location_Longitude__c (number).

2. Skyport__c with fields: Skyport_Code__c (text, unique like MIA-DT-01), Building_Name__c (text), Street_Address__c (textarea), City__c (text, default: Miami), Total_Landing_Pads__c (number), Available_Pads__c (number), Elevation_Feet__c (number), Weather_Restriction__c (checkbox), Latitude__c (number), Longitude__c (number).

3. Flight_Route__c with fields: Route_Number__c (auto-number FR-{0000}), Origin_Skyport__c (lookup to Skyport__c), Destination_Skyport__c (lookup to Skyport__c), Assigned_Air_Taxi__c (lookup to Air_Taxi__c), Scheduled_Departure__c (datetime), Actual_Departure__c (datetime), Scheduled_Arrival__c (datetime), Actual_Arrival__c (datetime), Passenger_Count__c (number 0-4), Flight_Status__c (picklist: Scheduled, Boarding, In Flight, Completed, Cancelled), Distance_Miles__c (number), Estimated_Battery_Usage__c (percent).

4. Maintenance_Alert__c with fields: Alert_Number__c (auto-number MA-{0000}), Air_Taxi__c (lookup to Air_Taxi__c), Alert_Type__c (picklist: Battery Degradation, Motor Anomaly, Sensor Malfunction, Rotor Imbalance, Emergency Landing Required), Severity__c (picklist: Low, Medium, High, Critical), Detected_DateTime__c (datetime), Resolution_Status__c (picklist: Open, Investigating, Parts Ordered, In Repair, Resolved), Assigned_Technician__c (text), Resolution_Notes__c (long textarea).

Create all master-detail and lookup relationships as specified. Make all objects available in Lightning Experience.
```

---

## Step 02 — Sample Data

**What this prompt does:** Generates sample data: **12 air taxis** (tail numbers, battery, status, Miami coordinates), **8 skyports** (e.g. MIA-DT-01, Brickell, Design District), **18 flight routes** (completed, in flight, scheduled), and **5 maintenance alerts** (Critical/Medium/Low, various resolution statuses).

```
Create realistic sample data for the SkyLane Dynamics air taxi network:

1. Create 12 Air_Taxi__c records with tail numbers like SL-001 through SL-012, mix of X1 and X2 Pro models, battery levels ranging from 23% to 98%, various statuses (6 In Flight, 3 Charging, 2 Maintenance, 1 Available), realistic Miami area coordinates (25.76 to 25.79 latitude, -80.19 to -80.13 longitude), flight hours between 847 and 3,291, last diagnostic checks within past 24 hours.

2. Create 8 Skyport__c records: MIA-DT-01 at Southeast Financial Center (200 S Biscayne Blvd, 3 pads, elevation 764 ft), MIA-BR-02 at Brickell City Centre (701 S Miami Ave, 4 pads, elevation 512 ft), MIA-WW-03 at Paramount Miami Worldcenter (851 NE 1st Ave, 2 pads, elevation 698 ft), MIA-DB-04 at Miami Design District Tower (3841 NE 2nd Ave, 2 pads, elevation 445 ft), MIA-AP-05 at Miami Airport Hotel (5101 Blue Lagoon Dr, 5 pads, elevation 187 ft), MIA-KB-06 at Key Biscayne Resort (455 Grand Bay Dr, 2 pads, elevation 312 ft), MIA-CB-07 at Coconut Grove Plaza (2700 S Bayshore Dr, 3 pads, elevation 531 ft), MIA-SB-08 at South Beach Marina Tower (1 Washington Ave, 2 pads, elevation 623 ft). Set realistic available pads (1-3 less than total), add realistic lat/long for each location.

3. Create 18 Flight_Route__c records connecting various skyports, with 10 marked Completed (past timestamps), 4 In Flight (departed 3-12 mins ago), 3 Scheduled (next 20-45 mins), 1 Boarding (departs in 4 mins). Assign different air taxis, passenger counts 1-4, realistic distances 2.3-8.7 miles, battery usage 8-15%.

4. Create 5 Maintenance_Alert__c records: 2 Critical severity (Emergency Landing Required for SL-003 with rotor imbalance detected 47 mins ago - status Investigating, Battery Degradation for SL-007 at 18% capacity detected 2 hours ago - status In Repair), 2 Medium severity (Motor Anomaly for SL-009 detected yesterday - status Parts Ordered, Sensor Malfunction for SL-011 detected 6 hours ago - status Investigating), 1 Low severity (routine inspection for SL-005 - status Resolved 3 days ago with resolution notes). Assign technician names like Rodriguez, Chen, Patel.
```

---

## Step 03 — Permission Sets

**What this prompt does:** Creates the **SkyLane_Command_Center_Admin** permission set with full CRUD, View All/Modify All, and API access on Air_Taxi__c, Skyport__c, Flight_Route__c, and Maintenance_Alert__c.

```
Create a Salesforce permission set called SkyLane_Command_Center_Admin that grants full read, create, edit, and delete access to all four custom objects (Air_Taxi__c, Skyport__c, Flight_Route__c, Maintenance_Alert__c) and all their custom fields. Include View All and Modify All permissions on these objects. Also grant access to create, edit, and delete records via API. Add the standard permissions for accessing Lightning Experience, running reports, and viewing dashboards. Set the permission set license to Salesforce if required.
```

---

## Step 04 — Flows & Automations

**What this prompt does:** Builds **Critical_Maintenance_Alert_Handler** (record-triggered on Maintenance_Alert__c—on Critical + Open/Investigating, sets Air_Taxi__c to Maintenance, sends email, creates Task) and **Low_Battery_Auto_Reroute** (scheduled flow—creates Maintenance_Alert__c for aircraft In Flight with battery &lt; 20%).

```
Create a Salesforce record-triggered Flow called 'Critical_Maintenance_Alert_Handler' that triggers when a Maintenance_Alert__c record is created OR updated. Add decision criteria: if Severity__c equals 'Critical' AND Resolution_Status__c equals 'Open' or 'Investigating', then execute these actions:

1. Update the related Air_Taxi__c record: set Current_Status__c to 'Maintenance'
2. Send an email alert to maintenance-critical@skylanedynamics.com with subject 'CRITICAL: {Air_Taxi__c.Tail_Number__c} - {Alert_Type__c}' and body including alert details, aircraft location coordinates, current battery level, and timestamp
3. Create a Task assigned to the Operations Manager (use a hardcoded UserId or get from a custom metadata type) with subject 'Emergency Response Required: {Air_Taxi__c.Tail_Number__c}' due today, high priority

Also create a second Flow called 'Low_Battery_Auto_Reroute' that runs every 5 minutes using a scheduled Flow, queries all Air_Taxi__c records where Battery_Level__c is less than 20% AND Current_Status__c equals 'In Flight', then for each result create a Maintenance_Alert__c record with Alert_Type__c = 'Battery Degradation', Severity__c = 'High', and Detected_DateTime__c = NOW(). Make both Flows active.
```

---

## Step 05 — Deploy to Org

**What this prompt does:** Runs deploy, assigns **SkyLane_Command_Center_Admin**, imports sample data in order (Skyports → Air Taxis → Flight Routes → Maintenance Alerts), and opens the org to verify.

```
Deploy all metadata to your Salesforce dev org or SDO. First, ensure your target org is set by running 'sf org display'. Then use 'sf project deploy start --target-org [your-org-alias]' to deploy all custom objects, fields, permission sets, and flows from the force-app/main/default directory. After deployment, run 'sf org assign permset --name SkyLane_Command_Center_Admin' to assign the permission set to the default user. Then use 'sf data import tree --plan data/sample-data-plan.json' to load all sample records in the correct order (Skyports first, then Air Taxis, then Flight Routes, then Maintenance Alerts to respect lookup relationships). Finally, run 'sf org open' to verify the deployment in the browser. Add error handling to check for deployment failures and log any field-level or object-level security issues.
```

---

## Step 06 — Lightning Web Component

**What this prompt does:** Builds the **airTaxiCommandCenter** LWC: grid of air taxi cards (tail number, status, battery with green/yellow/red), summary stats (In Flight, Maintenance, Critical Alerts), status filter, refresh, and Critical alerts section with NavigationMixin to record pages.

```
Create a Lightning Web Component called 'airTaxiCommandCenter' that serves as a real-time operations dashboard. The component should:

1. Query and display all Air_Taxi__c records using wire service or imperative apex
2. Show a visual grid/card layout with each air taxi displaying: Tail_Number__c, Model__c, Current_Status__c, Battery_Level__c (with color coding: green if >60%, yellow if 30-60%, red if <30%), and a small status indicator icon
3. Include a summary statistics section at the top showing: Total Aircraft, Currently In Flight count, In Maintenance count, Average Battery Level across all aircraft, Critical Alerts count (query Maintenance_Alert__c where Severity = Critical and Resolution_Status != Resolved)
4. Add a filter dropdown to show only aircraft by status (All, In Flight, Charging, Maintenance, Available)
5. Use Lightning Design System styling with slds-card, slds-grid, and slds-badge components
6. Make each air taxi card clickable to navigate to its record detail page using NavigationMixin
7. Add a refresh button that re-queries all data every 10 seconds using setInterval
8. Display Maintenance_Alert__c records in a separate section below showing Critical alerts only, with red warning styling and the tail number, alert type, and time since detection
9. Make the component flexible to work on record pages, app pages, and home pages by setting targets in the meta.xml

Use modern JavaScript (ES6+), proper error handling, and add @api properties for configurability. Include a meta.xml file that exposes it to Lightning App Builder for all page types.
```

---

## Step 07 — Push to GitHub (Optional)

This step is optional. Use it only if you want to version-control your project or share it on GitHub. You can skip it and still have a complete, deployed solution after Step 06.

**What this prompt does:** Initializes Git, adds .gitignore (e.g. .sfdx/, .sf/, .vscode/), stages metadata and config, creates initial commit and README (overview, dev org/SDO setup, deployment), and pushes to a repo such as skylane-dynamics-salesforce.

```
Initialize a new Git repository for this SkyLane Dynamics Salesforce project and push it to GitHub. First, run 'git init' in the project root directory. Create a comprehensive .gitignore file that excludes: .sfdx/, .sf/, .vscode/, .idea/, .DS_Store, node_modules/, and any local environment configs. Stage all Salesforce metadata with 'git add force-app/ config/ scripts/ data/ .gitignore README.md'. Create an initial commit with message 'Initial commit: SkyLane Dynamics air taxi command center - custom objects, flows, LWC dashboard, and sample data'. Then create a new GitHub repository called 'skylane-dynamics-salesforce' with description 'Autonomous air taxi fleet management system built on Salesforce - track aircraft status, skyport capacity, flight routes, and maintenance alerts in real-time'. Use 'gh repo create' or add the remote with 'git remote add origin' and the repo URL, then 'git push -u origin main'. Include instructions to create a README.md with project overview, setup instructions for dev org or SDO, data import steps, and deployment commands.
```

---

## Architecture Overview

```
Step 00: sf project generate          →  SFDX project scaffold
Step 01: Custom Objects               →  Air_Taxi__c, Skyport__c, Flight_Route__c, Maintenance_Alert__c
Step 02: Sample Data                  →  12 air taxis, 8 skyports, 18 flights, 5 alerts
Step 03: Permission Sets              →  SkyLane_Command_Center_Admin
Step 04: Flows                        →  Critical_Maintenance_Alert_Handler, Low_Battery_Auto_Reroute
Step 05: Deploy                       →  Metadata + data import + permission assignment
Step 06: LWC                          →  airTaxiCommandCenter (real-time fleet dashboard)
Step 07: Git (optional)             →  Push to GitHub
```

---

> **Disclaimer:** This scenario is for demonstration and educational purposes only. Do not deploy to production environments. Code is provided "as-is" without warranty.
