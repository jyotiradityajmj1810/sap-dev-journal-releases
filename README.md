# SAP DevJournal — Desktop App

A local-first, privacy-focused developer journaling app for **Windows, macOS, and Linux**.

Your entries are encrypted with **AES-256-GCM** and stored locally. Optional GitHub sync is **zero-knowledge** — only ciphertext ever leaves your device.

---

## Download & Install

Head to the [**latest release**](https://github.com/jyotiradityajmj1810/sap-dev-journal-releases/releases/latest) and pick the file for your system.

> **None of the builds are code-signed.** Every platform will warn you on first launch, and each one needs a different click-through to get past it. The steps below cover it.

### Windows

| Installer | When to use |
|---|---|
| `SAP DevJournal_<version>_x64-setup.exe` | **Recommended.** Standard installer (NSIS). |
| `SAP DevJournal_<version>_x64_en-US.msi` | For group-policy / silent installs. |

1. Download the `.exe` and double-click it.
2. SmartScreen will show "Windows protected your PC" — click **More info** → **Run anyway**.
3. Installs to `C:\Program Files\SAP DevJournal\`. Launch from the Start Menu.

Requires **Microsoft Edge WebView2 Runtime**, pre-installed on current Windows 10 and 11.

### macOS

| Installer | When to use |
|---|---|
| `SAP DevJournal_<version>_aarch64.dmg` | **Apple Silicon** — M1, M2, M3, M4. |
| `SAP DevJournal_<version>_x64.dmg` | **Intel** Macs. |

Not sure which you have: **&#63743; menu → About This Mac**. "Apple M*" means Apple Silicon; "Intel" means the x64 build.

1. Open the `.dmg` and drag **SAP DevJournal** into **Applications**.
2. Launch it. macOS will refuse, saying the app **"is damaged and can't be opened"**.

   The app is not damaged. macOS attaches a quarantine flag to un-notarized downloads, and on Apple Silicon it reports that as damage rather than as a signature warning. Strip the flag:

   ```sh
   xattr -cr "/Applications/SAP DevJournal.app"
   ```

3. Open it again — it launches normally from then on.

Right-click → Open, the usual workaround for unsigned apps, does **not** clear this on Apple Silicon. The `xattr` command is the one that works.

Requires **macOS 10.15 Catalina** or later.

### Linux

| Installer | When to use |
|---|---|
| `sap-dev-journal_<version>_amd64.AppImage` | **Any distro.** Self-contained, nothing to install. |
| `sap-dev-journal_<version>_amd64.deb` | Debian, Ubuntu, Mint, Pop!\_OS, Zorin. |
| `sap-dev-journal-<version>-1.x86_64.rpm` | Fedora, RHEL, openSUSE, Rocky, Alma. |

**AppImage** — works anywhere, bundles its own WebKit:

```sh
chmod +x sap-dev-journal_*_amd64.AppImage
./sap-dev-journal_*_amd64.AppImage
```

**Debian / Ubuntu family:**

```sh
sudo apt install ./sap-dev-journal_*_amd64.deb
```

**Fedora / RHEL family:**

```sh
sudo dnf install ./sap-dev-journal-*.x86_64.rpm
```

The `.deb` and `.rpm` depend on **webkit2gtk 4.1**, which your package manager pulls in automatically. If your distro ships only the older 4.0 (Ubuntu 20.04 and similar), use the **AppImage** instead — it carries its own copy and does not care what the system has.

x86_64 only for now. No ARM64 Linux build — open an issue if you need one.

### First-run setup

1. A short **welcome flow** introduces the app and ends with a palette and light/dark picker. Arrow keys or the dots move between steps; **Skip** exits at any point. It only appears once.
2. Set a **passphrase** — this encrypts everything on disk. There is no recovery; write it down.
3. (Optional) Enable **Cloud Sync** in Settings to back up encrypted data to your own GitHub repo.

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

1. Create a GitHub [**classic** Personal Access Token](https://github.com/settings/tokens/new?scopes=repo&description=DevJournal+Sync) with the `repo` scope.

   Fine-grained tokens will not work: the in-app repo picker lists and creates repositories on your account, which a repo-scoped fine-grained token cannot do.
2. In **Settings → Cloud Sync**, paste the token, click **Verify & Continue**.
3. Pick an existing repo or create a new one (e.g. `my-journal`) to hold your encrypted sync data.
4. Click **Connect**. From then on, use **Sync Now**, **Push**, or **Pull**.

Only encrypted ciphertext is ever pushed — GitHub cannot read your entries.

**What syncs:** journal entries, notes, tags, and your preferences — theme (light/dark mode, accent colour, font, density) and display name.

**What does not sync:** your passphrase. It is the encryption key, so sending it would defeat the zero-knowledge design — anyone with repo access could then decrypt everything. Type the same passphrase on each device instead; it never leaves the machine.

Sync works across platforms — the encrypted format is identical on Windows, macOS, and Linux.

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

- **Sync encryption:** AES-256-GCM with a key derived from your passphrase. Only ciphertext is uploaded — GitHub cannot read your entries.
- **When a passphrase is set, local data is encrypted at rest** with AES-256-GCM — entries are stored as opaque ciphertext, so reading the database file directly reveals nothing. Existing plaintext data is migrated on the first unlock after upgrading.
- **Without a passphrase there is no encryption.** The app cannot encrypt what it has no key for, so if you skip passphrase setup your entries are stored in the clear. Removing a passphrase later decrypts everything back to plaintext by design.
- **Keys never leave your device.** There is no recovery: if you forget the passphrase, nobody can decrypt your synced data, including you.

### Where your data lives

| Platform | Location |
|---|---|
| Windows | `%APPDATA%\com.sapdevjournal.app\` |
| macOS | `~/Library/Application Support/com.sapdevjournal.app/` |
| Linux | `~/.local/share/com.sapdevjournal.app/` |

**Uninstalling leaves this folder in place** on every platform, so reinstalling keeps your entries. Delete it by hand if you want a full wipe — and note that with no passphrase set, that folder is plaintext.

---

## Troubleshooting

**macOS: "SAP DevJournal is damaged and can't be opened"**
Expected on an unsigned download — the app is fine. Run `xattr -cr "/Applications/SAP DevJournal.app"` and open it again. Right-click → Open does not clear this on Apple Silicon.

**Linux: AppImage will not start**
Make sure it is executable (`chmod +x`). If it still fails, some minimal systems lack FUSE — either install `libfuse2`, or extract and run directly:
```sh
./sap-dev-journal_*.AppImage --appimage-extract
./squashfs-root/AppRun
```

**Linux: `.deb` or `.rpm` complains about webkit2gtk**
Your distro is on webkit2gtk 4.0 and the package needs 4.1. Use the AppImage, which bundles its own.

**Windows: SmartScreen blocks the installer**
Not code-signed. **More info → Run anyway**. If you need signed builds for enterprise rollout, open an issue.

**Windows: app will not launch**
Ensure **Microsoft Edge WebView2 Runtime** is installed (usually pre-installed on Windows 10/11): https://developer.microsoft.com/microsoft-edge/webview2/

**"Failed to fetch" or "Bad credentials" when connecting Cloud Sync**
You are almost certainly using a fine-grained token. Cloud Sync needs a **classic** token with the `repo` scope — the repo picker lists and creates repositories on your account, which fine-grained tokens cannot do.

---

## Reporting issues

This repo holds installers only. For bug reports and feature requests, open an issue here.

Please include your **OS and version**, the **installer** you used, and logs from the data folder listed above under `logs/`.

The macOS and Linux builds are newer than the Windows one and have seen far less real-world use — if something is broken on those, an issue is genuinely useful.
