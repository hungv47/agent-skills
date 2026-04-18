## Repository Structure

This root repo holds the README and shared docs, and tracks the skill repos as **git submodules**. Each submodule is an independent repo with its own history and remote:

- `research-skills/` — market research, audience, strategy, and experimentation skills
- `marketing-skills/` — brand, content creation, optimization, and measurement skills
- `product-skills/` — UX design, architecture, cleanup, and documentation skills
- `meta-skills/` — process-layer skills (scope, plan, analyze, verify, navigate)

Remotes: `github.com/hungv47/<name>-skills.git`. Submodule pointers (pinned commits) are recorded in `.gitmodules` and the root tree.

## Git Operations

- **Root repo**: tracks README.md, CLAUDE.md, .gitignore, shared docs, `.gitmodules`, and the submodule pointers.
- **Submodules**: each is a full git repo. Commit inside it, then update the pointer in root.

### Cloning / initializing

```bash
git clone --recurse-submodules <root-url>
# or, after a plain clone:
git submodule update --init --recursive
```

Submodules clone in a detached-HEAD state at the pinned commit. Before making changes in a submodule:

```bash
cd product-skills && git checkout main && git pull
```

### Making changes in a submodule (double-commit)

```bash
cd product-skills
# edit files
git add ... && git commit -m "..."
git push                      # (1) push submodule commit first

cd ..
git add product-skills        # stages the new pointer
git commit -m "Bump product-skills to <short-sha>"
git push                      # (2) push root's updated pointer
```

Forgetting step (2) leaves the root pointing at a stale commit. Always verify `git status` in root after committing inside a submodule.

### Pulling updates

```bash
git pull --recurse-submodules
# or, after a plain pull:
git submodule update --init --recursive
```

Plain `git submodule update` moves submodules back to the pinned commit (detached HEAD). Re-run `git checkout main` inside any submodule before resuming work.

## Skill Discovery

When unsure which skill to use, run `/navigate "your goal"` to get a recommended skill team with phased execution.
For artifact state only, run `/navigate status`.

## Quality Standard

This skill stack is premium. Every skill uses multi-agent orchestration, critic agents with quantitative rubrics, anti-sycophancy mechanisms, and signal-vs-noise verification. The bar for additions is high.

**When evaluating external skills, techniques, or patterns for adoption:**
- Default to SKIP. Only adopt what demonstrably improves output quality or fills a genuine domain gap.
- Check if we already have it under a different name before recommending anything.
- "Sounds interesting" is not a reason to adopt. "This would change the outcome of skill X in scenario Y" is.
- Techniques must survive adversarial review: Is this a real problem we have? Will the LLM actually follow this instruction? Is the implementation cost justified by the quality delta?
- Reference data and frameworks are worth stealing when they're practitioner-grade with cited research. Generic advice is noise.
- New skills must clear the same quality bar as existing skills — multi-agent architecture, critic gates, worked examples, anti-patterns. A lightweight conversational prompt is not a skill at our standard.

Signal, not noise. Protect the stack.

## Complexity Routing

Every skill declares a `budget` tier in its frontmatter: `fast`, `standard`, or `deep`. When invoking a skill, read its budget field and adjust execution:

| Budget | Execution | When |
|--------|-----------|------|
| **fast** | Single-agent, no sub-agent spawning, no critic gate. Respond directly. | discover, navigate, deploy-verify |
| **standard** | Reduced orchestration. Essential agents only, one critic pass. Skip optional refinement agents. | experiment, humanize, ship, user-flow, technical-writer, agent-room, task-breakdown, review-chain |
| **deep** | Full orchestration as documented. All layers, all agents, full critic gate. | All research skills (except experiment), copywriting, content-create, imc-plan, brand-system, seo, attribution, lp-optimization, system-architecture, code-cleanup |

**Auto-downgrade heuristics** (apply before dispatching agents):
- Input is ≤3 sentences AND doesn't reference prior artifacts AND skill budget is not deep → treat as fast
- Single-topic request with clear scope and no cross-domain needs → cap at standard
- References multiple artifacts, is cross-domain, or is ambiguous → use full skill tier

**Override:** User can request deeper execution on any tier ("run this thoroughly", "full analysis"). Budget is the default, not a ceiling.

## Completion Status Protocol

Every skill output ends with an explicit status. No implicit "here's the output" — the agent must declare the state.

| Status | Meaning | When |
|--------|---------|------|
| **DONE** | Output meets all requirements and passes critic gate | Clean completion |
| **DONE_WITH_CONCERNS** | Output delivered but with flagged risks or limitations | Critic passed but reviewer noted issues worth monitoring |
| **BLOCKED** | Cannot complete — missing input, external dependency, or unresolvable conflict | State what's needed to unblock |
| **NEEDS_CONTEXT** | Insufficient information to produce quality output | State what's missing and which upstream skill would provide it |

Skills that produce artifacts must include the status in the artifact frontmatter (`status: done | done_with_concerns | blocked | needs_context`). Skills that return inline results state the status at the end of their response.

## Design Philosophy

### Completeness Bias

When the complete implementation costs minutes more than the shortcut, do the complete thing. Every time.

**Lake vs. ocean:** A "lake" is boilable — achievable in one session with AI assistance. Full test coverage for a module, complete error handling, all edge cases. An "ocean" is not — multi-quarter platform migrations, full rewrites of mature systems. Boil lakes. Flag oceans as out of scope.

Skills should default to thoroughness. "Defer tests to a follow-up" is legacy thinking from when human engineering time was the bottleneck.

### Effort Compression

AI compresses implementation time. Use this table when evaluating build-vs-skip decisions in solution-design and system-architecture:

| Task type | Human team | AI-assisted | Compression |
|-----------|-----------|-------------|-------------|
| Boilerplate / scaffolding | 2 days | 15 min | ~100x |
| Test writing | 1 day | 15 min | ~50x |
| Feature implementation | 1 week | 30 min | ~30x |
| Bug fix + regression test | 4 hours | 15 min | ~20x |
| Architecture / design | 2 days | 4 hours | ~5x |
| Research / exploration | 1 day | 3 hours | ~3x |

An initiative that looks "High Effort" for a human team may be "Low Effort" with AI assistance. ICE scores should reflect AI-assisted effort, not raw human effort.

## Knowledge Management

Three mechanisms persist knowledge across sessions — each serves a different purpose:

| System | Location | Purpose |
|--------|----------|---------|
| Auto-memory | `MEMORY.md` | Cross-session user/project memory (preferences, context, references) |
| Learned rules | `.agents/meta/learned-rules.md` | Agent behavior corrections from user feedback |
| Experience docs | `.agents/experience/{domain}.md` | Domain-specific Q&A from discover sessions — reduces repeat questions |
