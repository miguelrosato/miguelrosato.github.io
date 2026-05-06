# miguelrosato.github.io

Personal site of **Miguel Rosato, CEng FEI** — Completions & Workover Engineering Lead, Basra Energy Company Limited.

Built with [Quarto](https://quarto.org). Hosted on [GitHub Pages](https://pages.github.com).

Live site: <https://miguelrosato.github.io>

---

## What this repo is

A multi-section personal portfolio bridging twenty-five years of HPHT, deepwater, and workover engineering with applied data analytics work from the Imperial College Data Analytics Professional Certificate. Built originally as the deliverable for Module 23 (*Publishing an interactive site or dashboard*); engineered to be the long-term canonical public surface for CV, publications, speaking, and analytics case studies.

## Repository layout

```
miguelrosato.github.io/
├── _quarto.yml                 # Site config: navbar, theme, build options
├── index.qmd                   # Homepage (assignment-required "About you")
├── about.qmd                   # Full About / career chronology
├── niches/
│   ├── hpht.qmd                # HPHT Completions & Well Testing
│   ├── deepwater.qmd           # Deepwater Subsea Completions
│   └── workover.qmd            # Workover Engineering
├── analytics/
│   ├── index.qmd               # Analytics overview
│   └── capstone.qmd            # Featured Imperial Capstone case study
│                               #   (holds the embedded interactive)
├── publications.qmd            # SPE technical papers
├── speaking.qmd                # Speaking, training, Distinguished Lecturer
├── cv.qmd                      # CV summary + PDF link
├── assets/
│   ├── interactive_wo_milp.html  # The Plotly interactive (centerpiece)
│   ├── styles.scss             # Imperial-navy theme variables
│   ├── styles.css              # Minor CSS overrides
│   ├── favicon.svg
│   ├── profile.jpg             # YOU NEED TO ADD THIS — see below
│   └── MiguelRosato_CV.pdf     # YOU NEED TO ADD THIS — see below
├── .github/workflows/publish.yml  # GH Actions: auto-render on push
├── .gitignore
└── README.md
```

---

## One-time local setup (your Mac)

You already have Python and VS Code. You need Quarto:

```bash
# Install Quarto via Homebrew (preferred):
brew install --cask quarto

# Verify:
quarto --version       # should print 1.4.x or later
```

Install the Python packages Quarto needs to render notebooks/Plotly:

```bash
pip install jupyter plotly pandas numpy --break-system-packages
```

(If you prefer a virtual environment, use one — the GitHub Actions workflow does its own clean install regardless.)

---

## Local preview (recommended — see the site before pushing)

From inside the repo folder:

```bash
quarto preview
```

This opens a live-reloading browser tab. Edit any `.qmd` file, save, and the tab refreshes automatically. Stop the preview with `Ctrl+C` in the terminal.

To do a one-shot render (without the live preview server) into the local `_site/` folder:

```bash
quarto render
```

`_site/` is gitignored — you do not commit the rendered HTML. GitHub Actions renders in the cloud on every push.

---

## Two files you need to add before going live

### `assets/profile.jpg`

A professional headshot, square, ideally 600×600 px. Drop it into `assets/` with the filename `profile.jpg`. The homepage About-block on `index.qmd` references this exact path.

If you do not have one ready, the easiest path is: take a clean LinkedIn-style photo against a plain background, crop square, save as `profile.jpg`. Until you add it, Quarto will show a broken image icon on the homepage — non-fatal but visible.

### `assets/MiguelRosato_CV.pdf`

Your most current PDF CV. Drop it into `assets/` with that exact filename. Linked from both the homepage and the dedicated [CV page](cv.qmd). Update it any time — overwrite, commit, push, the link automatically points to the new version.

---

## Publishing workflow

This repo is set up for **GitHub Actions** to render and deploy automatically on every push to `main`.

### One-time GitHub setup

1. **Create the GitHub repository.** On github.com (signed in as `miguelrosato`), create a new public repository named exactly `miguelrosato.github.io` — the name must match your username for it to serve at the root domain.

2. **Push this folder as the initial commit:**
   ```bash
   cd /path/to/miguelrosato.github.io
   git init -b main
   git add .
   git commit -m "Initial site — Imperial Module 23 deliverable"
   git remote add origin https://github.com/miguelrosato/miguelrosato.github.io.git
   git push -u origin main
   ```
   GitHub will prompt for authentication. Use either:
   - A **Personal Access Token** (recommended): Settings → Developer settings → Personal access tokens → Fine-grained tokens → New token, scope only this one repo with "Contents: read & write", set expiration to 90 days, copy the token, and use it as your password when git prompts. Revoke any time.
   - An **SSH key**: `ssh-keygen -t ed25519 -C "miguel.rosato@me.com"` then add `~/.ssh/id_ed25519.pub` to GitHub Settings → SSH and GPG keys, and change the remote to `git@github.com:miguelrosato/miguelrosato.github.io.git`.

3. **Configure GitHub Pages to use Actions:** In the repo on github.com → Settings → Pages → "Build and deployment" → Source = **GitHub Actions** (not "Deploy from a branch"). This is critical — without this step, the workflow runs but Pages does not serve the result.

4. **Wait ~2 minutes** for the first Actions run. Watch progress in the repo's Actions tab. When it goes green, your site is live at `https://miguelrosato.github.io`.

### Day-to-day workflow

```bash
# Edit any .qmd file in VS Code
# Preview locally:
quarto preview

# When happy:
git add .
git commit -m "Update Capstone case study with Section 12 results"
git push
```

GitHub Actions re-renders the site and re-deploys to Pages automatically. Site updates within 2–3 minutes of push.

---

## Adding the Imperial Capstone notebook as a live page

When Section 12 (ESP Failure Predictive Analysis) is complete, you can publish your `Capstone_Project_MRosato.ipynb` directly as a Quarto-rendered web page — the notebook becomes a first-class section of the site, with live Plotly charts, formatted equations, and optional code visibility.

```bash
# Copy (or symlink) the notebook into the analytics/ folder:
cp /path/to/Capstone_Project_MRosato.ipynb analytics/capstone_full.ipynb

# Add YAML metadata to the top of the notebook (first cell, raw):
# ---
# title: "Capstone — Full Notebook"
# subtitle: "End-to-end Imperial College submission, anonymised."
# format:
#   html:
#     code-fold: true     # readers can toggle code visibility
#     toc: true
# ---

# Then add it to _quarto.yml under the navbar Data Analytics menu.
```

**IP firewall reminder**: before publishing the full notebook, anonymise well names, exact lat/lon, per-well production rates, and any cost data that is not part of your already-disclosed CV record. Methodology, MILP formulation, feature engineering, and headline outcomes are publishable per your existing IP agreement. Confidential employer dataset is not.

---

## Troubleshooting

**The GitHub Actions build fails with "command not found: jupyter"**
The workflow installs jupyter in the Python step. If you renamed the workflow file or stripped that step, restore the `pip install jupyter plotly pandas numpy` line in `.github/workflows/publish.yml`.

**The interactive iframe is blank on the live site**
Ensure `assets/interactive_wo_milp.html` is committed (it's in this repo). If you want to regenerate it after editing the data, re-run `python build_interactive.py` (the script lives in the parent build folder).

**The site builds but uses the wrong theme / no styling**
Check that `assets/styles.scss` is committed and referenced correctly in `_quarto.yml` (under `format.html.theme.light`).

**404 at https://miguelrosato.github.io after the workflow ran green**
Three usual causes: (a) Settings → Pages → Source is still set to "Deploy from a branch" instead of "GitHub Actions"; (b) the repo is named anything other than exactly `miguelrosato.github.io`; (c) the repo is private and you do not have GitHub Pro.

---

## Licence

The site source (Quarto sources, CSS, build scripts) is released under MIT. The personal content (CV, biographical text, photographs) is © Miguel Rosato 2026, all rights reserved.
