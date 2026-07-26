# 9TC PMO Automation

Google Apps Script automation for the residential duplex development at **9 Turnbull Court, Brunswick West VIC 3055**.

## Capabilities

- Scans Gmail for project correspondence and attachments.
- Saves attachments into the configured Google Drive project folder.
- Detects duplicate files using SHA-256 hashes.
- Classifies documents through the OpenAI Responses API with deterministic local fallback.
- Extracts supplier, invoice, RFI, drawing, revision, amount and due-date metadata.
- Routes imported records to the PMO workbook registers.
- Installs a 15-minute Gmail import trigger.

## Repository files

- `Code.gs` — production Apps Script source.
- `appsscript.json` — Apps Script manifest and OAuth scopes.
- `.github/workflows/validate.yml` — automated JavaScript and manifest validation.
- `docs/INSTALLATION.md` — installation and commissioning procedure.

## Installation summary

1. Open the 9TC PMO Google Sheet.
2. Select **Extensions → Apps Script**.
3. Add the repository's `Code.gs` and `appsscript.json`.
4. Reload the spreadsheet.
5. Select **9TC PMO → OpenAI → Set / Replace API Key**.
6. Confirm the project Drive folder ID in `🤖 AUTOMATION_SETUP`.
7. Select **9TC PMO → Install / Repair Trigger**.
8. Run **9TC PMO → Run Gmail Import Now** for commissioning.

## Required workbook sheets

- `⚙️ CONFIG`
- `🤖 AUTOMATION_SETUP`
- `📥 GMAIL_IMPORT`
- `📚 DOCUMENT_REGISTER`
- `🧾 INVOICES`
- `📝 RFI_REGISTER`
- `📜 PERMITS_APPROVALS`
- `🛒 PROCUREMENT`
- `⚡ VARIATIONS`

## Security

- Never commit an OpenAI API key or Google credential.
- The API key is stored only in Apps Script Script Properties.
- OpenAI requests set `store: false`.
- Local classification remains available when AI is disabled or unavailable.
- Review the OAuth consent screen before granting Gmail and Drive access.

## Repository naming

The repository name currently starts with a hyphen: `-9tc-pmo-automation`. This is valid, but renaming it to `9tc-pmo-automation` would simplify command-line usage.
