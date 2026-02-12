# CLAUDE.md — Araseo-skill

## Project Overview

This repo is the **Claude Code Skill development** module of the [Araseo](https://github.com/94wogus/Araseo) project.

**Araseo (알아서)** — A tool that automatically converts AI conversation content into interactive visual mockups and diagrams. The name means "알아서 해" (it handles it on its own): feed it conversation content and it produces mockups automatically.

This repo is responsible for developing the `/araseo` skill, which enables Claude Code users to invoke the Araseo pipeline as a slash command.

## Owner

**스기리 (Sugiri)**

## Role

- Develop and maintain the `/araseo` Claude Code Skill
- The skill takes AI conversation content as input and produces interactive visual mockups and diagrams
- Integrate with the core Araseo pipeline (Parser → Renderer)

## Core Pipeline (Parent Project)

1. User converses with Claude, generating documents containing diagrams
2. Parser converts documents (markdown/ASCII diagrams) into structured JSON
3. Renderer transforms JSON into interactive flowcharts or UI mockups

## Repo Rules

- Work ONLY within this repo (`Araseo-skill/`). Touching other repos is STRICTLY PROHIBITED.
- Follow the Core Pipeline and Key Modules defined in the parent project (`Araseo/CLAUDE.md`).
- Tasks are assigned from the parent level. Work ONLY within the assigned scope.

## Writing Convention

- All directives and instructions in rules/CLAUDE.md MUST be written in English.
- Examples and sample user expressions MUST be written in Korean.
  - e.g. "이 대화 내용을 목업으로 변환해줘"

## Status

Greenfield — no code has been set up yet.
