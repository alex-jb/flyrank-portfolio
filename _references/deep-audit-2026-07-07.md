---
title: "Deep audit — Shadow · Orallexa · VibeXForge"
subtitle: "5 parallel agent scan of actual repo, deploy, and runtime state"
author: "Alex Xiaoyu Ji"
date: "2026-07-07 (late night, post-v1.5.16 ship)"
---

# What this is

Five Explore agents ran in parallel for about two hours against actual repo, deploy, and cron state — not against brain summaries or my own memory. Each agent got a specific dimension: Shadow full-stack, Orallexa full-stack, VibeXForge full-stack, cross-project design and brand, cross-project security/perf/cost.

The point was to find the gap between what the READMEs claim and what the code and data actually say. Some findings are ship-ready confirmations. Some are things that have been quietly broken for weeks. One is a genuinely uncomfortable financial reality about VibeXForge.

The rest of this doc is what I'd want to be able to grep in three months when I'm asking myself "wait, what did the audit find about X."

---

# The three things to fix tonight (about 30 minutes)

Ranked in the order I'd do them.

## 1. VibeXForge — apply migration 075 to production

`.private/migrations/APPLIED.md:110-112` states in plain text that migration `075_tip_interest.sql` has never been applied to the production Supabase project. The tip button on `/project/[id]` is wired (commit e7d3562, 2026-06-04), but the table it writes to does not exist. Every user who has clicked the tip button since 2026-06-04 has received a 500 error. That is thirty-three days of a broken production feature with no PMF signal being collected.

The fix is documented in the migration file itself: open Supabase Dashboard SQL Editor, paste `.private/migrations/075_tip_interest.sql`, run it, verify with `SELECT count(*) FROM tip_interest;`. Fifteen minutes.

The reason to do this first is that it is the only finding here where a real user action (someone trying to give me money) is currently failing.

## 2. Shadow — SKILL.md tool count is stale

`SKILL.md:14-19` lists six MCP tools. The repo actually ships eight: v1.5.12 added `shadow_verify_attestation` and v1.5.15 added `shadow_size_position`. The `mcp/server.js` TOOLS array has eight entries, the `api/mcp-manifest.js` CANONICAL_TOOLS has eight, `installer/tools.json` has eight, `lib/auth/oauth-scaffold.js` ALL_TOOLS has eight. Only SKILL.md — the file that appears on `skills.sh` for anyone browsing the marketplace — is behind.

Two new lines in the tools frontmatter. Five minutes.

## 3. Orallexa — README test count is off by about 290%

README.md line 20 claims "1300+ Tests." An actual pytest collection returns 333 tests across 82 files. This is not close to 1300. Under-promising is the correct direction here; over-promising in a testing claim is exactly the kind of thing a procurement reviewer or a hiring manager will catch and quietly hold against you.

Change the number to 333, or the phrase to "300+ tests," or something similar. Two minutes.

---

# Shadow

Ship-ready across almost every dimension the agent checked. This is the strongest case study in the portfolio and the one most aligned with the "AI infrastructure for regulated industries" proof statement.

## What checks out

- 760/761 tests are real code exercises, not CSV greps. Test files sum to 9,283 lines. The one skipped test is expected (OCR live smoke test gated on billing envelope, not a regression).
- CI runs Node 22 plus Python 3.9-3.13 matrix. The Python matrix is the "cross-language attestation regression" invariant, and it exists in `.github/workflows/test.yml`.
- All eight MCP tools are wired to real code, not stubs. The mcp/server.js dispatch has actual handlers for each tool.
- CITATION_MAP.md v1.1 shipped today. Every regulatory citation in the map (SR 26-2, Reg B, ECOA, BSA, OFAC, FinCEN, GDPR Art. 22, Schufa C-634/21, Reg BI) points to a real test file with a real assertion.
- The four case studies at `docs/case-studies/` are real numeric verdicts (FICO 640 → block; OFAC SDN hit → block; $2.5M CRE with PEP → escalate; clean auto loan with Macro Contrarian dissent → still approve). Not templates.
- Attestation surface signs both HMAC and Ed25519. v1.5.16 threads previous_hash across all three vertical dispatch paths. The chain-store singleton in `lib/attestation-chain-store.js` is what closed the last "blocks-procurement" gap identified by an earlier audit today.

## What needs work

**Mobile is broken.** `index.html:95` uses `grid-template-columns: 220px 1fr 380px` with no `@media` breakpoint. On an iPhone width the left sidebar plus center canvas plus right HUD all stack vertically and buttons pile on top of each other. If a hiring manager pulls up alexji.dev on their phone (they will), Shadow's case study lands on a broken layout. This is the single highest-leverage fix for portfolio readiness.

**Accessibility fails at two spots.** No `:focus-visible` states on any of the interactive buttons in `src/style.css`. And muted labels are gray (#6B7280) on dark (#0B0D12) which is about 3.1:1 — below WCAG AA. Both are fixable in about thirty minutes.

**"Per-voice routing coming next" is honest but incomplete.** README.md:35 says diagnostic-only, and `lib/provider-diversity.js` does compute the assignment without dispatching to different providers. Not a false claim, just a partially-shipped feature. Not blocking procurement.

**SKILL.md tool count is stale.** Covered in the tonight-fixes above.

## Portfolio readiness

Deepest case in the portfolio. Live Vercel demo at `shadow-mentor-o033hfcya-alex-jbs-projects.vercel.app` (Deployment Protection is currently OFF — `/api/health` returns 200 without auth). Signature metrics: 760/761 tests, 87±3 agentic score, Ed25519 attestation, MIT-licensed, regulatory citations. Ready to be the hero case at alexji.dev.

The one thing missing before the alexji.dev launch: capture a screenshot of the XREAL spatial AR panels from the demo (index.html:127-156). That is the most visually distinctive part of the surface, and it does not appear on the current README.

---

# Orallexa

Solid engineering, honest-vs-claim mismatches in the README, and a positioning question that matters for the portfolio.

## What checks out

- 333 tests across 82 modules, 17,978 test LOC. Test surface is real, the number just needs to match what the README says.
- CI enforces 70% Python coverage floor, runs vitest for the UI, and skips (does not fail) LLM tests when the billing envelope errors. The skip-not-fail rule is exactly the correct posture given the current credits situation.
- FinPos Risk Sizer contract tests pin all seven named invariants (`tests/test_risk_sizer_contract.py:8-12`). Risk Sizer voice never emits a direction, respects Kelly cap, scales inversely with volatility, shrinks under drawdown — all pinned.
- v1.2.0 → v1.2.1 wire-up of Risk Sizer into the LangGraph debate is documented and tested.
- The 5-voice debate architecture (Bull/Bear/Judge/Critic/Polyseer) uses Haiku for Bull/Bear/Critic and Opus for Judge synthesis. That model split is defensible.

## What does not check out

- **README says "1300+ Tests." Actual is 333.** Fix this before anyone reads the repo through a procurement lens.
- **Paper trading log has three closed trades.** Total. Cumulative P&L is -$22.41 across three losing positions. Zero wins. README does not disclose this. If someone reads the trading claims (README.md:76-91 has an evaluation table) and then goes to look at `paper_trades_closed.jsonl` they will find three losses and no wins.
- **The evaluation table is synthetic, not live.** Walk-forward Sharpe numbers in the table are backtest results, not paper-trading results. This is normal for research code but the README does not draw the distinction clearly.

## Portfolio positioning question

Orallexa is a quant trading system with prediction-market voting. It is technically strong. But it is not "AI infrastructure for regulated industries" — it is closer to "calibrated multi-agent decision-making in finance." The proof statement claim was specifically about regulated compliance. Two options:

1. Rewrite the alexji.dev `/work` narrative to include "calibrated multi-agent inference across financial verticals" as an umbrella that Shadow and Orallexa both fit under. This is honest and probably the right move.
2. Downgrade Orallexa to a secondary case study and lead the portfolio with Shadow plus Solo Founder OS. Solo Founder OS has shipping proof (500+ tests, PyPI live) that fits the "ship at cadence" habit story cleanly.

I think option 1 is stronger. Orallexa's paper-trading calibration log is a good example of the "publish the losses honestly" habit — that habit is a hiring signal.

## Runtime state

Kill state is `{"can_trade": true}` as of 2026-07-08 01:02 UTC. Cron plists are loaded but the LLM voices are dead because Anthropic credits are dry. Recent polymarket-decisions.jsonl fires show edge computed but no positions opened (which is correct behavior with credits out). No silent errors, no dropped rows.

---

# VibeXForge

This is the section that is hardest to write.

## What checks out

- 76 Supabase migrations applied, spanning full lifecycle (auth, RLS, triggers, RPCs, notifications, share tracking, rate limiting, directory submissions).
- Schema architecture is sound — RLS is tight, SECURITY DEFINER RPCs are gated per AGENTS.md, column-level grants for PII.
- Analytics stack is integrated: HyperDX and OpenPanel both have `NEXT_PUBLIC_*` client keys set. Instrumentation is in place.
- The admin metrics dashboard at `app/admin/metrics/page.tsx:42-114` queries live Supabase data and can bucket by referral source, evolution stage, and draft funnel outcome.
- The pre-launch runbook at `docs/LAUNCH_DAY.md` was executed and the site launched on 2026-05-13.
- Landing page design is genuinely polished — pixel forge, anvil animation, boot log, custom cursor. This is not weak craft.
- CI enforces a hardcoded-secrets scan, SQL migration lint, and a schema-reference audit script. Test-on-PR gate is present.

## The uncomfortable things

**Migration 075 has been unapplied for 33 days.** Covered above. Every tip button click since 2026-06-04 returns a 500. The PMF signal that the Backer Mode v0 was supposed to collect has been collected as zero, but not because interest is zero — because the endpoint is broken.

**No post-launch metrics report has been published.** The admin dashboard exists. The queries are wired. But no report has been written from the data. Brain claims 4 users, 22 views, 0 upvotes as of 2026-06-14. That is the most recent number available anywhere in the repo. Six weeks later, no follow-up. Nobody knows if it grew to 40, stayed at 4, or fell to 0.

**Devlogs from 2026-06-12 through 2026-06-28 are 237-byte autogen stubs.** The manually dense devlog days are earlier (2026-06-11 is 1,217 bytes for a security sweep). Development velocity feels like it dropped after launch, at exactly the moment when you would want it highest.

**Recent commits are cosmetic features on a product with no user growth signal.** June 15 through June 28: funeral feature (visual identity, OG cards, parchment scrolls, bells), then cracked-score, then cofounder-roast. All well-executed. None of them address the 4-user cold-start problem. This is the "shipping features instead of finding product-market fit" pattern.

**The unit economics do not work at current user count.** Brain flagged $131 in Vercel build minutes for April 2026. That number has been addressed via `ignoreCommand` for docs-only pushes, and current per-build cost is around 50 seconds ≈ $0.10. But the run-rate is still measured in tens of dollars per month on Vercel plus $25-100 on Supabase Pro. Against four users and twenty-two views, that is somewhere between $5 and $50 per project view. This is not a rounding error; it is a category that would kill a real startup.

**The "5-year commitment" has no written go/no-go metric.** Brain says Alex committed 5 years to VibeXForge per North Star lock 2026-05-07. Searching the repo, no such North Star document exists. The commitment exists in memory but not in writing, which means it also does not have a written condition under which it would be revisited.

That combination — no written commitment, no written condition to revisit, six weeks of cosmetic features on a product with unmeasured user growth — is the textbook shape of a sunk cost trap. I would not be writing this section this bluntly if the numbers were closer to reasonable.

## What to do about it

Not "kill VibeXForge tonight." Definitely not.

What to do tonight is apply migration 075 so that if users are trying to tip, they can. Fifteen minutes.

What to do this week is publish an honest post-launch metrics report — even a single-page spreadsheet — that tells the truth about users, projects, views, referral sources, and any evidence of the "10-channel distribution" claim actually delivering distribution. If the numbers are good, the sunk-cost concern evaporates. If they are not, you need to have looked at them anyway.

What to do this month is write the go/no-go decision document. Ninety-day gate. Specific thresholds: users, projects, engagement, or revenue. Above the line, continue. Below the line, either pivot or sunset. A 5-year commitment without a written 90-day gate is not a strategy, it is momentum plus habit.

For the portfolio specifically: right now VibeXForge is portfolio-blocking. The design is beautiful, but there is no user proof anywhere in the repo that a hiring manager could point to. Either (a) get real user data captured before Week 6-7 of the FlyRank track and use it, or (b) lead the portfolio with Shadow + Solo Founder OS and demote VibeXForge to a mentioned side project.

---

# Cross-project themes

## Visual and brand consistency

The three projects use three different accent colors (Shadow emerald, Orallexa gold, VibeXForge forge orange), three different typography systems (system sans, Lato, Press Start 2P plus Geist), and three different voice registers (B2B regulatory, technical quant, consumer indie). They read like three different founders shipped them.

Alex's proof statement claims "same person." The visual and copy evidence across the three projects contradicts that claim. For the alexji.dev launch, either unify the three project cards visually so they feel like they come from the same person, or lean into the contrast as an intentional "I work across surfaces" story. The latter is harder to sell.

## Copy positioning

Shadow's README reads B2B regulatory. Orallexa reads technical quant. VibeXForge reads consumer indie-hacker marketing. The proof statement is B2B regulatory. Two out of three do not reinforce the claim, and VibeXForge actively works against it because "distribution amplifier for solo AI creators" is a different job than "AI infrastructure engineer for banks."

Fix on the alexji.dev side, not by rewriting the three project READMEs. The alexji.dev `/work` page can tell one story that reads coherently even if the three underlying READMEs are each targeted at their own audiences.

## Case study depth ranking

Shadow → Orallexa → VibeXForge, in strength order. Shadow is portfolio-ready today. Orallexa is portfolio-ready with a positioning caveat. VibeXForge is portfolio-blocking until real user data exists.

If VibeXForge cannot get user proof before Week 6-7, consider using Solo Founder OS as the third case. Solo Founder OS has 500+ tests on PyPI and is a real production dependency for eleven agents. That is more shipping evidence than what VibeXForge currently has published.

## Mobile

Shadow demo is broken on iPhone width. VibeXForge landing is responsive. Orallexa dashboard is likely fine but was not tested in detail. Fix Shadow first, since that is the hero case study.

## Accessibility

Shadow fails focus states and label contrast. VibeXForge passes (color contrast, focus visible, alt text, ARIA). Orallexa was not tested in detail but the Art Deco palette on dark may have muted-text contrast issues worth checking.

---

# Security, performance, cost, ops

## Security

Secrets are clean. `.gitignore` covers `.env*.local` on all three repos. No hardcoded API keys found in git history. `SUPABASE_SERVICE_ROLE_KEY` in VibeXForge is used only in server-side RSC and API routes, not in client bundles.

Shadow public endpoints (`/api/attestation-info`, `/api/badge`, `/api/calibration`) return 200 without auth by design — those are intended to be publicly readable so bank auditors can independently discover the public key and verify attestations. Deployment Protection is confirmed OFF on Shadow (`/api/health` returns 200 without auth), which is the correct posture for public demo.

VibeXForge auth gates 401 on missing tokens. `/api/health` returns 307 (likely login redirect). Behavior looks correct.

## Cost

Anthropic key at `~/.config/anthropic_key` last modified 2026-06-15 (22 days ago). OpenAI key last modified 2026-06-21 (16 days). Both are stale relative to actual account state. Anthropic credits are confirmed dry (crons that depend on it are producing empty JSONL rows).

Vercel build cost peaked at $131 in April 2026 for VibeXForge and has been addressed via `ignoreCommand` for docs-only pushes. Current per-build is around $0.10.

Supabase runs on Pro tier for VibeXForge (needed for the service role key and RLS features).

## Performance

Shadow test suite is fast (Node bare `--test` runner, no pytest overhead). VibeXForge bundle is optimized (`optimizePackageImports` for lucide-react and framer-motion), though `next/image` is set to `unoptimized: true` because of the Vercel free-tier image transform quota — that trade-off is documented in the repo.

## Ops

Roughly 44 launchd entries loaded across the stack after tonight's unload of the ten LLM-dependent crons. Two dead plist files remain on disk (`com.orallexa.idea-scanner.plist` and `com.orallexa.polyalert-bot.plist`) — the plist is unloaded but the file is still there. Cleanup is optional but tidy.

---

# The 90-second version

Ship these tonight (30 minutes total):

1. VibeXForge: apply migration 075 via Supabase SQL Editor
2. Shadow: update SKILL.md tools list from 6 to 8
3. Orallexa: fix the "1300+ tests" claim to 333

Ship these this week when you have head-space:

4. Shadow: add mobile responsive breakpoint to index.html
5. Shadow: add `:focus-visible` and fix muted-text contrast
6. Orallexa: add a "paper-only, cumulative -$22.41" honesty section to the README

Decide this month, before alexji.dev launch:

7. Orallexa positioning: does it fit under "regulated industries" umbrella, or does it get demoted?
8. VibeXForge: publish the honest metrics report, then write the 90-day go/no-go doc.
9. Cross-project design: pick one accent palette and one voice register for the alexji.dev `/work` cards, even if the underlying READMEs stay heterogeneous.

Do not do these unless a specific reason emerges:

- Refactor Shadow's per-voice routing (four hours plus LLM cost, credits are dry, README is already honest about the diagnostic-only status).
- Build more VibeXForge features. There is a real product problem, not a features problem.
- Rewrite Orallexa's README. First decide product-vs-research, then rewrite once.
