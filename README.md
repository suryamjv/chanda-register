# Chanda Register

A single-page register for Vinayaka Chavithi collections (chanda) and festival
expenses. Runs as a static site on GitHub Pages — no server, no database,
no build step, and no external dependencies of any kind.

## What it does

- **Collections** — every contributor, this year's amount against last year's
  ledger entry, status, reconciliation notes and phone. Search, filter and sort.
- **Expenses** — the eleven standard festival heads (idol, pooja, tent, lighting,
  sound, annadanam, prasadam, police, visarjan, misc), with amount, who paid and
  payment mode.
- **Dashboard** — collected / spent / balance in hand, progress against target,
  largest contributions vs last year, expense breakdown, collection status.
- **Multi-year** — "Start next year" carries every name forward into a new year
  with amounts cleared and this year's figures set as the new benchmark. It asks
  for confirmation first. Switch between years from the header; a year created by
  mistake can be removed from Settings (the last remaining year cannot be, and a
  year holding recorded amounts warns you with the total before it goes).
- **Excel export** — downloads a three-tab `.xlsx` (Collections, Expenses,
  Summary & Balance) matching the original workbook layout.

## Access and privacy

The repository is public, so the ledger is **not** stored in the clear. `data.json`
holds the register encrypted with AES-256-GCM under a key derived from the access
code (PBKDF2-SHA256, 210,000 iterations). Anyone can fetch the file; without the
code it is meaningless.

Share the site link and the access code with the people who should see the register.
**If the code is lost, the data cannot be recovered** — keep a backup.

To change the access code: open the register, download a backup, then re-encrypt
that JSON under a new code and replace `data.json`.

## Editing

Anyone with the code can edit. Changes are kept in that person's browser until
they are published, so ordinary viewers cannot alter what others see.

To publish changes for everyone, open **Settings** and enter:

- GitHub username and repository
- Branch (usually `main`) and data file path (`data.json`)
- A fine-grained personal access token with **Contents: read and write** on this
  repository

The token is stored only in that browser. A **Publish** button then appears; it
commits the re-encrypted `data.json` straight to the repo, and GitHub Pages serves
the update within a minute or so. Without a token, use **Download backup** and
upload the file to the repository by hand.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire application — markup, styles, logic |
| `data.json` | The encrypted register |

No libraries are loaded at runtime. The spreadsheet writer (a minimal XLSX/ZIP
encoder) and the crypto (Web Crypto API) are built in, so the page works offline
once loaded.
