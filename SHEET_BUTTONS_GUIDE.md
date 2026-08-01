# Sheet Buttons Guide — Quick Action Buttons for Invoices & Gmail Import

## Overview

You now have two quick-action button functions:
1. **📧 Import Gmail Now** — Trigger Gmail import manually
2. **➕ Add Invoice Row** — Add a blank invoice row to the Invoices sheet

These are available via the **9TC PMO** menu, and you can also add them as drawing buttons directly on the sheets.

---

## Method 1: Use Menu (Easiest)

Just click the menu:
1. **9TC PMO** (top menu bar)
2. **📧 Import Gmail Now** — Imports emails immediately
3. **➕ Add Invoice Row** — Adds blank row to Invoices sheet

**No setup needed!** This is the fastest way.

---

## Method 2: Add Drawing Buttons on Sheets (Optional)

For a more visual interface, you can add drawing buttons directly on the sheets.

### Step 1: On the 📥 GMAIL_IMPORT Sheet

1. **Open the 📥 GMAIL_IMPORT sheet**
2. **Click Insert** (top menu) → **Drawing**
3. **Create a button shape:**
   - Click the **Shape tool** (rectangle icon)
   - Select a rounded rectangle shape
   - Draw it in the top-right area (e.g., column N, row 1)
   - Fill with blue, text in white
   - Type: **"📧 IMPORT NOW"**
4. **Click Save & Close** (button appears on sheet)
5. **Right-click the button** → **Assign script**
6. **Type:** `triggerGmailImport`
7. **Click OK**

**Result:** Clicking the button triggers the Gmail import immediately.

**What it does:**
- Imports emails from Gmail (based on search query in AUTOMATION_SETUP)
- Uploads attachments to Drive
- Classifies documents (rule-based + AI)
- Updates the import queue
- Shows success/error dialog

---

### Step 2: On the 🧾 INVOICES Sheet

1. **Open the 🧾 INVOICES sheet**
2. **Click Insert** (top menu) → **Drawing**
3. **Create a button shape:**
   - Click the **Shape tool**
   - Select a rounded rectangle
   - Draw it in the top-right area (e.g., column N, row 1)
   - Fill with green, text in white
   - Type: **"➕ ADD INVOICE"**
4. **Click Save & Close**
5. **Right-click the button** → **Assign script**
6. **Type:** `addInvoiceRow`
7. **Click OK**

**Result:** Clicking the button adds a new blank invoice row.

**What it does:**
- Adds a new row to the Invoices sheet
- Auto-fills the date to today
- Sets number formatting for currency/date columns
- Selects the first cell so you can start typing
- Shows a dialog confirming the row was added

---

## Using the Add Invoice Button

When you click **➕ ADD INVOICE**:

1. A new row appears at the bottom of your invoice data
2. The **Date** column auto-fills to today's date
3. You can immediately start filling in:
   - Invoice No
   - Company name
   - Status (Submitted / Approved / Paid)
   - Amount ex GST
   - Unit 1 amount
   - Unit 2 amount
   - etc.

**Formatting is automatic:**
- Dates format as DD/MM/YYYY
- Currency formats as $#,##0.00
- No manual formatting needed

---

## Using the Import Gmail Button

When you click **📧 IMPORT NOW**:

1. The script searches Gmail for documents matching your criteria (from AUTOMATION_SETUP sheet)
2. For each email with attachments:
   - Extracts metadata (from, subject, date)
   - Computes SHA-256 hash (detects duplicates)
   - Classifies the document (Invoice, RFI, Variation, etc.)
   - Uploads file to Drive
   - Adds row to 📥 GMAIL_IMPORT sheet
3. Shows success dialog with count of documents imported
4. You can then route them to the correct register

**If AutoRoute is enabled (AUTOMATION_SETUP):**
- Imported documents are automatically routed to Invoices, RFIs, Variations, etc.
- A second dialog appears: "Route Ready Items"

---

## Button Function Reference

### `triggerGmailImport()`
**Purpose:** Manually trigger Gmail import  
**Function:** Calls `runGmailImport()` and shows status dialog  
**Available via:** Menu or Drawing Button

**Example Response:**
```
✅ Gmail import completed!

Check the 📥 GMAIL_IMPORT sheet for new documents.
Route ready items via menu: 9TC PMO > Route Ready Items
```

### `addInvoiceRow()`
**Purpose:** Add a new invoice row to the 🧾 INVOICES sheet  
**Function:** 
- Inserts a new row below the last data row
- Auto-fills date to today
- Sets number formatting
- Activates the first cell for input

**Example Response:**
```
✅ New invoice row added at row 127

Fill in the details:
• Invoice No
• Date (auto-filled to today)
• Company
• Status
• Amounts (ex GST, Unit 1, Unit 2)
```

---

## Keyboard Shortcuts (Optional)

If you want keyboard shortcuts for these buttons:

1. Go to **Tools** → **Keyboard shortcuts**
2. Search for `triggerGmailImport` or `addInvoiceRow`
3. Assign a shortcut (e.g., `Ctrl+Shift+I` for Import)
4. Save

Now you can press the shortcut instead of clicking the menu!

---

## Troubleshooting

### Button doesn't work / "Script not found"
**Fix:**
1. Right-click the button again
2. Click **Assign script**
3. Make sure you typed the function name exactly: `triggerGmailImport` or `addInvoiceRow`
4. Click OK

### "Cannot find Invoices sheet"
**Fix:**
1. Make sure your Invoices sheet is named exactly: `🧾 INVOICES`
2. Check that it's not hidden (right-click sheet tab > Unhide if needed)
3. Try clicking the menu option first: 9TC PMO > ➕ Add Invoice Row

### Import shows error: "Gmail search query is blank"
**Fix:**
1. Go to 🤖 AUTOMATION_SETUP sheet
2. Fill in the "Gmail search query" field (e.g., `"9 Turnbull" OR "9TC"`)
3. Save and try Import again

### Newly added invoice doesn't show up in dashboard
**Fix:**
1. Wait a few seconds for the new row to be indexed
2. Re-run buildDashboard() from menu: 9TC PMO > 📊 Build/Update Dashboard
3. Dashboard will include the new invoice

---

## Best Practices

### For Gmail Import Button
- **Run weekly:** Every Wednesday before governance meeting
- **After:** You'll have 5-7 days of emails to process
- **Then:** Review the import sheet and route to registers
- **Tip:** Enable AutoRoute in AUTOMATION_SETUP for automatic routing

### For Add Invoice Button
- **Use when:** You receive an invoice via email or direct handover
- **Instead of:** Manually typing rows — much faster
- **Follow up:** Route the invoice to the correct register (status: Submitted → Approved → Paid)
- **Tip:** Set up a weekly process to add outstanding invoices

---

## Integration with Dashboard

After adding invoices or importing emails:

1. **Run buildDashboard()** to refresh KPIs (or wait for next scheduled rebuild)
2. New invoices appear in:
   - INVOICE PIPELINE section (Submitted count)
   - Cost Control (consumed %)
   - Data Validation Checks (reconciliation)

---

## Advanced: Custom Button Functions

Want to add more buttons? You can create custom functions:

**Example: Add RFI Row**
```javascript
function addRFIRow(){
  const ss = SpreadsheetApp.getActive();
  const sh = ss.getSheetByName(SHEETS.RFIS);
  if (!sh) { SpreadsheetApp.getUi().alert('❌ Cannot find RFI sheet'); return; }
  
  const lastRow = sh.getLastRow();
  const newRow = lastRow + 1;
  
  sh.getRange(newRow, 1, 1, 12).setValues([['', new Date(), '', '', '', '', '', 'Open', '', '', '', '']]);
  sh.getRange(newRow, 2).setNumberFormat('dd/mm/yyyy');
  sh.getRange(newRow, 1).activate();
  
  SpreadsheetApp.getUi().alert('✅ New RFI row added at row ' + newRow);
}
```

Then assign this to a drawing button on the RFI_REGISTER sheet!

---

## Summary

| Feature | How to Use | When to Use |
|---------|-----------|-----------|
| **Import Gmail** | Click 📧 IMPORT NOW button or menu | Weekly before governance meeting |
| **Add Invoice** | Click ➕ ADD INVOICE button or menu | When you receive an invoice |
| **Refresh Dashboard** | Click 📊 BUILD/UPDATE DASHBOARD menu | After major data changes or weekly |

**All three are one-click operations with no setup required!**

---

**Version:** 1.0  
**Last Updated:** August 2026  
**Status:** Ready to use!
