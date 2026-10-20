# Spike 0004: no-entity signup — Paddle vs Polar.sh, Hungary payouts

**Status:** research complete from public vendor docs (checked 2026-10-20). The live signup + KYC run
has **not** happened — it is a human step, by design. That is the open blocker on #254, and the
capture checklist below is what closes it.
**Feeds:** #254 (child of the wayfinder map #244; surfaced by #245 and #246). **Blocks:** #247
(decide payment rails + entity path).
**Caveat:** every fee and threshold here was read off the vendors' own pages on 2026-10-20 and will
drift. Re-check before #247 signs anything.

## TL;DR

Both providers publicly accept an **individual with no company**, and both can pay out to a
**Hungarian** resident. Neither needs a Hungarian entity, a VAT number, or an accountant to start —
they are both Merchants of Record, so they take on the sales-tax side entirely.

That answers the "is it even possible" half of #254 from documentation. It does **not** answer the
half that only a live run can: whether the actual form, in Hungarian, lets an individual through
without inventing a tax number, and whether the reviewer approves a one-person app studio. Hence the
checklist, not a conclusion.

The one finding that already looks decision-relevant for #247: **Polar's headline 5% + 50¢ is not
what a European customer base pays.** Polar adds **+1.5% for every non-US card**, so a mostly-EU
audience is ~6.5% + 50¢ — roughly 30% more than Paddle's flat 5% + 50¢ — and Polar additionally
passes through Stripe payout fees that Paddle does not charge on SEPA.

| | Paddle | Polar.sh |
| --- | --- | --- |
| Individual / sole trader accepted | Yes — ownership docs explicitly "not required for individuals or sole traders" | Yes — "personal account" is a first-class path in payout setup |
| Needs a company | No | No |
| Hungary payouts | Yes — HUF or EUR, wire or Payoneer | Yes — HUF via Stripe Connect Express |
| Payout cadence | Automatic, monthly, by the 15th | On demand (manual withdrawal) |
| Payout minimum | $100 equivalent | $10 (USD) / $13 (EUR) equivalent |
| Transaction fee | 5% + 50¢, all-inclusive | 5% + 50¢ **+1.5% non-US cards** |
| Payout fees | None on SEPA; $15 SWIFT only on currency mismatch | $2/mo while payouts are active, 0.25% + $0.25 per payout, +0.25% cross-border (EU) |
| Review wait | Identity typically instant; business check 2–4 business days if manual | Up to 14 days for the first payout review |

## What the public docs actually establish

### Paddle

Sources (all `paddle.com/help`, read 2026-10-20):

- [Business Identification](https://www.paddle.com/help/start/account-verification/what-is-business-verification) —
  "we'll check the information you provided at sign up to verify your business and its stakeholders",
  and on the document list: *"Please note, this step is not required for individuals or sole
  traders."* Business checks are "often instant", manual review estimated 2–4 business days.
- [Identity Verification](https://www.paddle.com/help/start/account-verification/what-is-identity-verification) —
  "If you are an individual or sole trader, you will be the person required to do the identity
  verification check." Government ID + proof of address through **Sumsub**, sometimes a liveness
  video. "Most sellers complete it in just a few minutes"; instant unless it needs manual review.
- [Account Verification](https://www.paddle.com/help/start/account-verification/what-is-account-verification) —
  three gates before Paddle acts as MoR: domain verification, business identification, identity
  verification.
- [When and how do I get paid?](https://www.paddle.com/help/manage/get-paid/when-and-how-do-i-get-paid) —
  no on-demand withdrawal; monthly run, balance above the seller-set threshold (**min $100**) pays
  out **by the 15th**, arrives within ~3 working days. Wire or Payoneer; "$15 SWIFT fee may be
  applicable" for some countries.
- [Is there a fee taken for payouts?](https://www.paddle.com/help/manage/get-paid/is-there-a-fee-taken-for-payouts) —
  "Paddle will make every effort to avoid fees… when your payout currency matches the local currency
  of your bank's country, the transfer is typically free because we can use local payment networks",
  e.g. EUR to a Eurozone bank via SEPA. SWIFT fallback is $15, deducted before invoicing.
- [Can I be paid in my local currency?](https://www.paddle.com/help/manage/get-paid/can-i-be-paid-in-my-local-currency) —
  payout currency list includes **Hungarian Forint (HUF)** and **Euro**; picking a currency other
  than the balance currency can carry up to 1.5% conversion margin.
- [Pricing](https://www.paddle.com/pricing) — 5% + 50¢ per checkout transaction, no monthly fee, "no
  hidden extras"; bespoke pricing only below ~$10 order value.

Reading of it: **HUF payout is supported, EUR payout is cheaper** (SEPA is free, HUF-in-Hungary may
still route locally, but EUR avoids the mismatch fee entirely if the receiving account is EUR).
Paddle's monthly-by-the-15th schedule is the one real product constraint — it means "first paid
dollar" lands weeks after the first sale, not days.

### Polar.sh

Sources (all `polar.sh/docs`, read 2026-10-20):

- [Payout Accounts](https://polar.sh/docs/features/finance/accounts) — "Select your country: if this
  is a **personal account**, pick your country of residence." Then Stripe Connect Express onboarding
  collects "identity details, business information (if applicable), and the bank account". A working
  payout account is a **prerequisite to going live**.
- [Account reviews](https://polar.sh/docs/merchant-of-record/account-reviews) — first payout review
  has three steps: submit for approval, **KYC via Stripe Identity** (passport / ID card / driving
  licence + selfie), connect a payout account. "Initial reviews can take up to 14 days." Also
  requires a **live website** and configured products — "build first, submit second".
- [Supported countries](https://polar.sh/docs/merchant-of-record/supported-countries) — **Hungary is
  listed** for payouts. "Any individual or company operating in our supported countries can receive
  payouts from Polar."
- [Payouts](https://polar.sh/docs/features/finance/payouts) — on-demand withdrawal once the balance
  clears the currency minimum (**$10 USD / $13 EUR** equivalent); **7-day settlement delay** for
  organizations created on or after 2026-05-12.
- [Balance](https://polar.sh/docs/features/finance/balance) — explicit HUF note: "Stripe requires
  payout amounts to be in whole currency units" for HUF, so fractional forints roll into the next
  payout.
- [Fees](https://polar.sh/docs/merchant-of-record/fees) — Starter (the rate for orgs created on or
  after 2026-05-27) is 5% + 50¢, **plus 1.5% for international (non-US) cards**, plus Stripe payout
  fees: **$2/month while payouts are active, 0.25% + $0.25 per payout, +0.25% cross-border (EU)**.
  Disputes are $15 each.

Reading of it: Polar is the more flexible payout story (on demand, low minimum, HUF native) and the
more expensive one at WhoCards' likely basket size — a $3.99 unlock from a Hungarian card is
5% + 50¢ + 1.5% ≈ **$0.76 on $3.99 (19%)**, where Paddle takes ≈ $0.70 (17.5%). On a $30 deck the
gap is ~$0.45. The $2/month payout fee also bites hardest exactly at the "first paid dollar" stage.

## The HITL blocker (why this issue cannot be closed by an agent)

#254 is explicit: KYC and identity are Avi's, and agents never perform KYC for him. That is not a
formality — creating an account in someone else's name and pushing their ID through Sumsub/Stripe
Identity is identity fraud regardless of intent, and it would also poison the very evidence the issue
wants (a real reviewer's decision on a real applicant).

So the split is:

- **Agent (done here):** establish what the docs promise, reduce it to the smallest set of facts a
  live run must confirm, and pre-write the capture table so the run produces comparable evidence
  instead of a pile of screenshots.
- **Avi (open):** create the two accounts, submit his own ID, and fill the table. ~20 minutes per
  provider plus the review wait, which runs in the background.

**Done-when for #254:** both tables below are filled, and either both flows reach "payout account
connected / first payout approved" as an individual, or the specific blocking screen is pasted (in
general form) with the reason. Either outcome unblocks #247; a documented block is a success, not a
failure.

## Capture checklist — run once per provider

Rules: no credentials, no ID numbers, no tax numbers, no screenshots of personal data in this public
repo. Record **what was asked** and **what was offered**, in general terms, plus the date.

### Paddle

1. Go to <https://www.paddle.com/pricing> → **Get started**. Record the URL it lands on.
2. Fill the signup form as an individual. Record, per field: label, whether required, and whether it
   accepted a personal (non-company) answer — especially **legal entity type**, **company
   registration number**, **VAT/tax number**, **website/domain**.
   - _Q1: is "individual" / "sole trader" an explicit option, or did you have to type a business
     name?_
3. After email verification, open **Verification / Account** in the dashboard. Record which gates
   appear and in what order (domain → business → identity).
   - _Q2: did the business step demand a registration document anyway, despite the help page saying
     individuals are exempt?_
4. Start identity verification. Record: provider named on screen (Sumsub expected), documents
   requested, whether liveness was demanded, and wall-clock time to a decision.
5. Open **Payout settings**. Record: payment methods offered (wire / Payoneer), **transfer currency
   options present** (is HUF there? EUR?), minimum threshold field, and any fee text shown at that
   moment.
   - _Q3: with a Hungarian address, is HUF offered, EUR offered, or both?_
6. Note the fee schedule shown **inside** the dashboard at signup (Paddle advertises 5% + 50¢) and
   whether any per-country or per-method surcharge is mentioned.
7. Record the review state after submitting: instant / "under review" / email follow-up, and the
   stated ETA.

### Polar.sh

1. Go to <https://polar.sh> → sign up with GitHub. Record what the organization creation form asks
   for (name, handle, website, country?).
2. **Finance → Account → Continue with account setup.** Record the country step and whether the
   "personal account" wording is what a Hungarian resident sees.
   - _Q1: does the flow distinguish personal vs business, and does personal avoid every company
     field?_
3. Continue into Stripe Connect Express. Record: fields requested, whether a **tax ID** is demanded
   for a personal account in HU, and which bank-account country/currency Stripe will accept.
   - _Q2: does Stripe accept a Hungarian HUF account, and does it also offer EUR?_
4. Complete KYC (Stripe Identity: ID + selfie). Record time to a decision and any manual-review
   bounce.
5. Submit the organization for approval (Finance → Account). Record the questions asked about
   products/use case and the stated ETA (docs say up to 14 days).
   - _Q3: did the reviewer ask for a company document or a tax number as a follow-up?_
6. Record the fee plan actually attached to the new org: Starter 5% + 50¢, or Early Member 4% + 40¢
   +0.5% (orgs created before 2026-05-27 only). Screenshot the plan name, not the balance.
7. Record whether the dashboard states the +1.5% international-card surcharge anywhere a seller
   would see it before going live.

### Table to fill (one row per provider, general form only)

| Field | Paddle | Polar.sh |
| --- | --- | --- |
| Date run | | |
| Individual selectable as entity type? | | |
| Company / registration number required? | | |
| Tax/VAT number required? (Y/N/optional) | | |
| Domain or website required? | | |
| KYC vendor + documents + liveness? | | |
| Time from submit to approved | | |
| Payout currencies offered for HU | | |
| Payout methods offered | | |
| Minimum payout | | |
| Fee schedule shown in dashboard | | |
| Any review/KYB wait or follow-up email | | |
| **Verdict: can an individual with no entity reach payout setup?** | | |

## What each outcome means for #247

- **Both pass as individual** → the entity question stops being a blocker for launch. #247 becomes a
  straight fee/mechanics choice, and on the numbers above Paddle wins on cost while Polar wins on
  payout latency and minimum. Nothing here forces a company.
- **Polar blocks individuals in HU** (e.g. Stripe demands a business tax ID) → Paddle is the rail,
  and Polar's flexibility argument dies. Record the exact screen.
- **Both demand a tax number** → the docs were wrong and the entity path reopens. The fallback is a
  Hungarian sole-trader registration (one-person, no company), which is cheap and fast but is a real
  admin step — escalate to #247 rather than absorbing it here.
- **Either flow stalls in review >14 days** → that is itself the finding: "first paid dollar in days"
  does not survive a two-week KYB, and the plan needs a different first-revenue surface.

## Open questions only the live run can answer

1. Whether the Hungarian address triggers a **tax-number requirement** at signup that the English
   help pages never mention.
2. Whether Paddle's business step is genuinely skipped for individuals, or merely "no shareholder
   documents" while still wanting a trading name.
3. Whether Stripe Connect (Polar) accepts a **HUF** bank account from a personal account, or pushes
   the seller to EUR — this changes which currency the balance settles in.
4. Whether a one-person app studio with a single €3.99 in-app unlock clears either reviewer's
   "is this a real business" bar without a company behind it.
5. What each provider shows a **Hungarian individual** about personal income tax and invoicing —
   MoR removes VAT registration, not the seller's own income-tax obligations. Neither help centre
   addresses this for HU; it likely needs an accountant's one-liner before #247 commits.

## Appendix — comment draft for #254

> **Agent pass: docs verified, live run still needs you.**
>
> Public docs (checked 2026-10-20) settle the "is it possible" half:
>
> - **Paddle** — business-verification help page says ownership/registration documents are "not
>   required for individuals or sole traders"; identity verification for an individual is done by
>   that individual via Sumsub (ID + proof of address, sometimes liveness). Payouts monthly by the
>   15th, min $100, wire or Payoneer, **HUF and EUR both listed** as payout currencies, SEPA free.
>   5% + 50¢ all-in.
> - **Polar.sh** — "personal account" is a first-class path (pick country of residence, no company
>   fields); **Hungary is on the supported-payout list**; KYC is Stripe Identity; payout account is a
>   prerequisite to going live; first review up to 14 days. Starter 5% + 50¢ **+1.5% on non-US
>   cards**, plus Stripe payout fees ($2/mo active, 0.25% + $0.25 per payout).
>
> Neither needs a company. The cost gap is bigger than the headline suggests for a European
> audience: ~6.5% + 50¢ on Polar vs 5% + 50¢ on Paddle, and the +1.5% hits almost every WhoCards
> buyer.
>
> What docs can't tell us, and what only your run can: whether the live form lets a Hungarian
> individual through without a tax number, and whether a one-person studio clears review. Checklist
> with the exact fields to record is in `docs/spikes/0004-individual-signup-paddle-polar.md` — ~20
> min per provider, then the review waits run in the background. Paste the filled table (general
> form, no personal data) back here and #247 is unblocked either way.
