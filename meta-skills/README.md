# Meta Skills

3 process-layer skills that wrap around domain skills to improve quality at every stage.

## Install

```bash
npx skills add hungv47/meta-skills
```

## Skills

| Skill | What it does |
|-------|-------------|
| `preflight` | Surface assumptions and define a 4-part success contract before building |
| `multi-lens` | Multi-agent debate (agents argue in rounds) or consensus polling (agents analyze independently) |
| `review-chain` | Fresh-eyes review chain — implement → review → resolve, max 2 rounds |

## How They Compose

```
/preflight → /system-architecture → /review-chain
   ↑ scope        ↑ build              ↑ verify
```

Meta-skills are horizontal — they attach to any point in the pipeline, not a fixed position.

| Mode | Skill | Example |
|------|-------|---------|
| Before (improve input) | `preflight` | `/preflight` → `/system-architecture` |
| Instead (replace decision) | `multi-lens` | `/multi-lens "tech stack debate"` → feed into architecture |
| After (verify output) | `review-chain` | `/code-cleanup` → `/review-chain` |

## Rigorous Technical Build (full recipe)

```
1. /preflight            → surface assumptions, define contract
2. /plan-interviewer     → spec.md (interactive)
3. /system-architecture  → system-architecture.md
4. /review-chain         → verify architecture
5. /task-breakdown       → tasks.md
6. (build tasks, /review-chain after each critical task)
7. /code-cleanup + /technical-writer (parallel)
```

## License

MIT
