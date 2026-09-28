<div align="center">

<img src="img/icon.png" width="96" alt="CPI MyDashboard icon">

# CPI MyDashboard

**All your customers' SAP Cloud Integration tenants in one Windows app.**

### [⬇ Download the latest version](https://github.com/davideborgonovo23-stack/MyDashboard-Setup/releases/latest)

<img src="img/screenshot.png" width="820" alt="CPI MyDashboard: message monitoring">

</div>

## What it is

CPI MyDashboard is a free desktop app for consultants who manage several customers on **SAP Integration Suite (Cloud Integration)**.
You enter each customer's API credentials once, for DEV, QLT and PRD. From then on you switch customer or environment with one click, with no browser, tabs or repeated logins.

## What you can do

- **Monitor** messages: filters, error details, attachments.
- Browse **packages** and **deployed** content, and download artifacts.
- **Transport** artifacts and packages DEV → QLT → PRD, with a history of who transported what and why.
- **Deploy / undeploy**, and back up a whole tenant to ZIP.
- Read **DataStores, Variables and Number Ranges**, and export them to Excel or CSV.
- Get a **Windows notification** when a message fails.

## Install

1. Download `MyDashboard-Setup-<version>.exe` from the [latest release](https://github.com/davideborgonovo23-stack/MyDashboard-Setup/releases/latest).
2. Run it. No administrator rights are needed. If Windows SmartScreen appears, choose **More info → Run anyway**: the installer is not code-signed yet.
3. On first start, choose a data folder and add your customers.

To update, install the new version over the old one: your data is kept.

**Requirements:** Windows 10 or 11 (64-bit), HTTPS access to your tenants, and an SAP BTP service key (*Process Integration Runtime*, plan `api`) for each environment. The app includes a step-by-step guide to create it.

## Your data stays with you

- Everything is stored in a folder on your PC that you choose.
- Credentials are encrypted with AES-256 and bound to your Windows user.
- The app talks only to your SAP tenants: no cloud service in between, no telemetry.

---

<sub>Made by Davide Borgonovo · vibe-coded with <a href="https://claude.com/claude-code">Claude Code</a> · each release lists its changes and the installer's SHA-256.</sub>
