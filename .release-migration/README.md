# Release migration — main = v1, refactor/v2.0 = v2

One-time cleanup to align GitHub Releases with the new v1/v2 positioning.

After applying the steps below, this directory can be deleted.

---

## End state

- **v2.0.0** (pre-release) — the fresh-start refactor on `refactor/v2.0`. Already exists; minor body polish optional (`v2.0.0-release-notes.md`).
- **v1.5.2** (latest, stable) — represents `main` HEAD (`a4bef2c`). New release. Body in `v1.5.2-release-notes.md`.
- Everything else (the 11 old releases below) — **deleted**.

## Steps (GitHub UI, mobile-friendly)

Open https://github.com/hungv47/agent-skills/releases

### 1. Delete these 11 releases and their tags

For each: tap the release → **Edit** (pencil) → scroll down → **Delete this release**. Then go to the **Tags** tab and delete the matching tag.

| Release tag | SHA | Why |
|---|---|---|
| `v3.0.0` | `af787b8` | Was latest stable on main; superseded by new `v1.5.2`. |
| `2.1.0` | `9225af5` | Retroactive doc-commit tag. |
| `2.0.1` | `9225af5` | Retroactive doc-commit tag. |
| `2.0.0` | `9225af5` | Retroactive doc-commit tag — **also clashes visually with new `v2.0.0` fresh-start**. Deleting removes the ambiguity. |
| `1.5.3` | `9225af5` | Retroactive doc-commit tag. |
| `1.5.2` | `9225af5` | Retroactive — will be replaced by new `v1.5.2` (with `v` prefix) at main HEAD. |
| `1.5.1` | `9225af5` | Retroactive doc-commit tag. |
| `1.5.0` | `9225af5` | Retroactive doc-commit tag. |
| `1.4.0` | `9225af5` | Retroactive doc-commit tag. |
| `1.3.0` | `9225af5` | Retroactive doc-commit tag. |
| `1.0.0` | `9225af5` | Retroactive doc-commit tag. |

### 2. Create the new `v1.5.2` release

Releases → **Draft a new release**.

- **Tag:** `v1.5.2` — create on commit `a4bef2c` (or just select `main` if main is at `a4bef2c`).
- **Target:** `main`.
- **Title:** `Agent Skills v1.5.2 — repositioned v1 line head`
- **Body:** paste from `v1.5.2-release-notes.md` in this folder.
- Leave "Set as the latest release" **checked**.
- Do **not** check pre-release.
- Publish.

### 3. (Optional) Polish the existing `v2.0.0` pre-release body

Releases → `v2.0.0` → Edit → replace body with `v2.0.0-release-notes.md`. Keep pre-release flag checked. The only changes are wording tweaks to match the new v1.5.2 framing; safe to skip if you're short on time.

### 4. Delete this `.release-migration/` directory

```bash
git rm -r .release-migration
git commit -m "cleanup: remove release-migration scratch dir"
git push
```

---

## Notes

- Marketplace.json on main still says `"version": "5.2.0"`. The user's chosen mapping treats the existing minor/patch (`.2.0`) as v1's minor/patch, hence `v1.5.2`. Marketplace.json is **not** edited by this migration — only GitHub Releases. If you want the in-tree version string to match, that's a follow-up commit.
- The new tag uses the `v` prefix (`v1.5.2`) to match `v2.0.0` and `v3.0.0` convention; the deleted old tags did not use the prefix.
- Anyone pinned to a deleted tag (e.g. `npx skills add hungv47/agent-skills@1.5.0`) will break. Per the chosen plan ("delete old, retag cleanly"), this is accepted.
