# E2E Test Scenario for /araseo Skill

This document provides a complete end-to-end test scenario to verify the /araseo skill is production-ready.

## Test Scenario 1: Flowchart Conversion

### Step 1: Prepare Test Input

**Test Planning Document** (save as `test-login-flow.md`):

```markdown
# 로그인 프로세스 플로우

사용자가 앱에 로그인하는 전체 과정

## 플로우 단계

1. **시작** - 사용자가 앱 실행
2. **로그인 화면** - 이메일/비밀번호 입력
3. **인증 확인** - 서버에 인증 요청
4. **인증 성공 여부 확인** - 결과 판단
   - 성공 → 메인 화면으로 이동
   - 실패 → 에러 메시지 표시 → 다시 로그인 화면
5. **메인 화면** - 대시보드 표시
6. **종료**

## 흐름도

```
시작 → 로그인 화면 → 인증 확인 → 인증 성공?
                                    ├─ 성공 → 메인 화면 → 종료
                                    └─ 실패 → 에러 메시지 ┐
                                             ↑           │
                                             └───────────┘
```
```

### Step 2: Invoke /araseo Skill

**Command:**
```
/araseo test-login-flow.md
```

**Or with inline content:**
```
/araseo "# 로그인 프로세스 플로우\n\n시작 → 로그인 화면 → 인증 확인 → ..."
```

**With explicit filename:**
```
User: "이 기획서를 flowchart로 변환하고 e2e-test-flow.json으로 저장해줘"
/araseo test-login-flow.md
```

### Step 3: Expected Behavior

**The skill should:**

1. **Read** the planning document (or use inline content)
2. **Detect** type as "flowchart" (keywords: 플로우, 흐름도, 단계)
3. **Parse** structure:
   - Identify nodes: 시작, 로그인 화면, 인증 확인, 인증 성공?, 메인 화면, 에러 메시지, 종료
   - Identify edges: arrows and flow connections
   - Determine shapes: 시작/종료 → oval, 인증 성공? → diamond, others → rectangle
4. **Generate** valid JSON following JSON Graph Format (JGF)
5. **Determine filename**:
   - If user specified: use it (e.g., `e2e-test-flow.json`)
   - Otherwise auto-generate: `login-process-flow.json`
6. **Write** to: `/Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/<filename>.json`
7. **Confirm** with user

### Step 4: Expected Output

**File:** `/Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/login-process-flow.json`

**Content:**
```json
{
  "graph": {
    "directed": true,
    "label": "로그인 프로세스 플로우",
    "type": "flowchart",
    "metadata": {
      "description": "사용자가 앱에 로그인하는 전체 과정",
      "created_by": "araseo"
    },
    "nodes": {
      "start": {
        "label": "시작",
        "metadata": {
          "shape": "oval",
          "color": "#4CAF50",
          "description": "사용자가 앱 실행"
        }
      },
      "login_screen": {
        "label": "로그인 화면",
        "metadata": {
          "shape": "rectangle",
          "color": "#2196F3",
          "description": "이메일/비밀번호 입력"
        }
      },
      "auth_check": {
        "label": "인증 확인",
        "metadata": {
          "shape": "rectangle",
          "color": "#2196F3",
          "description": "서버에 인증 요청"
        }
      },
      "auth_decision": {
        "label": "인증 성공?",
        "metadata": {
          "shape": "diamond",
          "color": "#FF9800",
          "description": "결과 판단"
        }
      },
      "main_screen": {
        "label": "메인 화면",
        "metadata": {
          "shape": "rectangle",
          "color": "#2196F3",
          "description": "대시보드 표시"
        }
      },
      "error_message": {
        "label": "에러 메시지",
        "metadata": {
          "shape": "rectangle",
          "color": "#2196F3",
          "description": "로그인 실패 메시지"
        }
      },
      "end": {
        "label": "종료",
        "metadata": {
          "shape": "oval",
          "color": "#4CAF50"
        }
      }
    },
    "edges": [
      {
        "source": "start",
        "target": "login_screen",
        "relation": "flows_to",
        "metadata": { "style": "solid" }
      },
      {
        "source": "login_screen",
        "target": "auth_check",
        "relation": "flows_to",
        "metadata": { "style": "solid" }
      },
      {
        "source": "auth_check",
        "target": "auth_decision",
        "relation": "flows_to",
        "metadata": { "style": "solid" }
      },
      {
        "source": "auth_decision",
        "target": "main_screen",
        "relation": "flows_to",
        "metadata": { "label": "성공", "style": "solid" }
      },
      {
        "source": "auth_decision",
        "target": "error_message",
        "relation": "flows_to",
        "metadata": { "label": "실패", "style": "solid" }
      },
      {
        "source": "main_screen",
        "target": "end",
        "relation": "flows_to",
        "metadata": { "style": "solid" }
      },
      {
        "source": "error_message",
        "target": "login_screen",
        "relation": "flows_to",
        "metadata": { "label": "다시 시도", "style": "dashed" }
      }
    ]
  }
}
```

### Step 5: Verify Renderer Integration

**Manual verification:**

1. Check file exists:
   ```bash
   ls -la /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/login-process-flow.json
   ```

2. Validate JSON syntax:
   ```bash
   cat /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/login-process-flow.json | jq .
   ```

3. Start renderer (if not running):
   ```bash
   cd /Users/wogus/Wogus/Araseo/Araseo-renderer
   npm run dev
   ```

4. Open browser: `http://localhost:5173`

5. Verify flowchart renders with:
   - 7 nodes (start, login_screen, auth_check, auth_decision, main_screen, error_message, end)
   - 7 edges (connections between nodes)
   - Correct shapes (ovals for start/end, diamond for decision, rectangles for processes)
   - Korean text displays correctly
   - Interactive features work (pan, zoom, drag)

### Step 6: Success Criteria

✅ **PASS if:**
- JSON file created at correct path
- JSON syntax is valid
- Schema matches JGF specification
- All required fields present
- Renderer loads and displays flowchart
- Korean text renders correctly
- Interactive features work

❌ **FAIL if:**
- File not created or wrong path
- JSON syntax errors
- Missing required fields
- Renderer fails to load
- Korean text garbled
- Flowchart doesn't render

---

## Test Scenario 2: UI Mockup Conversion

### Step 1: Prepare Test Input

**Test Planning Document** (save as `test-signup-page.md`):

```markdown
# 회원가입 페이지 UI 목업

## 페이지 구조

### 헤더
- 로고
- 네비게이션: 홈, 로그인

### 메인 콘텐츠
- 제목: "회원가입"
- 입력 필드:
  - 이름 (필수)
  - 이메일 (필수)
  - 비밀번호 (필수)
  - 비밀번호 확인 (필수)
- 버튼: "가입하기" (primary)
- 링크: "이미 계정이 있으신가요? 로그인"

### 푸터
- 저작권: "© 2026 Araseo. All rights reserved."
```

### Step 2: Invoke /araseo Skill

**Command:**
```
/araseo test-signup-page.md
```

**Or with explicit filename:**
```
User: "이 UI 기획서를 mockup으로 변환하고 signup-mockup.json으로 저장해줘"
/araseo test-signup-page.md
```

### Step 3: Expected Behavior

**The skill should:**

1. **Read** the planning document
2. **Detect** type as "ui-mockup" (keywords: UI, 목업, 페이지, 구조)
3. **Parse** structure:
   - Identify sections: 헤더, 메인 콘텐츠, 푸터
   - Identify components: 로고, 네비게이션, 입력 필드, 버튼, 링크
   - Extract component properties: labels, types, required flags
4. **Generate** valid JSON following WireMD-style schema
5. **Determine filename**: `signup-mockup.json` or `signup-page.json`
6. **Write** to: `/Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/<filename>.json`
7. **Confirm** with user

### Step 4: Expected Output

**File:** `/Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/signup-page.json`

**Content:**
```json
{
  "mockup": {
    "version": "1.0",
    "title": "회원가입 페이지",
    "metadata": {
      "style": "wireframe",
      "theme": "clean",
      "device": "desktop"
    },
    "layout": {
      "type": "single-page",
      "sections": [
        {
          "id": "header",
          "type": "header",
          "components": [
            {
              "type": "navbar",
              "items": [
                { "type": "text", "label": "로고" },
                { "type": "link", "label": "홈", "href": "/" },
                { "type": "link", "label": "로그인", "href": "/login" }
              ]
            }
          ]
        },
        {
          "id": "main",
          "type": "main",
          "components": [
            {
              "type": "heading",
              "level": 1,
              "text": "회원가입"
            },
            {
              "type": "input",
              "inputType": "text",
              "label": "이름",
              "placeholder": "이름을 입력하세요",
              "required": true
            },
            {
              "type": "input",
              "inputType": "email",
              "label": "이메일",
              "placeholder": "이메일을 입력하세요",
              "required": true
            },
            {
              "type": "input",
              "inputType": "password",
              "label": "비밀번호",
              "placeholder": "비밀번호를 입력하세요",
              "required": true
            },
            {
              "type": "input",
              "inputType": "password",
              "label": "비밀번호 확인",
              "placeholder": "비밀번호를 다시 입력하세요",
              "required": true
            },
            {
              "type": "button",
              "variant": "primary",
              "label": "가입하기",
              "onClick": "handleSignup"
            },
            {
              "type": "paragraph",
              "text": "이미 계정이 있으신가요? 로그인"
            }
          ]
        },
        {
          "id": "footer",
          "type": "footer",
          "components": [
            {
              "type": "paragraph",
              "text": "© 2026 Araseo. All rights reserved."
            }
          ]
        }
      ]
    }
  }
}
```

### Step 5: Verify Renderer Integration

**Manual verification:**

1. Check file exists:
   ```bash
   ls -la /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/signup-page.json
   ```

2. Validate JSON syntax:
   ```bash
   cat /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/signup-page.json | jq .
   ```

3. Renderer should be running at: `http://localhost:5173`

4. Verify UI mockup renders with:
   - 3 sections (header, main, footer)
   - Header with navbar
   - Main with heading, 4 input fields, button, paragraph
   - Footer with copyright text
   - Korean text displays correctly
   - Wireframe/sketch styling

### Step 6: Success Criteria

✅ **PASS if:**
- JSON file created at correct path
- JSON syntax is valid
- Schema matches WireMD-style specification
- All required fields present
- Renderer loads and displays UI mockup
- Korean text renders correctly
- Wireframe styling applied

❌ **FAIL if:**
- File not created or wrong path
- JSON syntax errors
- Missing required fields
- Renderer fails to load
- Korean text garbled
- UI mockup doesn't render

---

## Test Scenario 3: Auto-Reload Integration

### Step 1: Start Renderer Dev Server

```bash
cd /Users/wogus/Wogus/Araseo/Araseo-renderer
npm run dev
```

Browser opens at: `http://localhost:5173`

### Step 2: Convert First Document

```
/araseo "# Test Flow 1\n\nA → B → C"
```

Saves to: `test-flow-1.json`

### Step 3: Verify Initial Render

Browser should show flowchart with 3 nodes (A, B, C).

### Step 4: Convert Second Document (Same Session)

```
/araseo "# Test Flow 2\n\nX → Y → Z"
```

Saves to: `test-flow-2.json`

### Step 5: Verify Auto-Reload

**Expected behavior:**
- Vite HMR detects new file
- Browser automatically reloads WITHOUT manual refresh
- Flowchart updates to show X, Y, Z nodes

### Step 6: Success Criteria

✅ **PASS if:**
- Both JSON files created
- Browser auto-reloads on file change
- No manual refresh needed
- Renderer updates instantly

❌ **FAIL if:**
- Manual refresh required
- HMR doesn't trigger
- Renderer doesn't update

---

## Automated Testing Script (Future Enhancement)

```bash
#!/bin/bash
# e2e-test-araseo.sh

set -e

echo "=== E2E Test for /araseo Skill ==="

# Test 1: Flowchart
echo "Test 1: Flowchart conversion..."
# TODO: Invoke skill programmatically
# Verify output exists
test -f /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/login-process-flow.json
echo "✓ Flowchart JSON created"

# Validate JSON syntax
cat /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/login-process-flow.json | jq . > /dev/null
echo "✓ JSON syntax valid"

# Test 2: UI Mockup
echo "Test 2: UI mockup conversion..."
# TODO: Invoke skill programmatically
test -f /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/signup-page.json
echo "✓ UI mockup JSON created"

# Validate JSON syntax
cat /Users/wogus/Wogus/Araseo/Araseo-renderer/public/examples/signup-page.json | jq . > /dev/null
echo "✓ JSON syntax valid"

echo "=== All E2E tests passed! ==="
```

---

## Completion Checklist

Before considering the /araseo skill "production-ready", verify:

- ✅ Skill invocable with `/araseo` command
- ✅ Handles both file paths and inline content
- ✅ Auto-detects flowchart vs UI mockup type
- ✅ Generates valid JSON following correct schemas
- ✅ Saves to renderer's `public/examples/` directory
- ✅ Supports user-provided filenames
- ✅ Auto-generates filenames from titles
- ✅ Renderer integration works (auto-reload)
- ✅ Korean text renders correctly
- ✅ All test scenarios pass
- ✅ Documentation complete (SKILL.md, E2E_TEST.md)

**When all checkboxes are ticked → Skill is production-ready!**
