# /araseo Quick Start Guide (Production-Ready)

**TL;DR**: `/araseo <markdown-file>` converts planning documents to JSON and auto-displays in renderer.

## What's New in Production Version

✨ **Renderer Integration**: JSON saves directly to renderer's directory for instant visualization
✨ **Auto-Reload**: Vite HMR detects changes and updates browser automatically
✨ **Smart Filenames**: User-provided names or auto-generated from titles
✨ **E2E Tested**: Complete end-to-end test scenarios verified

## Prerequisites

**To see visualizations:**
1. Start renderer dev server:
   ```bash
   cd /Users/wogus/Wogus/Araseo/Araseo-renderer
   npm run dev
   ```
2. Open browser: `http://localhost:5173`
3. Run `/araseo` skill
4. Watch visualization appear instantly!

## 30-Second Tutorial

### Flowchart Example

**1. Create a planning doc:**
```markdown
# 로그인 플로우

시작 → 로그인 입력 → 인증 → (성공/실패)
- 성공 → 대시보드
- 실패 → 에러
```

**2. Run the skill:**
```bash
/araseo login-flow.md
```

**3. Get JSON output:**
```json
{
  "graph": {
    "nodes": { "start": {...}, "login": {...}, ... },
    "edges": [ {"source": "start", "target": "login"}, ... ]
  }
}
```

### UI Mockup Example

**1. Create a planning doc:**
```markdown
# 로그인 페이지

Header: 로고 | 메뉴
Main: 제목, 이메일 입력, 비밀번호 입력, 로그인 버튼
Footer: © 2026
```

**2. Run the skill:**
```bash
/araseo login-page.md
```

**3. Get JSON output:**
```json
{
  "mockup": {
    "layout": {
      "sections": [
        {"type": "header", "components": [...]},
        {"type": "main", "components": [...]},
        {"type": "footer", "components": [...]}
      ]
    }
  }
}
```

## Common Patterns

### Decision Flow
```
A → Decision?
  ├─ Yes → B
  └─ No → C
```
→ Creates diamond node with labeled edges

### Form Structure
```
Form:
- Label + Input (email)
- Label + Input (password)
- Submit Button
```
→ Creates form component with fields array

### Circular Flow
```
Start → Process → Check → (fail) → Back to Start
```
→ Use dashed edge style for backward flows

## Tips

1. **Add keywords** for auto-detection:
   - Flowchart: "flow", "process", "workflow", arrows (→)
   - UI Mockup: "page", "UI", "mockup", "form", sections

2. **Use descriptive IDs**:
   - Good: `login_button`, `email_input`, `auth_check`
   - Bad: `btn1`, `input2`, `node3`

3. **Preserve Korean text**:
   - Keep all Korean labels as-is
   - Don't translate or romanize

4. **Check output**:
   - Output file: `/Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/<filename>.json`
   - Verify JSON is valid (no code fences)
   - Confirm schema matches template

5. **View in renderer** (if dev server running):
   - Open browser: `http://localhost:5173`
   - Renderer auto-reloads when JSON changes
   - See interactive visualization instantly

## Need More Examples?

See `examples/` directory:
- `planning-docs/` - Input markdown files
- `output/` - Expected JSON outputs

## Full Documentation

- **SKILL.md** - Complete instructions and schemas
- **README.md** - Installation, usage, troubleshooting
- **templates/** - JSON schema references
