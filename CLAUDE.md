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

## Permissions (Sugiri-Specific)

**You have unrestricted write/edit access to the entire Araseo-skill/ repository.**

**What this means:**
- ✅ Create, edit, delete ANY files in `/Users/wogus/Wogus/Araseo/Araseo-skill/`
- ✅ Modify CLAUDE.md, rules, skills, any files in this repo
- ✅ NO permission prompts for operations within this repo
- ✅ Work freely without asking permission
- ❌ CANNOT modify files outside `Araseo-skill/` directory

**Example allowed operations:**
```
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/SKILL.md
✅ Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/CLAUDE.md
✅ Read: /Users/wogus/Wogus/Araseo/Araseo-skill/README.md
✅ Create: /Users/wogus/Wogus/Araseo/Araseo-skill/docs/guide.md
```

**Cross-repo operations:**
If you need to modify files outside `Araseo-skill/`, send a message to the appropriate repo owner (Team Lead for main repo, Nobi for Araseo-example/, Mole for mole/).

See `.claude/rules/permissions.md` for detailed guidelines.

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

## Ralph Loop - MANDATORY FOR ALL SKILL WORK (HIGHEST PRIORITY)

**⚠️ ABSOLUTE REQUIREMENT: Ralph Loop is MANDATORY for ALL skill development tasks. Working without Ralph Loop is STRICTLY PROHIBITED. ⚠️**

### What is Ralph Loop?

Ralph Loop implements the Ralph Wiggum technique - an iterative development methodology where the same prompt is fed to Claude repeatedly. Claude sees its own previous work in files and git history, creating a self-referential loop that iterates until genuine completion.

**Key concept**: The Stop hook intercepts exit attempts and feeds the same prompt back, allowing continuous improvement until the task is truly complete.

### Commands (Plugin-Provided)

**ALL users (team lead and teammates) use Skill tool syntax:**

```python
# Help
Skill(ralph-loop:help)

# Start ralph-loop
Skill(ralph-loop:ralph-loop, args: "task --completion-promise 'PROMISE TEXT' --max-iterations N")

# Cancel
Skill(ralph-loop:cancel-ralph)
```

**Usage examples:**
```python
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")

Skill(ralph-loop:ralph-loop, args: "스킬 템플릿 개선 --completion-promise 'TEMPLATE COMPLETE' --max-iterations 10")

Skill(ralph-loop:ralph-loop, args: "Add examples to /araseo skill --completion-promise 'EXAMPLES ADDED' --max-iterations 15")
```

**NOTE**: ralph-loop is a **plugin** that must be installed and configured in `.claude/settings.json`.

### MANDATORY Usage Rules (CRITICAL - NO EXCEPTIONS)

**🔴 SUGIRI MUST USE RALPH LOOP AUTOMATICALLY WITHOUT BEING TOLD:**

1. **Team Lead MAY include ralph-loop instruction in spawn prompt**
2. **Sugiri MUST automatically start ralph-loop for ANY skill development task**
3. **Working without ralph-loop is STRICTLY PROHIBITED**
4. **Sugiri does NOT wait for team lead to tell them to use ralph-loop**

**✅ ALWAYS use ralph-loop for (MANDATORY DEFAULT):**
- Skill creation (e.g., "스킬 만들어")
- Skill refinement or updates
- Documentation creation (e.g., "문서 작성해")
- Template or example creation
- ANY task taking more than 2 steps
- ANY task creating or editing files
- **DEFAULT: If unsure, USE ralph-loop**

**❌ ONLY skip ralph-loop for (RARE EXCEPTIONS):**
- Single-command operations (e.g., "git status", "ls")
- Pure reading/research without file creation
- Immediate one-line responses to questions

### Enforcement Mechanisms (Multi-Layer Protection)

**Layer 1: Spawn Prompt Injection (Team Lead)**
```
Team Lead spawns Sugiri with:
"CRITICAL: You MUST use ralph-loop for this task. Start with:
Skill(ralph-loop:ralph-loop, args: '<task description> --completion-promise \"TASK COMPLETE\" --max-iterations 20')"
```

**Layer 2: Sugiri Auto-Detection (MANDATORY SELF-ENFORCEMENT)**
```
Sugiri receives task → Immediately evaluate:
- Is this skill development? → YES → START ralph-loop IMMEDIATELY
- Does this create/edit files? → YES → START ralph-loop IMMEDIATELY
- Is this multi-step? → YES → START ralph-loop IMMEDIATELY
- Is this a single read command? → NO ralph-loop (exception)
```

**Layer 3: Team Lead Verification**
```
Team Lead checks Sugiri's first response:
- Did Sugiri start ralph-loop? → NO → IMMEDIATELY INSTRUCT TO USE IT
```

### Example Workflow (MANDATORY PATTERN)

```
User: "스기리한테 /araseo 스킬 만들라고 해"
Team Lead spawn prompt:
  "Create /araseo skill in Araseo-skill repo.
   MANDATORY: Use Skill tool to start ralph-loop immediately:
   Skill(ralph-loop:ralph-loop, args: 'Create /araseo skill --completion-promise \"ARASEO SKILL COMPLETE\" --max-iterations 20')"

Sugiri (AUTOMATICALLY - even without explicit instruction):
  → Sees task: "Create /araseo skill"
  → Recognizes it's skill creation (multi-step, file-creating)
  → IMMEDIATELY starts:
     Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")
  → Iteration 1: Create directory structure
  → Iteration 2: Write SKILL.md
  → Iteration 3: Add templates and examples
  → Iteration 4: Test skill
  → Iteration 5: Refine based on tests
  → Output: <promise>ARASEO SKILL COMPLETE</promise>
  → Skill(ralph-loop:cancel-ralph)
  → Report to team lead: "ARASEO skill complete"
```

### Completion Promise Rules

**CRITICAL**: Only output `<promise>TEXT</promise>` when the statement is **completely and unequivocally TRUE**.

- ✅ DO: Verify all work is complete before outputting promise
- ❌ DON'T: Output false promises to escape the loop
- ✅ Trust the process - the loop continues until genuine completion

### Violation Consequences

**If Sugiri works without ralph-loop:**
1. Team lead IMMEDIATELY stops the work
2. Team lead INSTRUCTS Sugiri to restart with ralph-loop
3. Previous work may be discarded
4. This is considered a CRITICAL VIOLATION

**If team lead forgets to include ralph-loop instruction:**
1. Sugiri MUST still use ralph-loop automatically
2. Sugiri is expected to SELF-ENFORCE this rule

**See `.claude/rules/ralph-loop.md` for comprehensive guide.**
**See `.claude/rules/ralph-loop-enforcement.md` for detailed enforcement mechanisms.**

## Status

Greenfield — no code has been set up yet.
