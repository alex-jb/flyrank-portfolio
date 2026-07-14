# Should Alex's portfolio go "数字人 IP / flashy" — or stay calm?

**Deep-research synthesis · 2026-07-13 · 5 angles, 21 sources, 93 claims → 24 key claims.**
*Caveat: the workflow's adversarial-verify + synthesis step hit a session limit, so I synthesized from the extracted claims myself. 1 claim was formally 2-0 confirmed (WCAG 2.3.3); the rest are from credible sources but not triple-voted. Treat tech-stack facts as reliable, hiring claims as strong-but-directional.*

## Verdict

**Do NOT rebuild as a flashy 3D / digital-human site. Keep the calm recruiter portfolio; add a thin layer of tasteful, accessible motion + ONE interactive proof-of-work demo.**

The evidence is lopsided for the *hiring* goal:

- "Flashy interactivity and flashing animated elements usually **hurt** a portfolio from a UX perspective." — [dev.to, reviewer of 40+ dev portfolios](https://dev.to/kethmars/what-i-learned-after-reviewing-over-40-developer-portfolios-9-tips-for-a-better-portfolio-4me7)
- "Recruiter-focused portfolios succeed on **content structure and UX** — visitors come to assess skills, not to be dazzled." — same source
- "Prioritize **visual polish and typography over animations**." — [opendoorscareers](https://blog.opendoorscareers.com/p/how-recruiters-and-hiring-managers-actually-look-at-your-portfolio)
- "Be cautious of portfolios heavy on animations/visuals that **lack demonstrated impact** — treat them as a design showcase, not proof of work. Substance outranks flashy aesthetics." — [rocketdevs](https://rocketdevs.com/blog/frontend-developer-portfolios)
- AI-portfolio reviewers run a **red-flag filter**; 2–3 flags and they move on. Unverifiable production claims are a *top* inflation flag — link concrete artifacts (endpoint, repo, Docker, monitoring). — [learnist](https://www.learnist.org/ai-case-study-red-flags-portfolio-2026/)
- "**Interactive, runnable work (live demos, runnable notebooks) dramatically outperforms static** for recruiter engagement," and portfolios with **measurable outcomes get substantially more callbacks.** — [dataexpert.io](https://www.dataexpert.io/blog/ultimate-guide-ai-engineering-portfolios)

This is your own thesis — *proof over adjectives* — backed by outside evidence. The Douyin "数字人 IP" genre optimizes a different game (social virality). Even in dev-portfolio land, flashy-3D and calm-minimal **coexist** as conventions ([emmabostian list](https://github.com/emmabostian/developer-portfolios)) — flashy is the style for *creative-dev / design* roles, not AI/ML engineering hires.

## The genre's patterns & stack (for reference, not to copy wholesale)

**Patterns/tells** ([svgator](https://www.svgator.com/blog/website-animation-examples-and-effects/), [creativedevjobs](https://www.creativedevjobs.com/blog/best-threejs-portfolio-examples-2025), [scrollytelling.ai](https://scrollytelling.ai/examples/)): scrollytelling/parallax, morphing/liquid motion, hover microinteractions, 3D/faux-3D, scroll-driven 3D narratives, first-person navigation, interactive discovery (hidden collectibles), viewport-triggered reveals with "breathing room."

**Stack** ([codolve](https://codolve.com/blog/gsap-vs-framer-motion), [svilenkovic](https://svilenkovic.com/3d/scrollytelling-trends-2026), [wawasensei](https://wawasensei.dev/tuto/build-a-3D-portfolio-with-react-three-fiber-framer-motion-scroll-animations)): heavy = Three.js / React Three Fiber + GSAP + GLSL shaders + Blender assets. Scroll = Lenis + CSS view-timeline + GSAP ScrollTrigger. Mainstream = React + Tailwind + Framer Motion + GSAP. **2026 advice: use stable tooling, NOT AI-generated 3D ("brittle for production").** Framer Motion = easy declarative React UI (~4.6KB); GSAP = scroll-storytelling (core 23KB +ScrollTrigger 7KB).

## Build plan — add to the current site (calm + tasteful "wow")

Everything below is **vanilla CSS + a little JS** — no React/framework, so your "no mystery code, I can explain it" pass criterion stays intact.

1. **Scroll-reveal** — sections fade+rise as they enter the viewport. `IntersectionObserver` + a CSS class toggling `opacity`/`translateY`. ~30 lines, no library.
2. **Number count-ups** on the hero proof chips (54 · 1,300+ · 4.0) — animate on first view. Reinforces the metrics = your identity.
3. **Animate the existing hero agent-lattice** subtly — the nodes/lines already there, slow + low-opacity.
4. **⭐ The high-leverage one: embed ONE interactive live demo of real work** — a mini "5-voice debate" widget or a runnable snippet. The evidence says interactive/runnable work is what actually moves recruiters. *"Impressive because the work is impressive."*
5. **Micro-interactions** — subtle card/button hover (transform/opacity only).

**Accessibility + performance rules (non-negotiable):**
- Honor **`prefers-reduced-motion`** — WCAG 2.3.3 (formally confirmed ✓): interaction-triggered motion must be disableable. `prefers-reduced-motion` has near-universal browser support.
- Animate **only `transform` + `opacity`** (GPU-friendly); never `top/left/width/height` (forces layout recalc).
- **Desktop-rich, mobile-simplified.** Motion harms vestibular / photosensitive / low-vision / ADHD users — real cost.

## What to AVOID

- Three.js / R3F 3D scenes, particle fields, a digital-human avatar, scroll-hijacking, background audio, a heavy 3D page-load. Heavy, brittle, tanks Core Web Vitals + accessibility, and reads as **slop for an AI/ML hire**.
- Adding React / Framer / GSAP *just for this* — vanilla JS + CSS covers the whole "add" list. Reach for GSAP ScrollTrigger only if you ever build true scroll-storytelling; not needed now.

## One-line answer to "可以做成这种的么"

Technically yes; strategically no — for landing an AI/ML job, the flashy genre works against you. Steal its *restraint-friendly* tricks (scroll reveals, count-ups, one live demo), skip its spectacle. The single best upgrade isn't an animation — it's **one embedded, runnable demo of your real work.**
