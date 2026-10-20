# Wayfinder 244 — WhoCards paid: the route to revenue

_Plan-only map. This is the in-repo copy of GitHub map [#244](https://github.com/whocards/whocards-platform/issues/244)
plus the decision pack its open children need. Agent pass: 2026-10-20._

> **What this file is for.** Every child of #244 is a decision ticket, and the decisions are Avi's.
> What an agent can do is make each one cheap to take: gather the evidence, name the options, recommend
> one, and write the resolution text. Sections 4–11 are that. A recommendation here is a proposal
> with a fallback — never a commitment. A ticket is settled when its resolution says so.
>
> **What this file is not.** Not a spec, not a build plan, and not a substitute for the HITL tickets.
> Nothing here decides anything about Avi's money, identity, or tax position.

---

## 1. Destination

Every decision needed to start charging and to aim at 1k MRR is made and closed: payment rails, entity
& banking path, pricing & packaging, the paid feature list and public roadmap, the marketing plan, and
the agent skills/workflows to run it. A follow-on execution effort then builds without stopping to
decide.

**Done when:** every child ticket is closed and execution can start from normal issues with nothing
left to decide.

## 2. The map at a glance

| Ticket                                                           | Type     | What it settles                                       | State  | Blocked by       |
| ---------------------------------------------------------------- | -------- | ----------------------------------------------------- | ------ | ---------------- |
| [#245](https://github.com/whocards/whocards-platform/issues/245) | research | Which rails are even possible for an individual       | closed | —                |
| [#246](https://github.com/whocards/whocards-platform/issues/246) | research | Entity & banking path for a Hungary-based founder     | closed | —                |
| [#247](https://github.com/whocards/whocards-platform/issues/247) | grilling | The rail combo, the entity path, the signup order     | open   | #245, #246, #254 |
| [#248](https://github.com/whocards/whocards-platform/issues/248) | grilling | Free line, unlocks, subscription contents, prices     | open   | #247             |
| [#249](https://github.com/whocards/whocards-platform/issues/249) | grilling | Accounts, device claim, where entitlements live       | open   | #247             |
| [#250](https://github.com/whocards/whocards-platform/issues/250) | grilling | Where the public roadmap lives and how it updates     | open   | —                |
| [#251](https://github.com/whocards/whocards-platform/issues/251) | grilling | The ordered paid feature list that fills it           | open   | #248, #250       |
| [#252](https://github.com/whocards/whocards-platform/issues/252) | grilling | Channels, order, owned-audience motions, campaigns    | open   | #248             |
| [#253](https://github.com/whocards/whocards-platform/issues/253) | grilling | Which agent skills/workflows the revenue effort needs | open   | —                |
| [#254](https://github.com/whocards/whocards-platform/issues/254) | task     | Whether Paddle/Polar let an individual through        | open   | Avi's KYC run    |
| [#255](https://github.com/whocards/whocards-platform/issues/255) | task     | The Hungarian tax specifics a web search can't settle | open   | Avi books it     |

Two tickets are open that are not really open: #254's agent half is done (`docs/spikes/0004-individual-signup-paddle-polar.md`)
and #255's question list is written (§11). Both wait on Avi's calendar, not on more work.

## 3. What is already settled

### 3.1 Premises (charting, 2026-09-02)

- The MRR engine is **consumer subscription**; patronage is a possible component; B2B is additive later.
- **The initial deck is always free.**
- Founder is a Hungary-based individual; **no legal entity exists**; the Apple/Google developer
  accounts are registered to the individual.
- **No self-fulfilled physical goods.**
- No hard dates; the operating budget is decided once rails + entity carrying costs are concrete.

### 3.2 Researched decisions

The #245/#246 spikes live on local branches (`research/payment-rails`, `research/entity-banking`) and
are **not in `main`**. This table is the in-repo copy of what they established.

| Rail                       | Individual, no entity? | Fee at €3.99 | Fee at €24.99 | Seller of record  |
| -------------------------- | ---------------------- | ------------ | ------------- | ----------------- |
| Paddle (MoR)               | Yes                    | ~€0.70 (17%) | ~€1.75 (7%)   | Paddle — all VAT  |
| Polar.sh (MoR)             | Yes                    | ~€0.76 (19%) | ~€1.90 (8%)   | Polar — all VAT   |
| Apple IAP (Small Business) | Yes                    | €0.60 (15%)  | €3.75 (15%)   | Apple             |
| Google Play IAP            | Yes                    | €0.60 (15%)  | €3.75 (15%)   | Google            |
| Stripe direct              | No — needs sole trader | ~€0.36 (9%)  | ~€1.10 (4%)   | Founder — VAT/OSS |

- Paddle: 5% + 50¢, monthly payout by the 15th, $100 minimum, HUF or EUR, SEPA free.
- Polar: 5% + 50¢ **+1.5% on non-US cards** (almost every WhoCards buyer), on-demand payout, $10
  minimum, HUF via Stripe Connect Express, first review up to 14 days.
- Stores: 15% under Apple's Small Business Program and Play's first-$1M tier.
- Entity: **egyéni vállalkozó + átalányadó** — free, same-day, online. US LLC ruled out (no tax
  saving, $25k Form 5472 penalty, treaty gap). Kft deferred until profit clears ~70–100k HUF/month.
- No entity is needed to take a first payment through an MoR; the sane no-entity window is
  **days-to-weeks**, because recurring profit-oriented activity needs registration.

### 3.3 What the repo already fixes

| Fact                                                                                                | Where it is written                                                            |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| The play engine is access-blind; one entitlement seam                                               | `docs/adr/0006-entitlement-seam.md`, `apps/mobile/src/lib/entitlements.ts`     |
| Global Game is a shared-progress overlay; Personal Game persists to the account                     | `docs/adr/0004-global-game-progress-overlay.md`                                |
| Accounts = better-auth (magic link + Google), Drizzle/Postgres, RBAC roles                          | `docs/adr/0008-authenticated-app-on-tanstack-start.md`, `apps/app/src/server`  |
| Access tiers are `free` / `unlock` / `subscription`; Pick a Card is `unlock`; Personal Game is paid | `CONTEXT.md`, `apps/mobile/src/lib/games.ts`                                   |
| Pick a Card is **built** and shipping as `early_access`                                             | `packages/decks/src/engine/games.ts`, CHANGELOG 1.1.0                          |
| Personal Game, Custom Decks, Facilitation Mode are **not built**                                    | engine has only `wh` and `pick`; CONTEXT.md marks Facilitation "not yet built" |
| Mobile has **no accounts at all** — anonymous Device ids only                                       | `packages/api/src/trpc.ts`, no auth in `apps/mobile/src`                       |
| A 61-question "Beautifully" deck exists but is unregistered                                         | `packages/decks/src/decks/beautifully.ts`                                      |
| Audience: 307 unique visitors in 3.5 weeks, 88% mobile, 75% HU, ~0% retention, 18 Library opens     | `docs/growth/01-analytics-baseline.md`                                         |

## 4. #247 — payment rails + entity path (the keystone)

**Recommendation.**

1. **Web: Paddle.** Cheapest MoR for a European basket, HUF and EUR payouts, no company needed.
   Polar is the documented fallback if Paddle's live signup rejects an individual (#254).
2. **Mobile: plain IAP on both stores**, at 15%, with Apple's Small Business Program enrolled on day
   one. No external-purchase links, no anti-steering experiments — that is a later optimization.
3. **Glue: one entitlement layer** (RevenueCat for store receipts; Paddle webhooks for web) writing to
   one server-side entitlement table. Web purchases are honored in-app under Apple Guideline
   3.1.3(b) **provided the same item is also purchasable via IAP** — so the SKU list is mirrored
   everywhere (§5).
4. **Entity: none at first sale.** DSA trader self-declaration as an individual, MoR checkout live,
   then register the sole proprietorship at or just before first real revenue. Not a company.
5. **Stripe direct is the graduation path**, not the start: it needs the sole trader anyway, and it
   moves VAT/OSS onto the founder.
6. **Patreon: optional patronage tier**, web-side perks only, never the in-app paywall.

**Signup order** (mechanics first, each step unblocks the next):

| #   | Step                                                      | Who         | Time             |
| --- | --------------------------------------------------------- | ----------- | ---------------- |
| 1   | DSA trader declaration, both stores                       | Avi         | 15 min           |
| 2   | Paddle account + identity verification (also closes #254) | Avi         | ~20 min + review |
| 3   | Apple Small Business Program enrolment                    | Avi         | 5 min            |
| 4   | Google Play payments profile + 15% tier confirmed         | Avi         | 15 min           |
| 5   | RevenueCat project, products, store credentials           | Avi + agent | ~1 h             |
| 6   | Sole-trader registration + community VAT number           | Avi         | free, same day   |
| 7   | Accountant consult (#255), before the first payout        | Avi         | 1 h + scheduling |
| 8   | Stripe direct — only when fees justify the VAT admin      | later       | —                |

**Operating budget, graduated.** Proposal: €0 fixed monthly spend until the first payout; after that,
a €50/month tooling cap (RevenueCat, domain, email) until 1k MRR. Carrying costs are now known:
mellékállású sole trader has no fixed floor; főállású is ~101,682 HUF/month; MoR ~5% + 50¢; stores 15%.

**The resolution must state:** the chosen MoR and the fallback; the store rails; the entitlement layer
by name; the entity path **and the trigger** for registering; the signup order with owners; the task
tickets spawned for steps 1–7; and the graduated budget. Then #248, #249, #252 stop being blocked.

## 5. #248 — pricing & packaging

**Recommendation.**

| Surface                                                                                                        | Tier           | Price                        |
| -------------------------------------------------------------------------------------------------------------- | -------------- | ---------------------------- |
| Library deck (66 questions, 14 languages), Classic game, Global Game, offline play, share cards                | `free`         | €0                           |
| Pick a Card (built)                                                                                            | `unlock`       | €2.99                        |
| Beautifully deck (61 questions, 7 languages — already in the repo)                                             | `unlock`       | €3.99                        |
| Event / themed decks                                                                                           | `unlock`       | €4.99                        |
| Pro subscription: Personal Game, cloud progress, all premium decks, Custom Decks, Facilitation Mode when built | `subscription` | €3.99/mo or €24.99/yr        |
| Founding Player — first 100 only, price locked forever                                                         | `subscription` | €19.99/yr                    |
| Patronage (Patreon, web only)                                                                                  | optional       | $5/mo — credit + early decks |

**Why these numbers.**

- 1k MRR is 251 monthly subscribers at €3.99 — or ~200 once unlocks contribute. At €1.99 it would need
  503, which this audience cannot supply; at €9.99 conversion collapses before PMF. €3.99 sits under
  the impulse threshold and matches consumer quiz/utility apps.
- The annual plan is ~48% off (two months free): it buys cash up front and halves churn exposure in
  year one, which matters more than the discount at this scale.
- Store fees leave €3.39 net on a €3.99 monthly. That is the price of distribution, not a reason to
  price higher in-app.
- **Languages stay free.** The 14-language surface is the SEO moat and the thing incumbents don't
  have; charging for it would tax the free funnel that feeds everything else.
- **Same SKUs and prices on web and in-app.** No web-only discount: it complicates 3.1.3(b) review and
  invites arbitrage. Web exists for desktop buyers and for Paddle's local currency and VAT handling.
- **Grandfathering:** everyone playing today is on `early_access` (ADR-0006). Grant them the features
  they already use, forever, as `founding`. Cheap goodwill, and ADR-0006 promised the transition would
  be communicated rather than sprung.

**The resolution must state:** the free line, the unlock list with prices, the subscription contents
with prices, whether a patronage tier exists and its perks, the grandfathering rule, and the
store-parity rule.

## 6. #249 — accounts & entitlement architecture

**Recommendation.**

- **Identity:** better-auth in `apps/app` is the only account system. Mobile signs into it (magic link
  via Resend, deep-linked back into the app; Google later). One account, two clients.
- **Claim:** on first sign-in the client sends its Device id(s); the server links them to the account
  and stamps existing Answer rows, so history follows the player. Idempotent, one-way, and it never
  merges two accounts without an explicit second step.
- **Entitlement state:** one Postgres table — `entitlement(account_id, sku, source, status,
  expires_at, external_ref, granted_at)`, unique on `(source, external_ref)`. Server is the source of
  truth; the client never decides.
- **Writes:** Paddle webhook for web, RevenueCat (App Store Server Notifications V2 / Play RTDN) for
  mobile. Both verify, then write the same table.
- **Reads:** one `entitlements.get` tRPC query returns the account's SKU set; mobile caches the last
  known set for a 7-day offline grace, then re-checks. `getEntitlement(tier)` in
  `apps/mobile/src/lib/entitlements.ts` swaps its body to that cache lookup — **no call site changes**,
  which is exactly what ADR-0006 was built for.
- **Engine untouched.** `packages/decks` keeps knowing nothing about money.
- **SKUs:** `unlock.pick_a_card`, `unlock.deck.beautifully`, `unlock.deck.event`, `sub.pro.monthly`,
  `sub.pro.annual` — one vocabulary across Paddle, StoreKit and Play.
- **Buying requires an account.** Paddle checkout carries the account id in `custom_data`; on mobile
  the purchase attaches to the signed-in account, so an anonymous player gets one sign-in screen
  first.
- **RevenueCat** is recommended for store receipts because it removes receipt-validation edge cases at
  roughly zero cost at this scale. It is not an MoR and handles no VAT — that is Paddle's job.

**The resolution must state:** the account model, the claim flow's role, where entitlement state
lives, the read/write paths, and what the rails' SDKs dictate. Execution then writes ADR-0009, which
supersedes ADR-0006's stub and records the purchase implementation.

## 7. #250 — the public roadmap surface

**Recommendation: a `/roadmap` page on whocards.cc.**

- **Format:** three lanes — **Shipped / Building / Exploring** — one line each, no dates (the map has
  none either), each line linking to the deck or feature it describes.
- **Source:** one data file in the repo (`apps/website/src/data/roadmap.json`) rendered by
  `apps/website/src/pages/roadmap.astro`, matching how the site already ships data-driven pages.
  Updating it is a one-file PR; because the repo is public, the diff is the changelog.
- **Cadence:** first Monday of the month, plus any paid release that changes the list, in the same PR
  as the release.
- **Distribution:** linked from the paywall sheet, Settings, the post-purchase email, and `/app`.
- **Honesty rule:** only list what has a ticket. "Exploring" is explicitly not a promise.

**Rejected.** GitHub Projects/Issues as the public surface — the audience is consumers (75% Hungary),
not developers, and a roadmap behind a GitHub account is a roadmap nobody reads. A third-party surface
(Canny, Featurebase) adds a login, a monthly cost and a second source of truth, and its free tier
carries their branding. A changelog alone is not enough: a changelog answers "what changed", a roadmap
answers "what am I buying next" — and that is part of the offer.

**The resolution must state:** the surface, the format, the cadence, and who keeps it honest.

## 8. #251 — paid feature list & roadmap content

The hand-off artifact. Execution works down this table; each row is one issue.

| #   | Feature                                           | Tier           | Price               | State          | Why here                                                        |
| --- | ------------------------------------------------- | -------------- | ------------------- | -------------- | --------------------------------------------------------------- |
| 1   | Pick a Card                                       | `unlock`       | €2.99               | built          | Tests the whole paywall with the least new surface              |
| 2   | Subscription v1: Personal Game + cloud progress   | `subscription` | €3.99/mo, €24.99/yr | not built      | The recurring engine; Personal Game is already promised as paid |
| 3   | Beautifully deck                                  | `unlock`       | €3.99               | content exists | Zero build: 61 questions already in the repo                    |
| 4   | Custom Decks                                      | `subscription` | included            | not built      | Makes the subscription own a workflow, not just a mode          |
| 5   | Facilitation Mode                                 | `subscription` | included            | not built      | The bridge to the B2B ladder; needs hosted-game work            |
| 6   | Recap / share extras (story sizes, branded recap) | `subscription` | included            | partly built   | Cheap perceived value on top of existing share cards            |

**Order rationale.** Mechanics first (1), then recurring revenue (2), then content leverage (3), then
workflow (4), then the B2B bridge (5), then polish (6). Rows 1–2 map to "a working paywall soonest";
row 2 doubles as the retention lever that gates acquisition (`docs/growth/03-growth-strategy.md`);
rows 5–6 only matter once there is a base to sell to.

**Deliberately not paid:** the Library deck, all 14 languages, the Global Game, share cards, offline
play. These are the funnel — charging for any of them shrinks the top of it.

**Rule for execution:** every paid-work issue must name one row of this table. If it names none, it is
not paid work and belongs in the normal backlog.

## 9. #252 — marketing plan to 1k MRR

### The arithmetic, stated plainly

| Shape                | Units for €1,000 MRR |
| -------------------- | -------------------- |
| All monthly at €3.99 | 251 subscribers      |
| All annual at €24.99 | 480 subscribers      |
| 70/30 mix            | ~290 paying accounts |

At 5% visitor→paid that is ~5–6k monthly visitors; at 2%, ~12–15k. Today the site sees ~300 visitors a
month. The growth doc's conservative scenario (2–4k MAU) at 5% lands at €400–800 MRR; the aggressive
one (10–25k MAU) at €2–5k. **1k MRR is the conservative scenario plus one additive lane** — either
events converting harder than assumed, or the AI-at-work B2B offer. It is not reachable on the current
traffic, and no campaign list changes that; retention does.

### Channels, in order

1. **Owned audience first** — email list, app waitlist, and everyone who has already played. Zero CAC,
   and it is the only honest test of willingness to pay. Launch the paywall here.
2. **Events, ≥1/month** — the one proven channel (one event was ~75% of all traffic). Sell the premium
   event deck, keep the sponsor-funded model, capture emails on site.
3. **SEO/AEO, multilingual** — compounding and already half-built (~900 question pages, 14 languages,
   already cited by ChatGPT). The free deck is the landing page that sells the paid one.
4. **Share cards, social cadence** — acquisition, not revenue; the auto-generated cards are the unit.
5. **AI-at-work B2B** — additive, not the engine: team decks at $5–10/seat against
   `docs/strategy/ai-at-work-business-plan.md`.

### First three campaigns

| Campaign                       | Audience                                  | Offer                                         | Success metric                    |
| ------------------------------ | ----------------------------------------- | --------------------------------------------- | --------------------------------- |
| **Founding Player**            | Existing list + waitlist + recent players | €19.99/yr, first 100, locked forever          | 100 seats in 2 weeks; ≥3% of list |
| **Bring it to your table**     | Next event's attendees                    | Free deck → €4.99 event deck unlock, QR + UTM | ≥2% of players unlock             |
| **The AI Check-In, for teams** | `/ai-at-work` visitors                    | Free deck → paid team pack at $5–10/seat      | 10 qualified team emails          |

**Guardrails:** no paid acquisition until week-1 retention clears ~20%; Hungarian-first copy with
local-currency display; every link UTM'd; B2B stays additive.

**The resolution must state:** the channels and their order, the owned-audience motions, the first
three campaigns, and the numbers they are aiming at.

## 10. #253 — agent skills & workflows for the revenue effort

### What exists

| Layer                  | Where                                                                                 | Job today                |
| ---------------------- | ------------------------------------------------------------------------------------- | ------------------------ |
| Agents                 | `.claude/agents/` (architect, coder, researcher, reviewer), `.codex/agents/`          | Build, review, research  |
| Repo skill             | `skills/whocards-questions/` (+ `.agents`, `.claude` mirrors)                         | Question authoring       |
| Release pipeline       | `.github/workflows/mobile-release.yml`, `release-it`, `scripts/release`               | Tag → build → submit     |
| Observability          | `packages/observability`                                                              | Errors, events, identity |
| External (not in repo) | wayfinder, grilling, domain-modeling, codebase-design, babysit, device-loop, research | Map, decide, loop        |

### Build now (before execution)

- **`mrr-review`** — a weekly loop: MRR, new/churned, ARPU and the visit→paywall→paid funnel, posted as
  a ten-line digest, flagging any channel whose conversion moved. Constraint: the PostHog MCP is
  unauthenticated in agent sessions, so this must read the PostHog HTTP API with a project-scoped key
  from the environment — not the MCP.
- **`paywall-qa`** — a device loop that buys, restores and cancels in sandbox on iOS and Android, and
  verifies a web (Paddle) purchase is honored in-app. Runs before every paid release; this is the one
  workflow whose absence would ship a broken paywall.

### Build when execution starts

- **`launch-copy`** — turns a roadmap row into store/email/social copy in EN + HU.
- **`revenue-triage`** — a babysit variant that picks the next item from §8 rather than the general
  backlog.
- **`roadmap-sync`** — regenerates `roadmap.json` from closed tickets and opens the monthly PR.

### Do not build

A metrics dashboard (PostHog is one), a release workflow (exists), or any agent that touches KYC,
payment credentials or tax filings. Those stay human, permanently.

## 11. #255 — the Hungarian accountant consult

The agent half is done: the question list exists (below). Avi books the call; answers get recorded on
the ticket in general form, never with personal figures.

| Question                                                    | What the answer changes                    |
| ----------------------------------------------------------- | ------------------------------------------ |
| Mellékállású or főállású status?                            | Whether a ~101,682 HUF/month floor applies |
| Is átalányadó the right regime for app and store revenue?   | Net margin, not list prices                |
| SME-scheme registration, community VAT number, A60 filings? | Admin load after registration              |
| How are MoR payouts characterised for Hungarian income tax? | Whether invoicing is needed at all         |
| Is Apple's US withholding a non-issue post-treaty?          | Store net revenue, at the margin           |

None of these change the direction (sole trader + átalányadó, no US LLC) or the prices in §5 — MoR
prices are VAT-inclusive, so the tax answers move margin, not the price tag.

## 12. Execution handoff

Open these as normal issues once the map closes. Nothing here needs another decision.

| #   | Issue                                                          | Done when                                                                 | Needs      |
| --- | -------------------------------------------------------------- | ------------------------------------------------------------------------- | ---------- |
| E1  | Paddle products, checkout, webhook → entitlement table         | A sandbox purchase grants the SKU to a real account                       | #247       |
| E2  | Store IAP: products, RevenueCat, restore, web-purchase honored | Sandbox buy + restore + cross-surface grant verified on both OSes         | #247       |
| E3  | Accounts on mobile + Device claim                              | A device's history follows it into a fresh install                        | #249       |
| E4  | Gate Pick a Card behind `unlock` + founding grants             | Locked for new players, free forever for early-access players             | E2, E3     |
| E5  | Subscription v1: Personal Game + cloud progress                | A subscriber's progress persists across devices; a free player's does not | E3         |
| E6  | `/roadmap` page live and linked                                | Page shipped, linked from paywall, settings and post-purchase email       | #250, #251 |
| E7  | Founding Player campaign                                       | 100 seats sold, or the price/offer tested and revised                     | E1, E2, E6 |
| E8  | `paywall-qa` and `mrr-review` live                             | Both run on schedule; a broken purchase is caught before release          | E1, E2     |

Sequence: E1/E2 in parallel, E3 next, E4/E5 after, E6 anytime, E7 last, E8 before E7.

## 13. Risks and fallbacks

| Risk                                        | Fallback                                                                                       |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Paddle rejects an individual at live signup | Polar.sh (documented individual path, HUF payouts)                                             |
| Both MoRs reject an individual              | Register the sole trader first — still no company, still same day                              |
| Store review rejects honoring web purchases | Honor entitlements only; never link out or mention web pricing in-app                          |
| Conversion under 1%                         | The offer is wrong, not the price: test annual-first and the €2.99 unlock before cutting price |
| PostHog gaps hide revenue                   | The entitlement table is the revenue source of truth, not analytics                            |
| Accountant surprises on tax                 | Margin moves, prices don't — MoR prices are VAT-inclusive                                      |

## 14. Closing this map

| Ticket | Closes when                                                                   |
| ------ | ----------------------------------------------------------------------------- |
| #247   | Rails, entity path, signup order and budget are stated; task tickets exist    |
| #248   | Free line, unlocks, subscription, prices, patronage and grandfathering stated |
| #249   | Account model, claim, entitlement storage and rail SDK constraints stated     |
| #250   | Surface, format, cadence and owner stated                                     |
| #251   | Ordered feature list posted and the roadmap populated from it                 |
| #252   | Channels, order, owned-audience motions, first three campaigns stated         |
| #253   | Skills/workflows listed with one-line jobs; build-now tickets opened          |
| #254   | The live-run table is filled (Avi), or the blocking screen is recorded        |
| #255   | The accountant's answers are recorded in general form (Avi)                   |

When those nine are closed, #244 closes and execution starts at §12. The only thing standing between
this file and that outcome is Avi's time on #247–#253 plus two appointments — not more research.

## Sources

- Map and children: [#244](https://github.com/whocards/whocards-platform/issues/244) and its
  sub-issues #245–#255.
- Rails and entity research: resolutions on #245/#246 (spikes on `research/payment-rails`,
  `research/entity-banking`, not in `main`).
- Live-run instrument: `docs/spikes/0004-individual-signup-paddle-polar.md`.
- Architecture: `docs/adr/0004`, `0006`, `0008`; `CONTEXT.md`.
- Demand and channels: `docs/growth/01-analytics-baseline.md`, `03-growth-strategy.md`,
  `04-app-launch-plan.md`; B2B ladder in `docs/strategy/ai-at-work-business-plan.md`.
