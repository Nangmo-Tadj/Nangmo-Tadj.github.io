# Wilfried Tadjeugue — personal website

A [Hugo](https://gohugo.io/) site using the [Congo](https://github.com/jpanther/congo) theme,
deployed to GitHub Pages. Bilingual (English at the root, French under `/fr/`), organised
around four sections: **home**, **research & publications**, **scientific activities**, and
**outreach & engagement**.

The visual language — azure and amber on a fixed decor of soft halos, content floating on
blue-tinted shadows, Outfit + Plus Jakarta Sans — matches the sibling projects
(`essentiel_prepa`, `web.lyamo.app`).

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
  languages.en.toml    English: title, author, bio, social links, hero copy
  languages.fr.toml    French: the same, translated
  menus.en.toml        English navigation (+ the EN/FR toggle and search entries)
  menus.fr.toml        French navigation
  params.toml          theme options (colour scheme, homepage layout, article settings)
  markup.toml          math + code-highlighting settings (do not remove)
i18n/
  en.yaml, fr.yaml     strings used by this site's own layouts
content/
  _index.md            home (hero + pillars + activity, rendered by the custom layout)
  about.md             about / bio
  research/            RESEARCH & PUBLICATIONS
    _index.md            hub: intro + publication list (from data/publications.yaml)
    interests.md         longer write-up
    projects/            long-form manuscripts with code
      dft-from-scratch/    the flagship DFT series
  activities/          SCIENTIFIC ACTIVITIES
    _index.md            hub: talks, posters, schools, teaching (data/activities.yaml)
  outreach/            OUTREACH & ENGAGEMENT
    _index.md            hub: the three channels + engagement (data/engagement.yaml)
    blog/                statistics-in-science essays
    videos/              programming screencasts
    courses/             hands-on formations, lesson by lesson
data/                  bilingual content that isn't a page (see below)
layouts/
  research/hub.html    the three hub layouts
  activities/hub.html
  outreach/hub.html
  courses/             course catalog, course landing, lesson layouts
  _partials/home/custom.html   the landing page
  _partials/translations.html  the EN/FR pill toggle
assets/css/
  custom.css           the whole visual layer
  schemes/tadjeugue.css  the azure + amber palette
themes/congo/          the theme (git submodule — don't edit directly)
assets/img/author.png  square photo used for the homepage avatar
static/                files served verbatim (e.g. static/cv.pdf)
```

## Both languages

Every page exists twice: `page.md` (English) and `page.fr.md` (French). Hugo pairs them by
filename, and the EN/FR toggle in the header links a page to its counterpart — or, if a page
has no counterpart yet, to that language's homepage.

**Adding a page:** create both files. A page that exists in only one language still builds,
it just won't appear in the other language's navigation.

**Data files** (`data/*.yaml`) hold publications, activities, news and engagement entries.
Each entry carries language sub-maps, so a date or a DOI is written once:

```yaml
- date: 2026-06-02
  kind: conference
  where: "Summer School — Location, Country"
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

## Personalisation checklist

- [ ] Replace placeholder social links in `config/_default/languages.{en,fr}.toml`
- [ ] Replace the homepage photo at `assets/img/author.png`
- [ ] Fill in `data/publications.yaml` and `data/activities.yaml` with real records
- [ ] Rewrite the hero copy in the `[params.hero]` block of each language file
- [ ] Fill in real research, bio, and CV (`static/cv.pdf`)
- [ ] Replace the sample blog post, video, and DFT chapters with your own
