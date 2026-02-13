# Ralph Loop Rules for Skill Development (Critical for Iterative Work)

## What is Ralph Loop?

Ralph Loop implements the **Ralph Wiggum technique** - an iterative development methodology based on continuous AI loops, pioneered by Geoffrey Huntley.

**Core concept for skill development**: The same prompt is fed to Claude repeatedly. Claude sees its own previous work in the files and git history, creating a self-referential loop where it iteratively improves until the skill is complete and working.

**Each iteration**:
1. Claude receives the SAME prompt
2. Works on the skill (creating/modifying SKILL.md, templates, examples)
3. Tests the skill if applicable
4. Tries to exit
5. Stop hook intercepts and feeds the same prompt again
6. Claude sees its previous work in the files and git history
7. Iteratively improves until completion

## When to Use Ralph Loop for Skill Development

**Good for:**
- Creating new Claude Code skills from scratch (e.g., `/araseo` skill)
- Refining skill instructions based on test results
- Writing comprehensive skill documentation
- Iterative improvements to skill templates and examples
- Multi-step skill implementations with dependencies

**Not good for:**
- Quick skill fixes or one-line changes
- Tasks requiring human judgment on UX/design
- Simple file operations
- Quick debugging or investigation

## Commands (Plugin-Provided via Skill Tool)

Ralph-loop is a **plugin** accessed via Skill tool. All users (including teammates) must use the Skill tool syntax.

### Start Ralph-loop

```python
Skill(ralph-loop:ralph-loop, args: "PROMPT --completion-promise 'TEXT' --max-iterations N")
```

**Usage examples:**
```python
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")

Skill(ralph-loop:ralph-loop, args: "스킬 템플릿 개선 --completion-promise 'TEMPLATE COMPLETE' --max-iterations 10")

Skill(ralph-loop:ralph-loop, args: "Add examples to /araseo skill --completion-promise 'EXAMPLES ADDED' --max-iterations 15")
```

**Options:**
- `--max-iterations <n>` - Max iterations before auto-stop (prevents infinite loops)
- `--completion-promise <text>` - Promise phrase to signal completion

**How it works for skills:**
1. Creates `.claude/.ralph-loop.local.md` state file
2. You work on the skill (create directories, write SKILL.md, add examples)
3. When you try to exit, stop hook intercepts
4. Same prompt fed back
5. You see your previous work in files
6. Continue refining until skill is complete
7. Output completion promise when done

### Cancel Ralph-loop

```python
Skill(ralph-loop:cancel-ralph)
```

**When to use:**
- Task is genuinely complete and you've output the completion promise
- User explicitly requests to stop
- You detect a blocker requiring user intervention

### Help

```python
Skill(ralph-loop:help)
```

Returns comprehensive ralph-loop documentation.

## Completion Promises for Skills

To signal skill completion, output a `<promise>` tag:

```
<promise>SKILL COMPLETE</promise>
```

**CRITICAL RULES:**
- ✅ Only output the promise when the skill is **completely functional and documented**
- ❌ Do NOT output false promises to escape the loop
- ✅ Trust the process - the loop helps you iterate until genuine completion
- ✅ Before promising completion, verify:
  - SKILL.md is complete with all required frontmatter
  - Templates are created (if needed)
  - Examples are added (if needed)
  - Skill follows Claude Code Skills spec
  - All files are properly formatted

## Skill Development Workflow with Ralph Loop

### Step 1: Start Ralph Loop for Skill Creation

**Example:**
```python
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ with JSON schema instructions --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")
```

### Step 2: Initial Iteration - Setup

**Iteration 1:**
- Create skill directory: `.claude/skills/araseo/`
- Create SKILL.md with frontmatter
- Write initial skill instructions

### Step 3: Subsequent Iterations - Refinement

**Iteration 2-N:**
- Read previous work
- Identify gaps or improvements
- Refine SKILL.md instructions
- Add templates if needed
- Create examples
- Test skill invocation (if possible)
- Review and improve based on results

### Step 4: Completion

**Final Iteration:**
- Verify skill is complete
- Check all files are properly formatted
- Confirm skill follows spec
- Output: `<promise>ARASEO SKILL COMPLETE</promise>`

### Step 5: Exit

- `Skill(ralph-loop:cancel-ralph)` to stop the loop
- Report completion to team lead

## Skill Development Best Practices

### 1. Always Set Max Iterations

**DO:**
```python
Skill(ralph-loop:ralph-loop, args: "스킬 만들기 --max-iterations 20")
```

**DON'T:**
```python
Skill(ralph-loop:ralph-loop, args: "스킬 만들기")  # Can run forever!
```

### 2. Use Clear Completion Promises

**GOOD:**
```
--completion-promise "ARASEO SKILL COMPLETE"
--completion-promise "SKILL DOCS COMPLETE"
--completion-promise "SKILL EXAMPLES ADDED"
```

**BAD:**
```
--completion-promise "done"  # Too vague
--completion-promise "maybe finished"  # Not definitive
```

### 3. Check Your Work Before Completion Promise

Before outputting `<promise>SKILL COMPLETE</promise>`:
- ✅ Verify SKILL.md exists and has all required frontmatter
- ✅ Check that skill instructions are clear and complete
- ✅ Confirm templates are created (if needed)
- ✅ Verify examples are added (if needed)
- ✅ Review git status to ensure all files are tracked
- ✅ Test skill invocation if possible

### 4. Iterate on Feedback

Each iteration:
- Read what you wrote in previous iterations
- Identify what's missing or unclear
- Improve incrementally
- Don't rush to completion

### 5. Don't Fight the Loop

If you're stuck:
- ❌ Don't output false promises to escape
- ✅ Analyze what's blocking completion
- ✅ Fix the blocker in the next iteration
- ✅ Trust that max-iterations will eventually stop if needed

## Example: Creating /araseo Skill with Ralph Loop

### Initial Task

**Team Lead assigns:**
```
"스기리, /araseo 스킬 만들어줘. JSON 스키마 변환 기능이 핵심이야."
```

### Start Ralph Loop

**Sugiri:**
```python
Skill(ralph-loop:ralph-loop, args: "Create /araseo skill in .claude/skills/araseo/ that teaches Claude to read markdown planning docs and write JSON schema for rendering --completion-promise 'ARASEO SKILL COMPLETE' --max-iterations 20")
```

### Iteration Breakdown

**Iteration 1: Setup**
- Create directory: `.claude/skills/araseo/`
- Create SKILL.md with frontmatter:
  ```yaml
  ---
  name: araseo
  description: Convert markdown planning docs to JSON for rendering
  argument-hint: "[markdown-file-path]"
  ---
  ```
- Write basic instructions in SKILL.md

**Iteration 2: Core Instructions**
- Add detailed JSON schema documentation
- Explain conversion rules (markdown → JSON)
- Add examples of expected input/output

**Iteration 3: Templates**
- Create `template.md` with JSON structure
- Add placeholder fields for Claude to fill

**Iteration 4: Examples**
- Create `examples/` directory
- Add sample markdown input
- Add corresponding JSON output

**Iteration 5: Refinement**
- Review all files
- Improve clarity of instructions
- Add edge case handling

**Iteration 6: Testing (if possible)**
- Try invoking the skill
- Check if Claude understands the instructions
- Refine based on results

**Iteration 7: Final Review**
- Verify all components are complete
- Check formatting
- Confirm skill follows spec

**Iteration 8: Completion**
- All checks pass
- Output: `<promise>ARASEO SKILL COMPLETE</promise>`

**After Loop:**
- `Skill(ralph-loop:cancel-ralph)`
- Report to team lead: "ARASEO skill creation complete at `.claude/skills/araseo/`"

## Integration with Git Workflow

Ralph Loop works seamlessly with git branch & PR workflow:

### Typical Flow

1. Start Ralph Loop for skill creation
2. **Iteration 1**: Create feature branch (`wogus/araseo-skill`)
3. **Iteration 2-N**: Work on skill, commit incrementally
4. **Iteration N**: Run final checks
5. **Iteration N+1**: Create PR
6. **Completion**: Output promise, `Skill(ralph-loop:cancel-ralph)`

### Git Commands Within Ralph Loop

Each iteration can:
- Create/switch branches
- Commit changes
- Push to remote
- Create PR (in final iteration)

**Example:**
```
Iteration 1: git checkout -b wogus/araseo-skill
Iteration 2: Create SKILL.md, git add, git commit
Iteration 3: Refine, git commit
...
Iteration N: Create PR, output promise
```

## Progress Reporting

During long Ralph Loops:
- Send intermediate messages to team lead with progress updates
- Example: "Iteration 5/20: SKILL.md complete, working on examples"
- Use TaskUpdate to mark milestones (if tasks are being tracked)

**Example:**
```
Iteration 3: SendMessage to team lead: "ARASEO SKILL.md drafted, adding templates next"
Iteration 6: SendMessage to team lead: "Templates and examples added, final review in progress"
```

## Troubleshooting

### Loop Running Too Long

**Symptom**: Ralph Loop exceeds expected iterations

**Solutions**:
- Check if completion criteria are too strict
- Verify you're making progress (git log)
- Check for blockers (missing context, unclear requirements)
- Trust max-iterations to stop eventually

### Can't Exit Loop

**Symptom**: Want to exit but loop keeps running

**Solution**:
- Do NOT output false promises
- Check if skill is genuinely complete
- If blocked, describe the blocker in output (team lead may see and help)
- Wait for max-iterations if set
- Team lead or user can `Skill(ralph-loop:cancel-ralph)` if needed

### Skill Not Working as Expected

**Symptom**: Skill created but doesn't work correctly

**Solution**:
- Start another Ralph Loop to fix it
- Use clear completion promise: "SKILL FIXED"
- Iterate until skill works as expected

## Learn More

- Original technique: https://ghuntley.com/ralph/
- Ralph Orchestrator: https://github.com/mikeyobrien/ralph-orchestrator
- Claude Code Skills spec: https://code.claude.com/docs/en/skills
