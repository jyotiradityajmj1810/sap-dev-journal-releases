# SAP DevJournal — Desktop App

A local-first, privacy-focused developer journaling app for Windows.

Your entries are encrypted with **AES-256-GCM** and stored locally. Optional GitHub sync is **zero-knowledge** — only ciphertext ever leaves your device.

---

## Download & Install

Head to the [**latest release**](https://github.com/jyotiradityajmj1810/sap-dev-journal-releases/releases/latest) and download one of:

| Installer | When to use |
|---|---|
| `SAP DevJournal_<version>_x64-setup.exe` | **Recommended.** Standard Windows installer (NSIS). |
| `SAP DevJournal_<version>_x64_en-US.msi` | Alternative MSI installer for group-policy / silent installs. |

### Install steps
1. Download the `.exe` installer.
2. Double-click to run. Windows SmartScreen may show "Windows protected your PC" — click **More info** → **Run anyway** (the app is unsigned).
3. Follow the installer prompts. The app installs to `C:\Program Files\SAP DevJournal\`.
4. Launch **SAP DevJournal** from the Start Menu.

### First-run setup
1. Set a **passphrase** — this encrypts everything on disk. There is no recovery; write it down.
2. (Optional) Enable **Cloud Sync** in Settings to back up encrypted data to your own GitHub repo.

The passphrase is entered one character per box. It can be any length (8 character minimum) — the boxes grow as you type. Use the eye icon to reveal what you have typed.

---

## Using the app

### Home
Quick overview of recent entries, upcoming reminders, and pinned notes.

### Notes
Free-form markdown notes. `⌘/Ctrl + N` to create.

### Journal
Timestamped daily entries. Great for dev logs, retros, decisions, and postmortems.

### Dashboard
Charts for writing streaks, tags used, and time-of-day activity.

The **writing streak** heatmap counts any activity that day — creating a journal entry, editing one, creating a note, or editing a note. Click a highlighted day to expand a panel listing exactly what changed, with the time and a link to open each item.

### Tags & Graph View
Every entry can be tagged. Graph View shows how notes connect via shared tags and backlinks.

### Cloud Sync (GitHub)
Optional end-to-end encrypted sync to a GitHub repo you control.

1. Create a GitHub **classic** Personal Access Token with the `repo` scope: https://github.com/settings/tokens
2. In **Settings → Cloud Sync**, paste the token, click **Verify & Continue**.
3. Pick an existing repo or create a new one (e.g. `my-journal`) to hold your encrypted sync data.
4. Click **Connect**. From then on, use **Sync Now**, **Push**, or **Pull**.

Only encrypted ciphertext is ever pushed — GitHub cannot read your entries.

**What syncs:** journal entries, notes, tags, and your preferences — theme (light/dark mode, accent colour, font, density) and display name.

**What does not sync:** your passphrase. It is the encryption key, so sending it would defeat the zero-knowledge design — anyone with repo access could then decrypt everything. Type the same passphrase on each device instead; it never leaves the machine.

### Keyboard shortcuts
| Shortcut | Action |
|---|---|
| `⌘/Ctrl + K` | Open command palette |
| `⌘/Ctrl + N` | New entry |
| `⌘/Ctrl + S` | Save |
| `⌘/Ctrl + /` | Focus search |
| `⌘/Ctrl + D` | Toggle dark mode |

---

## Data & privacy

- **Encryption:** AES-256-GCM with a key derived from your passphrase (Argon2id).
- **Storage:** Local IndexedDB (browser build) or app-data folder (desktop build).
- **Sync:** Zero-knowledge — the app encrypts before upload; keys never leave your device.
- **Uninstall:** removes the app; **your data folder is preserved** at `%APPDATA%\com.sapdevjournal.app\` — delete manually if you want a full wipe.

---

## Troubleshooting

**"Failed to fetch" when connecting Cloud Sync**
Your token is missing the `repo` scope, or you used a fine-grained token without repo permission. Create a classic token with `repo`.

**Windows SmartScreen blocks the installer**
The build is not code-signed. Click **More info → Run anyway**. If you need signed builds for enterprise use, open an issue.

**App won't launch**
Ensure **Microsoft Edge WebView2 Runtime** is installed (usually pre-installed on Windows 10/11). Download: https://developer.microsoft.com/microsoft-edge/webview2/

---

## Reporting issues

This repo holds installers only. For bug reports and feature requests, open an issue here — logs from `%APPDATA%\com.sapdevjournal.app\logs\` help a lot.
