# prd-writer — Examples

Copy-paste prompts for common scenarios. Adapt the `Context:` block to your project.

---

## 1. By prompt level

### Level 1 — Minimal
Use when the product has a thorough `README.md` or `PRODUCT.md`. The skill infers context from there.

```
Use the prd-writer skill to write a PRD for adding a CSV export feature to the data dashboard.
```

### Level 2 — Standard *(recommended)*
Use when the main decisions are already made. Pass them as context bullets.

```
Use the prd-writer skill to write a feature PRD for CSV export.

Context:
- Product: analytics dashboard for small business owners
- Problem: users manually copy-paste data to Excel every week
- Target users: paid plan users (team + business tiers)
- MVP scope: export current table view as CSV; no filters, no scheduling
- Out of scope: PDF export, scheduled exports, API access
- Success: 30% of paid users export at least once in the first 30 days
```

### Level 3 — Complete
Use when there are open decisions or unclear scope. The skill surfaces them as `[NEEDS CLARIFICATION]` rather than inventing answers.

```
Use the prd-writer skill to write a feature PRD for CSV export.

Decided:
- Target: paid users who manage data in the dashboard
- MVP: export the current table view

Still undecided:
- Should free users be able to export? (revenue vs growth trade-off)
- Do we need an export history / audit trail for enterprise?
- Is this gated behind a feature flag or a full rollout?
```

---

## 2. By scenario

### New product / greenfield initiative
```
Use the prd-writer skill to write a product PRD for a team collaboration feature
in our project management tool.

Context:
- We're a B2B SaaS tool for project tracking, 10k MAU, mostly solo users today
- Problem: teams using us have to share login credentials or use workarounds
- Target: teams of 3–10 people on the Business plan
- Strategic goal: increase Business plan adoption from 8% to 20% of signups
```

### Feature on an existing product
```
Use the prd-writer skill to write a feature PRD for onboarding improvements.

Context:
- Product: SaaS task manager, targeting freelancers
- Problem: 60% of new signups drop off before completing their first task
- We have session recordings showing users get stuck on the "project setup" step
- Goal: reduce day-1 drop-off from 60% to 40%
```

### Refactor / UX improvement (no new behavior)
```
Use the prd-writer skill to write a PRD for redesigning the settings page.

Context:
- Current page has 47 settings with no grouping; NPS feedback cites it as confusing
- Goal: users can find any setting in under 10 seconds (usability test benchmark)
- Out of scope: adding new settings, changing any existing setting behavior
```

### Internal / operational tooling
```
Use the prd-writer skill to write a PRD for an internal admin dashboard
to manage subscription upgrades and downgrades manually.

Context:
- Current process: support team uses direct DB queries — 20 min per ticket
- Target users: support team (5 people)
- Goal: reduce manual subscription change time to under 2 minutes
- Compliance requirement: all changes must be logged with agent ID + timestamp
```

---

## 3. Decomposition: large initiative → PRD graph

When the scope is broad, ask for a decomposition before writing any PRD.

```
Read the README and any existing PRDs in docs/prd/.

I need to launch a multi-tenant workspace feature. Before writing any PRD:

1. List each sub-feature that needs its own PRD.
2. For each, identify: target users, primary metric, owner.
3. Identify dependencies between sub-features.
4. Group sub-features that can be specced in parallel vs. those that depend on others.

Do not write any PRD yet.
```

Then proceed with the product PRD for the initiative, and feature PRDs for each sub-component.

---

## 4. Negative examples — what a bad PRD looks like

### Bad problem statement
> **Wrong:** "Users can't export their data."
> **Why wrong:** "Can't use X" is a missing feature, not a problem. Ask why that matters.
> **Right:** "Users who need to reconcile dashboard data with their accounting tools spend 20+ min/week manually copying rows into spreadsheets, leading to errors and abandoned workflows."

### Bad goal
> **Wrong:** "Improve user experience with exports."
> **Why wrong:** Not measurable. You can never declare this done or failed.
> **Right:** "30% of paid users export at least once in the first 30 days post-launch."

### Bad requirement
> **Wrong:** "The system shall implement a CSV serialization endpoint at `/api/v1/export` using streaming for files > 10MB."
> **Why wrong:** Implementation detail. This belongs in the spec, not the PRD.
> **Right:** "[P0] R1 — Users can export the current table view as a CSV file."

### Bad non-goal (implicit)
> **Wrong:** (no non-goals section)
> **Why wrong:** Implicit non-goals become implicit scope. The team will build PDF export, scheduled exports, and API access because no one said they were out of scope.
> **Right:** "N1 — This PRD does not cover PDF export. N2 — Scheduled or recurring exports are out of scope for MVP."
