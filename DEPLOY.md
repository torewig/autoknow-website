# AutoKnow website — structure and deployment notes

Static HTML site, no build step, no framework. Every page is self-contained and shares `assets/style.css` and `assets/site.js`.

## Files

| File | Purpose |
|---|---|
| `index.html` | Home: puzzle, hypotheses, measurements, approach, work packages, paper/team/news previews |
| `papers.html` | Research: detailed entry per paper (P01–P06, P08) plus the monograph and the project AI policy (`#ai-policy`) |
| `publications.html` | Journal articles, working papers, expected outputs (empty states for now) |
| `team.html` | Core team, co-authors and collaborators, open positions |
| `news.html` | Timeline of milestones |
| `media.html` | Outreach: press contact, media coverage, policy briefs, talks |
| `links.html` | Resources: repositories, data sources, institutions |
| `404.html` | Not-found page (uses absolute `/assets/...` paths; works once hosted at a domain root) |
| `robots.txt`, `sitemap.xml` | Search-engine files. Replace `REPLACE-WITH-DOMAIN` before going live |
| `assets/style.css` | Design system: colours, typography, layout, components |
| `assets/site.js` | Mobile navigation toggle and footer year |
| `assets/uio-logo.png`, `assets/erc-eu-logo.png` | Official logos (from the ERC/EU lockup and UiO logo files in the P02 folder) |
| `assets/favicon.svg` | Site icon |
| `_old_2026-06/` | Backup of the June 2026 version. Delete before deploying, or keep out of the deploy folder |

Fonts (Inter, Source Serif 4) load from Google Fonts. If you want zero third-party requests, download the two font families and self-host them in `assets/fonts/`.

## Updating content

- **New paper:** copy an `<article class="paper-detail">` block in `papers.html`, give it a new `id`, and add a chip in the `paper-index` nav. Optionally add a card to the "Current papers" section on `index.html`.
- **Status badges:** `b-scoping`, `b-design`, `b-data`, `b-analysis`, `b-draft`, `b-active`, `b-plan`.
- **News:** add a `<li>` at the top of the timeline in `news.html`, and update the three-item "Latest" list on `index.html`.
- **Team:** add a `<div class="member">` card in `team.html`. To use a photo, replace the initials in `.avatar` with `<img src="assets/team/name.jpg" alt="">`.
- **Publications:** replace the `.empty` block with a list; the `.tablewrap` table style works well for a publication list.

## Going live: options

All three free options handle a custom domain with automatic HTTPS. The site needs no server-side code.

| Option | Cost | Pros | Cons |
|---|---|---|---|
| **GitHub Pages** (recommended) | Free | Site lives in a GitHub repo next to the project code; push to publish; custom domain supported | Public repo unless the account has Pages-on-private (Pro, ~USD 4/month) |
| **Cloudflare Pages** | Free | Fast CDN, private repo allowed on free tier, drag-and-drop or git deploy | One more account to manage |
| **Netlify** | Free | Drag-and-drop deploy, simple redirects, form handling if ever needed | Free-tier bandwidth cap (generous for a project site) |
| **UiO web hosting (Vortex)** | Free | Institutional, `uio.no` address | Locked to UiO templates; this design cannot be used as-is |

**Domain:** a `.no` domain (e.g. `autoknow.no`) costs roughly NOK 150–200 per year from a Norwegian registrar such as Domeneshop. A `.eu` or `.org` domain is similar. Alternatively use the free `username.github.io/autoknow` address with no domain purchase.

## Deploying to GitHub Pages (step by step)

1. Create a repository, e.g. `torewig/autoknow-website`.
2. Copy the contents of this folder (excluding `_old_2026-06/`) into the repository root and push.
3. In the repository, go to *Settings → Pages*, set *Source* to *Deploy from a branch*, branch `main`, folder `/ (root)`.
4. The site appears at `https://torewig.github.io/autoknow-website/` within a few minutes.
5. For a custom domain: add a file named `CNAME` containing the domain (e.g. `autoknow.no`), then at the registrar create a `CNAME` record pointing `www` to `torewig.github.io` and `A` records for the apex to GitHub's four Pages IPs (listed in GitHub's Pages documentation). Enable *Enforce HTTPS* once the certificate is issued.
6. Replace `REPLACE-WITH-DOMAIN` in `robots.txt` and `sitemap.xml` with the final domain.

## Pre-launch checklist

- [ ] Confirm all names, affiliations, and roles on `team.html`
- [ ] Decide whether preliminary findings on `papers.html` (P01, P08) should be public
- [ ] Replace `REPLACE-WITH-DOMAIN` in `robots.txt` and `sitemap.xml`
- [ ] Remove or exclude `_old_2026-06/`
- [ ] Add team photos if desired (`assets/team/`)
- [ ] Register the site URL with the ERC / UiO project page

## Print version

`_print.html` stitches the `<main>` content of every page into one document with print styles. To regenerate the review PDF:

```bash
"C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" --headless=new --no-pdf-header-footer --virtual-time-budget=8000 --print-to-pdf="AutoKnow-website-full-text.pdf" "file:///C:/Users/torewig/Dropbox/!!!!FORSKNING!!!!!/AUTOKNOW_ERC_COG/website/_print.html"
```

`_print.html` and the PDF are review aids only; exclude them from the deployed site (see also the `_old_2026-06/` note above).
