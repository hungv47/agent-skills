# Agent Skills

23 skills for [AI agents](https://agentskills.io/home) that chain together — from problem diagnosis to shipped code.

Each skill writes structured artifacts to `.agents/`. Downstream skills read those artifacts automatically, so output compounds as you move through the stack.

## How It Works

<picture>
  <img src="./assets/overview.svg" alt="Cross-stack skill DAG showing how artifacts flow between Strategy, Comms, Design, and Prod stacks" width="100%">
</picture>

## Install

```bash
npx skills add hungv47/comms-skills
npx skills add hungv47/design-skills
npx skills add hungv47/prod-skills
npx skills add hungv47/strategy-skills
```

Update all skills:

```bash
npx skills update
```

Remove a skill pack:

```bash
npx skills remove hungv47/comms-skills
npx skills remove hungv47/design-skills
npx skills remove hungv47/prod-skills
npx skills remove hungv47/strategy-skills
```

## Skill Stacks

### Strategy — research, diagnose, prioritize, measure, test

> [`hungv47/strategy-skills`](https://github.com/hungv47/strategy-skills)

<picture>
  <img src="./assets/strategy.svg" alt="Strategy pipeline: market-research + problem-analysis → solution-design → funnel-planner → experiment" width="520">
</picture>

| Skill | What it does |
|-------|-------------|
| `market-research` | Competitive landscape, TAM/SAM/SOM sizing, whitespace opportunities |
| `problem-analysis` | Structured diagnosis, hypothesis development, root cause analysis |
| `solution-design` | Strategic options with evidence-backed ICE scoring and trade-off analysis |
| `funnel-planner` | Backward funnel modeling from revenue goals to traffic, conversions, unit economics |
| `experiment` | Minimum viable tests with clear decision rules |

### Comms — research, plan, create, optimize, measure

> [`hungv47/comms-skills`](https://github.com/hungv47/comms-skills)

<picture>
  <img src="./assets/comms.svg" alt="Comms pipeline: icp-research → imc-plan → content-create → attribution, plus standalone copywriting, lp-optimization, seo, humanize" width="520">
</picture>

| Skill | What it does |
|-------|-------------|
| `icp-research` | Deep audience research and Ideal Customer Profile development |
| `imc-plan` | Channel strategy, positioning, content calendar, budget allocation, GTM timelines |
| `content-create` | Draft social posts, ads, emails, blogs, case studies, video scripts in platform-native formats |
| `copywriting` | Craft-quality copy — headlines, hooks, CTAs, full-page copy with annotations |
| `attribution` | Map marketing activities to business outcomes, evaluate channel ROI |
| `lp-optimization` | Conversion audit — hero, CTA, social proof, objection handling, scored recommendations |
| `seo` | Technical audit, AI/GEO optimization, programmatic SEO, competitor pages |
| `humanize` | Strip AI patterns, inject voice, compress for density |

### Design — brand, flows

> [`hungv47/design-skills`](https://github.com/hungv47/design-skills)

<picture>
  <img src="./assets/design.svg" alt="Design pipeline: brand-system → user-flow" width="300">
</picture>

| Skill | What it does |
|-------|-------------|
| `brand-system` | Brand identity — strategy, personality, voice, visual identity, tokens |
| `user-flow` | Map screens, decisions, transitions, edge cases, and error states |

### Prod — plan, architect, build, verify, document

> [`hungv47/prod-skills`](https://github.com/hungv47/prod-skills)

<picture>
  <img src="./assets/prod.svg" alt="Prod pipeline: plan-interviewer → system-architecture → task-breakdown, plus standalone code-cleanup, technical-writer" width="520">
</picture>

| Skill | What it does |
|-------|-------------|
| `plan-interviewer` | Multi-round interviews to surface requirements before implementation |
| `system-architecture` | Technical blueprints — tech stack, database schema, API design, file structure, deployment plan |
| `task-breakdown` | Break implementation into granular, testable tasks with acceptance criteria |
| `code-cleanup` | Structural cleanup, AI slop removal, refactoring |
| `technical-writer` | Generate documentation and user guides from codebases |
| `skill-router` | Analyze a goal → suggest the right skill team → orchestrate multi-phase workflows (includes artifact scanning via `status` mode) |
| `multi-lens` | Multi-agent debate (agents argue in rounds) or consensus polling (agents analyze independently) for decisions |
| `review-chain` | Fresh-eyes review chain — implement → review → resolve, max 2 rounds |

multi-lens and review-chain are domain-agnostic process wrappers — they compose with any skill in any stack. Scope-locking (formerly `preflight`) is now `/plan-interviewer scope` (Route C).

## Example: Idea → Shipped Product

**Don't know where to start?** Run `/skill-router "your goal"` to get a recommended skill team with phased execution, parallel tracks, and checkpoints.

A typical end-to-end workflow:

```
1.  /icp-research        → understand your audience (creates product-context.md)
2.  /market-research      → map the competitive landscape
3.  /problem-analysis     → diagnose the core problem
4.  /solution-design      → prioritize what to build
5.  /funnel-planner       → set targets
6.  /brand-system         → define visual identity
7.  /user-flow            → map the screens
8.  /plan-interviewer     → sharpen the spec
9.  /system-architecture  → design the technical blueprint
10. /task-breakdown       → create buildable tasks
11. /copywriting          → write headlines, hooks, CTAs
12. /content-create       → write launch content
13. /lp-optimization      → optimize the landing page
14. /experiment           → design the validation test
```

Each step reads artifacts from previous steps — no copy-pasting between tools.

## Shared Artifacts

Skills communicate through markdown files in `.agents/`.

**Exception:** `technical-writer` writes directly to the project (README.md, docs/) rather than `.agents/` because its output is the project's documentation, not an intermediate artifact.

### Artifact Frontmatter

Every artifact uses this frontmatter:

```yaml
---
skill: [skill-name]
version: [N]           # increments on re-run
date: [YYYY-MM-DD]
status: draft | final
---
```

Some skills add extra fields (e.g., `compression` in humanize, `mode` in seo). These are optional extensions. The four fields above are the shared contract.

### Framework Conventions

**Step announcements:** When entering each numbered step in a skill, announce it with a one-line summary of what you found or decided. This gives the user visibility into progress and builds trust.

**Source citation:** When stating facts, statistics, or case study results in an artifact, cite the source. If from a web search, include the URL. If a fact cannot be attributed, flag it as `[UNVERIFIED]`. Strategy and research skills (market-research, problem-analysis, icp-research) should treat this as mandatory.

**Context loaded:** When producing an artifact, include which upstream artifacts were read and their versions/dates in the artifact body. This creates an audit trail for downstream skills.

### Artifact Table

| Artifact | Created by | Used by |
|----------|-----------|---------|
| `product-context.md` | `icp-research` | 12+ skills across all stacks |
| `mkt/icp-research.md` | `icp-research` | `imc-plan`, `brand-system`, `lp-optimization` |
| `market-research.md` | `market-research` | `solution-design` |
| `problem-analysis.md` | `problem-analysis` | `solution-design` |
| `solution-design.md` | `solution-design` | `imc-plan`, `system-architecture`, `funnel-planner` |
| `targets.md` | `funnel-planner` | `attribution`, `experiment` |
| `mkt/imc-plan.md` | `imc-plan` | `content-create`, `copywriting`, `seo`, `attribution` |
| `mkt/content/[slug].md` | `content-create` | `humanize`, `attribution` |
| `mkt/content/[slug].copy.md` | `copywriting` | `humanize`, `attribution` |
| `mkt/content/[slug].humanized.md` | `humanize` | Final content output |
| `mkt/attribution.md` | `attribution` | Re-run tracking (monthly cadence) |
| `mkt/seo-[mode].md` | `seo` | Implementation tracking |
| `design/brand-system.md` | `brand-system` | Informs visual decisions in content and landing pages |
| `design/user-flow.md` | `user-flow` | `system-architecture`, `task-breakdown` |
| `spec.md` | `plan-interviewer` | `system-architecture`, `task-breakdown` |
| `system-architecture.md` | `system-architecture` | `task-breakdown` |
| `tasks.md` | `task-breakdown` | Task execution (Phase 2) |
| `cleanup-report.md` | `code-cleanup` | Reference for follow-up cleanup |
| `experiment-[name].md` | `experiment` | `attribution` (confidence adjustments) |
| `workflow-plan.md` | `skill-router` | Multi-phase workflow orchestration |
| `meta/multi-lens-report.md` | `multi-lens` | Ephemeral — debate/poll synthesis |
| `meta/review-chain-report.md` | `review-chain` | Ephemeral — verification results |

## Multi-Agent Architecture

Every skill uses a **two-layer multi-agent orchestration** pattern optimized for parallel execution and quality control:

```
SKILL.md (Orchestrator)
  │
  ├─ Layer 1: Parallel specialists ──── run concurrently
  │     (research, audit, draft independent sections)
  │
  ├─ Merge Step ──────────────────────── assemble outputs
  │
  ├─ Layer 2: Sequential refiners ───── run in order
  │     (each refines the full document through one lens)
  │
  └─ Critic Agent ────────────────────── PASS / FAIL
        (max 2 rewrite cycles)
```

**How it works:**
- `SKILL.md` is the **orchestrator** — it routes tasks, dispatches agents, merges outputs, and enforces quality gates
- `agents/*.md` are **specialist instruction files** — each has a role, input/output contract, domain knowledge, and self-check
- `references/*.md` are **shared data catalogs** — formula lists, templates, and lookup tables read by multiple agents
- Every skill ends with a **critic agent** that scores output and returns PASS or FAIL with specific rewrite instructions
- Every orchestrator includes a **single-agent fallback** for non-multi-agent runtimes

**~139 specialized agents** across 21 domain skills (20 with multi-agent architectures, 1 utility). Each skill's CLAUDE.md documents its agent inventory and layer structure.

Additionally, 2 process-quality skills (`multi-lens`, `review-chain` in prod-skills) use a **dynamic agent spawning** pattern where agent count, roles, and instructions are defined at runtime. These do not have static agent rosters — see `prod-skills/CLAUDE.md`.

## License

MIT
