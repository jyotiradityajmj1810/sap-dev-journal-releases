# SAP DevJournal — Desktop App

A local-first, privacy-focused developer journaling app for **Windows and Linux**.

Your entries are encrypted with **AES-256-GCM** and stored locally. Optional GitHub sync is **zero-knowledge** — only ciphertext ever leaves your device.

---

## Download & Install

Grab the file for your system from the [**latest release**](https://github.com/jyotiradityajmj1810/sap-dev-journal-releases/releases/latest).

Nothing is code-signed, so both platforms warn on first launch. The steps below get past it.

### Windows

| Installer | When to use |
|---|---|
| `SAP DevJournal_<version>_x64-setup.exe` | **Recommended.** Standard installer (NSIS). |
| `SAP DevJournal_<version>_x64_en-US.msi` | For group-policy / silent installs. |

Double-click the `.exe`. SmartScreen will say "Windows protected your PC" — click **More info** → **Run anyway**. Installs to `C:\Program Files\SAP DevJournal\`.

Needs **Microsoft Edge WebView2 Runtime**, pre-installed on current Windows 10 and 11. If the app will not start, [install it manually](https://developer.microsoft.com/microsoft-edge/webview2/).

### Linux

x86_64 only. Open an issue if you need ARM64.

| Installer | When to use |
|---|---|
| `SAP.DevJournal_<version>_amd64.AppImage` | **Any distro.** Self-contained, nothing to install. |
| `SAP.DevJournal_<version>_amd64.deb` | Debian, Ubuntu, Mint, Pop!\_OS, Zorin. |
| `SAP.DevJournal-<version>-1.x86_64.rpm` | Fedora, RHEL, openSUSE, Rocky, Alma. |

**AppImage** — bundles its own WebKit, so it does not care what the system has:

```sh
chmod +x SAP.DevJournal_*_amd64.AppImage
./SAP.DevJournal_*_amd64.AppImage
```

**Debian / Ubuntu:**

```sh
sudo apt install ./SAP.DevJournal_*_amd64.deb
```

**Fedora / RHEL:**

```sh
sudo dnf install ./SAP.DevJournal-*.x86_64.rpm
```

The `.deb` and `.rpm` need **webkit2gtk 4.1**, which your package manager pulls in automatically. Distros shipping only 4.0 (Ubuntu 20.04 and similar) cannot run them — **use the AppImage there**.

If the AppImage will not start, your system may lack FUSE. Either install `libfuse2`, or extract and run it directly:

```sh
./SAP.DevJournal_*.AppImage --appimage-extract
./squashfs-root/AppRun
```

### First run

1. A short **welcome flow** introduces the app and ends with a palette and light/dark picker. Arrow keys or the dots move between steps; **Skip** exits. It only appears once.
2. Set a **passphrase** — this encrypts everything on disk. There is no recovery; write it down.

The passphrase is entered one character per box, any length from 8 up — the boxes grow as you type. The eye icon reveals what you typed.

---

## Using the app

**Journal** holds timestamped daily entries — dev logs, retros, decisions, postmortems. **Notes** are free-form markdown. Both can be tagged, and **Graph View** shows how they connect through shared tags and backlinks.

**Dashboard** charts writing streaks, tags, and time-of-day activity. The streak heatmap counts any activity that day — creating or editing an entry or a note. Click a highlighted day to see exactly what changed, with times and links to each item.

### Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `⌘/Ctrl + K` | Open command palette |
| `⌘/Ctrl + /` | Open command palette (same as above) |
| `⌘/Ctrl + N` | New entry — opens the template picker |
| `⌘/Ctrl + S` | Save (while editing) |
| `⌘/Ctrl + D` | Toggle dark mode |

### Cloud Sync (GitHub)

Optional end-to-end encrypted sync to a repo you control.

1. Create a GitHub [**classic** Personal Access Token](https://github.com/settings/tokens/new?scopes=repo&description=DevJournal+Sync) with the `repo` scope.

   Fine-grained tokens will not work. The in-app repo picker lists and creates repositories on your account, which a repo-scoped fine-grained token cannot do. A "Failed to fetch" or "Bad credentials" error on connect almost always means a fine-grained token.
2. In **Settings → Cloud Sync**, paste the token and click **Verify & Continue**.
3. Pick or create a repo to hold the encrypted data.
4. Click **Connect**, then use **Sync Now**, **Push**, or **Pull**.

**What syncs:** entries, notes, tags, and preferences — theme, accent colour, font, density, display name.

**What does not:** your passphrase. It is the encryption key, so syncing it would defeat the zero-knowledge design — anyone with repo access could decrypt everything. Type the same passphrase on each device; it never leaves the machine.

The encrypted format is identical on both platforms, so Windows and Linux sync to each other.

---

## Data & privacy

- **Sync encryption:** AES-256-GCM with a key derived from your passphrase. Only ciphertext is uploaded — GitHub cannot read your entries.
- **With a passphrase set, local data is encrypted at rest**, also AES-256-GCM. Entries are stored as opaque ciphertext, so reading the database file directly reveals nothing. Existing plaintext data is migrated on the first unlock after upgrading.
- **Without a passphrase there is no encryption.** The app cannot encrypt what it has no key for, so if you skip passphrase setup your entries are stored in the clear. Removing a passphrase later decrypts everything back to plaintext by design.
- **Keys never leave your device.** There is no recovery: if you forget the passphrase, nobody can decrypt your data, including you.

### Where your data lives

| Platform | Location |
|---|---|
| Windows | `%APPDATA%\com.sapdevjournal.app\` |
| Linux | `~/.local/share/com.sapdevjournal.app/` |

**Uninstalling leaves this folder in place**, so reinstalling keeps your entries. Delete it by hand for a full wipe — and note that with no passphrase set, its contents are plaintext.

---

## Reporting issues

This repo holds installers only. Open an issue here with your **OS and version**, the **installer** you used, and any logs from the `logs/` subfolder of the data directory above.

The Linux builds are new and have seen far less real-world use than the Windows ones, so reports against them are especially useful.
