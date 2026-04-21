# Using this repo with Cursor

This project includes a **Cursor project rule** so the Karpathy-inspired behavioral guidelines apply automatically when you work here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc) is committed with `alwaysApply: true`, so you do not need extra installation steps.
3. In Cursor, you can confirm it under **Settings → Rules** (or the project rules UI), where `karpathy-guidelines` should appear.

## Use the same guidelines in another project

**Cursor (recommended):** Copy `.cursor/rules/karpathy-guidelines.mdc` into that project’s `.cursor/rules/` directory (create the folders if needed). Adjust or merge with existing rules as you like.

**Other tools:** If a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead (or merge its contents into your existing instructions).

## Optional: personal Agent Skills

If you want the same content as a reusable skill under `~/.cursor/skills`, use [`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md). You can copy or symlink it into your personal skills directory; use whatever layout you use for other skills.

## Claude Code, Cursor, and Codex

- **Claude Code:** Install via the plugin marketplace and [`README.md`](README.md) instructions; the plugin exposes the skill from this repo. Per-project use can also rely on `CLAUDE.md`.
- **Cursor:** Use the committed `.cursor/rules/` file as described above. Cursor does not read `.claude-plugin/` or `CLAUDE.md` by default.
- **Codex:** Use the repo-local plugin payload under [`plugins/andrej-karpathy-skills/`](plugins/andrej-karpathy-skills/) and the repo marketplace catalog at [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json).

## For contributors

When you change the four principles, edit [`plugins/andrej-karpathy-skills/content/karpathy-guidelines.md`](plugins/andrej-karpathy-skills/content/karpathy-guidelines.md) and regenerate the derived files with:

```bash
python3 scripts/render_guideline_artifacts.py
python3 scripts/render_guideline_artifacts.py --check
```

Do not hand-edit **[`CLAUDE.md`](CLAUDE.md)**, **[`.cursor/rules/karpathy-guidelines.mdc`](.cursor/rules/karpathy-guidelines.mdc)**, **[`skills/karpathy-guidelines/SKILL.md`](skills/karpathy-guidelines/SKILL.md)**, or **[`plugins/andrej-karpathy-skills/skills/karpathy-guidelines/SKILL.md`](plugins/andrej-karpathy-skills/skills/karpathy-guidelines/SKILL.md)**.
