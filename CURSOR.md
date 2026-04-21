# Using this repo with Cursor

This project is a reusable skill repository that also commits a generated **Cursor project rule** so the Karpathy-inspired behavioral guidelines apply automatically when you work here.

> Upstream credit: This repository builds on [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills).

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc) is committed with `alwaysApply: true`, so you do not need extra installation steps.
3. In Cursor, you can confirm it under **Settings → Rules** (or the project rules UI), where `karpathy-guidelines` should appear.

## Use the same guidelines in another project

**Cursor rule copy:** Copy `.cursor/rules/karpathy-guidelines.mdc` into that project’s `.cursor/rules/` directory (create the folders if needed). Adjust or merge with existing rules as you like.

**Reusable skill copy or symlink:** Start from [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md) and copy or symlink the [`skills/karpathy-guidelines/`](skills/karpathy-guidelines/) directory into your local skills directory for tools that load reusable skills.

**CLAUDE.md compatibility:** If a tool wants a project-level instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project or merge its contents into the existing instructions.

## Artifact relationship

- [`skills/karpathy-guidelines/content.md`](skills/karpathy-guidelines/content.md) = editable source of truth
- [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md) = generated reusable skill artifact
- [`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc) = generated Cursor rule
- [`CLAUDE.md`](CLAUDE.md) = generated compatibility instruction file

## For contributors

When you change the four principles, edit [`skills/karpathy-guidelines/content.md`](skills/karpathy-guidelines/content.md) and regenerate the derived files with:

```bash
python3 scripts/render_guideline_artifacts.py
python3 scripts/render_guideline_artifacts.py --check
```

Do not hand-edit [`CLAUDE.md`](CLAUDE.md), [`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc), or [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md).
