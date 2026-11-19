# Pipeline Wiki

Documentation for people building Pipeline data packs. Live site: https://dv8fromthecode.github.io/Pipeline-Wiki/

## Local Preview

You need Python 3.8 or newer.

```bash
pip install -r requirements.txt
mkdocs serve
```

Then open `http://127.0.0.1:8000`.

## Build

```bash
mkdocs build
```

Output goes to `site/`.

## Deploy

Pushing to `main` triggers a GitHub Actions workflow that builds the site and publishes it to GitHub Pages. One-time setup in the repository: **Settings → Pages → Source: GitHub Actions**.

## Editing

- Documentation source is in `docs/`.
- Navigation is in `mkdocs.yml`.
- The site uses the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme.
