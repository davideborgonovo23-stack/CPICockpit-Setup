# Changelog

All notable changes to CPI Cockpit (called CPI MyDashboard up to version 1.5.1) are listed here, newest first.
The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and versions follow [Semantic Versioning](https://semver.org/).
Every version has its installer (`CPICockpit-Setup-<version>.exe` plus its SHA-256; `MyDashboard-Setup-<version>.exe` up to 1.5.1) in the repository's [Releases](https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases).

## [1.6.1] - 2026-09-30

### Changed
- **New icon:** a pixel-art cloud with a lever and the CPI lettering replaces the previous icon (app window, tray, Start menu, installer).
- **Backup, packages not downloaded:** each package shows a colored badge with the reason (Read-only for SAP standard packages, Draft, Error). For drafts, only the names of the draft artifacts are listed instead of the full message from the tenant.

## [1.6.0] - 2026-09-30

### Added
- **Find in the viewer:** Ctrl+F opens a search bar in the viewer of payloads, scripts, mappings and attachments: number of matches, previous and next (Enter goes to the next one), match case, regular expression. Esc closes it.

### Changed
- **New name and icon:** the app is now called CPI Cockpit and has a new icon (app window, tray, Start menu, installer). Data, settings and credentials are untouched, and the update installs over the previous version. The installer file is now `CPICockpit-Setup-<version>.exe`.
- **Monitoring opens on the last hour:** the message list starts with the "Last hour" period, so it loads quickly and without old messages. Choose another period to go further back; "Reset filters" returns to the last hour.
- **Environment band:** it shows only the full name of the environment, without repeating the DEV / QLT / PRD code, and the "configured" label is gone. "not configured" still appears when the credentials are missing.

## [1.5.1] - 2026-09-30

### Added
- **Terms of use:** the installer shows the terms of use (free use, provided "as is", no warranty, your responsibility for being authorized on each tenant). The same text is in `LICENSE.txt` on the download page.
- **Disclaimer:** the download page states that the app is an independent tool, not affiliated with or endorsed by SAP, with the SAP trademark notice.

## [1.5.0] - 2026-09-30

### Added
- **Audit page:** the orange button next to Settings on the customers page opens the log of all recorded actions, newest first. Filters by category, outcome, customer, system, user and period, search in every field, detail of each event with its hashes, CSV/XLSX export. A badge shows whether the hash chain is intact. A transport event opens the matching transport in the history.

### Fixed
- **Link to a disabled environment:** opening a customer page on an environment that is no longer enabled keeps the search and the selection after the redirect.

## [1.4.2] - 2026-09-30

### Changed
- **Update dialog shows every skipped version:** when the update spans more than one version, the dialog lists the news of all versions after the installed one, newest first, with the earlier ones under "Earlier versions". If the changelog cannot be read, only the latest version is shown as before.

### Fixed
- **Update check during a download:** a periodic check finishing while a download was running no longer resets the progress and re-enables the Update button.

## [1.4.1] - 2026-09-30

### Added
- **Column explanations:** hovering a table column header shows what the column means (for example External Key: the custom header "externalKey" written by the iFlow, not a standard SAP field).
- **Custom Status in Monitoring:** new column and filter; it is also included in the search.
- **HTML preview in the viewer:** HTML content can be switched between source and rendered preview. No script runs and no remote image is loaded; links are copied, not opened.
- **Environment check on credentials:** if the address looks like another environment (e.g. "prod" while configuring DEV) a warning stays visible on the credentials page.

### Changed
- **Deploy and undeploy disabled** for now: the menu entries are hidden.
- **Data Store entry:** one dialog with two tabs, Body and Headers; headers are shown as a name/value table.
- **Endpoints:** only the path is shown; the copy button still copies the full address.
- **Packages › artifacts:** an artifact that exists only as a draft shows just "draft" (in yellow) instead of "Active draft".
- **Environment band:** icons instead of letters (code for DEV, stethoscope for QLT, factory for PRD); status and BTP buttons have the same height as the icon; on PRD the text is just "Production environment".
- **Notifications:** shorter texts above the list; the long explanation moved to an info icon.
- **PRD write key missing:** the transport dialog now says the key is created in the app's general settings, not in the customer or environment settings.

## [1.4.0] - 2026-09-29

### Security
- **Writes to PRD need the write key.** Deploy, undeploy and transports to PRD ask every time for a write key (a GUID, typing or pasting allowed). It is created once in Settings › Writes to PRD and shown only once: the app keeps only its public part.
  - The key signs a permit valid only for that operation and tenant, with an expiry; it is closed when the operation ends.
  - Every call to SAP goes through a single filter. On any tenant configured as PRD (in any customer, even if also set up in another slot) every request other than GET, HEAD, OPTIONS and the token request is blocked unless it carries a valid permit. A bug that skipped the dialog would still be stopped.
  - Without a key, or if anything is uncertain, writes are blocked. The protection cannot be switched off.

### Added
- **Audit log.** Every relevant action is recorded with user, PC, customer, system and outcome, in a hash chain like the transport history. Recorded: deploy, undeploy and transports; PRD permits, wrong keys and blocked writes; files saved from the app (downloads, exports, DataStore download, backup); credentials saved or deleted (never their values); customers created, edited, archived and restored; .mdbx export and import; data folder change; backup restore; update installation. Simple views are not recorded. The audit page comes in the next version.
- **Archived customers.** A customer is no longer deleted: it is archived. Credentials, notification rules and artifact cache are removed; transport history and audit are kept and the history stays viewable. Restorable at any time from the customers page.

### Changed
- **Import "replace all" never loses history.** Customers not in the file are archived instead of deleted; those in the file keep their history and get credentials, artifact cache and rules from the file.

## [1.3.5] - 2026-09-29

### Added
- **Details in the update dialog:** a small "Details" button under the list expands every entry with its full description.

### Changed
- **Update dialog tags by type:** each entry shows Feature, Change, Fix, Perf, Removed or Security with its own colour, instead of the same label for every entry.

## [1.3.4] - 2026-09-29

### Performance
- **Transport opens instantly:** the list comes from the local cache (0 calls). It is refreshed from the tenant only when older than 15 minutes, or with **Refresh all** (1 + 3 calls per package, ~13 s on 107 packages). Before each transport only the packages of the selection are re-read (3 calls each). If a selected object changed, the list is updated and you are asked to check the selection. After a transport the list is updated locally instead of re-reading the tenant.
- **Notifications:** the list of deployed iFlows stays in memory like the other sections instead of being re-read (~900 KB) on every visit.
- **Automatic memory cleanup:** the data of a section left unused for 20 minutes is freed and reloaded on return. This never happens while a load is running.
- **Transport history:** after the first load only new transports, items and notes are read. Integrity is checked in the background (separate isolate): the full check runs when the history is opened, then only new records are checked. Search uses a precomputed text per transport.

### Changed
- **Variables:** values are no longer all loaded with the list (73 calls on a real tenant). A click on a variable reads its value (1 call) and shows it in a small dialog, and the value is then kept in memory.

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

[1.6.1]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.6.1
[1.6.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.6.0
[1.5.1]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.5.1
[1.5.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.5.0
[1.4.2]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.4.2
[1.4.1]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.4.1
[1.4.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.4.0
[1.3.5]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.5
[1.3.4]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.4
[1.3.3]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.3
[1.3.2]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.2
[1.3.1]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.1
[1.3.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.3.0
[1.2.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.2.0
[1.1.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.1.0
[1.0.0]: https://github.com/davideborgonovo23-stack/CPI-Cockpit/releases/tag/v1.0.0
