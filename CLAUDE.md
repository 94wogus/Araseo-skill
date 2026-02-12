# CLAUDE.md — Araseo-skill

## Project Overview

This repo is the **Claude Code Skill development** module of the [Araseo](https://github.com/94wogus/Araseo) project.

**Araseo (알아서)** — A tool that automatically converts AI conversation content into interactive visual mockups and diagrams. The name means "알아서 해" (it handles it on its own): feed it conversation content and it produces mockups automatically.

This repo is responsible for developing the `/araseo` skill, which enables Claude Code users to invoke the Araseo pipeline as a slash command.

## Owner

**스기리 (Sugiri)**

## Role

- Develop and maintain the `/araseo` Claude Code Skill
- The skill instructs Claude to read markdown planning docs and write structured JSON for rendering
- Integrate with the core Araseo pipeline (Claude reads & writes JSON via skill → Renderer)

## Core Pipeline (Parent Project)

1. User converses with Claude, producing a markdown planning document
2. Claude reads the planning doc and writes structured JSON (via `/araseo` skill) — there is NO separate parser module
3. Renderer transforms JSON into interactive flowcharts or UI mockups
4. When the planning doc changes, Claude auto-converts to updated JSON → rendering auto-updates

## Repo Rules

- Work ONLY within this repo (`Araseo-skill/`). Touching other repos is STRICTLY PROHIBITED.
- Follow the Core Pipeline and Key Modules defined in the parent project (`Araseo/CLAUDE.md`).
- Tasks are assigned from the parent level. Work ONLY within the assigned scope.

## Writing Convention

- All directives and instructions in rules/CLAUDE.md MUST be written in English.
- Examples and sample user expressions MUST be written in Korean.
  - e.g. "이 대화 내용을 목업으로 변환해줘"

## Skill Creation Rules

- Claude Code skills MUST be created inside this repo's `.claude/skills/` directory.
- Skill file structure:
  ```
  .claude/skills/<skill-name>/
  ├── SKILL.md           # Main instruction file (required)
  ├── template.md        # Template for Claude to fill (optional)
  ├── examples/          # Example outputs (optional)
  └── scripts/           # Execution scripts (optional)
  ```
- Follow the [Claude Code Skills spec](https://code.claude.com/docs/en/skills) for SKILL.md format.
- SKILL.md frontmatter fields: `name`, `description`, `argument-hint`, `allowed-tools`, `context`, `agent`, `model`
- Skills are scoped to THIS repo only — never create skills in other repos.

## Status

Greenfield — no code has been set up yet.
