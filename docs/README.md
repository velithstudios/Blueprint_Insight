# Blueprint Insight — GitHub Pages

This folder is the source for the **Blueprint Insight** GitHub Pages site. The [Blueprint_Insight](https://github.com/velithstudios/Blueprint_Insight) repo is the **official single source** for documentation, support, and feature information (no plugin source code; the plugin is distributed via [Fab](https://www.fab.com/)).

---

## Enabling GitHub Pages

1. In your GitHub repo: **Settings → Pages**.
2. Under **Build and deployment**:
   - **Source:** Deploy from a branch.
   - **Branch:** `main` (or your default branch).
   - **Folder:** `/docs`.
3. Click **Save**. The site will be available at:
   - **Project site:** `https://<username>.github.io/<repo>/`
   - Example: `https://velithstudios.github.io/Blueprint_Insight/`

---

## If the site is at a project URL (e.g. `/Blueprint_Insight`)

Edit `docs/_config.yml` and set:

```yaml
baseurl: "/YourRepoName"   # e.g. "/Blueprint_Insight"
```

Then commit and push. GitHub will rebuild the site.

---

## Local preview (optional)

Install [Ruby](https://www.ruby-lang.org/) and [Bundler](https://bundler.io/), then:

```bash
cd docs
bundle install
bundle exec jekyll serve
```

Open `http://localhost:4000`. For a project site, use:

`http://localhost:4000/YourRepoName/`

(Or set `baseurl: ""` temporarily and use `http://localhost:4000`.)

---

## Contents

- **Home** (`index.md`) — Overview and quick links.
- **Documentation** (`documentation.md`) — Installation, features, usage.
- **Support** (`support.md`) — Contact, FAQ, troubleshooting.

Update these Markdown files and push to the branch; GitHub Pages will rebuild automatically.
