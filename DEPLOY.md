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
| `404.html` | Not-found page. Uses root-relative `/...` paths (custom domain since 2026-09-24) |
| `robots.txt`, `sitemap.xml` | Search-engine files. Point at `autoknow.no`; update if the domain changes |
| `assets/style.css` | Design system: colours, typography, layout, components |
| `assets/site.js` | Mobile navigation toggle and footer year |
| `assets/erc-eu-logo.png` | Official ERC/EU funding lockup (from the P02 folder). The UiO logo was removed 2026-09-23: it may not be used on external websites |
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

## Live deployment (since 2026-09-16, custom domain since 2026-09-24)

- **URL:** https://autoknow.no/ (custom domain since 2026-09-24; https://torewig.github.io/autoknow-website/ redirects there). Domain registrar: [fill in]; DNS nameservers `ns1/ns2.no1.groupdnsservice.com`; DNS: four A records and four AAAA records to GitHub Pages, `www` CNAME to `torewig.github.io`. `CNAME` file in this folder holds the domain.
- **Repository:** https://github.com/torewig/autoknow-website (public; this folder is the working copy, `.gitignore` keeps the review files out)
- **Hosting:** GitHub Pages, branch `main`, folder `/ (root)`, HTTPS enforced. Every push to `main` redeploys within a minute or two.

To publish a change:

```bash
git add -A && git commit -m "Describe the change" && git push
```

### Custom domain (done 2026-09-24; kept for reference)

1. Buy the domain (`autoknow.no`; check that auto-renewal is on).
2. Add a file named `CNAME` to this folder containing only the domain, and push.
3. At the registrar create a `CNAME` record pointing `www` to `torewig.github.io` and `A` records for the apex to GitHub's four Pages IPs (listed in GitHub's Pages documentation).
4. Enable *Enforce HTTPS* in *Settings → Pages* once the certificate is issued.
5. Change `torewig.github.io/autoknow-website` to the new domain in `robots.txt` and `sitemap.xml`, and `/autoknow-website/` to `/` in `404.html`.

## Post-launch checklist

- [x] Site live and all pages verified (2026-09-16)
- [ ] Confirm all names, affiliations, and roles on `team.html`
- [ ] Decide whether preliminary findings on `papers.html` (P01, P08) should stay public
- [ ] Add team photos if desired (`assets/people/`)
- [ ] Enable "Enforce HTTPS" in Settings → Pages once the certificate is issued
- [ ] Register the site URL (https://autoknow.no) with the ERC / UiO project page

## Print version

`_print.html` stitches the `<main>` content of every page into one document with print styles. To regenerate the review PDF:

```bash
"C:/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" --headless=new --no-pdf-header-footer --virtual-time-budget=8000 --print-to-pdf="AutoKnow-website-full-text.pdf" "file:///C:/Users/torewig/Dropbox/!!!!FORSKNING!!!!!/AUTOKNOW_ERC_COG/website/_print.html"
```

`_print.html` and the PDF are review aids only; exclude them from the deployed site (see also the `_old_2026-06/` note above).
