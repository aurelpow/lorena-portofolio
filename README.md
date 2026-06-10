# Lorena Patri — Portfolio Website

Portfolio personal de Lorena Patri, Compensation & Benefits Analyst based in Madrid.

**Live site:** https://aurelpow.github.io/lorena-portofolio *(activo tras merge a `master`)*

---

## Stack

| Layer | Choice |
|-------|--------|
| Static site generator | Jekyll via `github-pages` gem |
| Hosting | GitHub Pages (auto-deploy on push to `master`) |
| Styling | Custom HTML/CSS — no framework |
| Fonts | Google Fonts (Cormorant Garamond + Jost) |

---

## Project structure

```
.
├── _config.yml            # Production config (baseurl, collections, GA)
├── _config.dev.yml        # Local dev overrides (empty baseurl)
├── Gemfile                # Ruby dependencies
├── index.md               # Home page
├── projects.md            # Projects listing page
├── _layouts/
│   ├── default.html       # Base layout (nav + footer)
│   ├── projects.html      # Projects grid
│   └── project.html       # Individual project detail
├── _includes/
│   ├── head.html          # <head> meta, fonts, CSS
│   ├── nav.html           # Fixed top navigation
│   └── footer.html        # Footer
├── _projects/             # One .md file per project
└── assets/
    ├── css/site.css       # Main stylesheet
    └── img/               # Profile photo and project images
```

---

## Run locally

Requires [Ruby 3.1 via Homebrew](https://brew.sh/):

```bash
brew install ruby@3.1
echo 'export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

Then:

```bash
bundle install
bundle exec jekyll serve --livereload --config _config.yml,_config.dev.yml
```

Open: http://127.0.0.1:4000

---

## Add a new project

1. Duplicate the closest existing file in `_projects/`
2. Edit its frontmatter — the required schema is:

```yaml
---
layout: project
title: "Project title"
year: "2025"
tool: "Excel · Power BI"
featured: true                      # optional
featured_label: "Label text"        # required when featured: true
description: "Short description"
highlights:                         # optional bullet list
  - "Point one"
tags:
  - "Tag A"
accent_tags:                        # subset of tags shown in terracotta
  - "Tag A"
link: "https://..."
link_label: "Ver informe"
---
```

3. No layout files need to be touched — the project appears automatically in the listing and `site.projects.size` updates.

---

## Branch strategy

| Branch | Purpose |
|--------|---------|
| `master` | Production — what GitHub Pages deploys. Merge commits only. |
| `develop` | Integration — all feature branches merge here first. |
| `feature/xxxxx` | One branch per task. Branch from `develop`, merge back to `develop`. |

Commit format on `master` and `develop`: `[CXXX] Short imperative message`

---

## Deployment

Push (or merge) to `master` → GitHub Pages auto-deploys within ~60 seconds.

Repo must be **public** and GitHub Pages configured to deploy from `master / (root)` in:  
*Settings → Pages → Branch → `master` / `/ (root)`*

---

## CMS (Decap) — editing without code

Lorena can add, edit, and delete projects at:

**`https://aurelpow.github.io/lorena-portofolio/admin/`**

She logs in with her GitHub account. No code required.

### One-time setup (Aurélien)

**1. Add Lorena as repo collaborator**  
*GitHub → repo Settings → Collaborators → Add people → Lorena's GitHub username*  
She needs **Write** access so the CMS can commit to `master`.

**2. Register a GitHub OAuth App**  
Go to https://github.com/settings/developers → *OAuth Apps → New OAuth App*

| Field | Value |
|-------|-------|
| Application name | Lorena Portfolio CMS |
| Homepage URL | `https://aurelpow.github.io/lorena-portofolio` |
| Authorization callback URL | `https://api.netlify.com/auth/done` |

Save the **Client ID** — you will not need to paste it anywhere; Netlify's auth proxy handles it.

**3. Register the site with Netlify's OAuth proxy**  
Create a free Netlify account (no hosting needed) at https://app.netlify.com  
→ *Team settings → OAuth applications → Register your site URL*:  
`https://aurelpow.github.io/lorena-portofolio`

This allows `api.netlify.com` to proxy GitHub OAuth for static sites — it is free and does not require hosting on Netlify.

Once these three steps are done, Lorena opens `/admin/`, clicks **Login with GitHub**, authorises once, and lands in the CMS dashboard where she can manage projects with a form UI.
