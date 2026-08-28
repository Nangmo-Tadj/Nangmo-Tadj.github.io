# Wilfried Tadjeugue — personal website

A [Hugo](https://gohugo.io/) site using the [Congo](https://github.com/jpanther/congo) theme,
deployed to GitHub Pages. Bilingual (English at the root, French under `/fr/`), organised
around four sections: **home**, **research & publications**, **scientific activities**, and
**outreach & engagement**.

The visual language — azure and amber on a fixed decor of soft halos, content floating on
blue-tinted shadows, Outfit + Plus Jakarta Sans — matches the sibling projects
(`essentiel_prepa`, `web.lyamo.app`).

## Editing the site

Everything the site says about you lives in files this repository owns, and
there is a form-based editor for them at **`/admin/`** — see
**[ADMIN.md](ADMIN.md)** for how to sign in (a personal access token works
immediately; a one-click GitHub button takes ten minutes to set up).

Nothing is written for you. `data/profile.yaml`, `data/publications.yaml`,
`data/activities.yaml`, `data/news.yaml` and `data/engagement.yaml` ship empty,
with their schema documented in comments. Pages show an empty state until you
put something in them.

## Prerequisites

- **Hugo extended** ≥ 0.164 (`brew install hugo`)
- **Git** (the theme is a submodule)

## Run locally

```bash
# first time, or after a fresh clone:
git submodule update --init --recursive

# live preview at http://localhost:1313  (use localhost, not 127.0.0.1 —
# the stylesheet is served with an integrity hash and a different host
# makes it a cross-origin request, which the browser then refuses)
hugo server
```

## Project layout

```
config/_default/     Site configuration (split by concern)
  hugo.toml            baseURL, languages, outputs, taxonomies
  languages.en.toml    English: site title, name, copyright
  languages.fr.toml    French: the same
  menus.en.toml        English navigation (+ EN/FR toggle and search entries)
  menus.fr.toml        French navigation
  params.toml          theme options (colour scheme, homepage layout, articles)
  markup.toml          math + code-highlighting settings (do not remove)
i18n/
  en.yaml, fr.yaml     strings used by this site's own layouts
data/                  everything the CMS writes, bilingual (see below)
  profile.yaml           photo, links, headline, bio, landing-page wording
  publications.yaml      the publication list
  activities.yaml        talks, posters, schools, teaching, service
  news.yaml              the homepage timeline
  engagement.yaml        the cards at the bottom of /outreach/
content/               one file per language: page.en.md and page.fr.md
  _index.*.md            home
  about.*.md             about
  research/              RESEARCH & PUBLICATIONS
    projects/              long-form manuscripts with code
  activities/            SCIENTIFIC ACTIVITIES
  outreach/              OUTREACH & ENGAGEMENT
    blog/  videos/  courses/
layouts/
  research/hub.html    the three section layouts
  activities/hub.html
  outreach/hub.html
  courses/             course catalog, course landing, lesson layouts
  robots.txt           keeps /admin/ out of search indexes
  _partials/home/custom.html     the landing page
  _partials/profile-links.html   contact links, read from data/profile.yaml
  _partials/translations.html    the EN/FR pill toggle
assets/css/
  custom.css           the whole visual layer
  schemes/tadjeugue.css  the muted azure + amber palette
static/
  admin/               the editing interface (index.html + config.yml)
  img/uploads/         where pictures uploaded from /admin/ land
themes/congo/          the theme (git submodule — don't edit directly)
```

## Both languages

Every page exists twice: `page.md` (English) and `page.fr.md` (French). Hugo pairs them by
filename, and the EN/FR toggle in the header links a page to its counterpart — or, if a page
has no counterpart yet, to that language's homepage.

**Adding a page:** create both files. A page that exists in only one language still builds,
it just won't appear in the other language's navigation.

**Data files** (`data/*.yaml`) hold publications, activities, news and engagement entries
under an `items:` key — the shape the editing interface reads and writes. Each entry
carries language sub-maps, so a date or a DOI is written once:

```yaml
items:
  - date: 2026-06-02
    kind: conference
    where: "Venue — City, Country"
    en: { title: "…", summary: "…" }
    fr: { title: "…", summary: "…" }
```

Category and status labels (`conference`, `preprint`, `submitted`, …) are translated in
`i18n/en.yaml` and `i18n/fr.yaml`, not in the data file.

## Writing content

Create a new post:

```bash
hugo new content outreach/blog/my-new-post.md
```

- **Math:** add `{{</* katex */>}}` once near the top of the page, then write
  `$$ ... $$` for display math and `\( ... \)` for inline math.
- **Code:** fenced blocks with a language tag get syntax highlighting and a copy button.
- **YouTube:** embed with `{{</* youtube VIDEO_ID */>}}` or the `video` shortcode.
- **Cards, galleries, PDFs:** see the `cards`, `gallery` and `pdf` shortcodes in
  `layouts/_shortcodes/`.
- **Drafts:** set `draft: true` in the front matter to hide from production builds.

## Deploy

Pushing to `main` triggers `.github/workflows/hugo.yml`, which builds the site and
publishes it to GitHub Pages. In the repo: **Settings → Pages → Build and deployment →
Source → GitHub Actions** (one-time setup).

### Custom domain

Add your domain in **Settings → Pages → Custom domain**, create a `static/CNAME` file
containing the domain, and point your DNS at GitHub Pages.

## What's left to fill in

- [ ] Sign in at `/admin/` — see [ADMIN.md](ADMIN.md)
- [ ] Profile: photo, links, headline, bio, landing-page wording (both languages)
- [ ] Publications, activities, the news feed — all empty on purpose
- [ ] The About page
- [ ] Optional: `static/cv.pdf`, then link it from About
