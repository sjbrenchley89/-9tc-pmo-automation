# Google Forms Integration — Mobile-Friendly Invoice Entry

## Quick Start

### Option A: Auto-Create Form (Easiest)

1. Open your 9TC PMO workbook
2. Click **9TC PMO** menu → **📋 Create Invoice Form**
3. System creates form and displays the URL
4. Share URL with your team

**That's it!** Responses auto-populate the Invoices sheet.

---

### Option B: Manual Form Setup (Customizable)

If auto-creation doesn't work, create manually:

1. Go to **google.com/forms**
2. Click **Create** → **Blank form**
3. Title: "9TC PMO — Invoice Entry"
4. Copy the fields below

---

## Form Fields

### Form Title
```
9TC PMO — Invoice Entry
```

### Field 1: Invoice Number
- **Type:** Text
- **Title:** Invoice Number
- **Description:** e.g., INV-2026-001
- **Required:** Yes

### Field 2: Invoice Date
- **Type:** Date
- **Title:** Invoice Date
- **Description:** (leave blank)
- **Required:** Yes

### Field 3: Company/Supplier
- **Type:** Text
- **Title:** Company/Supplier
- **Description:** Name of contractor or vendor
- **Required:** Yes

### Field 4: Amount ex GST
- **Type:** Text
- **Title:** Amount ex GST
- **Description:** Numeric value only, e.g., 45000
- **Required:** Yes

### Field 5: Unit 1 Amount
- **Type:** Text
- **Title:** Unit 1 Amount
- **Description:** Leave blank if not applicable, e.g., 22500
- **Required:** No

### Field 6: Unit 2 Amount
- **Type:** Text
- **Title:** Unit 2 Amount
- **Description:** Leave blank if not applicable, e.g., 22500
- **Required:** No

### Field 7: PO Number (Optional)
- **Type:** Text
- **Title:** PO Number
- **Description:** e.g., PO-2026-001 (links to PO Register for matching)
- **Required:** No

### Field 8: Variation Reference (Optional)
- **Type:** Text
- **Title:** Variation Reference
- **Description:** e.g., VO-001 (tracks invoice to change order)
- **Required:** No

### Field 9: Notes
- **Type:** Paragraph text
- **Title:** Notes
- **Description:** Any additional details about this invoice
- **Required:** No

---

## Configure Responses Destination

After creating form:

1. Click **Responses** tab (top)
2. Click **Google Sheets** icon 📊
3. Choose: **Create a new spreadsheet**
4. Name it: "9TC PMO — Invoice Form Responses"
5. Click **Create**

**Result:** New sheet is created. Form responses appear here automatically.

---

## Link Form to Workbook

The form responses go to a separate sheet by default. To consolidate with your invoices:

### Option 1: Import Responses Automatically (Preferred)

Use a formula to pull responses into your main Invoices sheet:

```
=IMPORTRANGE(
  "https://docs.google.com/spreadsheets/d/RESPONSE_SHEET_ID/edit",
  "Form Responses 1!A:I"
)
```

1. Open your Invoice Form Responses sheet
2. Copy the share link
3. Extract the sheet ID: `https://docs.google.com/spreadsheets/d/**SHEET_ID**/edit`
4. Paste into a new column in your main workbook

### Option 2: Manual Copy

1. Open Invoice Form Responses sheet
2. Select response data (excluding timestamp)
3. Copy
4. Paste into 🧾 INVOICES sheet (new row)
5. Format as needed

---

## Using the Form

### For Team Members

1. Click the form URL (in email or shared document)
2. Fill in invoice details:
   - Invoice Number: **INV-2026-001** (required)
   - Invoice Date: **01/08/2026** (required)
   - Company: **Contractor ABC** (required)
   - Amount ex GST: **45000** (required — numbers only)
   - Unit 1 Amount: **22500** (optional)
   - Unit 2 Amount: **22500** (optional)
   - PO Number: **PO-2026-001** (optional)
   - Variation Ref: **(leave blank)** (optional)
   - Notes: **Structural works** (optional)
3. Click **Submit**
4. ✅ "Your response has been recorded"

### Form Response Flow

```
┌─────────────────────────────────┐
│ Team member fills form          │
├─────────────────────────────────┤
│ Clicks "Submit"                 │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ Response appears in:            │
│ • Form Responses sheet (auto)   │
│ • Email to form creator         │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ Finance reviews Responses sheet │
│ • Check for errors              │
│ • Verify amounts                │
│ • Approve invoices              │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ Manual: Copy to Invoices sheet  │
│ OR Auto: IMPORTRANGE formula    │
└────────────┬────────────────────┘
             │
             ↓
┌─────────────────────────────────┐
│ Dashboard updates automatically │
│ • Invoice Pipeline KPI updates  │
│ • Cost Control recalculates     │
└─────────────────────────────────┘
```

---

## Admin Tasks

### Share Form with Team

1. Open form
2. Click **Share** (top-right)
3. Copy link
4. Paste in email or shared doc: `https://docs.google.com/forms/d/FORM_ID/viewform`

### View All Responses

1. Click **Responses** tab
2. View responses in summary or spreadsheet
3. Export as CSV if needed (click ⋮ menu)

### Edit Form Questions

1. Click **Questions** tab
2. Hover over any question
3. Click ✏️ to edit
4. Or click 🗑️ to delete

### Collect Email Addresses

1. Click **Settings** (gear icon)
2. Turn ON: "Collect email addresses"
3. Responses now include who submitted (form creator's email)

---

## Mobile-Friendly URL

Form is fully responsive. Share this URL:

```
https://docs.google.com/forms/d/YOUR_FORM_ID/viewform?usp=pp_url
```

Works on:
- ✅ Desktop
- ✅ Tablet
- ✅ Mobile phone
- ✅ Offline (draft mode)

---

## Advanced: Pre-fill Form Fields

Generate URLs that pre-populate form fields (useful for templates):

```
https://docs.google.com/forms/d/FORM_ID/viewform?usp=pp_url
&entry.FIELD_ID_1=Invoice%20Number
&entry.FIELD_ID_2=2026-08-01
```

**How to find field IDs:**

1. Right-click form field
2. Click **Inspect**
3. Look for: `name="entry.XXXXXXXXX"`
4. That's your field ID

---

## Validation Rules (Built-In)

The form itself doesn't validate data types, but Code.gs validation catches errors:

```
After user clicks Submit:

1. Form submission captured
2. Code.gs runs validation:
   ✓ Invoice No not empty?
   ✓ Amount is numeric?
   ✓ Amount > 0?
   ✓ Units add up to total?
3. If error: Alert user
   If valid: Log to Audit Trail + append to Invoices
```

---

## Troubleshooting

### Q: Form responses not appearing in sheet?

**Check:**
1. Is "Collect responses" turned ON in form settings?
2. Are responses going to the correct sheet?
3. Form created BEFORE you set destination?

**Fix:**
1. Create new form from scratch
2. Ensure destination sheet is selected

### Q: Email addresses not collecting?

**Check:**
1. Form Settings → "Collect email addresses" is ON?
2. User is logged into Google account?

**Fix:** Turn ON in Settings, form must be shared (not embedded)

### Q: Pre-filled URL not working?

**Check:**
1. Entry IDs copied correctly?
2. Special characters URL-encoded (spaces = %20)?
3. Form is shared (not private)?

### Q: Multiple people submitting at once?

✅ **No problem!** Form queues responses. All go through.

---

## Form Best Practices

### DO ✅

- **Use form for quick entry** — faster than typing into template sheet
- **Share link in email** — more accessible than navigating workbook
- **Collect on mobile** — field teams can submit from site
- **Archive responses** — download CSV monthly for record-keeping
- **Review responses sheet first** — before copying to main Invoices sheet

### DON'T ❌

- **Don't embed form in workbook** — doesn't link properly to responses
- **Don't share form edit access** — only creators should modify
- **Don't copy responses multiple times** — creates duplicates
- **Don't rely on form for data validation** — Code.gs does the real validation

---

## Data Flow Comparison

### Traditional Template Method

```
1. User opens 📋 INVOICE_TEMPLATE sheet
2. Fills in 8 cells (A1:A8)
3. Clicks "Add Invoice" button
4. System validates → appends to Invoices
5. Dashboard updates
```

**Pros:** Single workbook, no external tools  
**Cons:** Requires accessing workbook, not mobile-friendly

### Google Forms Method

```
1. User clicks form link (email, SMS, Slack)
2. Fills 9 fields in form interface
3. Clicks "Submit"
4. Response appears in Responses sheet
5. Finance reviews Responses sheet
6. Copy/import to main Invoices sheet
7. Dashboard updates
```

**Pros:** Mobile-friendly, easy sharing, collected by default  
**Cons:** Extra step to copy responses, requires review

### Hybrid Method (Recommended)

```
Use BOTH methods:
• Template sheet: Finance team (in office)
• Form: Field teams, contractors (on-site)
• Both route to same Invoices sheet
• Dashboard pulls from single source
```

---

## Summary

| Method | Entry Speed | Mobile | Sharing | Validation |
|--------|-------------|--------|---------|-----------|
| Template | Fast | No | Workbook link | Code.gs |
| Form | Medium | Yes | URL | Code.gs + Google Forms |
| Hybrid | Fast/Medium | Yes | Both | Code.gs |

---

**Version:** 1.0  
**Last Updated:** August 2026  
**Status:** Ready to Use ✅
