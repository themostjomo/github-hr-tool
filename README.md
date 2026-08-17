```
#...# ##### #...# .###.   ..### .###. #...# .###.
#..#. ..#.. ##..# #....   ....# #...# ##.## #...#
###.. ..#.. #.#.# #.###   ....# #...# #.#.# #...#
#..#. ..#.. #..## #...#   #...# #...# #...# #...#
#...# ##### #...# .###.   .###. .###. #...# .###.
```
<p align="center"><b>GitHub Batch Profile Checker</b> — built for DevRel & HR workflows, by King Jomo</p>

<p align="center">
  <img alt="python" src="https://img.shields.io/badge/python-3.8%2B-blue">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-green">
  <img alt="platform" src="https://img.shields.io/badge/platform-macOS%20%7C%20Windows%20%7C%20Linux%20%7C%20Android-lightgrey">
</p>

---

## What this is

A command-line tool that batch-checks GitHub usernames and pulls back public
profile signal useful for developer-relations and HR/technical-screening
workflows: account age, bio, company, location, public repo count, total
stars across owned repos, top languages, followers, and a recent public
activity estimate (public events / pushes in the last ~90 days).

Output can be saved as **CSV**, **XLSX (Excel)**, and/or **TXT** — pick one,
some, or all.

> **Honesty note:** GitHub does not expose a true lifetime commit count for
> any user (not even to themselves) — it would require scanning every repo
> they've ever touched, including private ones. This tool does **not**
> fabricate that number. Instead it reports public repo/star counts and a
> recent-activity estimate from GitHub's public Events API, clearly labeled
> as such. Treat these as activity signals, not exact commit totals.

---

## Features

- Batch-check any number of GitHub usernames in one run
- Works with or without a GitHub token (token raises your rate limit from
  60 req/hr to 5,000 req/hr)
- Reads usernames from direct input or a `.txt` file
- De-duplicates usernames automatically
- Outputs to CSV, XLSX, and/or TXT
- Optional `GITHUB_TOKEN` environment variable for non-interactive/CI use
- No data leaves your machine except calls to GitHub's public API

---

## Repo contents

| File | Purpose |
|---|---|
| `github_profile_checker.py` | The tool itself |
| `requirements.txt` | Python dependencies |
| `usernames.example.txt` | Template for a usernames list — copy to `usernames.txt` and edit |
| `.gitignore` | Keeps venvs, generated reports, and secrets out of git |
| `LICENSE` | MIT license |
| `README.md` | This file |

---

## Requirements

- Python 3.8 or newer
- Internet access
- (Optional, recommended) A GitHub Personal Access Token — no scopes needed,
  since this only reads public data

---

## Setup & usage by platform

### 🍎 macOS (Intel)

1. **Check Python:**
   ```bash
   python3 --version
   ```
   If missing, install from [python.org/downloads](https://www.python.org/downloads/)
   (choose the macOS installer) or via Homebrew: `brew install python3`.

2. **Clone the repo:**
   ```bash
   git clone https://github.com/<your-username>/github-hr-tool.git
   cd github-hr-tool
   ```

3. **Create and activate a virtual environment** (macOS's system Python is
   "externally managed," so a venv avoids install errors):
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

4. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

5. **Run it:**
   ```bash
   python3 github_profile_checker.py
   ```

6. Next time you come back, just `cd` into the folder and
   `source venv/bin/activate` again — no need to redo steps 3–4.

### 🍎 macOS (Apple Silicon / M1–M4)

Identical to the Intel steps above. Python installed via python.org or
Homebrew is universal/ARM-native — no special flags needed.

### 🪟 Windows

1. **Install Python:** download from
   [python.org/downloads](https://www.python.org/downloads/). During
   install, **check "Add python.exe to PATH"** — this avoids most
   `'python' is not recognized` errors later.

2. **Open PowerShell or Command Prompt**, then clone the repo:
   ```powershell
   git clone https://github.com/<your-username>/github-hr-tool.git
   cd github-hr-tool
   ```
   (No `git`? Download the ZIP from the repo's green "Code" button instead,
   extract it, and `cd` into the extracted folder.)

3. **Create and activate a virtual environment:**
   ```powershell
   python -m venv venv
   venv\Scripts\activate
   ```
   In PowerShell, if activation is blocked by execution policy, run:
   ```powershell
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
   ```
   then retry the `venv\Scripts\activate` command.

4. **Install dependencies:**
   ```powershell
   pip install -r requirements.txt
   ```

5. **Run it:**
   ```powershell
   python github_profile_checker.py
   ```

### 🐧 Linux

Same as macOS — `python3 -m venv venv`, `source venv/bin/activate`,
`pip install -r requirements.txt`, `python3 github_profile_checker.py`.
Most distros ship Python 3 already; if not, use your package manager
(`sudo apt install python3 python3-venv python3-pip` on Debian/Ubuntu).

### 📱 Android

Android doesn't run Python scripts natively, but you have two good options:

**Option A — Termux (runs the real script on-device):**
1. Install [Termux](https://f-droid.org/en/packages/com.termux/) from
   F-Droid (recommended over the Play Store version, which is outdated).
2. In Termux:
   ```bash
   pkg update && pkg install python git -y
   git clone https://github.com/<your-username>/github-hr-tool.git
   cd github-hr-tool
   pip install -r requirements.txt
   python github_profile_checker.py
   ```
3. Use Termux's built-in file access (`termux-setup-storage`) if you want
   to save reports to your phone's shared storage instead of Termux's
   private folder.

**Option B — GitHub Codespaces (runs in the cloud, works on any phone
browser):**
1. Open the repo on github.com in your phone's browser.
2. Tap the green **Code** button → **Codespaces** tab → **Create codespace
   on main**.
3. This opens a full cloud dev environment (VS Code in the browser) with a
   terminal. Run the same commands as the Linux instructions above.
4. Download the generated report files directly from the Codespaces file
   browser when done.

### 📱 iOS / iPad / "I just want to try it, no install"

iOS doesn't support running arbitrary Python scripts. Use **GitHub
Codespaces** (Option B above) — it works identically from Safari.

---

## Getting a GitHub token (optional but recommended)

1. Go to **[github.com/settings/tokens](https://github.com/settings/tokens)**.
2. Click **Generate new token → Generate new token (classic)**.
3. Name it anything (e.g. `devrel-checker`), set an expiration date, and
   **leave every scope checkbox unchecked** — this tool only reads public
   data, so no scopes are needed.
4. Click **Generate token** and **copy it immediately** — GitHub only shows
   it once.
5. When the script prompts for it, **paste and press Enter**. The input is
   hidden (no characters/dots appear) — that's expected, not a bug.

**For automation/CI**, skip the interactive prompt entirely by setting an
environment variable before running the script:
```bash
export GITHUB_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxx   # macOS/Linux
setx GITHUB_TOKEN "ghp_xxxxxxxxxxxxxxxxxxxx"   # Windows (new terminal needed after)
python3 github_profile_checker.py
```
Never commit your token to the repo. The included `.gitignore` blocks
common secret file patterns, but the token itself should only ever live in
your terminal session or a local, untracked `.env`.

---

## Usage

```bash
python3 github_profile_checker.py
```

You'll be prompted for:

1. **Token** — paste it, or press Enter to skip (rate-limited to 60 req/hr
   without one).
2. **Usernames** — comma-separated (e.g. `torvalds, gvanrossum, octocat`),
   or the path to a `.txt` file (see `usernames.example.txt` — copy it to
   `usernames.txt`, edit, and enter that filename here).
3. **Output format(s)** — `1` CSV, `2` XLSX, `3` TXT, or `4` for all three.

The tool prints a live summary table as it works, then writes the chosen
report file(s) into the current folder.

---

## Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| `command not found: pip` | `pip` isn't aliased on your system | Use `pip3` or `python3 -m pip` instead |
| `error: externally-managed-environment` | macOS/Linux blocks system-wide pip installs (PEP 668) | Use a virtual environment: `python3 -m venv venv && source venv/bin/activate`, then install again inside it |
| `'python' is not recognized...` (Windows) | Python wasn't added to PATH during install | Reinstall Python and check "Add python.exe to PATH", or use `py` instead of `python` |
| Script prints `zsh: command not found: @username` for every entry | Usernames were pasted one-per-line at the shell prompt instead of at the script's own username prompt | Run the script first, wait for the `>` prompt under "Enter GitHub usernames...", *then* paste — as one comma-separated line, with no `@` symbols |
| `ModuleNotFoundError: No module named 'requests'` (or `openpyxl`) | Dependency not installed in the active environment | Activate your venv, then `pip install -r requirements.txt` |
| Token paste "isn't showing anything" | Input is intentionally hidden (like a password field) | This is expected — paste, then press Enter; nothing will visibly appear |
| `Rate limited / forbidden` errors partway through a large batch | Hit the unauthenticated 60 req/hr limit, or an exhausted token limit | Add a token (or wait for the limit to reset — GitHub shows remaining calls each run via the rate-limit check at startup) |
| `User not found` for a username you're sure exists | Typo, or the account was renamed/deleted | Double-check the exact username in the profile's URL, not their display name |
| XLSX option does nothing / prints an install message | `openpyxl` isn't installed | `pip install openpyxl` (or `pip install -r requirements.txt`) |
| Report file isn't where you expected | Files save to your **current working directory**, not the script's folder if you ran it from elsewhere | `pwd` (or `cd` where you expect) before running, or check the printed "Saved: ..." path |
| `Permission denied` saving the report | Folder is read-only, or a file with that name is open in Excel | Close the open file, or run from a folder you own |
| Windows PowerShell blocks venv activation | Execution policy restricts scripts | `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`, then retry `venv\Scripts\activate` |

If you hit something not listed here, open an
[Issue](../../issues) with the exact command you ran and the full error text.

---

## Privacy & responsible use

This tool only reads **public** GitHub profile data via GitHub's official
API — the same data visible to anyone viewing a profile in a browser. It
doesn't access private repos, private emails, or anything requiring elevated
scopes. Recommended good practice for HR/DevRel use:

- Use profile data as one signal among many, not a sole hiring/evaluation
  criterion.
- Don't store generated reports longer than needed; the `.gitignore`
  excludes them from being committed by default.
- Be transparent with candidates/community members about what public data
  you review, where applicable to your org's policies.

---

## License

MIT — see [LICENSE](LICENSE).
