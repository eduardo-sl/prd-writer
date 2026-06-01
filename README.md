# prd-writer

> Turn "we need to build X" into a structured PRD that aligns stakeholders on the problem, the users, the requirements, and the success criteria — before anyone writes a line of code.

`prd-writer` is a single Markdown skill file (`SKILL.md`) that any AI coding or writing tool reads as instructions: Claude Code, Cursor, Windsurf, GitHub Copilot, Aider. The frontmatter routes it under Anthropic Agent Skills; the body is plain prose any tool can load.

It composes patterns from Figma's PRD approach (Problem / Solution / Launch Readiness), Carlin Yuen's PRD structure (problem-first, use-case bucketed requirements), and the `spec-writer` skill's five-phase process (investigate → clarify → scope → draft → self-check) into a workflow that forces the agent to understand the problem **before** writing requirements.

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

The agent reads your product context, determines the right PRD type (product vs. feature), surfaces open questions, and writes the PRD to `docs/prd/<feature-name>/PRD.md`.

For more prompts by scenario and level, see **[EXAMPLES.md](EXAMPLES.md)**.

---

## The problem this solves

You ask an agent to write a PRD. It writes one. It just skipped the part where it understood the problem.

The result: a requirements list that's really a feature wishlist, goals that can't be measured, non-goals that are implicit (which means they'll be built anyway), and a "problem statement" that reads "users can't do X" — which is not a problem, it's a missing feature.

`prd-writer` forces a five-phase process where reading context, clarifying the problem, and scoping the solution come *before* writing a single requirement.

---

## The five phases

1. **Understand context.** Read the product's context docs, existing PRDs, research, and strategy. Determine whether this is a Product PRD or Feature PRD.
2. **Clarify the problem.** Validate the problem statement, evidence, target users, and scope before drafting. Surface open questions.
3. **Scope the solution.** Define the elevator pitch, MVP, and top user flows at high level — no implementation details.
4. **Draft the PRD.** Use the conditional template. Requirements are user-facing functional statements, not technical requirements.
5. **Self-check.** Ten questions before saving. Anything that fails gets fixed.

---

## What a good PRD looks like vs. a bad one

### Problem statement

Rejected:
> Users can't export their data.

Accepted:
> Users who need to reconcile dashboard data with their accounting tools spend 20+ min/week manually copying rows into spreadsheets, causing errors and abandoned workflows (source: 12 customer interviews, Aug 2024; support tickets: 34 in Q3).

### Requirements

Rejected:
> The system shall implement a CSV serialization endpoint at `/api/v1/export` using streaming for files > 10MB with a 30-second timeout.

Accepted:
> [P0] R1 — Users can export the current table view as a CSV file.
> [P1] R2 — Users can see export progress for tables with more than 1,000 rows.

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
