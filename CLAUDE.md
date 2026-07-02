# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Leo Feng's personal academic website, built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. Nearly all engineering work in this repo is theme code inherited from upstream; the day-to-day work is **content editing** — CV data, publications, news, projects, about page, blog posts.

## Commands

```bash
# Local setup (standard, requires Ruby + Bundler)
bundle install
bundle exec jekyll serve --lsi     # serve locally with live rebuild

# Local setup (Docker, recommended on Windows)
docker-compose up

# Build (what CI runs)
bundle exec jekyll build --lsi     # via bin/cibuild
bundle exec jekyll build           # what .github/workflows/deploy.yml actually runs
```

There is no test suite or linter configured beyond `.pre-commit-config.yaml` (trailing-whitespace, end-of-file-fixer, check-yaml, check-added-large-files). Validate changes by running a local Jekyll build/serve and checking the rendered page.

## Deployment

Pushing to `master` automatically triggers `.github/workflows/deploy.yml`, which builds the site with Jekyll and deploys `_site/` to the `gh-pages` branch via `JamesIves/github-pages-deploy-action`. Do not manually run `bin/deploy` (a legacy manual GitHub Pages deploy script) unless asked — normal pushes to `master` are sufficient.

## Where content lives

- `_config.yml` — site-wide settings: name/title, social links, feature toggles (`enable_*` flags), Giscus comments config, related-posts config, etc.
- `_pages/about.md` — homepage/about content.
- `_pages/cv.md` + `_data/cv.yml` — CV page; the actual CV content (education, experience, awards, publications summary, etc.) lives in `_data/cv.yml`, rendered via `_layouts/cv.html` and `_includes/cv/`.
- `_bibliography/papers.bib` — publications, in BibTeX, rendered by `jekyll-scholar` on `_pages/publications.md`.
- `_data/coauthors.yml` — maps BibTeX author strings to profile links/affiliations for the publications page.
- `_data/venues.yml` — venue metadata used by the publications rendering.
- `_data/repositories.yml` — GitHub repos/users to display via the GitHub stats include.
- `_news/` — short announcement snippets shown on the homepage (`announcement_N.md`).
- `_projects/` — project cards (`N_project.md`), ordered/categorized per `_config.yml`'s `enable_project_categories`.
- `_posts/` — blog posts (standard Jekyll date-prefixed filenames); `_layouts/post.html` and `_layouts/distill.html` (for Distill-style posts) render them.
- `_data/cv.yml` uses YAML anchors/structure consumed by `_includes/cv/*` — keep entry `type` values (`time_table`, `map`, etc.) consistent with what those includes expect.

## Architecture notes

- Standard Jekyll conventions apply: `_layouts/` define page templates, `_includes/` are reusable partials, `_sass/` holds theme styles, `_plugins/` holds custom Ruby plugins (`details.rb`, `external-posts.rb`, `hideCustomBibtex.rb`).
- `_config.yml`'s `enable_*` flags are the main way theme features are turned on/off (dark mode, math via MathJax, masonry project layout, medium-zoom, progress bar, analytics, etc.) — check there before assuming a feature needs new code.
- `assets/` holds images, CSS, JS, and PDFs (e.g. CV PDF) referenced from pages/includes.
- Docker files (`Dockerfile`, `docker-compose.yml`, `docker-local.yml`, `bin/docker_*`) exist for local Docker-based development only; they are not part of the deployment path (deployment uses the GitHub Actions workflow directly).
