---
title: "What am I proving?"
subtitle: "FlyRank Week 1"
author: "Alex Ji"
date: "2026-07-07"
---

# What am I proving?

The short version is: I build AI systems for banks and other regulated industries. The kind that a compliance officer will actually sign off on, which is a very different thing from an AI system that "works."

## The one sentence I want a stranger to remember

I ship AI infrastructure that survives both a fast product cycle and a bank auditor's review.

Both parts matter. Anyone can write an agent that works on a demo screen. Not many people can write one that a bank's second-line risk officer will accept as evidence in a Reg B audit six months later. That second part is the whole game for me right now.

## Who I want to reach

Two groups, and they're closer to each other than they look.

The first is engineering hiring managers at AI-native banks, brokerages, and insurers. Fintechs that already ship real code and are hitting the "how do we make this defensible?" wall.

The second is founders who are 6-12 months in, sold their first pilot, and are realizing the compliance work is going to eat the roadmap unless they hire someone who's already lived it. I'd want to be their first or second engineer.

Same story reaches both because the evidence is the same.

## Evidence

Three shipped systems, listed in the order I'd point someone at.

**Shadow** — five-to-six voice AI compliance council for loan origination. Right now it's 761 tests, three verticals live (banking, trading, data-science), an Ed25519 attestation chain, MIT-licensed at github.com/alex-jb/shadow-mentor. I shipped v1.5.16 this afternoon and cut a GitHub release with cross-vertical hash-chain continuity. This is the anchor case. A bank counsel can grep the repo and find the exact test file that pins any regulatory citation I claim.

**Orallexa** — five-voice quantitative trading agent with a public paper-trading log. Bull / Bear / Judge / Critic / Polyseer plus a Risk Sizer. The interesting part is the calibration log. I publish the Brier score even when it's bad, and I've been paper-only for six weeks because the log says the edge isn't there yet. That discipline is what I'd tell a hiring manager to look at, more than any single winning trade.

**Solo Founder OS** — eleven-agent Python stack that automates the ops surface of a one-person company. Marketing, customer support, cost audit, bilingual sync, retro. 500+ tests. On PyPI. Less regulatory, more about proving I can ship at cadence without breaking things.

## How I actually work

Three habits, and they're the reason the evidence above holds up.

1. I write the test first. Or at least, I write the invariant I'm defending, then I write the test that fails until I've defended it. Shadow's 761 tests aren't there because I like tests — they're there because that's the only way a compliance officer will trust the code.

2. I publish the calibration honestly. Orallexa's paper-trading loss log is public. That's uncomfortable but it's the reason I'd expect anyone to trust the wins when they come.

3. I ship small. Fifteen commits today. Two GitHub releases. No branch got over 200 lines of diff. That's how you keep the "shipping code" habit and the "surviving audit" habit from fighting each other.

## The action I want visitors to take

Book a 15-minute call. That's the only thing on the site. No newsletter, no whitepaper, no "join the waitlist." A call is enough signal for me to know if there's a real fit, and it's the smallest thing I can ask a hiring manager or founder to do.

## Why this pitch, right now

Two things happened in the last three months that matter.

Anthropic shipped ten financial-services agents to LPL and Raymond James in early May. Thirty thousand advisors are now touching an agent daily. The regulated-AI question moved from "is this even possible" to "how do we make the ones we just shipped defensible." That's my window.

And in April the Fed / OCC / FDIC jointly rescinded SR 11-7 and replaced it with SR 26-2. GenAI and agentic AI got explicitly carved out of Tier 3. The class of AI I work on isn't governed by the traditional model risk framework anymore. Shadow is the companion control for that class.

So the pitch isn't a hypothesis. It's a working stack, in a policy window that just opened this quarter.
