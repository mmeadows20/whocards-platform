# Wayfinder maps

A **wayfinder map** is a GitHub issue whose children are _decision_ tickets, not deliverables. It
exists so that a stretch of work that cannot be built yet (because the decisions it depends on are
still open) has one place to see the whole route, and so that execution can later start from normal
issues with nothing left to decide.

Labels: `wayfinder:research` (find facts), `wayfinder:grilling` (settle a decision with Avi),
`wayfinder:task` (a real-world unblocker that does get done on the map — signups, paperwork).

## Convention

- One file per map: `docs/wayfinder/<parent-issue>-<slug>.md`.
- The file is the **durable copy** of the map plus the **decision pack** its open children need:
  destination, child index, what is already settled, a recommendation with a fallback per open
  decision, the execution handoff, and the closing checklist.
- Research tickets keep their evidence in `docs/spikes/`; decisions that change architecture land in
  `docs/adr/`. The map file links to both rather than duplicating them.
- A recommendation in a map file is **not** a decision. A decision is settled when the ticket's
  resolution says so. Anything touching money, identity, tax, or pricing is Avi's call — agents
  prepare the options and never perform KYC, signups, or payments on his behalf.
- Update the map file in the same PR that posts a resolution, so the file and the issue never drift.

## Maps

| Map | Route                                                      |
| --- | ---------------------------------------------------------- |
| 244 | [WhoCards paid: the route to revenue](./244-paid-route.md) |
