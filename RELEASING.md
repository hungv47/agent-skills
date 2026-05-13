# Releasing — Conventions and Cadence

This file is the canonical reference for how releases work in the `agent-skills` umbrella. It is tracked (unlike `CLAUDE.md`, which is gitignored), so anyone cloning the repo — humans or AI agents — sees the same rules.

For the **mechanics** of bumping versions in lockstep (the double-commit dance, marketplace.json sync, etc.), see `CLAUDE.md` § "Git Operations → Releasing — bump versions in lockstep" if you have a local copy. The rules below cover **what** to release and **how** to write the release notes.

---

## Release cadence — batch landings, don't ship per patch

Default to fewer, larger releases. Fresh-eyes patches, typo fixes, single-skill word-tweaks, and post-ship rationale clarifications land in the working tree and wait for the next substantive bundle. Don't release per commit.

Reference incident: the 2026-05-10 → 2026-05-12 window shipped 5 marketplace releases in 3 days (2.3.4 → 2.5.0 with three intermediate fresh-eyes patches). Each release was technically valid but the bundle was wrong — users got 5 update notifications when the actual user-visible change was 2 bundles.

| Change shape | Action |
|---|---|
| Substantive (new skill, new mode, behavior change, methodology expansion) | Release |
| Fresh-eyes patch on an *unreleased* version | Fold into that version before releasing |
| Fresh-eyes patch on an *already-released* version | Accumulate in working tree until next substantive bundle, release together |
| Cosmetic / typo / doc-only | Never a release trigger on its own |

Solo-operator stack, small user base, no SLA. Target: 1–2 releases/week, not 5-per-3-days.

---

## CHANGELOG entries — release notes, not journal

`CHANGELOG.md` entries are release notes the user sees on `/plugin update`. They are NOT the canonical record of everything that happened in a release window. Canonical lives in commit history + `skills-resources/meta/records/` (fresh-eyes reports) + `roadmap.md` (strategic decisions).

Each entry follows this shape:

```markdown
## [X.Y.Z] - YYYY-MM-DD

One-paragraph summary of user-visible change. What's different for someone running `/plugin update`? Frame from the user's seat, not the implementor's.

### {Added|Changed|Fixed|Removed}
- ≤4 bullets. Each bullet ≤2 lines. One user-visible change per bullet.

Full review: `skills-resources/meta/records/YYYY-MM-DD-fresh-eyes-{slug}.md`
```

### Anti-patterns

Observed in pre-convention entries; do not reproduce:

- **File-change inventory** (`### Files changed: ...`) — git diff is authoritative
- **Fresh-eyes pattern recap** — lives in the records dir, link to it instead
- **Anti-goals respected** — lives in `roadmap.md`
- **"What did NOT change" exhaustive lists** — assume nothing changed unless stated
- **Implementation rationale paragraphs** — belongs in commit messages

### Length target

≤20 lines per release entry. The `marketing-skills` CHANGELOG grew to 570 lines because pre-convention entries averaged 30–40 lines each; new entries cap at ≤20.

---

## GitHub Release bodies

The GitHub Release body for each stack should mirror the CHANGELOG entry verbatim, plus a one-line install hint at the bottom:

```
npx skills update
# or for a fresh install:
npx skills add hungv47/<stack-name>
```

The marketplace-level GitHub Release (umbrella) lists which stacks moved and points users at `/plugin marketplace update agent-skills`.

---

## Tooling

The `docs-writing` skill in `product-skills` has a `--release-notes` mode (Route E, from v3.2.0 onward) that consumes a git range + this convention and emits a compliant CHANGELOG entry. Invoke as `/docs-writing --release-notes <version>`. The mode's critic-agent enforces every anti-pattern listed above; outputs that violate the convention FAIL the critic and re-dispatch the writer.

The marketplace version bump helper is at `scripts/bump-marketplace.ts`:

```bash
bun scripts/bump-marketplace.ts <patch|minor|major> "<one-line summary>"
```

Run in the **same umbrella commit** that bumps submodule pointers.
