# Stacks — a single-file digital library system

A complete digital asset / document library that runs as **one HTML file**. No build step, no server,
no accounts, no dependencies, no tracking. Open it and it works — your data stays in your browser.

![status](https://img.shields.io/badge/status-working-brightgreen) ![deps](https://img.shields.io/badge/dependencies-none-blue) ![size](https://img.shields.io/badge/single%20file-112%20KB-informational)

## Run it

| Way | What to do |
| --- | --- |
| **Hosted (GitHub Pages)** | Open `https://<your-username>.github.io/<repo-name>/` after enabling Pages (below). |
| **Download & open** | Save `index.html`, double-click it. Works offline from `file://`. |
| **Local server** | `python3 -m http.server 8000` in this folder → http://localhost:8000 |

On first run: **Add asset**, **Import** a CSV/JSON file ([`sample-catalog.csv`](sample-catalog.csv) is a working example), or **Load demo library** to try 14 filled-in items.

> ⚠️ Data lives in your browser's `localStorage` for that origin/file location. Clearing browser data deletes it, and each browser keeps its own copy. **Export a JSON backup** (Export & backup → Download backup) after any big change — that file is your portable copy, restorable on any device.

## Features

- **Catalog any format** — books, e-books, PDFs, documents, articles, video, audio, images, datasets, maps, other; with creator, year, tags, collection, identifier (ISBN/DOI/call no.), URL or file path, notes, favourites.
- **Search & filter** — full-text across title, creator, tags, notes, URL, identifier, collection; filters by type / status / collection / favourites; five sort orders.
- **Two views** — cover grid or sortable table, remembered per browser.
- **Circulation** — check out with borrower and due date (loan period configurable), renew, check in, automatic overdue detection, due-soon warnings.
- **Collections** — colour-coded grouping with descriptions, edit/delete, per-collection browsing.
- **Import** — CSV, TSV, JSON; automatic column mapping (Title/Author/Format/Keywords/Category… recognised), editable mapping, row preview, duplicate skipping, auto-created collections, type guessing from file extensions.
- **Export** — full JSON backup + restore (replace or merge), CSV catalog, copy JSON, print catalogue.
- **Dashboard** — totals, on loan, overdue, favourites, activity feed, add-rate chart, due-soon list, breakdown by type.
- Light/dark theme, keyboard shortcuts (`/` search, `n` new asset, `Esc` close), responsive down to phone width, accessible focus states.

## Import format

Any header close to these is matched automatically — and you can remap columns in the UI before importing:

| Field | Recognised headers |
| --- | --- |
| title | title, name, item, asset, filename |
| creator | creator, author(s), artist, director, by |
| type | type, format, kind, media type |
| year | year, date, published |
| tags | tags, keywords, subjects |
| collection | collection, category, folder, shelf |
| description | description, notes, summary, abstract |
| url | url, link, location, path, file path |
| isbn | isbn, identifier, doi, call number, barcode |
| status | status, availability (`available` / `on loan` / `reference` / `missing`) |
| dueDate | due, due date, return by |
| holder | holder, borrower, on loan to |

JSON import takes either an array of objects or `{ "assets": [...] }` — a Stacks backup works directly.

## Put it on GitHub (from this folder)

Full click-by-click walkthrough, including the no-terminal route and troubleshooting: **[HOSTING.md](HOSTING.md)**.

```bash
git init -b main
git add .
git commit -m "Stacks: single-file digital library system"
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

With the GitHub CLI it is two commands:

```bash
gh repo create stacks-library --public --source=. --remote=origin --push
```

## Publish with GitHub Pages

The included workflow [`.github/workflows/pages.yml`](.github/workflows/pages.yml) deploys on every push.

1. Push the repo (above).
2. Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. Push again (or run the workflow from the **Actions** tab). Your app appears at
   `https://<your-username>.github.io/<repo-name>/`.

Prefer the simple route? Skip the workflow: **Settings → Pages → Deploy from a branch → `main` / root** — same result for a static file.

## Notes & limits

- This version **catalogues** assets and points at them via the URL / file path field; it does not upload file contents. A browser-only file can't hold a real document store — that needs a backend (e.g. Node + SQLite + a file folder). The data model already has the fields for it, so that's a contained upgrade.
- No login or multi-user sync by design; it's a single-user, local-first tool.
- Tested in Chromium; works offline once loaded (no external fonts, scripts, or images).

## License

[MIT](LICENSE) — do what you like, attribution appreciated.
