# Invoice Template Workflow — Automated Data Population & Dashboard Updates

## Overview

This is a **complete invoice entry workflow** that:
1. ✅ Uses a simple **Invoice Template** sheet to collect invoice data
2. ✅ One-click button to submit the template
3. ✅ Automatically populates the **Invoice Summary** sheet with formatted data
4. ✅ Calculates GST (inc GST = ex GST × 1.1)
5. ✅ Clears the template for the next invoice
6. ✅ Rebuilds the **Dashboard** automatically
7. ✅ Updates all KPIs in real-time

---

## Step 1: Create the Invoice Template Sheet

### Create a New Sheet

1. Open your 9TC PMO workbook
2. Right-click any sheet tab → **Insert sheet**
3. Name it: `📋 INVOICE_TEMPLATE`
4. Click Create

### Set Up the Template

Copy this exact layout into your template sheet:

| Cell | Field | Example | Notes |
|------|-------|---------|-------|
| A1 | **Invoice No** | INV-2026-001 | Unique identifier (required) |
| A2 | **Invoice Date** | 01/08/2026 | Format: DD/MM/YYYY (required) |
| A3 | **Company** | Contractor ABC | Supplier/subcontractor name (required) |
| A4 | **Status** | Submitted | Submitted / Approved / Paid (required) |
| A5 | **Amount ex GST** | 45000 | Total invoice amount excluding GST (required) |
| A6 | **Unit 1 Amount** | 22500 | Amount allocated to Unit 1 |
| A7 | **Unit 2 Amount** | 22500 | Amount allocated to Unit 2 |
| A8 | **Notes** | Structural works | Optional notes/description |

**Visual Layout:**

```
┌──────────────────────────────────────────┐
│  Invoice Template                        │
├──────────────────────────────────────────┤
│                                          │
│  Invoice No:        INV-2026-001         │
│  Invoice Date:      01/08/2026           │
│  Company:           Contractor ABC       │
│  Status:            Submitted            │
│                                          │
│  Amount ex GST:     $45,000              │
│  Unit 1 Amount:     $22,500              │
│  Unit 2 Amount:     $22,500              │
│                                          │
│  Notes:             Structural works    │
│                                          │
│  ┌──────────────────────────────────┐   │
│  │  ➕ ADD INVOICE                   │   │
│  │  (Click to submit template)      │   │
│  └──────────────────────────────────┘   │
│                                          │
└──────────────────────────────────────────┘
```

### Format the Template Sheet

1. **Column A:** Set width to 30
2. **Column B:** Set width to 40
3. Add a header in row 0 (optional):
   ```
   A0 = "INVOICE ENTRY TEMPLATE"
   ```
4. Make it visually distinct (light blue background for cells A1:A8)

---

## Step 2: Add the Button to the Template Sheet

### Option A: Menu Button (Easiest)
- Click **9TC PMO** menu → **➕ Add Invoice from Template**
- Works anywhere in your workbook

### Option B: Drawing Button on Template Sheet (Visual)

1. Click **Insert** → **Drawing**
2. Click the **Rectangle shape** tool
3. Draw a rounded rectangle at the bottom of your template
4. Format:
   - Fill: Green
   - Text: "➕ ADD INVOICE"
   - Font: Bold, size 14, white color
5. Click **Save & Close**
6. Right-click the button → **Assign script**
7. Type: `addInvoiceFromTemplate`
8. Click OK

The button is now active!

---

## Step 3: How It Works (The Workflow)

### User: Fills in the Template

Someone on the team (e.g., Finance Officer) receives an invoice and fills in the template:

```
Invoice No:        INV-2026-0847
Invoice Date:      31/07/2026
Company:           Foundation & Concrete Ltd
Status:            Submitted
Amount ex GST:     89,500
Unit 1 Amount:     45,000
Unit 2 Amount:     44,500
Notes:             Concrete slabs completed
```

### User: Clicks "Add Invoice"

They click the **➕ ADD INVOICE** button (menu or drawing).

### System: Validates & Processes

The button function:

1. **Reads** the template data (Invoice No, Date, Company, Amount, etc.)
2. **Validates** that all required fields are filled:
   - Invoice No ✓
   - Invoice Date ✓
   - Company ✓
   - Amount ex GST ✓
   - Unit 1 & Unit 2 amounts ✓
3. **Calculates** inc GST:
   - Formula: `Amount ex GST × 1.1`
   - Example: `89,500 × 1.1 = 98,450`
4. **Appends** a new row to the **🧾 INVOICES** sheet with:
   - Invoice No (A column)
   - Status (B column)
   - Date (C column)
   - Company (D column)
   - Amount ex GST (E column)
   - Unit 1 Amount (F column)
   - Unit 2 Amount (G column)
   - Amount inc GST (H column) — auto-calculated
   - Unit 1 + Unit 2 total (I column) — auto-calculated
   - Additional fields pre-formatted (columns J–Q)
5. **Formats** all currency and date columns automatically:
   - Dates: DD/MM/YYYY
   - Currency: $#,##0.00
6. **Clears** the template for the next invoice
7. **Rebuilds** the Dashboard immediately (pulls fresh data)
8. **Shows** success confirmation

### System: Updates Dashboard Automatically

Because all dashboard formulas use `QUERY` and `IMPORTRANGE` (not static values):

- **Invoice Pipeline KPI** shows new invoice count/value
- **Cost Control** updates consumed % and EAC
- **Data Validation Checks** verify reconciliation
- **Risk Banner** updates if thresholds crossed

---

## Step 4: Success Confirmation

After clicking "Add Invoice", you see:

```
┌────────────────────────────────────────────┐
│  ✅ Invoice added successfully!            │
├────────────────────────────────────────────┤
│                                            │
│  Invoice No: INV-2026-0847                 │
│  Amount: $89,500 ex GST                    │
│  Status: Submitted                         │
│  Company: Foundation & Concrete Ltd        │
│                                            │
│  📊 Dashboard updated automatically!       │
│                                            │
│                                [OK]        │
└────────────────────────────────────────────┘
```

Click **OK** and the template is ready for the next invoice.

---

## Data Flow Diagram

```
┌─────────────────────────────────┐
│  📋 INVOICE_TEMPLATE            │
│  (User fills in invoice details)│
└────────────┬────────────────────┘
             │
             ↓
    ┌────────────────────────┐
    │  Click "Add Invoice"   │
    │  Button                │
    └────────────┬───────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Validation:                       │
    │  ✓ Invoice No filled?              │
    │  ✓ Amount > 0?                     │
    │  ✓ All required fields complete?   │
    └────────────┬───────────────────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Calculate:                        │
    │  • Inc GST = Ex GST × 1.1          │
    │  • Unit 1 + Unit 2 total           │
    │  • Due Date (30 days out)          │
    └────────────┬───────────────────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Append to 🧾 INVOICES Register:   │
    │  • New row with all data           │
    │  • Auto-formatted (currency/date)  │
    └────────────┬───────────────────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Clear 📋 INVOICE_TEMPLATE         │
    │  (Ready for next invoice)          │
    └────────────┬───────────────────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Rebuild 📊 DASHBOARD:             │
    │  • Refresh all formulas            │
    │  • Update Cost Control KPIs        │
    │  • Update Invoice Pipeline         │
    │  • Update Data Validation Checks   │
    │  • Refresh Risk Banner             │
    └────────────┬───────────────────────┘
             │
             ↓
    ┌────────────────────────────────────┐
    │  Show Success Dialog               │
    │  "✅ Invoice added successfully!"  │
    └────────────────────────────────────┘
```

---

## Example Workflow: Daily Invoice Entry

### Monday 9 AM: Finance receives invoices from weekend

1. **Open** 9TC PMO workbook
2. **Go to** 📋 INVOICE_TEMPLATE sheet
3. **Fill in** first invoice:
   - Invoice No: INV-2026-0847
   - Date: 31/07/2026
   - Company: Foundation & Concrete Ltd
   - Status: Submitted
   - Amount ex GST: $89,500
   - Unit 1: $45,000
   - Unit 2: $44,500
4. **Click** ➕ ADD INVOICE
5. **See** success dialog
6. **Repeat** for invoice #2, #3, etc.
7. **Done!** All invoices now in the system and dashboard updated

### Wednesday: Cost Controller reviews dashboard

1. **Click** 9TC PMO → 📊 Build/Update Dashboard (optional refresh)
2. **Review** Invoice Pipeline:
   - Submitted: 5 invoices worth $340,000
   - Approved: 3 invoices worth $125,000
   - Paid: 2 invoices worth $85,000
3. **Approve** invoices by updating Status to "Approved" in the 🧾 INVOICES sheet
4. **Dashboard** updates automatically

### Thursday: Client payment run

1. **Filter** 🧾 INVOICES sheet for Status = "Approved"
2. **Process** payments
3. **Update** Status to "Paid"
4. **Dashboard** shows fresh paid invoice count

---

## Field Definitions

### Required Fields

| Field | Purpose | Example | Validation |
|-------|---------|---------|-----------|
| Invoice No | Unique identifier | INV-2026-0847 | Cannot be blank |
| Invoice Date | Date received/issued | 01/08/2026 | Must be valid date |
| Company | Supplier/subcontractor | Foundation Ltd | Cannot be blank |
| Amount ex GST | Total excluding GST | 89500 | Must be > 0 |

### Optional Fields

| Field | Purpose | Example | Notes |
|-------|---------|---------|-------|
| Status | Current state | Submitted | Default: "Submitted" |
| Unit 1 Amount | Unit 1 allocation | 45000 | Can be 0 |
| Unit 2 Amount | Unit 2 allocation | 44500 | Can be 0 |
| Notes | Description/reference | Concrete slabs | For tracking |

### Auto-Calculated Fields (System)

| Field | Calculation | Example |
|-------|-------------|---------|
| Amount inc GST | Amount ex GST × 1.1 | $98,450 |
| Total (Unit 1 + 2) | Unit 1 + Unit 2 | $89,500 |
| Due Date | Invoice Date + 30 days | 30/08/2026 |
| Date Added | Today's date | 01/08/2026 |

---

## Troubleshooting

### Error: "Cannot find 📋 INVOICE_TEMPLATE sheet"

**Cause:** Template sheet doesn't exist or is named differently

**Fix:**
1. Create a new sheet named exactly: `📋 INVOICE_TEMPLATE`
2. Make sure it's not hidden (right-click sheet tabs > Unhide if needed)
3. Try the button again

### Error: "Invoice Template incomplete"

**Cause:** One or more required fields are empty

**Fix:**
1. Go to 📋 INVOICE_TEMPLATE sheet
2. Fill in all fields:
   - ✓ Invoice No
   - ✓ Invoice Date
   - ✓ Company
   - ✓ Status
   - ✓ Amount ex GST
   - ✓ Unit 1 Amount
   - ✓ Unit 2 Amount
3. Click "Add Invoice" again

### Invoice added but Dashboard doesn't update

**Cause:** Dashboard cache or formula delay

**Fix:**
1. Go to 📊 DASHBOARD sheet
2. Click 9TC PMO menu → 📊 Build/Update Dashboard
3. Dashboard refreshes with latest data

### Invoice Amount shows as $0.00

**Cause:** Amount field is empty or contains text (not a number)

**Fix:**
1. Go to 📋 INVOICE_TEMPLATE
2. Check cell A5 (Amount ex GST)
3. Make sure it contains a number: `89500` (not `$89,500` or `"$89,500"`)
4. Click "Add Invoice" again

---

## Advanced: Custom Invoice Processing

### What if you need approval before adding?

**Workflow:**
1. Fill template (Draft status)
2. Supervisor reviews template sheet
3. Changes status to "Ready"
4. Finance clicks "Add Invoice"
5. Invoice appears in Register

**To implement:** Update validation in `addInvoiceFromTemplate()` to check for "Ready" status first.

### What if you want to email confirmation?

**Workflow:**
1. Click "Add Invoice"
2. System emails Finance team: "New invoice added: INV-2026-0847"
3. Dashboard updates

**To implement:** Add `GmailApp.sendEmail()` after the invoice is added.

### What if you want to log to a history sheet?

**Workflow:**
1. Click "Add Invoice"
2. System logs the entry to an "Audit Trail" sheet
3. Includes: timestamp, invoice no, amount, user, status

**To implement:** After appending to INVOICES, also append to a history sheet.

---

## Best Practices

### ✅ DO

- **Use the template** for all new invoices (consistency)
- **Fill in all fields** before clicking the button
- **Review the success dialog** to confirm data was entered
- **Check the 🧾 INVOICES sheet** to verify the new row
- **Update Status field** as invoices move through approval process
- **Rebuild dashboard weekly** to ensure KPIs are fresh

### ❌ DON'T

- **Manually type rows** into the Invoice sheet (use the template instead)
- **Modify the template** structure (keep cells A1–A8 for the function to work)
- **Leave template fields blank** and click the button (validation will reject it)
- **Delete old invoices** from the register (historical data matters for reconciliation)
- **Hardcode amounts** in the dashboard (always use formulas)

---

## Integration with Other Processes

### Gmail Import + Template Workflow

**Hybrid Approach:**
1. **Email invoices** → Auto-import via Gmail button
2. **Extracted data** → Pre-fills template (optional enhancement)
3. **Click "Add Invoice"** → Appends to Register
4. **Dashboard** → Auto-updates

### PO Matching

**Link invoices to POs (future enhancement):**
1. When adding invoice, reference PO number
2. System matches invoice amount to PO amount
3. Alerts if variance > 5%

### Three-Way Reconciliation

**Invoice ↔ PO ↔ Receipt (future enhancement):**
1. PO amount, Receipt quantity, Invoice amount
2. System flags mismatches
3. Blocks payment if variance unexplained

---

## Summary

| Step | Action | Output |
|------|--------|--------|
| 1 | Fill 📋 INVOICE_TEMPLATE | Template populated |
| 2 | Click ➕ ADD INVOICE button | Validation checks run |
| 3 | System appends to 🧾 INVOICES | New invoice row added, formatted |
| 4 | Template cleared | Ready for next invoice |
| 5 | 📊 DASHBOARD rebuilt | All KPIs updated automatically |
| 6 | Success dialog shown | Confirmation to user |

**Total time per invoice: ~60 seconds**  
**Manual data entry: Eliminated** ✅  
**Dashboard updates: Automatic** ✅  
**GST calculations: Automatic** ✅  
**Formatting: Automatic** ✅  

---

**Version:** 2.0 (Automated Template Workflow)  
**Last Updated:** August 2026  
**Status:** Production Ready ✅
