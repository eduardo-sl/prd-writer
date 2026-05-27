| name        | prd-writer                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description | Use this skill when the user asks to "write a PRD", "create a product requirements document", "write product requirements", "spec out a product", "document what we need to build and why", "draft a feature proposal", or otherwise needs a document that aligns stakeholders on the problem, the target users, the proposed solution, and the success criteria. Works for new products, major initiatives, and feature proposals. Produces focused, stakeholder-readable PRDs — not implementation specs. |
| version     | 1.0.0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| license     | MIT                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |

# prd-writer

Produce Product Requirements Documents that align stakeholders on **what to build and why** — grounded in a real problem, real users, and measurable outcomes.

A PRD is a vehicle for alignment. It is **not** an implementation spec. It should be readable by PMs, designers, engineers, marketing, and leadership without assuming deep technical context. Technical decisions (architecture, libraries, APIs) belong in a separate spec doc.

This skill does not invent user research, fabricate metrics, or resolve undecided strategy questions. When something is genuinely unknown, mark it `[NEEDS CLARIFICATION: ...]` and surface it.

## Output

Default location: `docs/prd/<feature-name>/PRD.md` — lowercase, hyphenated. Create the directory if it does not exist. If the user specifies a different path, use it.

If `docs/prd/README.md` does not exist, create it as an index: feature name | type (`product` / `feature`) | status (`draft` / `review` / `approved` / `shipped`) | owner | last updated.

## PRD types

Before drafting, determine which type applies:

| Type | When to use | Altitude | Audience |
|---|---|---|---|
| **Product PRD** | New product or major initiative | High — opportunity, users, vision | Leadership, all XFN |
| **Feature PRD** | Specific feature on existing product | Medium — requirements, flows, scope | PM, design, eng, GTM |

A product PRD links to feature PRDs for its sub-components. A feature PRD links back to the product PRD if one exists. Neither replaces the other.

## Process

Five phases. Phases 1–2 are where most PRDs fail — they get skipped and the team builds the wrong thing. None can be skipped.

### Phase 1 — Understand the context

The most common PRD failure is writing a solution in search of a problem. Read first:

- The product's primary context doc (in priority order — first match): `AGENTS.md`, `CLAUDE.md`, `README.md`, `PRODUCT.md`, any strategy or vision doc.
- Existing PRDs for adjacent features (under `docs/prd/`, `docs/specs/`, or wherever convention places them). Match their style and depth.
- Any user research, customer feedback, support tickets, or NPS/CSAT data referenced in the context.
- Analytics or instrumentation that quantifies the problem.
- Competitive analysis if referenced.
- Any technical constraints or architecture principles that bound the solution space.

Then detect the **feature class** before drafting, because it determines which sections apply:

| Class | Examples | Key sections |
|---|---|---|
| New product | Greenfield launch, new market | Full product PRD template |
| Core feature | CRUD flows, primary workflows | Full feature PRD template |
| Growth/monetization | Onboarding, pricing, activation | + conversion metrics, funnel |
| Platform/API | Developer-facing, extensibility | + DX, versioning, migration |
| Operational | Admin tools, internal tooling | + stakeholder map, ops metrics |
| Refactor / UX improvement | No new behavior, existing flow | Goals = invariants + improved metrics |

### Phase 2 — Clarify the problem

The problem statement is the most important part of a PRD. A bad problem statement produces a PRD that reads well and ships the wrong thing.

Before drafting, verify:

- Is the problem statement a **user or business pain**, not "users can't use `<my solution>`"? A lack of Feature X is almost never the root problem.
- Is there **evidence** for the problem? (data, research, customer quotes, support volume). A PRD without evidence is a hypothesis.
- Is the **scope** clear? Product PRD or feature PRD? MVP or full vision?
- Are **target users** specific? "All users" is not a valid target segment.
- Is **success measurable**? Vague goals like "improve UX" will never be declared done.
- Are **non-goals** explicit? Non-goals do more work than goals — they prevent scope creep.

Ask in one batch, not one question at a time. Mark anything unresolved:

```
[NEEDS CLARIFICATION: is this targeting free users, paid users, or both?]
```

A PRD is not "ready" while any `[NEEDS CLARIFICATION]` remains. Collect them under "Open questions."

### Phase 3 — Scope the solution

Before writing requirements, define the solution at high level:

- What is the **elevator pitch** in 2–3 sentences? A non-expert should understand it.
- What is **explicitly out of scope** for this PRD? (Non-goals)
- What is the **MVP** — the minimum set of functionality for initial adoption?
- What are the top 3 **user flows** this covers?

Do not specify implementation details (no database choices, no API contracts, no UI component names). Those belong in the spec. The PRD defines what users can do and why it matters.

### Phase 4 — Draft the PRD

Use the template below. Include only sections that apply to the feature class. For each omitted optional section, leave a one-line `N/A — <reason>`.

Write requirements as user-facing functional statements, not technical requirements:

- **DO:** "Users can export their data as CSV in one click."
- **DON'T:** "The system shall implement a CSV serialization endpoint at `/api/v1/export`."

Prioritize requirements using MoSCoW or P0/P1/P2:

- `[P0]` — Required for MVP. Without this, users cannot adopt the feature.
- `[P1]` — High value. Important for a minimally delightful experience.
- `[P2]` — Nice-to-have. Defer if time-constrained.

Use a maximum of **3 phases or milestones**. By the time you reach the third phase, the world will have changed.

### Phase 5 — Self-check before saving

Verify each item. Anything that fails must be addressed before delivering.

1. Is the problem statement a user/business pain — not a description of a missing feature?
2. Is there evidence cited for the problem (data, research, quotes)?
3. Are target users specific — not "everyone"?
4. Are goals measurable? Could someone look at a dashboard in 6 months and call this done?
5. Are non-goals explicit and specific?
6. Do requirements describe **user behavior**, not system implementation?
7. Are all P0 requirements truly required for initial adoption — or just nice-to-have?
8. Are there more than 3 phases? If yes, consolidate.
9. Are open questions surfaced under "Open questions" rather than silently resolved?
10. Is this the right document type? A bug fix or local refactor doesn't need a PRD.

## PRD template

Sections marked **required** must appear. Sections marked **conditional** appear only when relevant; otherwise leave a one-line `N/A — <reason>`.

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
Three sentences max. What this is, who it's for, and why it matters now.
Never more than 3 sentences. If you can't summarize it in 3, the scope is unclear.

## Open questions (required while non-empty; remove when empty)
- [NEEDS CLARIFICATION: ...]

---

## 1. Problem (required)

### 1.1 Problem statement
What is the user or business pain? State it as a pain, not as the absence of a solution.
Why does it exist? Ask "why" one more time than you think you need to.

### 1.2 Evidence
What data, research, or customer signal validates that this problem is real and worth solving?
- Quantitative: metrics, survey results, conversion rates, support volume
- Qualitative: user research findings, customer quotes, NPS themes

Without evidence, this is a hypothesis — label it as such.

### 1.3 Current state
How are users solving this problem today? What workarounds exist?
This section is often the strongest argument for why the problem is painful.

---

## 2. Users (required)

### 2.1 Target users
Who specifically are we solving for? Segment by role, plan tier, use case, or behavior — not demographics.
"All users" is not a valid answer.

### 2.2 Target use cases
What are the 2–3 core scenarios this addresses? Anchor on the most frequent or highest-value ones.
List them as: "As a [user], I want to [action] so that [outcome]."

### 2.3 Out-of-scope users (conditional — when there's risk of scope confusion)
Who are we explicitly NOT solving for in this PRD?

---

## 3. Goals (required)

### 3.1 Goals
2–3 bullets max. Each must be measurable — define the metric, the direction, and ideally the target.
- G1 — [Metric] increases from [baseline] to [target] within [timeframe]
- G2 — ...

It's acceptable to include qualitative goals (e.g., "Users describe the experience as straightforward in usability testing") if they can be verified.

### 3.2 Non-goals (required)
What this PRD explicitly does NOT cover. Non-goals are the most important scope tool you have.
- N1 — This PRD does **not** address ...
- N2 — ...

---

## 4. Proposed solution (required)

### 4.1 Elevator pitch
2–3 sentences. Plain language — a non-expert should understand it.
Optionally include a high-level conceptual diagram (ASCII or linked Figma/FigJam).

### 4.2 Key user flows
The 2–3 most important flows a user will take. Describe the journey, not the implementation.
Use numbered steps or a simple diagram.

### 4.3 Key features
A short list of the capabilities this introduces. Written as user-facing functionality.
These are what marketing and product education will use to describe the feature.

---

## 5. Requirements (required)

Functional requirements bucketed by use case or user journey. Written as user-facing statements.
Do not include technical requirements (databases, APIs, latency SLAs) unless you have direct user evidence they are required for adoption.

### Use case: [Name of first use case]
- [P0] R1 — [User-facing functional statement]
- [P0] R2 — ...
- [P1] R3 — ...
- [P2] R4 — ...

### Use case: [Name of second use case]
- [P0] R5 — ...

**Telemetry (required)**
- [P0] T1 — Product team can monitor [key action] adoption via [metric/event].
- [P1] T2 — ...

---

## 6. Success metrics (required)

How will we know this worked? Tie back to goals in §3.1.

| Metric | Baseline | Target | Timeframe | Owner |
|---|---|---|---|---|
| ... | ... | ... | ... | ... |

Include a **counter-metric**: what should NOT regress? (e.g., if optimizing for activation, ensure retention doesn't drop.)

---

## 7. Scope and phasing (required)

### MVP
What is the minimum set of P0 requirements needed for initial adoption?
Everything else is post-MVP.

### Phase 2 (conditional — only if clearly defined)
What comes after MVP, with a concrete trigger for when to start.
Do not include Phase 3 or beyond — it will be stale before you get there.

---

## 8. UX and design (conditional — required for user-facing changes)
- Link to Figma, wireframes, or prototype.
- Key states to design: loading, empty, error, success, edge cases.
- Accessibility requirements (keyboard navigation, screen reader, contrast).
- Localization / internationalization needs.

---

## 9. Risks and dependencies (required)

### Risks
| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| ... | High/Med/Low | High/Med/Low | ... |

### Dependencies
What teams, systems, or decisions must be resolved before or during execution?
- [Dependency] — [owner] — [needed by when]

---

## 10. Launch readiness (conditional — required for user-facing launches)

Checklist of cross-functional readiness criteria. Each item has an owner.

- [ ] Legal / compliance review — owner: [name]
- [ ] Privacy / data review — owner: [name]
- [ ] Marketing / messaging ready — owner: [name]
- [ ] Support trained — owner: [name]
- [ ] Documentation / help center updated — owner: [name]
- [ ] Feature flag / rollout plan defined — owner: [name]
- [ ] Instrumentation / telemetry in place — owner: [name]
- [ ] Rollback plan documented — owner: [name]

---

## Appendix (optional)
Links to supporting materials. Do not embed them inline — if you can't get alignment on §1–3, the rest is irrelevant.
- User research report: [link]
- Competitive analysis: [link]
- Technical spec: [link]
- Related PRDs: [link]
```

## Quality bar for requirements

- **User-facing language.** Requirements describe what a user can do, see, or experience — not how a system implements it.
- **Independently testable.** Someone else should be able to verify each requirement without asking the author.
- **Prioritized.** Every requirement has a priority. Unprioritized requirements are all secretly P0 in disguise.
- **Scoped.** A requirement without a boundary will expand to fill all available time.
- **Evidence-backed for P0.** P0 requirements should tie to validated user needs. "We think users want this" is not P0.

## Patterns to avoid

| Anti-pattern | Why it's a problem |
|---|---|
| `"Users can't use <Feature X>"` as problem statement | Missing feature is rarely the root problem — ask why again. |
| Goals that can't be measured | You'll never be able to declare this done or failed. |
| Requirements with implementation details | Constrains engineering unnecessarily and dates the PRD. |
| More than 3 phases/milestones | The third phase will need a new PRD by the time you get there. |
| `"All users"` as target segment | No segment = no prioritization = everyone unhappy. |
| PRD written before user research | You're writing fiction with extra steps. |
| Telemetry as an afterthought | If it's not in the PRD, it won't be in the build. |
| Non-goals left implicit | Implicit non-goals become implicit scope. |
| No counter-metrics | Optimizing one metric while regressing another is not success. |
| PRD for a bug fix or small refactor | Overhead disproportionate to task. Write a well-described issue instead. |

## Working with multiple PRDs

A major initiative usually produces a **graph of focused PRDs** rather than one monolith. Decomposition rule: if a feature requires a separate team, a separate release, or has independent success metrics, it deserves its own PRD.

Before drafting any PRD for broad work, produce a decomposition plan:

1. List sub-features or sub-initiatives, each as a candidate PRD.
2. For each, identify the target users and primary metric.
3. Group those that share the same users and flows (good candidates for a single PRD).
4. Identify dependencies between them.

Then draft a product PRD for the initiative, linked to feature PRDs for each component.

### Anti-pattern: the monolith PRD

A single 20-page PRD covering the entire initiative will be ignored, impossible to review section by section, and stale before it ships. Split it. Smaller PRDs are read, reviewed, and executed faster.
