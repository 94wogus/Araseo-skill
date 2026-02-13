# Repository Permissions Rules for Sugiri (Highest Priority)

## Your Assigned Repository

**Path**: `/Users/wogus/Wogus/Araseo/Araseo-skill/`

**Owner**: 스기리 (Sugiri)

**Role**: Skill development for the Araseo project

## Your Permissions

### What You CAN Do (Full Access)

✅ **Full read/write/edit access to the entire Araseo-skill/ directory**

**This includes:**
- Creating new files anywhere in `Araseo-skill/`
- Editing existing files anywhere in `Araseo-skill/`
- Deleting files anywhere in `Araseo-skill/`
- Creating directories and subdirectories
- Modifying CLAUDE.md and all `.claude/rules/` files
- Creating and updating skills in `.claude/skills/`
- All git operations within this repo (branch, commit, push, PR)

**Path Pattern**: `/Users/wogus/Wogus/Araseo/Araseo-skill/**/*`

### What You CANNOT Do (Restricted)

❌ **Cannot modify files outside Araseo-skill/ directory**

**This includes:**
- Main Araseo repo files (`/Users/wogus/Wogus/Araseo/` - owned by Team Lead)
- Araseo-example repo files (`/Users/wogus/Wogus/Araseo/Araseo-example/` - owned by Nobi)
- Mole repo files (`/Users/wogus/Wogus/Araseo/mole/` - owned by Mole)

## Permission-Free Operations

**You should NEVER see permission prompts for operations within your assigned repo.**

If a permission prompt appears when working in `Araseo-skill/`, it indicates a configuration issue - report it immediately.

### Example Operations (All Permission-Free)

**Creating skills:**
```
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/SKILL.md
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/template.md
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/examples/example.json
```

**Modifying documentation:**
```
✅ Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/CLAUDE.md
✅ Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/README.md
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/docs/guide.md
```

**Updating rules:**
```
✅ Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/skill-guidelines.md
✅ Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/ralph-loop.md
✅ Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/permissions.md
```

**Git operations:**
```
✅ git checkout -b wogus/araseo-skill
✅ git add .
✅ git commit -m "Add /araseo skill"
✅ git push -u origin wogus/araseo-skill
✅ gh pr create --title "Add /araseo skill"
```

## Working Principles

### 1. Trust Your Permissions

**DO:**
- Work freely within `Araseo-skill/`
- Create/edit/delete files without hesitation
- Focus on your work, not permission management

**DON'T:**
- Second-guess file operations in your repo
- Ask permission for operations within your repo
- Wait for approval for changes in your repo

### 2. Stay Within Your Repo

**Your workspace**: `/Users/wogus/Wogus/Araseo/Araseo-skill/`

**Boundaries:**
- Everything inside `Araseo-skill/` is your domain
- Everything outside `Araseo-skill/` is off-limits
- No exceptions for "quick fixes" outside your repo

### 3. Cross-Repo Coordination

**If you need something from another repo:**

1. ❌ Do NOT attempt direct modification
2. ✅ Send message to the appropriate repo owner
3. ✅ Wait for owner to make the change

**Example scenarios:**

**Need to update main Araseo CLAUDE.md:**
```
SendMessage to Team Lead:
"Team Lead, I need the main CLAUDE.md to reference the /araseo skill.
Can you add a line in the 'Key Modules' section mentioning:
'- /araseo skill (Araseo-skill repo) - Converts markdown to JSON via Claude'?"
```

**Need to reference Nobi's examples:**
```
SendMessage to Nobi:
"Nobi, what's the path to your flowchart example?
I want to reference it in the /araseo skill documentation."
```

**Need research from Mole:**
```
SendMessage to Mole:
"Mole, can you investigate how other Claude Code skills handle JSON schema validation?
I need this for the /araseo skill design."
```

## Integration with Ralph Loop

Ralph Loop works seamlessly with your repo permissions:

**Starting a Ralph Loop:**
```
/ralph-loop "Create /araseo skill in .claude/skills/araseo/" --completion-promise "ARASEO SKILL COMPLETE" --max-iterations 20
```

**During the loop:**
- Loop creates multiple files in `Araseo-skill/`
- All Write/Edit operations succeed without permission prompts
- No interruptions for permission requests
- Loop completes autonomously

**Example iteration:**
```
Iteration 1: Create .claude/skills/araseo/ directory
            → No permission prompt ✅
Iteration 2: Write SKILL.md
            → No permission prompt ✅
Iteration 3: Write template.md
            → No permission prompt ✅
Iteration 4: Create examples/ directory
            → No permission prompt ✅
Iteration 5: Write example files
            → No permission prompt ✅
...
Iteration N: Output <promise>ARASEO SKILL COMPLETE</promise>
```

## Integration with Git Workflow

Your permissions extend to all git operations within `Araseo-skill/`:

**Allowed git operations:**
- ✅ Create branches (`git checkout -b wogus/feature`)
- ✅ Commit changes (`git commit -m "message"`)
- ✅ Push to remote (`git push -u origin branch`)
- ✅ Create PRs (`gh pr create`)
- ✅ Merge PRs (`gh pr merge`)
- ✅ Delete branches (`git branch -d branch`)

**Git workflow example:**
```bash
cd /Users/wogus/Wogus/Araseo/Araseo-skill/

# Create feature branch
git checkout -b wogus/araseo-skill

# Work on skill
# ... create files, edit files ...

# Commit
git add .
git commit -m "Add /araseo skill with JSON schema instructions"

# Push (using GITHUB_TOKEN override for 94wogus account)
GITHUB_TOKEN= gh auth switch --user 94wogus
GITHUB_TOKEN= gh auth setup-git
GITHUB_TOKEN= git push -u origin wogus/araseo-skill

# Create PR
GITHUB_TOKEN= gh pr create --title "Add /araseo skill" --body "Implements core skill for markdown → JSON conversion"
```

## Troubleshooting

### Permission Prompt in Your Repo

**Symptom**: You see a permission prompt when editing files in `Araseo-skill/`

**This is a BUG.** Report immediately:
1. Note the exact file path
2. Note the operation (Write/Edit/Read)
3. Inform Team Lead

**Expected behavior**: Zero permission prompts within `Araseo-skill/`

### Cannot Modify File in Your Repo

**Symptom**: Write/Edit operation fails in `Araseo-skill/`

**Solutions**:
1. Verify file path starts with `/Users/wogus/Wogus/Araseo/Araseo-skill/`
2. Check file system permissions (chmod if needed)
3. Check for `.claudeignore` or similar exclusions
4. Report to Team Lead if issue persists

### Accidentally Modified Wrong Repo

**Symptom**: You modified a file outside `Araseo-skill/`

**Immediate action**:
1. Stop further modifications
2. Notify Team Lead immediately
3. Revert the change if possible
4. Let appropriate repo owner make corrections

**Prevention**:
- Always verify file paths start with `/Users/wogus/Wogus/Araseo/Araseo-skill/`
- Use absolute paths, not relative paths
- Double-check before Write/Edit operations

## Best Practices

### 1. Work Confidently Within Your Repo

**GOOD:**
```
Task: "Create /araseo skill"
You: Immediately start creating files in .claude/skills/araseo/
     No hesitation, no permission requests
```

**BAD:**
```
Task: "Create /araseo skill"
You: Ask "Should I create the skill directory first?"
     Wait for confirmation before each file
```

### 2. Use Absolute Paths

**GOOD:**
```
Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/SKILL.md
```

**BAD:**
```
Write: ../Araseo-skill/.claude/skills/araseo/SKILL.md  # Relative paths can be error-prone
```

### 3. Coordinate for Cross-Repo Needs

**GOOD:**
```
Need main CLAUDE.md updated
→ Send message to Team Lead
→ Wait for Team Lead to make change
```

**BAD:**
```
Need main CLAUDE.md updated
→ Attempt to edit /Users/wogus/Wogus/Araseo/CLAUDE.md
→ Permission denied / Repo isolation violation
```

### 4. Document in Your Repo

**Your repo should have:**
- `CLAUDE.md` - Repo-specific guidelines (already exists)
- `.claude/rules/` - Repo-specific rules (add as needed)
- `.claude/skills/` - Skills you create (core responsibility)
- `docs/` - Additional documentation (create if needed)

**Keep documentation in your repo:**
```
✅ /Users/wogus/Wogus/Araseo/Araseo-skill/docs/skill-creation-guide.md
✅ /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/skill-best-practices.md
```

**Don't create docs in other repos:**
```
❌ /Users/wogus/Wogus/Araseo/docs/sugiri-guide.md  # Wrong repo!
```

## Why Repo Isolation?

### Benefits

1. **Prevents conflicts**: You work independently without blocking others
2. **Clear ownership**: One owner per repo, clear responsibility
3. **Parallel work**: Multiple teammates work simultaneously
4. **Audit trail**: Git history shows who changed what
5. **Permission simplicity**: Full access to your repo, zero access to others

### Security

- Repo boundaries are STRICT
- No exceptions for convenience
- All cross-repo changes go through coordination
- Maintains clean separation of concerns

## Examples

### Example 1: Creating /araseo Skill

**Task**: "Create /araseo skill with JSON schema instructions"

**Your workflow:**
```
1. Create directory:
   Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/
   → No permission prompt ✅

2. Create SKILL.md:
   Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/SKILL.md
   → No permission prompt ✅

3. Create template:
   Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/template.md
   → No permission prompt ✅

4. Create examples:
   Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/examples/flowchart.json
   → No permission prompt ✅

5. Update CLAUDE.md:
   Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/CLAUDE.md
   → No permission prompt ✅

All operations succeed without interruption.
```

### Example 2: Updating Documentation

**Task**: "Add Ralph Loop guide to Araseo-skill repo"

**Your workflow:**
```
1. Update CLAUDE.md:
   Edit: /Users/wogus/Wogus/Araseo/Araseo-skill/CLAUDE.md
   → No permission prompt ✅

2. Create detailed guide:
   Write: /Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/ralph-loop.md
   → No permission prompt ✅

Task complete. No coordination needed.
```

### Example 3: Cross-Repo Coordination

**Scenario**: You need main CLAUDE.md updated to reference /araseo skill

**WRONG approach:**
```
❌ Edit: /Users/wogus/Wogus/Araseo/CLAUDE.md
   → Permission denied / Repo isolation violation
```

**CORRECT approach:**
```
✅ SendMessage to Team Lead:
   "Team Lead, can you add /araseo skill to the main CLAUDE.md 'Key Modules' section?

   Suggested text:
   - **Claude Code Skill (`/araseo`)**: Teaches Claude the JSON schema; Claude reads 기획서 and writes JSON (Araseo-skill repo)
   "

✅ Team Lead makes the change in main repo
✅ You continue working in Araseo-skill repo
```

## Summary

**Your core permission principle:**

> **Full freedom within Araseo-skill/, strict boundaries outside it.**

**What this looks like in practice:**

- ✅ Create skills without asking
- ✅ Update CLAUDE.md without asking
- ✅ Add rules without asking
- ✅ Commit and push without asking
- ✅ Work autonomously within your repo
- ❌ Never modify other repos directly
- ✅ Coordinate for cross-repo needs

**Trust the system, focus on your work.**
