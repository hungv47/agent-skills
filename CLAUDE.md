## Repository Structure

This root repo holds the README and shared docs. The skill repos live as sibling directories but are gitignored — each has its own independent git repo and remote:

- `comms-skills/` — marketing communication skills
- `design-skills/` — brand and UX design skills
- `prod-skills/` — engineering productivity skills
- `strategy-skills/` — business strategy skills

## Git Operations

- **Root repo**: only tracks README.md, CLAUDE.md, .gitignore, and any shared docs
- **Skill repos**: each subdirectory has its own git history and remote — commit inside them directly

When changing skill code, operate inside the specific skill repo:

```bash
cd prod-skills && git add ... && git commit ...
```

Never `git add` a skill directory from the root — they are gitignored intentionally.
