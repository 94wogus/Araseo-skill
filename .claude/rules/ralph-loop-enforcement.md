# Ralph Loop Enforcement Rules for Sugiri (MANDATORY - HIGHEST PRIORITY)

**⚠️ CRITICAL: These rules are MANDATORY and OVERRIDE all other behaviors. Failure to comply is a VIOLATION. ⚠️**

## Core Principle

**Ralph Loop is NOT optional. It is the DEFAULT mode of operation for ALL skill development work.**

- When you receive a task, your FIRST action is to evaluate if ralph-loop is required
- If task involves creating/editing files or is multi-step → ralph-loop is MANDATORY
- You do NOT ask permission, you do NOT wait for instructions
- You AUTOMATICALLY start ralph-loop

## Auto-Detection Algorithm (MANDATORY EXECUTION)

**Every time you receive a task, execute this algorithm:**

```
1. Read task description
2. Evaluate task type:
   a. Is this skill creation/modification? → MANDATORY ralph-loop
   b. Does this involve creating files? → MANDATORY ralph-loop
   c. Does this involve editing files? → MANDATORY ralph-loop
   d. Is this multi-step (more than 2 steps)? → MANDATORY ralph-loop
   e. Is this documentation creation? → MANDATORY ralph-loop
   f. Is this a single read-only command (ls, git status, etc.)? → NO ralph-loop (exception)
   g. Is this an immediate text response? → NO ralph-loop (exception)
3. If MANDATORY ralph-loop → START IMMEDIATELY (do NOT proceed to step 4)
4. If exception → Proceed without ralph-loop
```

**Decision tree:**

```
Task received
    ↓
Is it skill development? ──YES──→ MANDATORY ralph-loop (STOP, START LOOP NOW)
    ↓ NO
Does it create files? ──YES──→ MANDATORY ralph-loop (STOP, START LOOP NOW)
    ↓ NO
Does it edit files? ──YES──→ MANDATORY ralph-loop (STOP, START LOOP NOW)
    ↓ NO
Is it multi-step (>2 steps)? ──YES──→ MANDATORY ralph-loop (STOP, START LOOP NOW)
    ↓ NO
Is it single read command? ──YES──→ NO ralph-loop (exception)
    ↓ NO
DEFAULT → MANDATORY ralph-loop (when in doubt, USE IT)
```

## Task Type Classification (with Examples)

### Category A: MANDATORY ralph-loop (99% of tasks)

**Skill creation:**
- "스킬 만들어"
- "Create /araseo skill"
- "Add new skill for X"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Create skill --completion-promise 'SKILL COMPLETE' --max-iterations 20")`

**Skill modification:**
- "스킬 수정해"
- "Update /araseo skill to support Y"
- "Fix skill bug"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Update skill --completion-promise 'SKILL UPDATED' --max-iterations 15")`

**Documentation creation:**
- "문서 만들어"
- "Add guides to Araseo-skill"
- "Create README"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Create documentation --completion-promise 'DOCS COMPLETE' --max-iterations 15")`

**Template/example creation:**
- "템플릿 만들어"
- "Add examples to skill"
- "Create template for X"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Create templates --completion-promise 'TEMPLATES COMPLETE' --max-iterations 10")`

**File creation of any kind:**
- "파일 만들어"
- "Create X.md"
- "Set up directory structure"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Create files --completion-promise 'FILES COMPLETE' --max-iterations 10")`

**File editing of any kind:**
- "파일 수정해"
- "Update CLAUDE.md"
- "Fix typo in X"
→ **IMMEDIATE ACTION**: `Skill(ralph-loop:ralph-loop, args: "Update files --completion-promise 'FILES UPDATED' --max-iterations 5")`

### Category B: NO ralph-loop (1% of tasks - RARE EXCEPTIONS)

**Single read command:**
- "git status"
- "ls .claude/skills/"
- "cat SKILL.md"
→ **ACTION**: Execute command directly, no ralph-loop

**Immediate text response:**
- "What is the /araseo skill for?"
- "Explain the skill structure"
- "Status report"
→ **ACTION**: Respond immediately, no ralph-loop

**Already inside ralph-loop:**
- You're already in an active ralph-loop session
→ **ACTION**: Continue current loop, don't nest

## Response Patterns (MANDATORY FORMAT)

### CORRECT Response Pattern (for MANDATORY ralph-loop tasks)

```
Task: "스기리, /araseo 스킬 만들어"

Sugiri (IMMEDIATE RESPONSE):
"Starting ralph-loop for skill creation."

[IMMEDIATELY invoke Skill tool]
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ with JSON schema instructions for markdown → JSON conversion --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")

[Ralph loop begins automatically]
[Iterations proceed]
[Work continues until completion]
[Output: <promise>ARASEO SKILL COMPLETE</promise>]
[Call Skill(ralph-loop:cancel-ralph)]

"ARASEO skill creation complete. Skill available at .claude/skills/araseo/SKILL.md"
```

### WRONG Response Pattern (VIOLATION - NEVER DO THIS)

```
❌ Task: "스기리, 스킬 만들어"

❌ Sugiri (WRONG):
"I'll create the skill directory first..."
[Starts working without ralph-loop]
[Creates files manually]
"Skill directory created. Now I'll write SKILL.md..."

→ VIOLATION! STOP IMMEDIATELY!
→ Team lead will instruct to restart with ralph-loop
```

### WRONG Response Pattern (VIOLATION - ASKING PERMISSION)

```
❌ Task: "스기리, 문서 만들어"

❌ Sugiri (WRONG):
"Should I use ralph-loop for this task?"

→ VIOLATION! DO NOT ASK!
→ You MUST automatically use ralph-loop for file creation tasks
→ Asking means you failed to auto-detect
```

## Self-Enforcement Checklist (RUN BEFORE EVERY TASK)

**Before starting any task, ask yourself:**

- [ ] **Did I receive a task from team lead?**
  - ✅ YES → Continue to next check
  - ❌ NO → Wait for task

- [ ] **Is this task skill-related (creation/modification)?**
  - ✅ YES → MANDATORY ralph-loop (STOP, START NOW)
  - ❌ NO → Continue to next check

- [ ] **Does this task create new files?**
  - ✅ YES → MANDATORY ralph-loop (STOP, START NOW)
  - ❌ NO → Continue to next check

- [ ] **Does this task edit existing files?**
  - ✅ YES → MANDATORY ralph-loop (STOP, START NOW)
  - ❌ NO → Continue to next check

- [ ] **Is this task multi-step (more than 2 steps)?**
  - ✅ YES → MANDATORY ralph-loop (STOP, START NOW)
  - ❌ NO → Continue to next check

- [ ] **Is this a single read-only command or immediate response?**
  - ✅ YES → NO ralph-loop (exception)
  - ❌ NO → DEFAULT: MANDATORY ralph-loop (STOP, START NOW)

- [ ] **Have I started ralph-loop?**
  - ✅ YES → Proceed with work
  - ❌ NO → VIOLATION! START IT NOW!

## Common Mistakes and Corrections

### Mistake 1: Working Without Ralph Loop

**Symptom:**
```
Task: "Create skill"
Sugiri: [Starts creating files without ralph-loop]
```

**Correction:**
```
STOP IMMEDIATELY!
Discard current work if needed
Start over with:
Skill(ralph-loop:ralph-loop, args: "Create skill --completion-promise 'SKILL COMPLETE' --max-iterations 20")
```

### Mistake 2: Asking for Permission

**Symptom:**
```
Task: "Update CLAUDE.md"
Sugiri: "Should I use ralph-loop for this?"
```

**Correction:**
```
DO NOT ASK!
The auto-detection algorithm tells you: file editing → MANDATORY ralph-loop
Start immediately:
Skill(ralph-loop:ralph-loop, args: "Update CLAUDE.md --completion-promise 'CLAUDE.MD UPDATED' --max-iterations 10")
```

### Mistake 3: Thinking Ralph Loop is Optional

**Symptom:**
```
Task: "Create documentation"
Sugiri: "I can do this without ralph-loop..."
```

**Correction:**
```
NO! Ralph loop is NOT optional!
File creation → MANDATORY ralph-loop
Start immediately:
Skill(ralph-loop:ralph-loop, args: "Create documentation --completion-promise 'DOCS COMPLETE' --max-iterations 15")
```

### Mistake 4: Waiting for Explicit Instruction

**Symptom:**
```
Task: "스킬 만들어"
Sugiri: [Waits for team lead to say "use ralph-loop"]
```

**Correction:**
```
DO NOT WAIT!
You MUST auto-detect that skill creation → MANDATORY ralph-loop
Start immediately WITHOUT waiting:
Skill(ralph-loop:ralph-loop, args: "Create skill --completion-promise 'SKILL COMPLETE' --max-iterations 20")
```

## Integration with Team Workflow

### Team Lead Spawn Prompt (What to Expect)

**Team lead SHOULD include ralph-loop instruction:**
```
"Create /araseo skill in Araseo-skill repo.
MANDATORY: Start ralph-loop immediately with:
Skill(ralph-loop:ralph-loop, args: 'Create /araseo skill --completion-promise \"ARASEO SKILL COMPLETE\" --max-iterations 20')"
```

**Your response:**
```
"Starting ralph-loop as instructed."
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")
[Proceed with loop]
```

### Team Lead Spawn Prompt Without Ralph Loop Instruction (STILL REQUIRED)

**Team lead forgets to include ralph-loop instruction:**
```
"스기리, 스킬 만들어"
```

**Your response (MANDATORY SELF-ENFORCEMENT):**
```
[Auto-detect: skill creation → MANDATORY ralph-loop]
"Starting ralph-loop for skill creation."
Skill(ralph-loop:ralph-loop, args: "Create skill in Araseo-skill repo --completion-promise 'SKILL COMPLETE' --max-iterations 20")
[Proceed with loop]
```

**NOTE**: Even if team lead forgets, YOU MUST still use ralph-loop. Self-enforcement is REQUIRED.

### Reporting to Team Lead

**During long loops:**
- Send progress updates every 3-5 iterations
- Example: "Iteration 5/20: SKILL.md complete, adding templates"

**After completion:**
- Report task completion
- Include deliverable locations
- Example: "ARASEO skill complete at .claude/skills/araseo/SKILL.md"

## Ralph Loop Plugin Availability

### Expected Behavior

Ralph Loop is a **plugin** accessed via Skill tool (`Skill(ralph-loop:ralph-loop, args: "...")`). It must be installed and configured in `.claude/settings.json`.

**Required configuration:**
```json
{
  "enabledPlugins": {
    "ralph-loop@claude-plugins-official": true
  }
}
```

**Verification:**
- Plugin is configured in `/Users/wogus/Wogus/Araseo/Araseo-skill/.claude/settings.json` ✓
- Commands should be available via Skill tool

### If Plugin is Unavailable (EMERGENCY FALLBACK)

**Error:**
```
Error: Unknown skill: ralph-loop
```

**IMMEDIATE RESPONSE (MANDATORY):**
```
"CRITICAL ERROR: ralph-loop plugin is not available in my environment.
This may indicate missing plugin installation or configuration issue.

Ralph Loop is MANDATORY for this task. I CANNOT proceed without it.

REQUEST: Verify .claude/settings.json has ralph-loop plugin configured:
{
  \"enabledPlugins\": {
    \"ralph-loop@claude-plugins-official\": true
  }
}

FALLBACK: If plugin is genuinely unavailable, I will work in manual iteration mode (degraded),
but this is NOT the preferred approach and may be less effective."
```

### Manual Iteration Mode (LAST RESORT ONLY)

**⚠️ USE ONLY IF RALPH-LOOP PLUGIN IS GENUINELY UNAVAILABLE**

If ralph-loop plugin is truly unavailable:

1. **Report the issue immediately**
2. **Work in manual iteration mode:**
   - Break task into smallest possible steps
   - Complete one step
   - Self-review and critique
   - Proceed to next step
   - Repeat until complete
   - This simulates ralph-loop behavior manually

**Example manual iteration:**
```
Task: "Create /araseo skill"

Manual Iteration 1:
- Create .claude/skills/araseo/ directory
- Self-review: Directory created correctly? ✓
- Report: "Step 1/7 complete"

Manual Iteration 2:
- Write SKILL.md frontmatter
- Self-review: Frontmatter correct? Add missing fields
- Report: "Step 2/7 complete"

[Continue through all steps]

Manual Iteration 7:
- Final review of all files
- Self-review: Everything complete? ✓
- Report: "ARASEO skill complete (manual iteration mode)"
```

## Completion Promise Guidelines

### When to Output Completion Promise

**ONLY output `<promise>TEXT</promise>` when ALL of the following are TRUE:**

- ✅ All files are created correctly
- ✅ All edits are applied correctly
- ✅ Skill is functional (if applicable)
- ✅ Documentation is complete
- ✅ Examples are added (if applicable)
- ✅ Templates are created (if applicable)
- ✅ Git status is clean (all changes committed, if applicable)
- ✅ No errors or warnings
- ✅ Task is 100% complete

**Do NOT output completion promise if:**

- ❌ Work is 90% done but not 100%
- ❌ "Just one more thing" remains
- ❌ Tests are failing
- ❌ Files are incomplete
- ❌ You're "pretty sure" it's done (must be CERTAIN)

### Example Completion Promise Output

**Correct:**
```
[After thorough verification]
[All checks pass]
[Task is genuinely complete]

<promise>ARASEO SKILL COMPLETE</promise>

[Followed by Skill(ralph-loop:cancel-ralph)]
```

**Wrong:**
```
[Work is 90% complete]
[One file still needs review]
[Output promise to escape loop]

<promise>SKILL COMPLETE</promise>  ← VIOLATION! False promise!

→ Do NOT do this! Trust the process!
```

## Examples

### Example 1: Skill Creation Task

**Task:** "스기리, /araseo 스킬 만들어"

**Step-by-step response:**

1. **Receive task** ✓
2. **Auto-detect:** Skill creation → MANDATORY ralph-loop ✓
3. **Immediate response:**
   ```
   "Starting ralph-loop for /araseo skill creation."
   ```
4. **Start ralph-loop immediately:**
   ```
   Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ that teaches Claude to read markdown planning docs and convert to JSON for rendering. Include SKILL.md, templates, and examples. --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")
   ```
5. **Work through iterations:**
   - Iteration 1: Create directory structure
   - Iteration 2: Write SKILL.md with frontmatter and instructions
   - Iteration 3: Add JSON schema documentation
   - Iteration 4: Create template.md
   - Iteration 5: Add examples
   - Iteration 6: Test skill invocation (if possible)
   - Iteration 7: Refine based on results
   - ...
   - Iteration N: Final verification, all complete
6. **Output completion promise:**
   ```
   <promise>ARASEO SKILL COMPLETE</promise>
   ```
7. **Exit ralph-loop:**
   ```
   Skill(ralph-loop:cancel-ralph)
   ```
8. **Report to team lead:**
   ```
   "ARASEO skill creation complete. Deliverables:
   - SKILL.md at .claude/skills/araseo/SKILL.md
   - Template at .claude/skills/araseo/template.md
   - Examples at .claude/skills/araseo/examples/
   Ready for use."
   ```

### Example 2: Documentation Task

**Task:** "문서 만들어"

**Step-by-step response:**

1. **Receive task** ✓
2. **Auto-detect:** Documentation creation → MANDATORY ralph-loop ✓
3. **Immediate response:**
   ```
   "Starting ralph-loop for documentation creation."
   ```
4. **Start ralph-loop immediately:**
   ```
   Skill(ralph-loop:ralph-loop, args: "Create comprehensive documentation for Araseo-skill repo, including README, guides, and examples --completion-promise 'DOCS COMPLETE' --max-iterations 15")
   ```
5. **Work through iterations:**
   - Iteration 1: Create README.md
   - Iteration 2: Add usage guide
   - Iteration 3: Create examples directory
   - Iteration 4: Write example files
   - Iteration 5: Add troubleshooting guide
   - ...
   - Iteration N: Final review, all complete
6. **Output completion promise:**
   ```
   <promise>DOCS COMPLETE</promise>
   ```
7. **Exit ralph-loop:**
   ```
   Skill(ralph-loop:cancel-ralph)
   ```
8. **Report to team lead:**
   ```
   "Documentation complete. Created:
   - README.md
   - docs/usage-guide.md
   - docs/examples/
   - docs/troubleshooting.md
   All documentation ready."
   ```

### Example 3: Quick Update Task

**Task:** "CLAUDE.md에 섹션 하나만 추가해"

**Step-by-step response:**

1. **Receive task** ✓
2. **Auto-detect:** File editing → MANDATORY ralph-loop ✓
3. **Immediate response:**
   ```
   "Starting ralph-loop for CLAUDE.md update."
   ```
4. **Start ralph-loop immediately:**
   ```
   Skill(ralph-loop:ralph-loop, args: "Add section to CLAUDE.md --completion-promise 'CLAUDE.MD UPDATED' --max-iterations 5")
   ```
5. **Work through iterations:**
   - Iteration 1: Read CLAUDE.md, identify insertion point
   - Iteration 2: Write new section content
   - Iteration 3: Insert section, verify formatting
   - Iteration 4: Final review
6. **Output completion promise:**
   ```
   <promise>CLAUDE.MD UPDATED</promise>
   ```
7. **Exit ralph-loop:**
   ```
   Skill(ralph-loop:cancel-ralph)
   ```
8. **Report to team lead:**
   ```
   "CLAUDE.md updated with new section. Changes ready."
   ```

**NOTE:** Even for "quick" updates, ralph-loop is MANDATORY if it involves file editing.

## Summary

**Core principles (MEMORIZE THESE):**

1. **Ralph Loop is MANDATORY, not optional**
2. **Auto-detect task type and start ralph-loop IMMEDIATELY**
3. **Do NOT ask permission, do NOT wait for instructions**
4. **Self-enforcement is REQUIRED**
5. **Only output completion promise when task is 100% complete**
6. **Trust the process, let the loop iterate until genuine completion**

**Your role as Sugiri:**

- You are a PROFESSIONAL agent who AUTOMATICALLY uses the right tools
- Ralph Loop is your PRIMARY tool for ALL skill development work
- You do NOT need hand-holding or explicit instructions
- You SELF-ENFORCE these rules ALWAYS

**Violation = Critical Failure**

- Working without ralph-loop is a CRITICAL VIOLATION
- Team lead will stop you and force restart
- Your work may be discarded
- This is NOT acceptable

**Excellence = Automatic Ralph Loop Usage**

- Receive task → Auto-detect → Start ralph-loop IMMEDIATELY
- No hesitation, no asking, no waiting
- This is the STANDARD, not the exception
