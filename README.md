# Wilfried Tadjeugue — personal website

A [Hugo](https://gohugo.io/) site using the [Congo](https://github.com/jpanther/congo) theme,
deployed to GitHub Pages. It houses research activities, computational-physics manuscripts
(including *DFT from scratch*), a statistics blog, and programming videos.

## Prerequisites

- **Hugo extended** ≥ 0.164 (`brew install hugo`)
- **Git** (the theme is a submodule)

## Run locally

```bash
# first time, or after a fresh clone:
git submodule update --init --recursive

# live preview at http://localhost:1313
hugo server
```

## Project layout

```
config/_default/     Site configuration (split by concern)
  hugo.toml            baseURL, outputs, taxonomies
  languages.en.toml    title, author, bio, social links
  menus.en.toml        top navigation
  params.toml          theme options (colours, homepage layout, article settings)
  markup.toml          math + code-highlighting settings (do not remove)
content/
  _index.md            home page (profile layout)
  about.md             about / bio
  research/            research interests & publications
  projects/            long-form manuscripts w/ code
    dft-from-scratch/    the flagship DFT series
  blog/                statistics-in-science essays
  videos/              programming screencasts
themes/congo/         the theme (git submodule — don't edit directly)
assets/img/author.jpg add a square photo here for the homepage avatar (optional)
static/               drop files here to serve verbatim (e.g. static/cv.pdf)
```

## Writing content

Create a new post:

```bash
hugo new content blog/my-new-post.md
```

- **Math:** add `{{</* katex */>}}` once near the top of the page, then write
  `$$ ... $$` for display math and `\( ... \)` for inline math.
- **Code:** fenced blocks with a language tag get syntax highlighting and a copy button.
- **YouTube:** embed with `{{</* youtube VIDEO_ID */>}}`.
- **Drafts:** set `draft: true` in the front matter to hide from production builds.

## Deploy

Pushing to `main` triggers `.github/workflows/hugo.yml`, which builds the site and
publishes it to GitHub Pages. In the repo: **Settings → Pages → Build and deployment →
Source → GitHub Actions** (one-time setup).

### Custom domain

Add your domain in **Settings → Pages → Custom domain**, create a `static/CNAME` file
containing the domain, and point your DNS at GitHub Pages.

## Personalisation checklist

- [ ] Replace placeholder social links in `config/_default/languages.en.toml`
- [ ] Add a homepage photo at `assets/img/author.jpg`
- [ ] Fill in real research, publications, and CV (`static/cv.pdf`)
- [ ] Replace the sample blog post, video, and DFT chapter with your own
- [ ] Pick a colour scheme in `params.toml` (`congo`, `avocado`, `cherry`, `ocean`, …)
