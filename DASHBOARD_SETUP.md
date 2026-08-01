# Project Dashboard & KPI Tracker — 9 Turnbull Court PMO

## Overview

The **📊 DASHBOARD** tab is a fully automated, live-updating project control center for 9 Turnbull Court (Brunswick West, VIC 3055). It consolidates cost, invoice, variation, programme, procurement, and defect data from all source registers into executive-ready KPI cards and drill-down navigation.

---

## How to Build / Update the Dashboard

1. Open the **9 Turnbull Court PMO Master Control** Google Sheet
2. Look for the **9TC PMO** menu in the menu bar
3. Click **📊 Build/Update Dashboard**
4. A confirmation dialog will appear when complete

**Note:** This operation clears any existing dashboard and rebuilds it fresh from source data. Run it at least **weekly** as part of governance reviews, or immediately after major cost/invoice/variation updates.

---

## Dashboard Sections

### 1. **COST CONTROL** (KPI Cards)
- **Forecast EAC (Estimate at Completion):** Sum of all commitments to date
- **Budget Variance:** Approved Control Budget ($1,950,000) less Forecast EAC
- **Variance %:** Shows overrun or headroom as % of budget
- **Consumed %:** Invoiced amount as % of approved budget
- **Contingency Remaining:** Budget not yet committed
- **Unit 1 / Unit 2 Variance:** Split cost tracking by dwelling
- **Status Indicator:** Green (on track), Yellow (at risk), Red (overrun)

**Colour Coding:**
- 🟢 Green: Variance < 5% or positive
- 🟡 Amber: Variance 5–10% or approaching limit
- 🔴 Red: Variance > 10% or critical

### 2. **INVOICE PIPELINE**
- **Submitted / Approved / Paid:** Count and value by status
- **Overdue:** Invoices past due date, still unpaid
- **Retention Held:** Amounts withheld from payment (per contract)
- **Status:** Current, 30–60 days, 60–90 days, 90+ days aged analysis
- **Quick Metrics:** Total invoice value (ex GST and inc GST)

**Auto-triggers:** 
- Invoices overdue >0 days → Amber flag
- Invoices unpaid >30 days → Red flag

### 3. **VARIATION PIPELINE**
- **Submitted / Approved / Pending / Rejected:** Count and value by status
- **At Risk (>14 days):** Unapproved variations pending >14 days
- **Total Exposure:** $ value of all unapproved variations
- **Status Indicator:** Managed (low exposure), At Risk (high exposure)

**Alert Rules:**
- Variation pending >14 days without approval → Amber
- Variation exposure >$50,000 → Red

### 4. **PROGRAMME STATUS**
- **Tasks on Track / At Risk / Overdue:** Count by schedule status
- **Float (days):** Remaining buffer on critical path
- **Critical Path:** Key milestones and constraint identification
- **Practical Completion Forecast:** vs. contractual date
- **Status Indicator:** On Schedule, At Risk, Delayed

### 5. **PROCUREMENT STATUS**
- **RFQ Issued / Quotes Received / Awarded / Delivered:** Count by trade package
- **By Trade:** Breakdown of procurement stage by specialist
- **Completion %:** Overall procurement progress
- **Status Indicator:** On Track, Behind Schedule

### 6. **DEFECTS & HOLD POINTS**
- **Open Defects:** Count by dwelling (Unit 1 / Unit 2) and criticality
- **Critical Defects:** Safety or structural priority items
- **Hold Points:** Items blocking payment or final approval
- **Outstanding:** Age and priority of oldest open item
- **Last Updated:** Timestamp of most recent defect entry

---

## Data Consolidation & Formula Structure

### Source Sheets (Live Links)
All KPIs pull from these core registers using `QUERY` and `IMPORTRANGE`:
- **BOQ:** Trade-level quantity take-off and unit rates
- **Budget:** Approved control budget by trade and dwelling
- **Quotes:** RFQ status and received quotations
- **Commitments:** Purchase orders and subcontract awards
- **Invoices:** Invoice register (submitted, approved, paid, retention)
- **Variations:** Variation register (submitted, approved, pending, rejected)
- **Programme:** Task schedule, dates, float, critical path flags
- **Procurement:** RFQ log, quote tracking, award tracking
- **Defects:** Defect register by dwelling, priority, and hold status

### Formula Hygiene
- **No static paste values:** All KPI metrics are formulas linked to source sheets
- **Column-header matching:** Formulas use column names, not fixed row numbers, so they survive row insertions
- **Locale:** All numbers format as Australian currency (AUD), dates as DD/MM/YYYY
- **Timezone:** Sydney (Australia/Sydney) for all date/time stamps
- **Error handling:** `IFERROR()` wraps all aggregations to show "—" if source data is missing

---

## Conditional Formatting Rules

The dashboard applies automatic colour highlighting:

| Condition | Formatting | Action |
|-----------|-----------|--------|
| Cost variance > 10% | 🔴 Red background | Escalate to PM and client |
| Cost variance 5–10% | 🟡 Amber background | Monitor closely, forecast review |
| Invoice overdue (unpaid) | 🔴 Red text | Flag for payment action |
| Variation pending > 14 days | 🟡 Amber text | Chase approval |
| Stale data (not updated 7+ days) | 🔴 Red flag + timestamp | Trigger manual data entry |
| Missing mandatory fields | 🔴 Red highlight | Audit source sheet |
| Formula error (#REF!, #VALUE!) | 🔴 Red outline | Fix broken link or reference |

---

## Drill-Down Navigation

Every KPI metric is hyperlinked to its source:
- **Cost cards** → BOQ and Commitments tabs
- **Invoice counts/values** → Invoices register (filtered by status)
- **Variation cards** → Variations register (filtered by approval status)
- **Programme items** → Programme tab (schedule view)
- **Procurement cards** → Procurement register (by trade)
- **Defects count** → Defects register (by dwelling/priority)

Click any cell in a KPI card to jump to the detailed register.

---

## Data Validation Checks Panel

The dashboard includes a **📋 DATA VALIDATION CHECKS** section that automatically flags:

1. **Formula Integrity:** No #REF!, #VALUE!, #N/A!, #DIV/0! errors
2. **IMPORTRANGE Links:** All external range connections active
3. **Stale Data:** Warns if last data entry >7 days old
4. **GST Consistency:** Confirms all invoice amounts marked ex/inc GST
5. **BOQ→EAC Reconciliation:** Checks that commitments total within 2% of BOQ
6. **Unreconciled Totals:** Flags budget vs. quote vs. award vs. commitment mismatches
7. **Missing Mandatory Fields:** Highlights blank trade, dwelling, or amount cells
8. **Benchmark Outliers:** Flags any cost line >10% above/below 3-record historical median

---

## Weekly Governance Review Workflow

### Before the Meeting (Wednesday EOD)
1. Run "📊 Build/Update Dashboard" from the menu
2. Review the **Risk Summary Banner** (RAG status across Cost, Cash Flow, Programme, Procurement, Variations, Defects)
3. Check the **DATA VALIDATION CHECKS** panel for any red flags
4. Prepare talking points on any Amber or Red indicators

### During the Meeting
- **Cost Control:** Discuss variance, forecast EAC, and contingency burn
- **Invoices:** Review overdue and retention strategy; approve payments
- **Variations:** Decide on pending items; track approval timeline
- **Programme:** Confirm float, identify critical path risks
- **Procurement:** Update award status; chase delayed quotes
- **Defects:** Prioritize outstanding items; clear resolved holds

### After the Meeting
- Update source registers with meeting decisions (approvals, scope changes, payments)
- Run the dashboard rebuild on the next visit
- Archive the prior week's PDF export (File > Download > PDF) for audit trail

---

## Technical Details

### Spreadsheet Locale & Timezone
- **Locale:** en_AU (Australian English)
- **Timezone:** Australia/Sydney
- **Currency:** AUD (no currency symbol prefix on formulas; display only in cells)

### Named Ranges (For Easy Updates)
The following named ranges let you update thresholds in one place:
- `BUDGET_CAP` = 1,950,000 (Approved Control Budget)
- `OVERRUN_THRESHOLD_HIGH` = 10% (triggers Red)
- `OVERRUN_THRESHOLD_MED` = 5% (triggers Amber)
- `STALE_DATA_DAYS` = 7 (data refresh warning)
- `VAR_PENDING_THRESHOLD` = 14 (days before Amber flag)

To edit, select a named range in **Data > Named ranges** and update the value.

### Sheet Protection
- Dashboard layout is **sheet-protected** to prevent accidental formula deletion
- **Editable ranges:** Notes columns and input cells only
- **Unlock:** Go to **Data > Protect sheets and ranges**, click the dashboard lock, then "Edit"

### Mandatory Source Sheet Columns
For formulas to work, each source sheet MUST have these columns:
- **Invoices:** Invoice No, Due Date, Amount, Status, Retention
- **Variations:** Variation No, Amount, Date Submitted, Approval Status
- **Commitments:** Trade, Amount ex GST, Dwelling
- **Programme:** Task ID, Start Date, Finish Date, Float (days)
- **Procurement:** Trade, RFQ Date, Quote Received, Award Status

If columns are renamed or reordered, formulas will break and show #REF! errors. Update the relevant QUERY or INDEX/MATCH references.

---

## Troubleshooting

### Dashboard Not Updating
**Symptom:** KPI values are stale; date stamp is old.  
**Fix:** Run "📊 Build/Update Dashboard" again. Check that source sheets have new data.

### Formula Shows #REF! or #VALUE!
**Symptom:** One or more KPI cells show an error code.  
**Fix:** 
1. Click the error cell
2. Check the formula bar for broken sheet references (e.g., misspelled sheet name)
3. If using IMPORTRANGE, verify the source sheet is shared with edit/view permissions
4. Rebuild the dashboard

### IMPORTRANGE Permission Denied
**Symptom:** IMPORTRANGE cells show "Error: Loading..." indefinitely.  
**Fix:**
1. Open the source sheet (e.g., BOQ tab in the same workbook, or a linked workbook)
2. Share it with the automation service account (check Automation Setup tab for the email)
3. Re-run the dashboard builder

### Drill-Down Links Not Working
**Symptom:** Clicking a KPI cell doesn't navigate to source data.  
**Fix:**
1. Confirm the source sheet exists and is not hidden
2. In the dashboard, right-click the cell > Edit link > Re-enter the target sheet name
3. Save and try again

### Conditional Formatting Not Triggering
**Symptom:** Amber/Red flags don't appear even though values suggest they should.  
**Fix:**
1. Select the cell with the condition
2. Go to **Format > Conditional formatting**
3. Verify the rule condition (e.g., `>= 10%` for overrun threshold)
4. Apply or adjust the rule

---

## Next Steps & Future Enhancements

1. **Automated alerts:** Add email notifications when KPIs turn Red
2. **Historical trending:** Track weekly dashboard snapshots to show variance trend
3. **Scenario modelling:** Add "What-if" sheets for cost and schedule forecasts
4. **Mobile-friendly view:** Create a simplified dashboard for smartphone review
5. **Integration with PM calendar:** Pull key dates from Google Calendar for timeline view
6. **Custom branding:** Apply project logo, colour scheme, and sign-off fields

---

## Support & Questions

For issues or requests, contact the Project Manager or Cost Controller.  
Documentation last updated: August 2026  
Dashboard version: 1.0  
