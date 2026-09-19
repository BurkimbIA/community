# BurkimbIA Community

The community blog of [BurkimbIA](https://burkimbia.com), live at
<https://community.burkimbia.com>. It is a bilingual (French default, English)
[Hugo](https://gohugo.io) site built on the [Blowfish](https://blowfish.page)
theme, styled to match the BurkimbIA landing page.

## Requirements

- [Hugo **extended**](https://gohugo.io/installation/), version **0.163.0 to 0.166.0**
  (the range the theme supports; developed with 0.165.0)
- Git

## Get the code

The theme is a Git submodule, so clone with submodules:

```bash
git clone --recurse-submodules git@github.com:BurkimbIA/community.git
cd community
```

If you already cloned without them:

```bash
git submodule update --init
```

Without the submodule `themes/blowfish` is empty and the site will not build.

## Run locally

```bash
hugo server
```

Open <http://localhost:1313>. The page reloads when you save a file.
Drafts are hidden; add `-D` to see them.

## Write content

Each post is a folder (a page bundle) under `content/posts/`, with one file
per language:

```
content/posts/my-post/
├── index.fr.md
├── index.en.md
└── featured.jpg      # thumbnail used on cards and as the article hero
```

Create one with:

```bash
hugo new content posts/my-post/index.fr.md
```

The new file starts with `draft = true`. Fill in the front matter (`title`,
`description`, `categories`, `tags`), change it to `draft = false` when the post
is ready, and repeat with `index.en.md` for the English version. Pages that share a folder and file name are linked as
translations, which is what makes the FR/EN switch work.

Projects, activities and datasets work the same way under `content/projects/`,
`content/activities/` and `content/datasets/`.

## Configure

| What | Where |
|---|---|
| Site URL, default language, taxonomies | `config/_default/hugo.toml` |
| Theme options (header, homepage, article and list layouts) | `config/_default/params.toml` |
| Site title, description, copyright, social links | `config/_default/languages.fr.toml`, `languages.en.toml` |
| Navigation and footer menus | `config/_default/menus.fr.toml`, `menus.en.toml` |
| Colours (BurkimbIA palette) | `assets/css/schemes/burkimbia.css` |
| Fonts, background, hero and card styling | `assets/css/custom.css` |
| Homepage hero layout | `layouts/partials/home/landing.html` |
| Homepage text, buttons and image | `content/_index.fr.md`, `content/_index.en.md` |

Do not edit files inside `themes/blowfish/`. Override them from the site's own
`layouts/` and `assets/` folders instead, so theme updates stay clean.

## Build

```bash
hugo --gc --minify
```

The static site is written to `public/`. It is not committed.

## Deploy

The site is deployed to **GitHub Pages** by the workflow in
`.github/workflows/deploy.yml`. Every push to `main` builds the site with Hugo
and publishes it. You can also run it by hand from the repository's **Actions**
tab (**Deploy to GitHub Pages → Run workflow**).

### One-time setup

1. In the repository go to **Settings → Pages** and set **Source** to
   **GitHub Actions**.
2. Push to `main` (or run the workflow manually). The first run creates the
   `github-pages` environment and publishes the site.
3. Add the custom domain `community.burkimbia.com`:
   - At your DNS provider, create a `CNAME` record: `community` →
     `burkimbia.github.io`.
   - In **Settings → Pages → Custom domain**, enter `community.burkimbia.com`
     and save. Once GitHub has checked the DNS, tick **Enforce HTTPS**.
   - Optional but recommended: verify `burkimbia.com` in the organisation's
     **Settings → Pages**, so nobody else can claim the domain.

With the GitHub Actions source, no `CNAME` file is needed in the repository.

### How it works

- The workflow installs Hugo extended, checks out the code **with the theme
  submodule**, builds with `hugo --gc --minify`, and uploads `public/` as the
  Pages artifact.
- The base URL is supplied by GitHub (`actions/configure-pages`), so the build
  uses `community.burkimbia.com` once the custom domain is set. Before that, it
  uses the default `burkimbia.github.io/community` address, which makes it easy
  to check a first deployment.
- The Hugo version is pinned by `HUGO_VERSION` in the workflow. Keep it inside
  the range the theme supports (see [Requirements](#requirements)).

### Deploy by hand

Normally you should not need to. If you must publish without Actions, build with
`hugo --gc --minify` and upload the contents of `public/` to any static host.

## Update the theme

```bash
git submodule update --remote --merge
```

Check the site with `hugo server` before committing, and keep Hugo within the
version range above.

## Troubleshooting

- **Blank or unstyled site**: Hugo is not the *extended* edition, or the theme
  submodule was not fetched (see [Get the code](#get-the-code)).
- **The workflow succeeds but the site does not update**: check that
  **Settings → Pages → Source** is set to **GitHub Actions**.
- **Build fails with a Hugo version error**: install a version between 0.163.0
  and 0.166.0, and set `HUGO_VERSION` in `.github/workflows/deploy.yml` to match.
- **A post does not appear**: it is still `draft = true`, or its date is in the
  future.
- **Links or the sitemap point to the wrong address**: check `baseURL` in
  `config/_default/hugo.toml`.
