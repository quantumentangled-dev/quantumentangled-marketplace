# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **Claude Code plugin marketplace** — a curated registry of plugins published under the `quantumentangled` namespace. This repo contains no plugin source code; the actual plugins live in separate GitHub repositories and are referenced from `marketplace.json`.

End users add the marketplace (`/plugin marketplace add quantumentangled-dev/quantumentangled-marketplace`), then install plugins with `/plugin install <plugin-name>@quantumentangled`. The `@quantumentangled` suffix matches the `name` field in `marketplace.json` — keep them aligned.

## Source-of-truth files (keep in sync)

- `.claude-plugin/marketplace.json` — the manifest Claude Code consumes. Top-level fields: `name`, `owner`, `description`, `plugins[]`. The `$schema` URL inside the file enables editor JSON-schema autocomplete (Claude Code ignores it at load time).
- `README.md` — the human-readable "Available plugins" table.

**Every plugin in `marketplace.json` needs a matching row in the README table, and vice versa.**

There is no build system, test suite, linter, package manager, or CI in this repo. Changes are JSON edits + Markdown updates. Don't go looking for `package.json` / `Makefile` / `.github/workflows/` — they're not here by design.

## Plugin entry shape

Current `threejs-skills` entry (verbatim from `.claude-plugin/marketplace.json`):

```json
{
  "name": "threejs-skills",
  "source": {
    "source": "github",
    "repo": "quantumentangled-dev/threejs-skills",
    "ref": "main",
    "sha": "bd9eaf9b7a803e9722a5f32793a1eae0f68f6731"
  },
  "category": "development",
  "tags": ["threejs", "graphics", "skills"]
}
```

`source.source` supports `github` (used here), `url` (any git URL), `git-subdir` (subdir of a monorepo, sparse-clone), `npm`, or a `./relative-path` string for plugins inside this repo. For the git-based forms, `ref` is an optional branch/tag (defaults to the repo's default branch) and `sha` is an optional 40-character commit pin. Plugin `name` must be kebab-case — the Claude.ai marketplace sync rejects other casings.

## Version-resolution gotcha (read before bumping)

Claude Code resolves a plugin's effective version (used as the cache key and for update detection) from **the first of these that is set**:

1. `version` in the **upstream plugin's** `plugin.json` (e.g. `quantumentangled-dev/threejs-skills/.claude-plugin/plugin.json`)
2. `version` in this marketplace entry
3. The git commit SHA

`threejs-skills` currently declares `"version": "0.1.0"` in its own `plugin.json`. That string is what existing users compare against. **Bumping `sha` here without bumping the upstream `version` is invisible to existing users** — Claude Code sees the same version string and keeps the cached copy.

Release recipe for an existing plugin:

1. In the upstream plugin repo, bump `version` in `.claude-plugin/plugin.json` (or omit the field entirely so each commit is treated as a new version).
2. In this repo, update `sha` (and `ref` if it changed) on the matching `plugins[]` entry.
3. Update the README row only if the user-visible description changed.

Avoid setting `version` in both `plugin.json` and the marketplace entry — `plugin.json` wins silently, which can mask intent.

## Common workflows

- **Add a plugin**: append an entry to `plugins[]` (with the upstream repo's current HEAD `sha`); add a row to the README table.
- **Update a plugin**: bump `sha` (and optionally `ref`) in the manifest; check the upstream `plugin.json` `version` per the gotcha above.
- **Remove a plugin**: delete the entry from `plugins[]` and its README row.

## Validate and test locally

```bash
# Validate manifest + plugin frontmatter:
claude plugin validate .

# Local-add this marketplace before pushing, then install from it:
claude plugin marketplace add /Users/pmartinez/Documents/git/quantumentangled/quantumentangled-marketplace
# in Claude Code:
/plugin install threejs-skills@quantumentangled
/plugin marketplace update quantumentangled   # refresh after editing
```

## Sibling layout

Plugin source repos are checked out as **siblings on disk** (e.g. `../threejs-skills`). When verifying a pinned `sha` or checking the upstream `plugin.json` `version`, read directly from the sibling rather than re-cloning.

## Reference docs

- Marketplace authoring: <https://code.claude.com/docs/en/plugin-marketplaces.md>
- Full schemas + CLI reference: <https://code.claude.com/docs/en/plugins-reference.md>
- Plugin authoring (skills, agents, hooks): <https://code.claude.com/docs/en/plugins.md>
