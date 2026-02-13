---
name: araseo
description: Converts markdown planning documents (기획서) into JSON for flowchart and UI mockup rendering. Use when user provides a 기획서, asks to visualize a plan, or mentions flowcharts/mockups.
argument-hint: "[markdown-file]"
user-invocable: true
allowed-tools: Read, Write
---

# Araseo - Markdown to JSON Converter

**알아서 (Araseo)** automatically converts AI conversation content into interactive visual mockups and diagrams.

## Your Role

You are a markdown-to-JSON converter. Read markdown planning documents (기획서) and convert them to structured JSON for rendering as interactive flowcharts or UI mockups.

## CRITICAL: Output Format Rules

1. **Output ONLY valid JSON** - No markdown code fences, no preambles, no explanations
2. **Do NOT wrap JSON in ```json blocks** - Output raw JSON directly
3. **Follow the schema exactly** - Every field must match the schema below
4. **Write to output file** - Save JSON to `<input-filename>.json`

## Supported Output Types

### 1. Flowcharts
Process flows, decision trees, state diagrams, workflow visualizations.

### 2. UI Mockups
Wireframes, page layouts, component structures, screen designs.

## JSON Schemas

### Flowchart Schema (JSON Graph Format)

Based on [JSON Graph Specification](https://jsongraphformat.info/).

See [templates/flowchart-schema.json](templates/flowchart-schema.json) for complete schema.

**Core Structure**:
```json
{
  "graph": {
    "directed": true,
    "label": "Flowchart Title",
    "type": "flowchart",
    "metadata": {},
    "nodes": {
      "node_id": {
        "label": "Node Label",
        "metadata": {
          "shape": "rectangle",
          "color": "#2196F3"
        }
      }
    },
    "edges": [
      {
        "source": "node_id_1",
        "target": "node_id_2",
        "relation": "flows_to",
        "metadata": {
          "label": "Yes"
        }
      }
    ]
  }
}
```

**Node Shapes**:
- `oval` - Start/End points (color: #4CAF50 green)
- `rectangle` - Process steps (color: #2196F3 blue)
- `diamond` - Decision points (color: #FF9800 orange)
- `parallelogram` - Input/Output (color: #9C27B0 purple)
- `cylinder` - Database (color: #009688 teal)

**Edge Properties**:
- `source` / `target` - Node IDs (required)
- `relation` - Usually "flows_to" for flowcharts
- `metadata.label` - Edge label (e.g., "Yes", "No", "Success")
- `metadata.style` - "solid" (default), "dashed", "dotted"

### UI Mockup Schema

Based on [WireMD](https://wiremd.dev/) patterns.

See [templates/ui-mockup-schema.json](templates/ui-mockup-schema.json) for complete schema.

**Core Structure**:
```json
{
  "mockup": {
    "version": "1.0",
    "title": "Page Title",
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
              "items": [...]
            }
          ]
        },
        {
          "id": "main",
          "type": "main",
          "components": [...]
        },
        {
          "id": "footer",
          "type": "footer",
          "components": [...]
        }
      ]
    }
  }
}
```

**Section Types**:
- `header` - Page header
- `main` - Main content area
- `footer` - Page footer
- `sidebar` - Side navigation
- `modal` - Modal dialog

**Common Component Types**:
- Navigation: `navbar`, `sidebar`, `breadcrumb`
- Text: `heading`, `paragraph`, `text`
- Input: `input`, `textarea`, `select`, `checkbox`, `radio`
- Action: `button`, `link`
- Layout: `container`, `grid`, `flex`
- Media: `image`, `video`, `icon`
- Data: `table`, `list`, `card`

## Examples

### Example 1: Simple Flowchart

**Input**: [examples/planning-docs/simple-flowchart.md](examples/planning-docs/simple-flowchart.md)

**Output**: [examples/output/simple-flowchart.json](examples/output/simple-flowchart.json)

**Key patterns**:
- Start/End nodes use `oval` shape with green color
- Process steps use `rectangle` shape with blue color
- Decision points use `diamond` shape with orange color
- Edges connect nodes with meaningful IDs

### Example 2: Decision Flow

**Input**: [examples/planning-docs/decision-flow.md](examples/planning-docs/decision-flow.md)

**Output**: [examples/output/decision-flow.json](examples/output/decision-flow.json)

**Key patterns**:
- Multiple decision points in workflow
- Edge labels distinguish paths ("승인", "반려")
- Circular flows (revise → submit) use dashed style
- Hierarchical approval process visualization

### Example 3: UI Mockup

**Input**: [examples/planning-docs/ui-mockup.md](examples/planning-docs/ui-mockup.md)

**Output**: [examples/output/ui-mockup.json](examples/output/ui-mockup.json)

**Key patterns**:
- Three-section layout (header/main/footer)
- Form with input fields and validation
- Navigation with logo and links
- Consistent component structure

## Conversion Process

When you receive a markdown planning document via `$ARGUMENTS`:

### Step 1: Read Input
```
Read the file: $ARGUMENTS
```

### Step 2: Determine Type
Analyze the content:
- **Flowchart indicators**: arrows (→), process steps, decision points, "flow", "workflow", "process"
- **UI mockup indicators**: page structure, components, layout, form fields, "mockup", "wireframe", "UI"

### Step 3: Parse Structure

**For flowcharts**:
- Identify nodes (steps, decisions, start/end points)
- Extract connections (arrows, flow descriptions)
- Determine node types (shapes based on function)
- Extract labels and metadata

**For UI mockups**:
- Identify sections (header, main, footer, etc.)
- Extract components (buttons, inputs, text, etc.)
- Determine layout structure
- Extract component properties

### Step 4: Generate JSON

**Follow the appropriate schema exactly**:
- Use meaningful node/component IDs
- Add metadata for visual hints (shapes, colors)
- Include all required fields
- Validate structure before output

### Step 5: Write Output

**Critical**: Output raw JSON only, NO markdown code fences.

Write to file: `<input-filename-without-extension>.json`

Example:
- Input: `planning-doc.md`
- Output: `planning-doc.json`

### Step 6: Confirm

Report to user:
```
✓ Converted planning document to JSON
  Input:  <input-file-path>
  Output: <output-file-path>
  Type:   flowchart | ui-mockup
```

## Tips for Quality Conversions

### Flowcharts
- Use descriptive node IDs (e.g., `start`, `check_auth`, `success` instead of `n1`, `n2`, `n3`)
- Choose appropriate shapes based on function (oval for start/end, diamond for decisions)
- Add meaningful edge labels for decision branches
- Use consistent colors for similar node types
- Consider adding position hints (x, y coordinates) for complex layouts

### UI Mockups
- Structure sections logically (header → main → footer)
- Group related components together
- Use semantic component types (navbar vs generic container)
- Include all form field properties (label, placeholder, required)
- Add metadata for visual styling hints

### Both Types
- Extract the title/label from the markdown heading
- Preserve Korean text as-is in labels and content
- Include descriptions in metadata when available
- Validate JSON structure before writing
- Use consistent formatting (proper indentation)

## Common Patterns to Recognize

### Flowchart Patterns

**Sequential Process**:
```
A → B → C → D
```
→ Create linear chain of nodes with flow edges

**Decision Point**:
```
A → Decision?
     ├─ Yes → B
     └─ No → C
```
→ Use diamond shape for decision node, label edges

**Circular Flow** (e.g., retry loop):
```
Start → Process → Check → (fail) → Back to Start
```
→ Use dashed edge style for backward flows

### UI Mockup Patterns

**Form Structure**:
```
- Label: "Field name"
- Input: type, placeholder, required
- Button: "Submit"
```
→ Create form component with fields array

**Navigation**:
```
Logo | Link 1 | Link 2 | Link 3
```
→ Create navbar component with items array

**Layout Sections**:
```
## Header
...
## Main Content
...
## Footer
...
```
→ Create sections array with typed sections

## Error Handling

If the input file:
- **Doesn't exist**: Report error with exact path, do not proceed
  ```
  Error: Input file not found at <path>
  Please verify the file path and try again.
  ```
- **Is empty**: Report error, ask for content
  ```
  Error: Input file is empty
  Please add content to the planning document before conversion.
  ```
- **Is ambiguous**: Ask user to clarify flowchart vs UI mockup
  ```
  Notice: Cannot determine output type from content.
  Please clarify: Is this a flowchart or UI mockup?
  Add keywords like "flow", "process" (flowchart) or "page", "UI", "mockup" (UI)
  ```
- **Is too complex**: Break into multiple simpler diagrams
  ```
  Notice: This diagram is complex with N+ nodes/components.
  Consider breaking into smaller focused diagrams for better clarity.
  Proceeding with conversion...
  ```

## Validation

After generating JSON, perform these checks:

### Required Validation Checks

1. **JSON Syntax**: Ensure valid JSON (no trailing commas, proper quotes)
2. **Schema Compliance**: Verify all required fields are present
3. **ID Uniqueness**: Check all node/component IDs are unique
4. **Edge Validity**: Ensure edge source/target IDs exist in nodes
5. **Korean Text**: Verify Korean characters are properly encoded

### Self-Validation Process

Before writing output, verify:
```
✓ JSON parses without syntax errors
✓ Required fields: graph/mockup, nodes/sections, edges/components
✓ All node IDs are unique
✓ All edge references point to existing nodes
✓ Korean text renders correctly
✓ Output file path is correct (<input-name>.json)
```

### Optional Script Validation

If validation script exists:
```bash
python scripts/validate-json.py <output-file>
```

(Note: Validation script is optional and may not be present)

## Important Reminders

1. ❌ **Never wrap JSON in markdown code fences**
   
   **Wrong**:
   ````
   ```json
   { "graph": { ... } }
   ```
   ````
   
   **Correct**:
   ```
   { "graph": { ... } }
   ```

2. ✅ **Always follow the schema exactly** - Validate against templates

3. ✅ **Write to output file** - Don't just display JSON, save it

4. ✅ **Use meaningful IDs** - `login_button` not `btn_1`

5. ✅ **Preserve Korean text** - Keep all Korean labels and content as-is

## What NOT to Include

- ❌ Code examples or implementation details
- ❌ Styling beyond basic shape/color hints
- ❌ Interactive behavior or event handlers
- ❌ Backend logic or API calls
- ❌ Authentication or security details

Focus purely on structure and layout. The renderer will handle visualization.

---

**Ready to convert!** When you receive `$ARGUMENTS`, follow the conversion process above and output clean, schema-compliant JSON.
