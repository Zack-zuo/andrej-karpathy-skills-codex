# Skill-Oriented Repository Refactor Design

**Date:** 2026-04-21

**Status:** Proposed and approved for implementation planning

## Goal

Refactor this repository so it is clearly distributed and maintained as a reusable skill, not as a Claude Code or Codex plugin. The repository should present the skill as the primary product, remove plugin packaging, and keep derived compatibility artifacts only where they still support non-plugin workflows.

## Current State

The repository currently mixes two distribution models:

- A root reusable skill at `skills/karpathy-guidelines/SKILL.md`
- A Codex plugin package under `plugins/andrej-karpathy-skills/`
- A repo-local plugin marketplace entry at `.agents/plugins/marketplace.json`
- Generated compatibility artifacts for `CLAUDE.md` and Cursor project rules

That structure makes the project read as plugin-oriented even though the core value is the guideline content itself.

## Goals

- Make the repository skill-first in structure and documentation
- Remove plugin packaging and plugin marketplace scaffolding
- Move canonical content ownership into the root skill area
- Keep `CLAUDE.md` and Cursor rule generation as compatibility outputs
- Update documentation so installation and usage are described in skill-oriented terms
- Preserve the existing four Karpathy-inspired principles without rewriting their substance

## Non-Goals

- Renaming the project into a broader multi-skill distribution
- Changing the four principles themselves
- Introducing new features, new skills, or new installation systems
- Reworking unrelated examples or repository organization beyond what the refactor needs

## Recommended Approach

Use the root skill directory as the single source of truth and remove all plugin-specific distribution paths.

This approach matches the requested outcome with minimal churn:

- The repository becomes structurally honest about what it ships
- The generator becomes simpler because it no longer renders plugin payloads
- Documentation can describe one primary usage model instead of mixing skill and plugin concepts

## Source of Truth

Canonical guideline content should move from the plugin tree into the skill tree.

Recommended structure:

- Canonical body: `skills/karpathy-guidelines/content.md`
- Generated skill artifact: `skills/karpathy-guidelines/SKILL.md`
- Generated compatibility artifact: `CLAUDE.md`
- Generated compatibility artifact: `.cursor/rules/karpathy-guidelines.mdc`

The generator should read only `skills/karpathy-guidelines/content.md` and render those three outputs.

## Repository Changes

### Remove plugin-oriented files and directories

Delete the following plugin-specific paths:

- `.agents/plugins/marketplace.json`
- `plugins/andrej-karpathy-skills/.codex-plugin/plugin.json`
- `plugins/andrej-karpathy-skills/assets/icon.png`
- `plugins/andrej-karpathy-skills/content/karpathy-guidelines.md`
- `plugins/andrej-karpathy-skills/skills/karpathy-guidelines/SKILL.md`
- Any now-empty plugin directories left behind after deletion

### Update generator ownership

Modify `scripts/render_guideline_artifacts.py` so that:

- `CANONICAL_SOURCE` points to `skills/karpathy-guidelines/content.md`
- Plugin output constants are removed
- `render_outputs()` returns only the skill file, `CLAUDE.md`, and the Cursor rule
- The script behavior and generated notices remain otherwise consistent

### Update tests

Modify `tests/test_render_guideline_artifacts.py` so that:

- Test fixtures use the new canonical source path
- Generated file assertions cover only:
  - `skills/karpathy-guidelines/SKILL.md`
  - `CLAUDE.md`
  - `.cursor/rules/karpathy-guidelines.mdc`
- Drift detection expectations continue to validate `--check`
- No test references to plugin paths remain

## Documentation Changes

### README.md

Reposition the project as a reusable skill repository rather than a plugin package.

Expected changes:

- Title and introduction should emphasize skill-oriented usage
- Installation guidance should lead with using the skill directly
- `CLAUDE.md` and Cursor rule should be described as derived compatibility artifacts
- Remove Claude plugin marketplace installation language
- Remove Codex plugin package and local marketplace language
- Contributor guidance should point edits to `skills/karpathy-guidelines/content.md`

### README.zh.md

Apply the same documentation shift in Chinese:

- Remove plugin-oriented framing
- Present the repo as a reusable skill
- Update contributor instructions to the new canonical content path

### CURSOR.md

Update wording so the relationship is:

- The canonical content lives in the skill directory
- Cursor consumes the generated `.cursor/rules/karpathy-guidelines.mdc`
- Claude-style tools can consume the generated `CLAUDE.md`

The document should no longer reference Claude Code plugin installation or Codex plugin payloads.

## Expected File Ownership After Refactor

- `skills/karpathy-guidelines/content.md`: editable source of truth
- `skills/karpathy-guidelines/SKILL.md`: generated artifact
- `CLAUDE.md`: generated artifact
- `.cursor/rules/karpathy-guidelines.mdc`: generated artifact
- `scripts/render_guideline_artifacts.py`: generator logic
- `tests/test_render_guideline_artifacts.py`: generator verification
- `README.md`, `README.zh.md`, `CURSOR.md`: user and contributor documentation

## Data Flow

The post-refactor generation flow should be:

1. Edit `skills/karpathy-guidelines/content.md`
2. Run `python3 scripts/render_guideline_artifacts.py`
3. Regenerate:
   - `skills/karpathy-guidelines/SKILL.md`
   - `CLAUDE.md`
   - `.cursor/rules/karpathy-guidelines.mdc`
4. Verify with `python3 scripts/render_guideline_artifacts.py --check`

## Error Handling and Safety

- The refactor should be surgical: remove only plugin-specific assets and references
- Generated file markers should remain so contributors do not hand-edit derived files
- Documentation should not promise unsupported install commands after the plugin paths are removed
- The change should avoid broad branding churn that would make the diff larger than necessary

## Testing and Verification

Implementation should verify:

- `python3 scripts/render_guideline_artifacts.py` completes successfully
- `python3 scripts/render_guideline_artifacts.py --check` passes after regeneration
- `python3 -m unittest tests/test_render_guideline_artifacts.py` passes
- A repo-wide search shows no remaining documentation that markets the repository as a plugin package for Claude Code or Codex

Recommended grep checks during implementation:

- `rg -n "plugin marketplace|/plugin install|\\.codex-plugin|plugins/andrej-karpathy-skills|repo-local Codex plugin|Claude Code plugin|Codex plugin" README.md README.zh.md CURSOR.md scripts tests skills .agents plugins`
- `rg -n "plugins/andrej-karpathy-skills|\\.agents/plugins/marketplace.json" .`

## Implementation Scope Boundaries

This refactor is complete when:

- Plugin packaging is removed from the repository
- Canonical guideline content is owned by the skill directory
- The render script and tests reflect the new ownership model
- The README and related docs describe the project as skill-oriented rather than plugin-oriented

This refactor is not required to:

- Redesign the overall visual presentation of the repository
- Add new automation beyond the existing render script and tests
- Change the substantive guideline content
