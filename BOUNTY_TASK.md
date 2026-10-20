# Bounty Objective: Wayfinder map — WhoCards paid: the route to revenue
Target: whocards/whocards-platform Issue #244
URL: https://github.com/whocards/whocards-platform/issues/244

## Issue Description
## For humans

**What:** The shared map of every decision between today and "WhoCards charges money, with a plan to 1k MRR."
**Why:** All work from here on is meant to be paid features, but rails, legal entity, pricing, roadmap, and marketing are undecided — nothing can be built until they're settled.
**Done when:** Every child ticket is closed and execution can start from normal issues with nothing left to decide.

---

## Destination

Every decision needed to start charging and to aim at 1k MRR is made and closed: payment rails, entity & banking path, pricing & packaging, the paid feature list and public roadmap, the marketing plan, and the agent skills/workflows to run it. A follow-on execution effort then builds without stopping to decide.

## Notes

- **Plan-only map**: tickets resolve decisions, not deliverables. Exception: `wayfinder:task` tickets for real-world unblockers (service signups, entity paperwork) do get done on-map.
- **Operating principle (Avi, 2026-09-02): mechanics ASAP, iterate.** Bias every decision toward whatever gets a working paywall soonest.
- **Settled premises from charting (2026-09-02):**
  - The MRR engine is **consumer subscription**; patronage (Patreon-style) is researched as a possible component; B2B is additive later, not the engine.
  - **The initial deck is always free.**
  - Founder is a Hungary-based individual; **no legal entity exists**; Apple/Google developer accounts are registered to the individual.
  - **No self-fulfilled physical goods** — prior fulfillment experience ruled it out.
  - No hard dates; operating budget is decided once rails + entity carrying costs are concrete.
- **This repo is public.** Tickets and resolutions carry no personal financial data, account numbers, or tax details.
- Vocabulary: `CONTEXT.md` — especially **Access tier** (`free` / `unlock` / `subscription`), **Personal Game**, **Pick a Card**, **Device**→account claim.
- Prior art every session should load: `docs/growth/03-growth-strategy.md` (retention gates acquisition), `docs/growth/04-app-launch-plan.md`.
- HITL tickets grill Avi (skills: `grilling` + `domain-modeling`). Research tickets use the `research` skill, findings on a `research/<name>` branch.

## Decisions so far

- [Research: payment rails for WhoCards](https://github.com/whocards/whocards-platform/issues/245): web via a merchant of record (Paddle or Polar.sh) — first payment possible in days, pre-entity, VAT on the platform; mobile must use IAP at the 15% small-developer rates with a shared entitlement layer (web purchases honorable in-app under Guideline 3.1.3(b)); Stripe direct is the graduation path (requires registered sole trader); Patreon only as an optional patronage tier.
- [Research: entity & banking path for a Hungary-based founder](https://github.com/whocards/whocards-platform/issues/246): register egyéni vállalkozó on átalányadó (free, same-day, online) at or just before first real revenue; a US LLC is ruled out (no tax reduction, $25k-penalty filing, dual-residence risk); Kft deferred until profits clear its ~70–100k HUF/month overhead; charging can technically start pre-registration via an MoR, but only for days-to-weeks.

## Not yet specified

- Print-on-demand merch (stickers, shirts) as a perk or side lane — waits on pricing & packaging.
- The B2B additive offer (events playbook, AI-at-work angle, team decks) — waits on the consumer engine being settled.
- Operating budget — carrying costs now researched (mellékállású EV: no fixed floor; főállású: ~101,682 HUF/month; MoR ~5% + $0.50/txn; stores 15%): the budget decision graduates at "Decide: payment rails + entity path".
- Migrating the individual Apple/Google developer accounts to an organization — waits on the entity decision.
- Build order for the retention loop (accounts, claim flow) — the architecture decision is ticketed; sequencing graduates after it.

## Out of scope

- Selling or shipping the remaining physical deck inventory — fulfillment is ruled out by experience. Decks may still be given away at events, but no store, no shipping.
- Building the paid features themselves — that is the follow-on execution effort this map hands off to.



## Instructions for Kuro (Autonomous Solver):
1. Use skills: `systematic-debugging` and `code-review-and-quality`.
2. Inspect the repository structure, locate relevant files, and understand the bug/feature.
3. Apply the minimal, robust fix or implementation.
4. Run any existing test suites or linters in the workspace.
5. Ensure the fix adheres to the project's existing code style.
6. When done, output a summary of files modified and verification results.
