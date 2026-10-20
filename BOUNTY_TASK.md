# Bounty Objective: Task: verify Paddle and Polar.sh no-entity signup flows
Target: whocards/whocards-platform Issue #254
URL: https://github.com/whocards/whocards-platform/issues/254

## Issue Description
## For humans

**What:** Actually start the Paddle and Polar.sh signup flows to confirm an individual with no company can onboard and reach Hungary payout setup.
**Why:** The whole "first paid dollar in days, before any entity" plan leans on their docs' promise being true in practice.
**Done when:** Each flow is confirmed through KYC as an individual, or the blocker is documented on this issue.

<details><summary>For AI — implementation detail</summary>

### Context

- Child of the wayfinder map #244; surfaced by the resolutions of #245 and #246 as the highest-value real-world check before #247 decides.
- **HITL — Avi runs the signups himself**: KYC and identity are his; agents never perform KYC for him. An agent may prepare a checklist of what to capture.
- Capture per provider (in general form, public repo — no credentials, no personal data): what the signup asks for (entity/tax-ID fields?), whether "individual" is a first-class option, Hungary payout methods offered, any review/KYB wait, fee schedule shown at signup.
- Blocks "Decide: payment rails + entity path" (#247).

</details>


## Instructions for Kuro (Autonomous Solver):
1. Use skills: `systematic-debugging` and `code-review-and-quality`.
2. Inspect the repository structure, locate relevant files, and understand the bug/feature.
3. Apply the minimal, robust fix or implementation.
4. Run any existing test suites or linters in the workspace.
5. Ensure the fix adheres to the project's existing code style.
6. When done, output a summary of files modified and verification results.
