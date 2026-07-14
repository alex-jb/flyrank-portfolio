# Ship It Live — the portfolio is public

**Alex Ji · FlyRank AI-Fluency · "Ship It Live"** · 2026-07-13

## 🔗 Live URL

### https://alex-jb.github.io/portfolio/

Every sitemap page is reachable (all return HTTP 200):

| Page | URL |
|---|---|
| Home (Hero → Work → About → Contact) | `/portfolio/` |
| Case — Orallexa | `/portfolio/work-orallexa.html` |
| Case — Solo Founder OS | `/portfolio/work-solo-founder-os.html` |
| Case — Shadow | `/portfolio/work-shadow.html` |

Nav works; every Work card opens its case; every CTA points to the one action (book a call).

## How it's built (no mystery code — I can explain every line)

- **Plain static HTML + one CSS file.** No framework, no build step, no JavaScript. Four `.html` pages + `style.css`.
- **Pages:** `index.html` (Home) links to three `work-*.html` case pages. Home nav is anchor links (`#work`, `#about`, `#contact`); case pages link back with relative hrefs.
- **Type:** IBM Plex Sans + IBM Plex Mono, loaded from Google Fonts with a single `<link>`.
- **Colour:** four CSS variables from my identity kit — `--ink #13202B`, `--text #1F2530`, `--bg #FAFBFC`, `--accent #0F766E`.
- **Images:** the hero texture and favicon are the PNGs I made in the identity-kit week (`assets/`).
- **Hosting:** GitHub Pages, repo `alex-jb/portfolio`, `main` branch, served at `/portfolio/`. Push to main = redeploy.
- **CTA:** the "Book a call" button is a `mailto:` link for now (Calendly pending — see still-ugly).

## Still ugly (I already know these are rough)

1. **No real work screenshots yet** — the cases are text + repo links; the Orallexa demo, Solo Founder OS test run, and Shadow audit-chain captures are still to be taken.
2. **About photo is a placeholder** monogram, not a real headshot.
3. **CTA is `mailto:`**, not a real Calendly booking link.
4. **Test-count needs reconciling** — I show "1,300+"; my resume says 745. Pick one, use everywhere.
5. **No mobile nav menu** — the nav links hide on small screens (only the logo + Book-a-call show).
6. **On a `github.io` subpath**, not a custom domain (`alexji.dev` planned ~week 6–7).
7. **Home Work cards truncate** "What I did" with "…"; the full three beats only show on the case page.

## The one real person (I still need to do this — for the pass)

Send the live link to one person in or near AI/ML and capture their reaction. Draft message + template below.

**Message to send:**
> Hey — I just put my portfolio live and I'd love a 60-second gut-check before I polish it. Not looking for nice, looking for honest: what did you understand it I do, what confused you, and did any of the work land? https://alex-jb.github.io/portfolio/

**Capture (fill in after they reply):**
- Who / their field:
- What they saw / understood I do:
- What confused them:
- Did the work land? (which case, if any):
- One change they'd make:


## Update (2026-07-13) — added a "tasteful wow" layer

After a deep-research pass on the flashy-vs-calm question (`../_research/2026-07-13-flashy-vs-calm-portfolio.md`), I kept the calm site and added, all reduced-motion-safe:
- scroll reveals, hero number count-ups, an animated agent-node hero lattice, hover micro-interactions
- **an interactive 5-voice debate demo** (home "Live demo" section + the Orallexa case page) — the research's highest-leverage move: interactive/runnable work beats visual flash.

**Still ugly (updated):** real work screenshots + a real headshot + a Calendly link are still pending; test-count (1,300+ vs 745) still needs reconciling; mobile nav menu still absent.
