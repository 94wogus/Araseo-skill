# Araseo Skill

**`/araseo`** - Markdown to JSON converter for flowcharts and UI mockups

## Overview

The `/araseo` skill converts markdown planning documents (기획서) into structured JSON for rendering as interactive flowcharts or UI mockups. This is a core component of the Araseo project.

**Key insight**: There is NO separate parser module. Claude itself IS the parser — the skill teaches Claude the JSON schema, and Claude converts markdown to JSON.

## Installation

This skill is located in the Araseo-skill repository at:
```
/Users/wogus/Wogus/Araseo/Araseo-skill/.claude/skills/araseo/
```

## Usage

### Manual Invocation

```bash
/araseo <path-to-markdown-file>
```

**Example**:
```bash
/araseo planning-docs/user-flow.md
```

### Auto-Invocation

The skill automatically triggers when the user:
- Provides a 기획서 (planning document)
- Asks to visualize a plan
- Mentions flowcharts or mockups

## Directory Structure

```
araseo/
├── SKILL.md                          # Main skill instructions
├── README.md                         # This file
├── templates/
│   ├── flowchart-schema.json         # JSON Graph Format schema
│   └── ui-mockup-schema.json         # UI component schema
├── examples/
│   ├── planning-docs/
│   │   ├── simple-flowchart.md
│   │   ├── decision-flow.md
│   │   ├── complex-flowchart.md
│   │   ├── ui-mockup.md
│   │   └── ui-mockup-login.md
│   └── output/
│       ├── simple-flowchart.json
│       ├── decision-flow.json
│       ├── complex-flowchart.json
│       ├── ui-mockup.json
│       └── ui-mockup-login.json
└── scripts/
    └── (validation scripts - optional)
```

## Supported Conversion Types

### 1. Flowcharts

Converts process flows, decision trees, state diagrams, and workflows to **JSON Graph Format**.

**Schema**: Based on [JSON Graph Specification](https://jsongraphformat.info/)

**Node types**:
- Oval - Start/End points
- Rectangle - Process steps
- Diamond - Decision points
- Parallelogram - Input/Output
- Cylinder - Database operations

**Example**:
- Input: `examples/planning-docs/simple-flowchart.md`
- Output: `examples/output/simple-flowchart.json`

### 2. UI Mockups

Converts page structures, wireframes, and component layouts to **UI Mockup JSON**.

**Schema**: Inspired by [WireMD](https://wiremd.dev/)

**Section types**:
- Header
- Main
- Footer
- Sidebar
- Modal

**Component types**:
- Navigation (navbar, breadcrumb)
- Text (heading, paragraph)
- Input (input, textarea, select)
- Action (button, link)
- Layout (container, grid, flex)
- Media (image, video, icon)
- Data (table, list, card)

**Example**:
- Input: `examples/planning-docs/ui-mockup.md`
- Output: `examples/output/ui-mockup.json`

## Output Format

**CRITICAL**: The skill outputs raw JSON only, with NO markdown code fences.

❌ **Wrong** (wrapped in code fence):
````
```json
{ "graph": { ... } }
```
````

✅ **Correct** (raw JSON):
```
{ "graph": { ... } }
```

## Examples

### Example 1: Simple Login Flow

**Input** (`simple-flowchart.md`):
```markdown
# 간단한 로그인 플로우

시작 → 자격증명 입력 → 인증 확인 → (성공/실패)
- 성공 → 대시보드
- 실패 → 에러 메시지
```

**Output** (see `examples/output/simple-flowchart.json`):
- Graph with 6 nodes
- Start/End nodes use oval shape (green)
- Process nodes use rectangle shape (blue)
- Decision node uses diamond shape (orange)
- Edges labeled "성공" / "실패"

### Example 2: Document Approval Process

**Input** (`decision-flow.md`):
```markdown
# 문서 승인 프로세스

문서 제출 → 1차 검토 → 검토 결과?
  ├─ 승인 → 2차 승인 → 최종 승인?
  │   ├─ 승인 → 문서 게시
  │   └─ 반려 → 수정 요청
  └─ 반려 → 수정 요청
```

**Output** (see `examples/output/decision-flow.json`):
- Multi-level decision workflow
- Circular flow (수정 요청 → 문서 제출)
- Dashed edge style for return paths

### Example 3: Login Page Mockup

**Input** (`ui-mockup.md`):
```markdown
# 로그인 페이지

Header: 로고 | 로그인 | 도움말
Main: 제목, 이메일 입력, 비밀번호 입력, 로그인 버튼
Footer: © 2026 회사명
```

**Output** (see `examples/output/ui-mockup.json`):
- Three-section layout (header/main/footer)
- Navbar with logo and links
- Form with input fields and button
- Footer with copyright text

## How It Works

1. **User provides markdown planning document** (기획서)
2. **Skill invokes Claude** with the markdown content
3. **Claude analyzes** structure and determines type (flowchart vs UI mockup)
4. **Claude generates JSON** following the appropriate schema
5. **JSON is written to file** (`<input-filename>.json`)
6. **Renderer** (separate module) visualizes the JSON

## Integration with Araseo Pipeline

```
User conversation → Markdown 기획서 → /araseo skill → JSON → Renderer → Interactive Visual
```

**Key principle**: Claude IS the parser. No separate parsing module needed.

## Development

### Testing the Skill

1. Create a test planning document:
   ```bash
   echo "# Test Flow\nStart → Process → End" > test-flow.md
   ```

2. Invoke the skill:
   ```bash
   /araseo test-flow.md
   ```

3. Check output:
   ```bash
   cat test-flow.json
   ```

4. Verify JSON structure matches schema

### Adding New Examples

1. Create planning document in `examples/planning-docs/`
2. Run `/araseo examples/planning-docs/<new-doc>.md`
3. Move output to `examples/output/<new-doc>.json`
4. Verify quality and add to examples list in SKILL.md

### Modifying Schemas

**Flowchart schema** (`templates/flowchart-schema.json`):
- Based on JSON Graph Format standard
- Modify only if renderer requires new fields
- Coordinate with renderer team before changes

**UI mockup schema** (`templates/ui-mockup-schema.json`):
- Based on WireMD patterns
- Add new component types as needed
- Document in SKILL.md

## Troubleshooting

### Issue: JSON wrapped in code fences

**Symptom**: Output looks like ` ```json ... ``` `

**Solution**: The skill explicitly instructs Claude NOT to use code fences. If this happens:
1. Check SKILL.md "CRITICAL: Output Format Rules" section
2. Ensure it states "Do NOT wrap JSON in code blocks"
3. Refine the instruction to be more emphatic

### Issue: Schema mismatch

**Symptom**: JSON doesn't validate against schema

**Solution**:
1. Check templates/ for current schema
2. Verify SKILL.md matches schema
3. Run validation script (if available)
4. Update examples to match schema

### Issue: Wrong type detected

**Symptom**: Flowchart input produces UI mockup JSON (or vice versa)

**Solution**:
1. Review "Conversion Process → Step 2" in SKILL.md
2. Add clearer type indicators in markdown
3. Use explicit keywords ("flowchart", "mockup", "UI", "workflow")

## Best Practices

1. **Use descriptive node/component IDs** - `login_button` not `btn1`
2. **Add metadata for visual hints** - shapes, colors, positions
3. **Preserve Korean text as-is** - Don't translate labels
4. **Follow schema exactly** - Validate against templates
5. **Keep examples simple and clear** - Focus on patterns, not complexity

## Related Documentation

- **Main repo CLAUDE.md**: `/Users/wogus/Wogus/Araseo/CLAUDE.md`
- **Skill development rules**: `/Users/wogus/Wogus/Araseo/Araseo-skill/.claude/rules/`
- **Mole's research guide**: `/Users/wogus/Wogus/Araseo/mole/research-reports/araseo-skill-development-guide.md`

## Standards and References

- [JSON Graph Specification](https://github.com/jsongraph/json-graph-specification)
- [JSON Graph Format Info](https://jsongraphformat.info/)
- [WireMD](https://wiremd.dev/)
- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)

## Changelog

### 2026-02-13 - Initial Creation

- ✅ Created skill directory structure
- ✅ Implemented SKILL.md with flowchart and UI mockup support
- ✅ Added JSON Graph Format schema for flowcharts
- ✅ Added UI mockup schema (WireMD-inspired)
- ✅ Created 3 flowchart examples (simple, decision, complex)
- ✅ Created 2 UI mockup examples (basic, login page)
- ✅ Documented conversion process and best practices

**Developer**: 스기리 (Sugiri)
**Based on**: Mole's 910-line research guide

---

**Ready to convert markdown to JSON!** 🚀
