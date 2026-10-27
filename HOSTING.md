# Putting Stacks on GitHub — step by step

Two routes. **Route A** needs no terminal (browser only, ~5 minutes). **Route B** uses git and takes
about 60 seconds. Either way you end up with the same repo and a live link.

Files that should end up in the repo:

```
index.html                      ← the whole app
README.md                       ← docs
LICENSE                         ← MIT (put your name in it)
sample-catalog.csv              ← example import file
.gitignore
.nojekyll                       ← stops GitHub's Jekyll processing
.github/workflows/pages.yml     ← auto-deploy to GitHub Pages
```

Everything is already committed locally in `library/` as commit `5aa7d40`, so Route B is just "add remote, push".

---

## Route A — browser only (no terminal)

**Step 1 · Create the repository**
1. Sign in at <https://github.com>.
2. Top-right **+** → **New repository**.
3. Repository name: `stacks-library` (any name works).
4. Visibility: **Public** (required for free GitHub Pages; Private needs a paid plan for Pages).
5. Do **not** tick "Add a README" — you already have one.
6. **Create repository**.

**Step 2 · Upload the files**
1. Download `stacks-github-ready.zip` from this workspace and unzip it on your computer.
2. On the new repo page click **uploading an existing file** (or **Add file → Upload files**).
3. Drag in everything from the unzipped folder **including the hidden `.github` folder and `.gitignore`**.
   - Tip: on macOS press ⌘+Shift+. in Finder to see hidden files; on Windows, View → Hidden items.
   - If your file manager refuses to drag hidden items, skip them for now and do Step 2b.
4. Commit message: `Stacks: single-file digital library system` → **Commit changes**.

**Step 2b · Only if `.github/` did not upload**
1. In the repo click **Add file → Create new file**.
2. Name it exactly: `.github/workflows/pages.yml`
3. Paste the contents of `library/.github/workflows/pages.yml` → **Commit changes**.
4. Repeat with **Create new file** named `.nojekyll` and a single blank line, so Jekyll is skipped.

**Step 3 · Turn on GitHub Pages**
1. Repo **Settings** (top bar, gear icon).
2. Left sidebar → **Pages**.
3. Under **Build and deployment → Source**, choose **GitHub Actions**.
   *(If you skipped the workflow file: instead choose **Deploy from a branch** → Branch `main` → folder `/ (root)` → **Save**.)*
4. Nothing else to configure — no build command, no output folder. It is a static file.

**Step 4 · Trigger the first deploy**
1. Go to the **Actions** tab.
2. If a run is already in progress, wait for it. If there is none, open **Deploy to GitHub Pages** →
   **Run workflow** → **Run workflow** (branch `main`).
3. Wait for the green tick (usually 20–60 seconds).

**Step 5 · Open your library**
1. Back on **Settings → Pages**, the banner now shows *Your site is live at …*.
2. Visit `https://<your-username>.github.io/stacks-library/`
3. Bookmark it. That link is your library, from any device.

---

## Route B — terminal (git)

**Step 1 · Create an empty repo on GitHub**
<https://github.com/new> → name `stacks-library` → **Public** → no README → **Create repository**.
Copy the HTTPS URL it shows you.

**Step 2 · Point the existing local repo at it and push**

```bash
cd library
git remote add origin https://github.com/<your-username>/stacks-library.git
git branch -M main
git push -u origin main
```

If git asks for a password, it needs a **Personal Access Token**, not your account password:
GitHub → *Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token (classic)*
→ tick **repo** → copy the token → paste it as the password.
Or install the GitHub CLI and run `gh auth login` once, then:

```bash
gh repo create stacks-library --public --source=. --remote=origin --push
```

**Step 3 · Enable Pages**
Repo → **Settings → Pages → Source: GitHub Actions**. Then:

```bash
git commit --allow-empty -m "Trigger Pages deploy" && git push
```

or run the workflow from the **Actions** tab.

---

## Finish line — check three things

1. **The site loads:** `https://<your-username>.github.io/<repo>/` shows the *Welcome to Stacks* screen.
2. **Storage works there:** click **Load demo library**, refresh the page — the 14 items should still be there.
   If a yellow banner appears saying storage is blocked, you are inside a preview frame; open the URL in its own tab.
3. **Pages source is right:** *Settings → Pages* shows the live URL and source `GitHub Actions`.

---

## Afterwards

**Moving your real catalog to the hosted version**
The hosted site is a different origin, so it starts empty. In your local copy: **Export & backup →
Download backup**, then on the hosted site: **Export & backup → Restore from file** (choose *replace*).
Do this again whenever you want to sync them.

**Remember:** whenever you change data in the app it saves to *your browser only* — it does not write to
the repo. Only code changes go through git.

**Updating the app later**

```bash
# edit library/index.html, then
git add -A && git commit -m "Update library app" && git push
```

Pages redeploys automatically in under a minute.

**Custom domain (optional)**
Buy/own a domain → repo **Settings → Pages → Custom domain** → enter `library.example.com` →
GitHub creates a `CNAME` file → at your DNS provider add a `CNAME` record pointing your subdomain to
`<your-username>.github.io`. Tick **Enforce HTTPS** once the certificate is issued.

---

## If something goes wrong

| Symptom | Cause / fix |
| --- | --- |
| Pages says "Not found" | Wait for the green Actions run, then hard-refresh (Ctrl/⌘+Shift+R). |
| Actions run fails at `configure-pages` | Set **Settings → Pages → Source: GitHub Actions** first, then re-run. |
| Site shows the README instead of the app | `index.html` is missing or misnamed — it must sit at the repo root. |
| 404 on `pages.yml` in the Actions tab | The file must be at `.github/workflows/pages.yml` (folders are case-sensitive). |
| Changes don't appear | Check the Actions tab: the deploy takes 20–60s, and browsers cache aggressively. |
| "Public repo required" | Private repos need a paid plan for Pages — make the repo public or host elsewhere. |
| Data vanished | Clearing browser data wipes `localStorage`. Restore from a backup JSON. |

## Is it safe to make public?

Yes. `index.html` contains only the app; your catalog lives in your browser and never touches the repo
or the hosted page. Nothing in the repo is secret. If you later add a backend with credentials, keep
those in repository **Secrets**, never in the code.
