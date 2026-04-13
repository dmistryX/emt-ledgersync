# Emendate LedgerSync

Emendate LedgerSync is a desktop application for importing bills and invoices from Excel into QuickBooks Online.

## Features

- Import bills and invoices with multiple line items
- Validate vendors, customers, accounts, and items before import
- OAuth 2.0 authentication with local callback handling
- Support multiple companies (sandbox and production)
- Configurable Excel column mappings
- Write import status back to Excel
- Local encrypted credential/token storage (SQLite + Fernet)

## Requirements

- Python 3.10+
- QuickBooks Online company
- Intuit developer app (Client ID / Client Secret)

## Run Locally

1. Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

2. Start the app:

    ```bash
    python app/qb_run.py
    ```

## QuickBooks Developer Setup

1. Go to `https://developer.intuit.com`
2. Create a QuickBooks Online app
3. Add redirect URI: `http://localhost:8080/callback`
4. Copy sandbox/production credentials into app settings

## First-Time App Setup

1. Open **Settings > API Credentials**
2. Add credentials for `sandbox` and/or `production`
3. Open **Settings > Companies**
4. Add company name, realm ID, and environment
5. Authenticate company from the app

## Excel Support

The app supports:

- Bills sheet import
- Invoices sheet import
- Configurable column mappings (0-based) from **Settings**
- Status write-back to the configured Status column after import

Sample files are included:

- `sample_bills.xlsx`
- `sample_invoices.xlsx`

## Local Data Storage

When packaged/installed, app data is stored under:

- `%APPDATA%\<install-folder-name>\`

Main files:

- `Emendate_LedgerSync.sqlite3`
- `Emendate_LedgerSync.log`

## Build Windows Executable + Installer

Use the installer scripts in `installer/`:

- `installer/els_imports.spec` (PyInstaller spec)
- `installer/els_build.bat` (build + optional Inno compile)
- `installer/els_setup.iss` (Inno Setup script)

Run:

```bat
installer\els_build.bat
```

Expected build output:

- `dist\EmendateLedgerSync\EmendateLedgerSync.exe`
- `dist\installer\EmendateLedgerSync_Setup_v<version>.exe`

## Web Pages

Project includes branded support pages in `web/`:

- `index.html`
- `callback.html`
- `disconnect.html`
- `privacy.html`
- `eula.html`

## Troubleshooting

- **Validation not found errors**: Ensure names exactly match QuickBooks display names.
- **Auth issues**: Clear tokens/re-authenticate from app settings.
- **Excel status write failure**: Close the Excel file, then retry.
- **Port conflict on 8080**: Update redirect URI in both app settings and Intuit app.

## Tech Stack

- `requests` (QBO API calls)
- `flask` (local OAuth callback server)
- `openpyxl` (Excel read/write)
- `ttkbootstrap` + `ttkwidgets` (desktop UI)
- `cryptography` (token/credential encryption)
