<div align="center">

<img src="img/icon.png" width="96" alt="CPI Cockpit icon">

# CPI Cockpit

**All your customers' SAP Cloud Integration tenants in one Windows app.**

### [⬇ Download the latest version](https://github.com/davideborgonovo23-stack/CPICockpit-Setup/releases/latest)

[What's new in each version (changelog)](CHANGELOG.md)

<img src="img/monitoring.png" width="820" alt="CPI Cockpit: message monitoring with the chart of the last 24 hours">

</div>

## What it is

CPI Cockpit is a free desktop app for consultants who manage several customers on **SAP Integration Suite (Cloud Integration)**.
You enter each customer's API credentials once, for DEV, QLT and PRD. From then on you switch customer or environment with one click, with no browser, tabs or repeated logins.

## What you can do

- Work with **workspaces and tabs**: one workspace per customer, its tabs restored at the next start; **Ctrl+K** finds sections, customers and messages.
- **Monitor** messages: chart of the selected period, filters with value dropdowns, error details, attachments, and one viewer for payloads and Data Store entries.
- Browse **packages** and **deployed** content, and download artifacts.
- **Transport** artifacts and packages DEV → QLT → PRD, with a history of who transported what and why.
- **Deploy / undeploy**, and back up a whole tenant to ZIP.
- Read **DataStores, Variables and Number Ranges**, and export them to Excel or CSV.
- Get a **Windows notification** when a message fails.

<table>
  <tr>
    <td><img src="img/message.png" alt="Message detail"></td>
    <td><img src="img/transport.png" alt="Transport to QLT"></td>
  </tr>
</table>

<sub>Screenshots use sample data.</sub>

## Install

1. Download `CPICockpit-Setup-<version>.exe` from the [latest release](https://github.com/davideborgonovo23-stack/CPICockpit-Setup/releases/latest).
2. Run it. No administrator rights are needed. If Windows SmartScreen appears, choose **More info → Run anyway**: the installer is not code-signed yet.
3. On first start, choose a data folder and add your customers.

**Updates:** from version 1.2.0 the app checks this page by itself. When a new version is out it offers to update: one click downloads the installer, checks its SHA-256 fingerprint, installs it and reopens the app. Your data is always kept.

**Requirements:** Windows 10 or 11 (64-bit), HTTPS access to your tenants, and an SAP BTP service key (*Process Integration Runtime*, plan `api`) for each environment. The app includes a step-by-step guide to create it.

## Your data stays with you

- Everything is stored in a folder on your PC that you choose.
- Credentials are encrypted with AES-256 and bound to your Windows user.
- The app talks only to your SAP tenants: no cloud service in between, no telemetry.

## Terms and disclaimer

CPI Cockpit is free to use under its [terms of use](LICENSE.txt): provided "as is", without warranty. You are responsible for being authorized to access each tenant and for complying with the agreements and policies that apply to it, including the SAP API Policy.

CPI Cockpit is an independent tool: it is **not developed, endorsed, certified or supported by SAP**. SAP, SAP Integration Suite, SAP Business Technology Platform and other SAP products and services mentioned are trademarks or registered trademarks of SAP SE in Germany and other countries.

---

<sub>Made by Davide Borgonovo · vibe-coded with <a href="https://claude.com/claude-code">Claude Code</a> · each release lists its changes and the installer's SHA-256.</sub>
