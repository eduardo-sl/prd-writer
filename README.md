# prd-writer

> Turn "we need to build X" into a structured PRD that aligns stakeholders on the problem, the users, the requirements, and the success criteria — at the right depth for whoever is reading it.

`prd-writer` is a single Markdown skill file (`SKILL.md`) that any AI coding or writing tool reads as instructions: Claude Code, Cursor, Windsurf, GitHub Copilot, Aider. The frontmatter routes it under Anthropic Agent Skills; the body is plain prose any tool can load.

It composes patterns from Amazon's Working Backwards (lead with impact, no prize for extra pages), Marty Cagan / SVPG (requirements are often hypotheses in disguise — trace them back to the problem), Shreyas Doshi (executives read at the level of impact; track usage metrics, not just impact metrics), and the `spec-writer` skill's five-phase process (investigate → clarify → scope → draft → self-check) into a workflow that forces the agent to understand the problem **before** writing requirements.

---

## What makes it different: one skill, two registers

A PRD a CTO approves and a PRD an engineering team builds from are different documents. Most templates pick one and force the other to fit. `prd-writer` picks the register from the reader:

- **Executive** — read by leadership, directors, CTOs. ~1 page, leads with the business bet and impact, pushes detail into appendices and links. Default for Product PRDs.
- **Operational** — read by the team that will build it. Every requirement, edge case, state, and metric resolved enough to act without a follow-up meeting. Default for Feature PRDs.

The skill picks the default from the PRD type, then overrides on an explicit signal in your prompt ("one-pager for the board" → executive; "detailed PRD for eng" → operational). Both registers keep the same section order and the same bar: every sentence must change what someone decides after reading. Neither licenses vagueness or padding.

---

## Install

### Via `npx skills` *(recommended)*

```bash
npx skills add eduardo-sl/prd-writer
```

### Single file via `curl`

```bash
curl -fsSL https://raw.githubusercontent.com/eduardo-sl/prd-writer/main/SKILL.md -o SKILL.md
```

### Per-tool placement

| Tool | Path |
| --- | --- |
| Claude Code | `.claude/skills/prd-writer/SKILL.md` |
| Cursor | `.cursor/rules/prd-writer.mdc` |
| Windsurf | `.windsurf/rules/prd-writer.md` |
| GitHub Copilot | reference from `.github/copilot-instructions.md` |
| Aider | `aider --read SKILL.md` |

One-liner for Claude Code:

```bash
mkdir -p .claude/skills/prd-writer && \
  curl -fsSL https://raw.githubusercontent.com/eduardo-sl/prd-writer/main/SKILL.md \
       -o .claude/skills/prd-writer/SKILL.md
```

---

## Quick start

Once the skill is loaded:

```
Use the prd-writer skill to write a feature PRD for adding CSV export.

Context:
- Product: analytics dashboard for small business owners
- Problem: users copy-paste data to Excel manually every week
- Target users: paid plan users
- MVP: export current table view as CSV
- Success: 30% of paid users export at least once in 30 days
```

The agent reads your product context, picks the PRD type and register, surfaces open questions, and writes the PRD to `docs/prd/<feature-name>/PRD.md`.

To force the register, name the reader:

```
Use prd-writer to write a one-page executive PRD for the leadership review
on launching a usage-based pricing tier.
```

```
Use prd-writer to write a detailed PRD for the eng team implementing
SSO — include all edge cases and the telemetry plan.
```

For more prompts by scenario, see **[EXAMPLES.md](EXAMPLES.md)**.

---

## The problem this solves

You ask an agent to write a PRD. It writes one. It just skipped the part where it understood the problem — and it wrote at whatever depth it felt like.

The result: a requirements list that's really a feature wishlist, goals that can't be measured, non-goals that are implicit (so they get built anyway), and a "problem statement" that reads "users can't do X" — which isn't a problem, it's a missing feature. And the whole thing is either a one-pager handed to engineers who now need a meeting, or twenty pages handed to a CTO who skims and forgets.

`prd-writer` forces a five-phase process where reading context, clarifying the problem, and choosing the register come *before* writing a single requirement.

---

## The five phases

1. **Understand context.** Read the product's context docs, existing PRDs, research, and strategy. Determine whether this is a Product PRD or Feature PRD.
2. **Clarify the problem.** Validate the problem statement, evidence, target users, and scope. A requirement handed to you is often a hypothesis in disguise — trace it back to the problem it assumes. Surface open questions in one batch.
3. **Choose the register, then scope.** Set executive vs. operational from the type and any explicit signal, then define the elevator pitch, MVP, and top user flows — no implementation details.
4. **Draft the PRD.** Use the conditional template. Requirements are user-facing functional statements, not technical ones. In the executive register, lower sections collapse to a line plus a link; in the operational register, they're spelled out in full.
5. **Self-check.** Eleven questions before saving, starting with: is the register consistent? Anything that fails gets fixed.

---

## What a good PRD looks like vs. a bad one

### Problem statement

Rejected:
> Users can't export their data.

Accepted:
> Users who reconcile dashboard data with their accounting tools spend 20+ min/week manually copying rows into spreadsheets, causing errors and abandoned workflows (source: 12 customer interviews, Aug 2024; support tickets: 34 in Q3).

### Requirements

Rejected:
> The system shall implement a CSV serialization endpoint at `/api/v1/export` using streaming for files > 10MB with a 30-second timeout.

Accepted:
> [P0] R1 — Users can export the current table view as a CSV file.
> [P1] R2 — Users can see export progress for tables with more than 1,000 rows.

### Metrics

Rejected:
> *(impact metrics only)*

Accepted:
> **Usage** — % of paid users who export at least once in 30 days (is it being used?).  
> **Impact** — reduction in support tickets tagged "manual export" (did it move the business?).  
> **Counter-metric** — dashboard load time must not regress.

### Non-goals

Rejected:
> *(no non-goals section)*

Accepted:
> N1 — This PRD does **not** cover PDF export.  
> N2 — Scheduled or recurring exports are out of scope for MVP.  
> N3 — API access to export data is out of scope.

---

## PRD vs. Spec

`prd-writer` produces alignment documents. `spec-writer` produces implementation specs. They are complementary, not competing.

| | PRD | Spec |
| --- | --- | --- |
| **Audience** | PM, design, eng, GTM, leadership | Engineers, AI coding agents |
| **Answers** | What to build and why | How to build it |
| **Language** | User-facing, plain language | Technical, system-level |
| **Output** | `docs/prd/<name>/PRD.md` | `specs/<name>/SPEC.md` |
| **Sequence** | PRD first | Spec after PRD is approved |

---

## Repository structure

```
prd-writer/
├── SKILL.md              ← The skill — install this
├── templates/
│   ├── FEATURE_PRD.md    ← Template for feature PRDs
│   └── PRODUCT_PRD.md    ← Template for product / initiative PRDs
├── EXAMPLES.md           ← Copy-paste prompts for common scenarios
├── README.md             ← This file
└── LICENSE
```

---

## References

The skill's approach is grounded in primary sources, not generic templates:

- **Amazon — *Working Backwards* (Colin Bryar & Bill Carr)** — the PR/FAQ process; lead with the customer, no prize for extra pages, distilled thinking over documented effort.
- **Marty Cagan / SVPG** — [*Requirements Are Not*](https://www.svpg.com/requirements-are-not/) and [*The End of Requirements*](https://www.svpg.com/the-end-of-requirements/); requirements are often hypotheses or constraints in disguise.
- **Shreyas Doshi** — the three levels of product work (execution / impact / optics) and the case for tracking usage metrics, not just impact metrics.

---

## Contributing

Issues and PRs welcome, especially for:

- Worked examples in specific domains (B2B SaaS, consumer, developer tools, marketplace).
- Refinements to Phase 2 (clarification batching) for ambiguous or vague requests.
- A gallery of negative examples — PRDs that failed and why.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Related

- [spec-writer](https://github.com/eduardo-sl/spec-writer) — implementation specs for the same feature, after the PRD is approved
- [tech-leads-club/agent-skills](https://github.com/tech-leads-club/agent-skills) — skill registry this follows conventions from
