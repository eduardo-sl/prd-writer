---
name: prd-writer
description: Use this skill when the user asks to "write a PRD", "create a product requirements document", "write product requirements", "spec out a product", "document what we need to build and why", "draft a feature proposal", or otherwise needs a document that aligns stakeholders on the problem, the target users, the proposed solution, and the success criteria. Adapts its depth to the reader — a one-page executive register for leadership and CTOs that leads with impact, and a fuller operational register for the team that will implement. Produces stakeholder-readable PRDs — not implementation specs.
version: 2.0.0
license: MIT
---

# prd-writer

Produce Product Requirements Documents that align stakeholders on **what to build and why** — grounded in a real problem, real users, and measurable outcomes.

A PRD is a tool for alignment and clarity of thought. It is **not** an implementation spec, and it is **not** a record of everything the author did. There is no prize for extra pages. But "how long" is the wrong question — the right one is **who reads this, and what do they decide?** That answer sets the document's *register*, and the register sets everything else.

Two registers:

- **Executive** — read by leadership, directors, CTOs. They give the document a few minutes and decide whether to fund, approve, or kill it. They operate at the level of *impact*. Lead with the bet and the outcome; push detail into appendices and linked docs; one page is the target, not an accident.
- **Operational** — read by the team that will build it. They need every requirement, edge case, state, and metric resolved enough to act without a follow-up meeting. Detail here is the deliverable, not bloat.

The register is not a tone setting; it changes which sections carry weight and how much detail each holds. Phase 3 decides it explicitly. Neither register licenses vagueness or padding — both demand that every sentence change what someone decides after reading it.

This skill does not invent user research, fabricate metrics, or resolve undecided strategy questions. When something is genuinely unknown, mark it `[NEEDS CLARIFICATION: ...]` and surface it rather than papering over it with confident prose.

## The readability bar

Every PRD this skill produces clears these before delivery, in either register. They come first because they are the difference between a document people act on and one they skim and forget.

1. **The TL;DR stands alone.** A reader who reads only the first paragraph knows what is being built, for whom, why now, and what success looks like. Everything after it is supporting evidence, not new information. (This matters most in the executive register, where the TL;DR may be all a director reads — but it costs nothing in the operational register.)
2. **Lead with impact, not execution.** Executives operate at the level of *impact* — the effect on the business, the user, the brand. PMs default to the level of *execution* — the mechanics of building. A document that opens with mechanics loses its most senior readers in the first thirty seconds. State the outcome and the bet before the build, regardless of register.
3. **Every claim is decision-relevant.** If a sentence does not change what a reader decides, approves, or questions, cut it. "Nice to know" background belongs in an appendix or a linked doc. This is how the operational register stays detailed without becoming bloated: detail that an implementer acts on stays; detail that merely demonstrates effort goes.
4. **Concise is not vague.** Short sentences, plain language, no jargon, no hedging. Every requirement is still specific enough to be independently verified. Brevity comes from cutting filler, never from cutting precision.
5. **Claims carry evidence or a label.** A problem statement without data is a hypothesis — say so. Confident prose around an untested assumption is the most expensive kind of error in a PRD, because it hides the risk from the people best positioned to catch it.

## Output

Default location: `docs/prd/<feature-name>/PRD.md` — lowercase, hyphenated. Create the directory if it does not exist. If the user specifies a different path, use it.

If `docs/prd/README.md` does not exist, create it as an index: feature name | type (`product` / `feature`) | status (`draft` / `review` / `approved` / `shipped`) | owner | last updated.

## PRD types

Before drafting, determine which type applies. The wrong altitude is a common failure — a feature written at product altitude wastes the reader's time; a product initiative written at feature altitude misses the strategic question entirely. The type also sets the **default register** (Phase 3 confirms or overrides it):

| Type | When to use | Primary reader | Default register |
|---|---|---|---|
| **Product PRD** | New product or major initiative | Leadership, CTO, all XFN | Executive |
| **Feature PRD** | Specific feature on existing product | PM, design, eng, GTM | Operational |

A product PRD links to feature PRDs for its sub-components. A feature PRD links back to the product PRD if one exists. Neither replaces the other. A large initiative often wants an executive product PRD on top and operational feature PRDs underneath — same skill, different register per document.

## Process

Five phases. Phases 1–2 are where most PRDs fail — they get skipped, and the team builds the wrong thing well. None can be skipped.

### Phase 1 — Understand the context

The most common PRD failure is a solution in search of a problem. Read first:

- The product's primary context doc (in priority order — first match): `AGENTS.md`, `CLAUDE.md`, `README.md`, `PRODUCT.md`, any strategy or vision doc.
- Existing PRDs for adjacent features (under `docs/prd/`, `docs/specs/`, or wherever convention places them). Match their style and depth; a reader should not have to relearn your format every document.
- Any user research, customer feedback, support tickets, or NPS/CSAT data referenced in the context.
- Analytics or instrumentation that quantifies the problem.
- Competitive analysis if referenced.
- Technical constraints or architecture principles that bound the solution space.

Then detect the **feature class**, because it determines which sections apply:

| Class | Examples | Emphasis |
|---|---|---|
| New product | Greenfield launch, new market | Full product PRD; business case is load-bearing |
| Core feature | CRUD flows, primary workflows | Full feature PRD |
| Growth / monetization | Onboarding, pricing, activation | Conversion metrics, funnel, counter-metrics |
| Platform / API | Developer-facing, extensibility | DX, versioning, migration cost |
| Operational | Admin tools, internal tooling | Stakeholder map, ops metrics |
| Refactor / UX improvement | No new behavior | Goals = invariants held + metrics improved |

### Phase 2 — Clarify the problem

The problem statement is the most important part of a PRD. A bad one produces a document that reads well and ships the wrong thing — and that error survives all the way to launch precisely because the prose was confident.

A "requirement" handed to you is often a hypothesis in disguise. Customers and stakeholders state solutions ("we need a dashboard") that encode an unstated problem ("I can't tell if the feature is working"). Trace every requirement back to the problem it assumes. Before drafting, verify:

- The problem statement is a **user or business pain**, not "users can't use `<my solution>`." A missing feature is almost never the root problem.
- There is **evidence**: data, research, customer quotes, support volume. Without it, label the problem a hypothesis explicitly.
- The **scope** is clear: product or feature PRD? MVP or full vision?
- **Target users** are specific. "All users" is not a segment.
- **Success is measurable.** "Improve UX" can never be declared done.
- **Non-goals** are explicit. Non-goals do more work than goals — they are how you stop scope creep before it starts.

Ask the open questions in one batch, not one at a time. Mark anything unresolved inline:

```
[NEEDS CLARIFICATION: is this targeting free users, paid users, or both?]
```

A PRD is not "ready" while any `[NEEDS CLARIFICATION]` remains. Collect them under "Open questions" near the top so a reviewer cannot miss them.

### Phase 3 — Choose the register, then scope the solution

**First, set the register.** It is the single decision that shapes the rest of the draft, and it is cheap to get right and expensive to retrofit.

- Start from the type's default: Product PRD → executive; Feature PRD → operational.
- Override on explicit signal. "One-pager for the leadership review", "PRD for the board", "exec summary" → **executive**, even for a feature. "Detailed PRD for the eng team", "include all edge cases and telemetry" → **operational**, even for a product. When the prompt names the reader, the reader decides.
- When in doubt, ask — one question, not a guess: *"Who's the primary reader — leadership approving the bet, or the team building it?"*

What the register changes:

| | Executive register | Operational register |
|---|---|---|
| **Target length** | ~1 page; detail linked, not inline | As long as implementation needs; no padding |
| **Requirements (§5)** | Top P0s only; full list linked | Complete, every P0/P1/P2, bucketed by use case |
| **Design / flows (§4)** | The 2–3 flows that carry the bet | All core flows, all states |
| **Metrics (§6)** | Headline impact metric + counter-metric | Full usage + impact instrumentation plan |
| **UX, risks, readiness (§8–10)** | Summarized or linked | Spelled out with owners |
| **What leads** | Impact and the business bet | Impact first, then the full requirement set |

Both registers keep the same section *order* — the executive one just collapses lower sections into a line plus a link instead of dropping them, so a reader who wants depth knows where it lives.

**Then scope the solution.** Define enough that the chosen reader can evaluate it:

- The **elevator pitch** in 2–3 sentences a non-expert understands.
- What is **explicitly out of scope** (non-goals).
- The **MVP** — the minimum that earns initial adoption.
- The top 2–3 **user flows** (executive) or all core flows (operational).

No implementation details — no database choices, API contracts, or component names. Those belong in the spec. The PRD defines what users can do and why it matters.

### Phase 4 — Draft the PRD

Use the template below. Include only sections that apply to the feature class; for each omitted optional section, leave a one-line `N/A — <reason>` so reviewers know it was considered, not forgotten. Apply the register from Phase 3: in the executive register, lower sections collapse to a summary line plus a link to where the detail lives; in the operational register, they are spelled out in full.

Write requirements as user-facing functional statements, not technical ones:

- **DO:** "Users can export the current table view as a CSV in one click."
- **DON'T:** "The system shall implement a CSV serialization endpoint at `/api/v1/export`."

Prioritize with P0/P1/P2:

- `[P0]` — Required for MVP. Without it, users cannot adopt the feature.
- `[P1]` — High value. Needed for a minimally good experience.
- `[P2]` — Nice-to-have. First to be cut under time pressure.

Track **two kinds of metric, not one.** Impact metrics (did it move the business?) get all the attention; usage metrics (is anyone actually using it, and how?) get forgotten — and without them you cannot diagnose a flat impact metric. Require both.

Use a maximum of **three phases or milestones.** By the time the third arrives, the world will have changed enough to need a new PRD.

### Phase 5 — Self-check before saving

Verify each item. Anything that fails is fixed before delivering.

1. Is the register chosen and consistent — executive document kept to ~a page with detail linked, or operational document complete enough to build from without a follow-up?
2. Does the TL;DR stand alone — build, user, why-now, success — in three sentences or fewer?
3. Does the document lead with impact, with execution detail pushed downward?
4. Is the problem statement a user/business pain, not a missing feature?
5. Is there evidence cited — or is the problem explicitly labeled a hypothesis?
6. Are target users specific, not "everyone"?
7. Are goals measurable? Could someone look at a dashboard in six months and call this done or failed?
8. Are non-goals explicit and specific?
9. Do requirements describe user behavior, not system implementation?
10. Are both usage and impact metrics defined, plus a counter-metric?
11. Three phases or fewer, open questions surfaced, and the right document type for the work (a bug fix or local refactor does not need a PRD)?

## PRD template

Sections marked **required** must appear. **Conditional** sections appear only when relevant; otherwise leave a one-line `N/A — <reason>`. The order is deliberate: a reader who stops early has still read the most important parts.

```markdown
# [Feature / Product Name] — PRD

**Type:** product | feature
**Status:** draft | review | approved | shipped
**Owner:** <single name>
**Contributors:** <names>
**Last updated:** <ISO date>
**Related:** <strategy docs, research, prior PRDs, issues>

---

## TL;DR (required)
Three sentences max. What this is, who it's for, why now, and what success looks like.
This paragraph must stand alone — a director who reads nothing else knows the bet being made.
If you can't say it in three sentences, the scope is not yet clear.

## Open questions (required while non-empty; remove when empty)
- [NEEDS CLARIFICATION: ...]

---

## 1. Problem and impact (required)

### 1.1 Problem statement
The user or business pain — stated as a pain, not as the absence of a solution.
Why it exists. Ask "why" one level deeper than feels necessary.

### 1.2 Why now / impact of solving it
What changes for the business or the user if this is solved — and what is the cost of not solving it.
This is the paragraph your most senior reader cares about most. Lead the document's substance here.

### 1.3 Evidence
What validates that this problem is real and worth solving?
- Quantitative: metrics, conversion rates, support volume, survey results.
- Qualitative: research findings, customer quotes, NPS themes.
Without evidence, label this a hypothesis — do not disguise it as fact.

### 1.4 Current state (conditional — when workarounds illuminate the pain)
How users solve this today. Often the strongest argument for why the problem hurts.

---

## 2. Users (required)

### 2.1 Target users
Who specifically — by role, plan tier, use case, or behavior, not demographics.
"All users" is not an answer.

### 2.2 Target use cases
The 2–3 core scenarios, highest-value or highest-frequency first.
Format: "As a [user], I want to [action] so that [outcome]."

### 2.3 Out-of-scope users (conditional — when scope confusion is likely)
Who this PRD explicitly does not serve.

---

## 3. Goals (required)

### 3.1 Goals
2–3 bullets max. Each measurable: metric, direction, target, timeframe.
- G1 — [Metric] from [baseline] to [target] within [timeframe].
- G2 — ...
Qualitative goals are acceptable only if verifiable (e.g., "users describe the flow as straightforward in usability testing").

### 3.2 Non-goals (required)
What this PRD explicitly does not cover. Your sharpest scope tool.
- N1 — This PRD does **not** address ...
- N2 — ...

---

## 4. Proposed solution (required)

### 4.1 Elevator pitch
2–3 sentences, plain language. Optionally a high-level conceptual diagram (ASCII or linked).

### 4.2 Key user flows
The 2–3 most important journeys. Describe the path, not the implementation.

### 4.3 Key capabilities
The user-facing functionality this introduces — what marketing and education will describe to users.

---

## 5. Requirements (required)

User-facing functional statements, bucketed by use case. No technical requirements
(databases, APIs, latency SLAs) unless direct user evidence makes them adoption-critical.

### Use case: [first use case]
- [P0] R1 — [user-facing statement]
- [P0] R2 — ...
- [P1] R3 — ...
- [P2] R4 — ...

### Use case: [second use case]
- [P0] R5 — ...

---

## 6. Success metrics (required)

Tie back to §3.1. Track both, because one without the other can't diagnose a miss:

**Usage** — is it being used, and how? (adoption, frequency, depth)
**Impact** — did it move the business? (revenue, retention, conversion)

| Metric | Type | Baseline | Target | Timeframe | Owner |
|---|---|---|---|---|---|
| ... | usage / impact | ... | ... | ... | ... |

**Counter-metric** — what must NOT regress (e.g., optimizing activation without dropping retention).

---

## 7. Scope and phasing (required)

### MVP
The minimum set of P0 requirements that earns initial adoption. Everything else is post-MVP.

### Later phases (conditional — only if concretely defined, max 2)
What comes next, each with a concrete trigger for when to start.
Do not plan a fourth phase — it will be stale before you reach it.

---

## 8. UX and design (conditional — required for user-facing changes)
- Link to Figma, wireframes, or prototype.
- States to design: loading, empty, error, success, edge cases.
- Accessibility: keyboard, screen reader, contrast.
- Localization / i18n needs.

---

## 9. Risks and dependencies (required)

### Risks
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| ... | High/Med/Low | High/Med/Low | ... |

### Dependencies
Teams, systems, or decisions that must be resolved before or during execution.
- [Dependency] — [owner] — [needed by].

---

## 10. Launch readiness (conditional — required for user-facing launches)
Cross-functional checklist, each item with an owner.
- [ ] Legal / compliance review — owner: [name]
- [ ] Privacy / data review — owner: [name]
- [ ] Marketing / messaging ready — owner: [name]
- [ ] Support trained — owner: [name]
- [ ] Docs / help center updated — owner: [name]
- [ ] Feature flag / rollout plan — owner: [name]
- [ ] Instrumentation / telemetry in place — owner: [name]
- [ ] Rollback plan documented — owner: [name]

---

## Appendix (optional)
Supporting material, linked not embedded. If §1–3 don't earn alignment, nothing here will.
- User research: [link]
- Competitive analysis: [link]
- Technical spec: [link]
- Related PRDs: [link]
```

## Quality bar for requirements

- **User-facing language.** What a user can do, see, or experience — never how the system does it.
- **Independently testable.** Someone else can verify each one without asking the author.
- **Prioritized.** Every requirement has a priority. Unprioritized requirements are all secretly P0.
- **Scoped.** A requirement without a boundary expands to fill all available time.
- **Evidence-backed for P0.** A P0 ties to a validated need. "We think users want this" is not P0.

## Patterns to avoid

| Anti-pattern | Why it's a problem |
|---|---|
| Opening with mechanics, not impact | The most senior reader leaves before the part that matters to them. |
| `"Users can't use <Feature X>"` as the problem | A missing feature is rarely the root problem — ask why again. |
| Confident prose over an untested assumption | Hides the biggest risk from the people best able to catch it. Label hypotheses. |
| Goals that can't be measured | You will never declare this done or failed. |
| Requirements with implementation detail | Constrains engineering unnecessarily and dates the document. |
| Padding for thoroughness | There's no prize for pages. In the operational register, detail an implementer acts on is the deliverable; detail that only demonstrates effort is bloat. Cut the second kind in both registers. |
| Wrong register for the reader | A one-pager handed to the build team forces a follow-up meeting; a 20-page operational doc handed to a CTO gets skimmed and forgotten. Match the register to who decides. |
| More than 3 phases | The third phase needs a new PRD by the time you reach it. |
| `"All users"` as a segment | No segment = no prioritization = everyone unhappy. |
| Impact metrics but no usage metrics | A flat impact number becomes undiagnosable. |
| No counter-metric | Winning one metric while quietly losing another is not success. |
| Non-goals left implicit | Implicit non-goals become implicit scope. |
| A PRD for a bug fix or small refactor | Overhead disproportionate to the task — write a well-described issue. |

## Working with multiple PRDs

A major initiative usually wants a **graph of focused PRDs**, not one monolith. Decomposition rule: if a sub-feature needs a separate team, a separate release, or has independent success metrics, it deserves its own PRD.

Before drafting for broad work, produce a decomposition plan:

1. List sub-features, each a candidate PRD.
2. For each, identify target users and primary metric.
3. Group those that share users and flows (candidates for a single PRD).
4. Identify dependencies between them.

Then draft a product PRD for the initiative, linked to feature PRDs per component.

### Anti-pattern: the monolith PRD

A 20-page PRD covering an entire initiative gets skimmed, not reviewed, and is stale before it ships. Split it. Smaller PRDs are read, reviewed, and executed faster — and a reader can actually hold one in their head.
