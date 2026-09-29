# Changelog

All notable changes to CPI MyDashboard are listed here, newest first.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versions follow [Semantic Versioning](https://semver.org/).
Every version has its installer (`MyDashboard-Setup-<version>.exe` plus its SHA-256) in the repository's [Releases](https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases).

## [1.3.4] - 2026-09-29

### Changed
- **Transport opens instantly:** the list comes from the local cache (0 calls). It is refreshed from the tenant only when older than 15 minutes, or with **Refresh all** (1 + 3 calls per package, ~13 s on 107 packages). Before each transport only the packages of the selection are re-read (3 calls each). If a selected object changed, the list is updated and you are asked to check the selection. After a transport the list is updated locally instead of re-reading the tenant.
- **Variables:** values are no longer all loaded with the list (73 calls on a real tenant). A click on a variable reads its value (1 call) and shows it in a small dialog, and the value is then kept in memory.
- **Notifications:** the list of deployed iFlows stays in memory like the other sections instead of being re-read (~900 KB) on every visit.
- **Automatic memory cleanup:** the data of a section left unused for 20 minutes is freed and reloaded on return. This never happens while a load is running.
- **Transport history:** after the first load only new transports, items and notes are read. Integrity is checked in the background (separate isolate): the full check runs when the history is opened, then only new records are checked. Search uses a precomputed text per transport.

## [1.3.3] - 2026-09-28

### Changed
- **Author in Settings › Information:** "Author: Davide Borgonovo", below the app name and version.

## [1.3.2] - 2026-09-28

### Changed
- **Update test release.** No functional changes: this version exists to verify the automatic update from 1.3.1 end to end (dialog, download, silent install, restart).

## [1.3.1] - 2026-09-28

### Fixed
- **Automatic update did not install.** The helper process that was supposed to start the installer after the app closed got stuck, so the app closed and the old version stayed. Now the app starts the installer directly and closes; the installer waits for the app to exit (up to 60 seconds), installs and reopens it. Each update writes a log in `%LOCALAPPDATA%\MyDashboard\updates`.

### Changed
- **Update check at startup**, no longer after 20 seconds.
- **Release notes in the update dialog** shown as a compact table: type (New / Changed / Fixed) and a short title for each item.
- **Outdated installer.** If you run an older setup, it offers to download and install the latest version right away, checking its SHA-256.

## [1.3.0] - 2026-09-28

### Added
- Two labelled buttons in the environment band (next to "configured"):
  - **Integration Suite**: the Integration Suite home of that tenant. The address is derived from the API URL, so nothing needs to be configured.
  - **BTP Cockpit** on the environment's subaccount. The subaccount ID comes from the OAuth token. The customer's global account ID is an optional field in New/Edit customer, with an info icon that explains where to find it; if it is missing, the button asks for it once. The address is always the EMEA cockpit.

## [1.2.0] - 2026-09-28

### Added
- **Automatic updates.** The app checks the public [MyDashboard-Setup](https://github.com/davideborgonovo23-stack/MyDashboard-Setup) page 20 seconds after start and then every 24 hours; there is also a manual check in Settings. When a newer version exists it shows a dialog:
  - **Update** downloads the installer, checks its SHA-256 against the published fingerprint, then installs silently and reopens the app;
  - **Later** closes the dialog and leaves a highlighted button next to language and theme to reopen it;
  - **Repository** opens the download page with the full changelog.
- **Endpoints** section at the top of an iFlow's detail page, read from `/ServiceEndpoints`: protocol (REST/SOAP), endpoint URL with a copy button, and date and time of the last update.
- **OK** button on the connection settings page. It saves pending changes and opens the customer on the environment just configured.

### Changed
- Pages keep working when you switch to another one.
  - A backup keeps running in the background; next to BACKUP in the sidebar a small ring fills up as it progresses. The page shows the progress or the last result when you come back.
  - The transport history and the notifications section remember search, filters and selection.
- Notification check interval: from 5 minutes up, in steps of 5 (5, 10 … 60). The default is now 5 minutes instead of 2. Older settings (1 or 2 minutes) become 5.
- The notifications section says so when no iFlow is monitored. In that case the app makes no calls to SAP at all, not even the OAuth token request.

## [1.1.0] - 2026-09-28

### Added
- **Transport history**: a new sidebar section, one list per customer, with every transport and its details:
  - code (`TR-2026-0001`), description, optional reference (ticket or change request);
  - Windows user and PC, start, end and duration, route, result;
  - each object with its result and error.
- The Transport dialog asks for a **description** (at least 3 characters) and an optional **reference**. It shows the transport code while it runs and ends with **Open in history**.
- **Notes** on a transport. Notes are append-only: they can be added but never edited or removed.
- **SHA-256 integrity chain** with a badge ("History intact" / "History altered"). Records edited or deleted outside the app are detected; the one known limit is that deleting the very last record leaves no trace.
- **Re-transport failed objects** in one click. It reopens the Transport with the failed objects selected and the description already filled in.
- **History of this object** in the right-click menu of the Transport list.
- History export to Excel, CSV or TXT, either the whole filtered list or a single transport.
- Deleting a customer that has a transport history shows a warning and suggests deactivating it instead. Deletion is still possible.

### Changed
- The transport log can no longer be deleted: the "Reset history" button and the 500-row limit are gone.
- Database schema 2. On first start the app copies the database to `backups/`, then groups the existing log rows into "earlier transports" (same route, at most 5 minutes apart).
- `.mdbx` export format version 2 carries the history with its fingerprints.
  - Import adds history without duplicates.
  - "Replace everything" keeps the history of the customers that are in the file and warns about the others.
  - Older app versions refuse version 2 files instead of silently dropping the history.
- Transports left open by a crash or by closing the app are closed as "interrupted" on the next start.

## [1.0.0] - 2026-09-27

First release: Windows desktop rewrite of the SAP_CPI_MyDashboard web app.

### Added
- Multi-customer workspace with DEV, QLT and PRD environments.
- Monitoring:
  - Message Processing Logs with filters, quick time ranges and custom headers;
  - error details and attachments.
- Packages and deployed content: artifacts, versions, configurations and resources, with downloads.
- Transport between environments:
  - single artifact or whole package, with step-by-step progress and a transport log;
  - a stronger confirmation for PRD.
- Deploy and undeploy, and a ZIP backup of a whole tenant.
- DataStores and entries, Variables and Number Ranges, with export to Excel, CSV and TXT.
- Read-only code viewer with syntax colors, line numbers and collapsible blocks.
- Windows notifications for failed messages, system tray icon, start with Windows.
- Import from the web app, and `.mdbx` export/import protected by a password.
- OAuth credentials encrypted with AES-256-GCM, with the key protected by Windows DPAPI; HTTPS only.
- Local data folder chosen by the user, with an automatic daily backup.
- Light and dark theme, 6 languages, custom window frame.
- Per-user installer that needs no administrator rights; minimal SAP-blue icon.

[1.3.4]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.3.4
[1.3.3]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.3.3
[1.3.2]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.3.2
[1.3.1]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.3.1
[1.3.0]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.3.0
[1.2.0]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.2.0
[1.1.0]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.1.0
[1.0.0]: https://github.com/davideborgonovo23-stack/CPI-MyDashboard/releases/tag/v1.0.0
