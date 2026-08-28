# Editing the site from `/admin/`

The site is a set of files in this repository. The admin interface at
**<https://nangmo-tadj.github.io/admin/>** gives those files a form-based
editor: you sign in with your GitHub account, fill in fields, press save, and
the change is committed and deployed about a minute later.

Nothing appears on the site that you did not type. Posts and courses are
created as drafts by default — untick *Draft* when you want one published.

---

## Signing in

There are two ways in. **The first needs no setup and works right now.**

### A. With a personal access token — nothing to deploy

1. Open <https://github.com/settings/personal-access-tokens/new> (Settings →
   Developer settings → Personal access tokens → **Fine-grained tokens**).
2. **Repository access:** *Only select repositories* → `Nangmo-Tadj.github.io`.
3. **Permissions → Repository permissions:** set **Contents** to
   *Read and write*. (Metadata read-only is added for you.) Nothing else.
4. Choose an expiry you are comfortable with and generate the token. Copy it —
   GitHub shows it once.
5. Open <https://nangmo-tadj.github.io/admin/>, choose **Sign In Using Access
   Token**, and paste it.

The token stays in your browser. When it expires, generate another one.

### B. With a "Sign in with GitHub" button — one-time setup

Nicer day to day: one click, no token to keep. It needs a small relay, because
GitHub Pages serves static files and cannot run the sign-in exchange itself.
The relay is an existing open-source worker — you deploy it, you do not write
it.

1. **Register the OAuth application.** GitHub → Settings → Developer settings →
   **OAuth Apps** → *New OAuth App* (<https://github.com/settings/developers>).
   Homepage URL: `https://nangmo-tadj.github.io`. Leave the callback URL for
   now. Keep the **Client ID**, and generate a **Client Secret**.
2. **Deploy the relay.** Create a free Cloudflare account, open *Workers &
   Pages*, and deploy <https://github.com/sveltia/sveltia-cms-auth> — its
   README has a one-click deploy button. Cloudflare gives it an address such
   as `https://sveltia-cms-auth.your-name.workers.dev`.
3. **Give the worker three variables** (Settings → Variables):
   `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`, and `ALLOWED_DOMAINS` set to
   `nangmo-tadj.github.io`.
4. **Set the callback URL** on the GitHub OAuth App to
   `https://<your worker address>/callback`.
5. **Point the site at it.** In [`static/admin/config.yml`](static/admin/config.yml),
   replace the placeholder on the `base_url` line with your worker's address,
   and commit that one line.

### C. Editing offline

**Work with Local Repository** on the sign-in screen opens the same interface
against a folder on your own machine — no network, no token. Useful for
drafting; you commit and push yourself afterwards.

---

## What you can edit

| Section | What it writes | Where it shows |
|---|---|---|
| **Profile** | `data/profile.yaml` | Your photo, links, headline, bio and the landing-page wording — in both languages |
| **Publications** | `data/publications.yaml` | `/research/` |
| **Scientific activities** | `data/activities.yaml` | `/activities/` |
| **Activity feed** | `data/news.yaml` | The timeline on the homepage |
| **Engagement** | `data/engagement.yaml` | The cards at the bottom of `/outreach/` |
| **Blog posts** | `content/outreach/blog/*.md` | `/outreach/blog/` |
| **Videos** | `content/outreach/videos/*.md` | `/outreach/videos/` |
| **Courses / Course lessons** | `content/outreach/courses/**` | `/outreach/courses/` |
| **Pages** | `content/about.*.md`, section headings | About, and each section's title |

Every entry has an **English** and a **Français** side. Fill in one or both —
a page with no French version simply keeps the reader on the English one.

## Notes

- **Deleting** works the same way as adding: remove the entry, save. The list
  pages show an empty state when there is nothing in them, rather than a
  placeholder.
- **Images** you upload land in `static/img/uploads/` and are served from
  `/img/uploads/`.
- **A lesson** needs the folder name of its course — the last part of the
  course's address. For `/outreach/courses/dft/`, that is `dft`.
- **Nothing here is a database.** Every save is a commit, so the full history
  of what the site said, and when, is in `git log` — and anything can be
  reverted.

## Editing without the interface

The interface is a convenience, not a dependency. Every file it writes is
plain YAML or Markdown with comments explaining the fields; editing them in
GitHub's web editor, or locally, produces exactly the same result.
