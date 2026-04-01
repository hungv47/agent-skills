## Repository Structure

This root repo holds the README and shared docs. The skill repos live as sibling directories but are gitignored — each has its own independent git repo and remote:

- `research-skills/` — market research, audience, strategy, and experimentation skills
- `marketing-skills/` — brand, content creation, optimization, and measurement skills
- `product-skills/` — UX design, architecture, cleanup, and documentation skills
- `meta-skills/` — process-layer skills (scope, plan, analyze, verify, navigate)
- `design-skills/` — dissolved (brand-system → marketing, user-flow → product). Stub kept for git history.

## Git Operations

- **Root repo**: only tracks README.md, CLAUDE.md, .gitignore, and any shared docs
- **Skill repos**: each subdirectory has its own git history and remote — commit inside them directly

When changing skill code, operate inside the specific skill repo:

```bash
cd product-skills && git add ... && git commit ...
```

Never `git add` a skill directory from the root — they are gitignored intentionally.

## Skill Discovery

When unsure which skill to use, run `/skill-router "your goal"` to get a recommended skill team with phased execution.
For artifact state only, run `/skill-router status` or `/artifact-status`.
