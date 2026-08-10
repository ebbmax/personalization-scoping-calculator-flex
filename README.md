# Personalization Scoping Calculator (Flex)

A self-contained HTML tool for Salesforce Account Executives to scope Personalization opportunities under the new **Flex Credits** pricing & packaging and generate a recommended SKU bill of materials (BOM).

**Live URL:** [https://ebbmax.github.io/personalization-scoping-calculator-flex/](https://ebbmax.github.io/personalization-scoping-calculator-flex/)

---

## What's New in the Flex Version

This is the updated version of the [original Personalization Scoping Calculator](https://github.com/ebbmax/personalization-scoping-calculator-flex) — refreshed to reflect the new Salesforce Personalization (Flex) pricing & packaging.

Key changes:

- **New "Deal Type" step** — every scoping starts by selecting **New Customer** or **Renewal**. MCP ownership/usage questions only appear for Renewals.
- **Flex Credits everywhere** — all copy that referenced "Personalization Credits" now references **Flex Credits**.
- **Updated SKU recommendation logic:**
  - **New Customer** → always recommends **Salesforce Personalization (Flex)** as the primary SKU.
  - **Renewal + not using MCP** → always recommends **Salesforce Personalization (Flex)**.
  - **Renewal + owns AND actively using MCP** → recommends **Marketing Cloud Personalization+ ({Region} Flex)** — region matches the provisioning region (AMER / EMEA / APAC).
  - **Retail Triggers no longer auto-recommend MCP+** — retail triggers are now supported by MCA + Data Cloud.
- **New credit formula** — the calculator no longer recommends the **Salesforce Personalization Card (1M Credits)** SKU. Instead it recommends **Flex Credits (100k)** using:

  ```
  Flex Credit units = ⌈ (Total Decisions × 0.1) ÷ 100,000 ⌉
  ```

  Where **Total Decisions** is the sum of decisions across all selected channels for the full contract duration.

---

## Overview

The calculator walks an AE through a guided 4-step flow to capture a customer's personalization requirements and outputs a recommended list of SKUs with quantities and notes — ready to use in a quote.

---

## Features

- **Welcome screen** — choose to start a new calculator or load a previously saved one
- **Export config file** — save all inputs as a `.scopingcalc` file, named `{Customer Name}_{datetime}.scopingcalc`
- **Load existing calculator** — drag and drop or browse for a `.scopingcalc` file to reload a prior session and adjust inputs
- **Input validation** — required fields are enforced at each step before progressing
- **SLDS2 design** — built with Salesforce Lightning Design System tokens, no external CSS dependencies
- **Single file** — no build step, server, or dependencies required; open directly in any modern browser

---

## The 4-Step Flow

### Step 1 — Scoping Context

- Customer name
- Contract length (1–5 years)
- Provisioning region (AMER / EMEA / APAC)
- **Deal Type** *(required)*
  - **New Customer** — for prospects that don't own any personalization SKUs from Salesforce
  - **Renewal** — for customers renewing MCP or Salesforce Personalization
- *(Renewal only)* Does the customer own Marketing Cloud Personalization today?
- *(Renewal only)* Is the customer actively using MCP to power personalized experiences?

### Step 2 — Channel Scoping

Select the channels to personalize:

- Web
- Mobile App
- Marketing Cloud Engagement (Email + Messaging)
- Marketing Cloud Advanced (Email Recommendations)
- Retail Triggers *(with trigger type sub-selection)*

### Step 3 — Channel Volumes

Enter volume inputs for each selected channel:

- **Web** — number of domains + Monthly Unique Visitors (MUV) per domain
- **Mobile** — number of apps + Monthly Active Users (MAU) per app
- **Email** — monthly emails sent + % of emails using personalized recommendations
- *(Renewal + owns + actively using MCP only)* Number of Named Profiles in MCP

Each of the **Web** and **Mobile** sections includes an **Advanced Settings** panel where the *Decisions Per MUV* (web) and *Decisions Per MAU* (mobile) used in the credit-calculation formula can be overridden. Both default to **8**.

### Step 4 — Recommended SKUs

Displays a generated BOM with:

- SKU name
- Recommended quantity
- Notes and guidance

Also shows a scoping summary of all inputs and contextual callouts (e.g. outbound-only channel warnings, Data Cloud prerequisite guidance).

---

## Flex Credit Formulas

**Web:** `MUVs × Decisions Per MUV × 12 × Contract Length × 0.1`
**Mobile:** `MAUs × Decisions Per MAU × 12 × Contract Length × 0.1`
**Email:** `Monthly Emails × % Personalized × 1.25 × 12 × Contract Length × 0.1`

Total Flex Credit units = ⌈ (sum of all channel credits) ÷ 100,000 ⌉

---

## Config Files

`.scopingcalc` files are JSON exports of a complete calculator session. They capture every input across all steps and can be reloaded via **Update Existing Calculator** on the welcome screen to pick up where you left off or share a draft with a colleague.

---

## Tech Stack

- Vanilla HTML, CSS, JavaScript — no frameworks
- Salesforce SLDS2 design tokens (inlined)
- No build step or server required
