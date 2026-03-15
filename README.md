# Qnovate Pricing Portal — SAP Cloud for Customer (C4C)

> A duration-based customer pricing solution built on SAP C4C for Miele, featuring structured approval workflows, time-bound validity enforcement, PDF export, and full audit trail tracking.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [How It Works](#how-it-works)
- [Portal Screens](#portal-screens)
- [Approval Workflow](#approval-workflow)
- [Data Model](#data-model)
- [PDF Export](#pdf-export)
- [Change History](#change-history)
- [Approval Status Reference](#approval-status-reference)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [Contributing](#contributing)

---

## Overview

The **Qnovate Pricing Portal** is a custom SAP Cloud for Customer (C4C) extension built for Miele. It provides sales teams with a single, structured interface to create, manage, and approve customer-specific pricing agreements — without leaving the SAP ecosystem.

Key business problems solved:

- Pricing agreements previously scattered across systems with no single source of truth
- No enforcement of pricing validity periods — contracts often applied outside agreed dates
- Manual approval processes with no audit trail
- No way to generate a shareable, printable summary of a pricing record

---

## Features

| Feature | Description |
|---|---|
| **Pricing List View** | Browse all pricing agreements with search and filter by customer, date, or status |
| **Quick Create Form** | Simple form to create or edit pricing — customer, price, start/end date |
| **Duration Enforcement** | System automatically calculates duration and deactivates pricing outside its validity window |
| **Approval Workflow** | Built-in manager approval step before any pricing goes live |
| **Role-Based Visibility** | Sales reps, managers, and admins each see only what is relevant to them |
| **Instant Notifications** | Managers notified on submission; sales reps notified on approval or rejection |
| **PDF Export** | Full pricing record (root data + all node data) rendered as a branded, downloadable PDF |
| **Change History** | Complete audit log of every change at both root and node level, with export to spreadsheet |
| **Involved Parties Tab** | Track which customers and contacts are attached to each pricing agreement |
| **Attachments Tab** | Attach supporting documents directly to the pricing record |

---

## How It Works

The portal is organised into three steps:

```
Step 1: Browse the Pricing List
  └── Search, filter, and select any existing pricing record
  └── Or start a new one

Step 2: Create or Update a Price
  └── Fill in customer, price, start date, end date
  └── Duration is auto-calculated

Step 3: Review Details & Submit
  └── Open the full record detail view
  └── Review involved parties, attachments, change history, approval status
  └── Click "Submit for Approval" to trigger the workflow
```

---

## Portal Screens

### Pricing List (Browse View)
The entry point for all pricing activity. Displays all records with their current approval status, validity dates, and customer information. Supports filtering by customer, date range, and status.

### Quick Create / Edit Form
A clean, minimal form collecting:
- **Pricing ID** (auto-generated)
- **Status** (Draft / Pending / Approved / Rejected)
- **Combination Data** (e.g. SoldTo-Mat-ShipCond)
- **Sales Organisation**
- **Primary Approver**
- **Quantity Data**
- **Start Date / End Date / Duration**
- **Price Amount**
- **Organisational Data** — Sold-To Party, Ship-To Party, Material ID, SC/CC Data, Incoterm

### Detail View (Thing Inspector)
Full record view with four tabs:

| Tab | Purpose |
|---|---|
| **Change History** | Full audit log of all edits at root and node level |
| **Involved Parties** | Party ID, postal code, role, email, active status, main contact |
| **Approval** | Current approval status and workflow history |
| **Attachments** | Upload and view supporting documents |

---

## Approval Workflow

The approval process follows six steps:

```
1. Sales Rep     → Opens the pricing list and starts a new record
2. Sales Rep     → Fills in all pricing details
3. Sales Rep     → Reviews the full record and clicks "Submit for Approval"
4. System        → Routes the record to the correct manager and sends a notification
5. Manager       → Reviews the record and clicks Approve or Reject
6. System        → Updates status to Approved (Status 4) and notifies the sales rep
```

If a record is **Rejected**, the sales rep receives a notification and can revise and resubmit.

---

## Data Model

### Root Object — Pricing Record

| Field | Description |
|---|---|
| `PricingID` | Unique identifier for the agreement |
| `Status_Data` | Current approval status (1–4) |
| `Combination_data` | Pricing combination type (e.g. SoldTo-Mat-ShipCond) |
| `Sales_Organization` | The selling entity |
| `Primary_Approver_Data` | Assigned approving user |
| `Quantity_Data` | Associated quantity |
| `Start_Date` | Date pricing becomes valid |
| `End_Date` | Date pricing expires |
| `Duration` | Auto-calculated from start/end dates |
| `Price_Amount_Data` | Agreed price value |

### Organisational Data (embedded section)

| Field | Description |
|---|---|
| `Sold_To_Party_ID` | The billing customer |
| `Ship_To_Party_ID` | The delivery customer |
| `Material_ID` | Associated product or material |
| `SC_Data` | Shipping condition |
| `CC_Data` | Customer condition |
| `Incotem_data` | Incoterms for the agreement |

### Involved Parties Node (line items)

| Field | Description |
|---|---|
| `Party_ID` | Name / ID of the involved party |
| `Postal_Code` | Party postal code |
| `Role` | Party role code |
| `Email` | Contact email address |
| `Is_Active` | Whether the party is currently active |
| `Main_Contact` | Primary contact name |

---

## PDF Export

The PDF Preview feature generates a complete, branded printable summary by combining data from all sections of a pricing record.

**Data sources included in the PDF:**

1. **Root Record Data** — Pricing ID, Status, Start/End Date, Duration, Price Amount, Sales Organisation, Primary Approver, Combination Data
2. **Organisational Data** — Sold-To Party, Ship-To Party, Material ID, SC/CC Data, Incoterm
3. **Involved Parties (Node)** — All party rows iterated from the node table and rendered as a structured table
4. **Rendered Output** — Single-page PDF with Qnovate header, ready to download, email, or attach to the approval record

---

## Change History

Every modification to a pricing record is automatically captured in the Change History tab — with no manual logging required.

**What is tracked:**

- Changes to any root-level field (Status, Start Date, End Date, Duration, Price Amount, etc.)
- Changes inside node-level sub-sections (Involved Parties, Approval data, etc.)

**Each change record includes:**

| Column | Description |
|---|---|
| `Change Date/Time` | Exact timestamp of the change |
| `Changed By` | Full name and user key of the person who made the change |
| `User ID` | SAP system user ID |
| `Attribute` | Which field was changed |
| `Value Changed From` | The previous value |
| `Value Changed To` | The new value |
| `Modification Type` | Update / Create / Delete |

**Filtering:** Filter by date range (`Changes Made From` / `Changes Made To`), by user, or by specific attribute.

**Export:** The full history can be exported to a spreadsheet for compliance reporting or dispute resolution.

**Example log entry:**
```
03/12/2026 3:43 PM — Developer06 Aryash (DEVELOPER06)
  Duration:    27  →  19       [Update]
  Start Date:  03/04/2026  →  03/12/2026  [Update]
```

---

## Approval Status Reference

| Status Code | Label | Meaning |
|---|---|---|
| `1` | **Draft** | Record created but not yet submitted for approval |
| `2` | **Pending** | Submitted and awaiting manager review |
| `3` | **Rejected** | Manager declined — sales rep must revise and resubmit |
| `4` | **Approved** | Signed off — pricing is now live and active |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Platform** | SAP Cloud for Customer (C4C) |
| **Custom Development** | SAP C4C SDK — Business Object (BO) framework |
| **UI Layer** | OWL (Object Work List), Quick Create (QC), Thing Inspector (TI) |
| **Approval Engine** | SAP C4C Approval Framework |
| **PDF Generation** | SAP C4C PDF Output Channel |
| **Integration** | SAP ERP / S/4HANA (pricing conditions sync) |
| **Presentation** | Qnovate-branded PowerPoint deck (PptxGenJS) |

---

## Getting Started

### Prerequisites

- SAP Cloud for Customer (C4C) tenant with developer access
- SAP C4C SDK configured in your development environment
- Appropriate user roles: `Business Configuration` + `Application and User Management`

### Deployment

1. **Import the Business Object** — Upload the Pricing BO definition via the SAP C4C Solution Transport.
2. **Configure Approval Framework** — Set up the approval rule for the Pricing BO in Business Configuration.
3. **Assign Roles** — Grant sales reps and managers the appropriate work centre access.
4. **Configure PDF Template** — Upload the branded PDF form template via Output Management.
5. **Enable Notifications** — Configure workflow notification rules for approval events.

---

## Project Structure

```
pricing-portal/
├── BusinessObjects/
│   └── Pricing.bo               # Root BO definition with all fields and nodes
├── UIComponents/
│   ├── PricingOWL.uicomponent   # Object Work List (browse screen)
│   ├── PricingQC.uicomponent    # Quick Create form
│   └── PricingTI.uicomponent    # Thing Inspector (detail view + tabs)
├── ApprovalRules/
│   └── PricingApproval.xml      # Approval framework configuration
├── PDFTemplates/
│   └── PricingRecord.xdp        # Branded PDF output template
├── Presentation/
│   └── Qnovate_Pricing_Portal.pptx
└── README.md
```

---

## Contributing

This project is maintained by **Qnovate**. For questions, enhancements, or support requests, please contact your Qnovate delivery team or raise an issue in this repository.

---

*Built by [Qnovate](https://www.qnovate.com) — SAP CX, ERP, BTP & Data specialists.*
