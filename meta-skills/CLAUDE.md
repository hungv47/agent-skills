# Meta Skills

Domain-agnostic process patterns that wrap around other skills to improve input quality, decision quality, or output quality.

## Skills

| Skill | What it does | Composition |
|-------|-------------|-------------|
| `preflight` | Surface assumptions + define success contract | Run BEFORE any domain skill |
| `multi-lens` | Multi-agent debate or consensus polling | REPLACE a decision step in any skill |
| `review-chain` | Fresh-eyes review → resolve chain | Run AFTER any domain skill |

## Composition Modes
- **Pre-skill:** `preflight` (run BEFORE a domain skill to improve its input)
- **Post-skill:** `review-chain` (run AFTER a domain skill to verify its output)
- **Alternative:** `multi-lens` (replace a single-agent decision with multi-perspective analysis)

## Artifacts
Meta-skill artifacts save to `.agents/meta/`:
- `.agents/meta/multi-lens-report.md` (+ `multi-lens-transcript.json` for debate mode)
- `.agents/meta/review-chain-report.md`
- `preflight` produces inline output (no persistent artifact)

All artifacts are ephemeral — overwritten on each run. These are process tools, not domain outputs.

## Orchestration Patterns

Meta-skills use two patterns, both different from the static two-layer model used by domain skills:

1. **Methodology** (`preflight`): Single-pass interactive — asks questions, defines contract, no subagents
2. **Dynamic spawning** (`multi-lens`, `review-chain`): Agent count, roles, and instructions defined at runtime based on the problem. No `agents/` directory — prompts are inline templates in SKILL.md

## Cross-Stack

All meta-skills are domain-agnostic. They compose with any skill in any stack:
- `preflight` before `system-architecture`, `task-breakdown`, or any build skill
- `multi-lens` for architecture decisions, strategic choices, or design trade-offs
- `review-chain` after `system-architecture`, `code-cleanup`, or any critical artifact
