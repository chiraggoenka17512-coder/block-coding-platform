# Educational Block-Coding Platforms: Reverse-Engineering & Comparative Study

This report documents the live architectural, UX, and pedagogical reverse-engineering of five foundational visual programming environments: **Scratch**, **Google Blockly**, **Code.org**, **Microsoft MakeCode**, and **Snap!**.

---

## 📊 Comparative Architectural Matrix

| Dimension | Scratch (MIT) | Google Blockly | Code.org | Microsoft MakeCode | Snap! (UC Berkeley) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Primary Target** | Creative storytelling, games & media | Modular embeddable library for dev platforms | K-12 structured curriculum progression | Physical computing & text-transition | Computer science principles & functional abstractions |
| **Block Types** | Hat, Stack, Reporter (oval), Boolean (hexagon), Cap, C-Block | Statements (notches), Value expressions (plugs), Mutators | Puzzle-constrained stacks, event hats, loops | Event hats, Stacks, Reporters, Booleans | Command, Reporter, Predicate, Hat, Rings (Lambdas) |
| **Typing Enforcement** | Strict visual geometry prevents syntax errors | Typed socket connection rules (`Number`, `String`, `Boolean`) | Fixed puzzle constraints prevent invalid snaps | Static TypeScript type safety mapped to visual shapes | Geometric shape types + dynamic first-class types |
| **Text Code Bridge** | None (pure visual environment) | Multi-target compilation (Python, JS, Lua, Dart, PHP) | Read-only "Show Code" modal (JavaScript) | Full bidirectional round-trip (Blocks ↔ JS ↔ Python) | Smalltalk/JS reflection primitives |
| **Extensibility** | "My Blocks" (command procedures, no return values) | Custom JSON/JS definitions & code generators | Authoring environment for puzzle modules | Extensions gallery + custom TypeScript packages | Full meta-programming: custom reporters, predicates, and lambdas |
| **Error Feedback** | Silent fail-safe execution (no crashing) | Silent prevention (invalid connections impossible) | Encouraging, growth-mindset dialogs + hints | Monaco compiler errors + runtime red banners | Explicit error dialogs + stack traces |

---

## 1. Scratch (MIT)
* **Live Target**: `https://scratch.mit.edu/projects/editor/`
* **Evidence Screenshots**:
  * [Workspace Overview](screenshots/scratch_workspace_overview.png)
  * [Sensing Category (Reporters & Booleans)](screenshots/scratch_sensing_shapes.png)
  * [Operators (Arithmetic & Relational)](screenshots/scratch_operators.png)
  * [Control (C-Blocks & Cap Blocks)](screenshots/scratch_control_c_blocks.png)

### A. Block Shapes & Type System (Grammar of Blocks)
Scratch relies on self-enforcing physical metaphors:
1. **Stack Blocks (Commands)**: Rectangular with a top notch and bottom tab. They represent imperative actions (`move (10) steps`, `change y by (10)`).
2. **Hat Blocks (Events / Triggers)**: Smooth rounded tops with no notch, featuring a bottom tab. They define execution roots (`when green flag clicked`, `when key pressed`).
3. **Cap Blocks (Terminators)**: Top notch, flat bottom without a socket (`stop [all]`, `delete this clone`). They physically prevent attaching dead code.
4. **Reporter Blocks (Values / Expressions)**: Rounded oval pills. They yield numeric or string values (`x position`, `( ) + ( )`). Oval slots can accept typed text or dropped reporters.
5. **Boolean Blocks (Predicates)**: Elongated hexagons with 60° angled ends (`< >`). Return strictly `true` or `false`. They fit exclusively into hexagonal condition sockets (`if < > then`, `repeat until < >`).
6. **C-Blocks (Mouth / Container Blocks)**: Resemble a capital "C" with an internal upper notch and lower tab inside the interior mouth (`repeat (10)`, `if < > then`). The `forever` block is a hybrid C-block and Cap-block with a flat bottom.

### B. Input Field Behavior
* White rounded input pills allow inline typing without quotes.
* Dropping an oval reporter into a field displays a glowing white halo; on release, the reporter dynamically expands the host block.
* Contextual UI elements pop up on click (e.g., circular 360° clock dial for direction, visual color picker eyedropper).

### C. Stage, Coordinate Space, & Output
* **Stage Area**: Top-right quadrant (480 x 360 pixels).
* **Coordinate System**: Centered origin `(0, 0)`. X ranges `[-240, 240]`, Y ranges `[-180, 180]`.
* **Output Metaphor**: Speech bubbles dynamically anchored to sprite bounding boxes (`say [Hello!] for (2) secs`) rather than a scrolling terminal log.

---

## 2. Google Blockly
* **Live Target**: `https://blockly-demo.appspot.com/static/demos/code/index.html`
* **Evidence Screenshots**:
  * [Toolbox Categories](screenshots/blockly_categories.png)
  * [Python Code Generation](screenshots/blockly_python_generation.png)
  * [JavaScript Code Generation](screenshots/blockly_javascript_generation.png)
  * [XML AST Serialization](screenshots/blockly_xml_ast.png)

### A. Toolbox Structure & Mutators
* **Flyout Categories**: Standardized groups (`Logic`, `Loops`, `Math`, `Text`, `Lists`, `Variables`, `Functions`).
* **Connection Logic**:
  * Statement connections: Female top notch, male bottom tab (`previousStatement`, `nextStatement`).
  * Value connections: Left male tab (`output`) plugging into horizontal right female sockets (`input_value`).
* **Mutators (Structural Extension)**:
  * Complex blocks (like `controls_if`) feature a gear icon.
  * Clicking the gear opens a sub-workspace bubble where users drag `else if` and `else` clauses into the `if` trunk, dynamically mutating the host block's physical sockets and code generator template.

### B. Real-Time Code Generation Engine
* **Visitor Architecture**: The code generator traverses root blocks on the workspace, calling block-specific generator methods (`pythonGenerator.forBlock['controls_if']`).
* **Clean Indentation**: Generates clean, PEP8-compliant Python:
  ```python
  if False:
    pass
  ```
* **AST Serialization**: Workspaces serialize directly to standard XML or JSON trees, enabling cross-platform state persistence and exact block-to-code mapping.

---

## 3. Code.org
* **Live Target**: `https://studio.code.org/s/express-2025/lessons/1/levels/2`
* **Evidence Screenshots**:
  * [Curriculum Progression](screenshots/code_org_curriculum_progression.png)
  * [Puzzle Workspace Interface](screenshots/code_org_puzzle_interface.png)
  * [Error Feedback Dialog](screenshots/code_org_error_feedback.png)
  * [Scaffolded Lightbulb Hint](screenshots/code_org_scaffolded_hint.png)

### A. Beginner Scaffolding & Cognitive Load Management
* **Level Progression Bar**: Linear stage tracking with numbered circles (puzzles), diamonds (concept videos), and milestone flags. Green fills indicate completed puzzles.
* **Pre-Staged Workspace**: Workspaces load with partial block setups, focusing the learner's attention on completing the missing logic rather than hunting through menus.
* **Multimodal Instructions**: Concise goal cards with an integrated text-to-speech speaker button for pre-readers.

### B. Growth-Mindset Error Feedback
* **Non-Punitive Language**: Avoids red alerts, stack traces, or negative phrasing. When code fails, an encouraging popup appears:
  > *"Keep coding! Something's not quite right yet."*
* **Scaffolded Lightbulb Hints**: The hint button does not give away the answer immediately. It asks *"Do you want a hint?"*, fostering productive struggle before revealing visual diagrams.

---

## 4. Microsoft MakeCode
* **Live Target**: `https://makecode.microbit.org/`
* **Evidence Screenshots**:
  * [Blocks Mode](screenshots/makecode_blocks_mode.png)
  * [JavaScript Mode (Monaco)](screenshots/makecode_javascript_mode.png)
  * [Python Mode](screenshots/makecode_python_mode.png)
  * [Round-Trip Decompilation](screenshots/makecode_roundtrip_blocks.png)

### A. Dual Visual / Text Editing Paradigm
* **Top Central Switcher**: Prominent `[Blocks | JavaScript | Python]` segmented toggle button.
* **Monaco Integration**: The text view uses Microsoft Monaco (VS Code core), offering autocomplete, linting, and syntax highlighting.

### B. Bidirectional Round-Tripping (Decompilation)
* Translating `on start` and `forever` blocks generates TypeScript:
  ```typescript
  basic.forever(function () {
  })
  ```
* Switching to Python transpiles this into idiomatic Python:
  ```python
  def on_forever():
      pass
  basic.forever(on_forever)
  ```
* Switching back to Blocks parses the TypeScript AST and faithfully reconstructs the visual block layout.
* **Grey Blocks Safety Net**: Code with no direct block visual equivalent is preserved inside grey container blocks, preventing user code corruption during round trips.

---

## 5. Snap! (UC Berkeley)
* **Live Target**: `https://snap.berkeley.edu/snap/snap.html`
* **Evidence Screenshots**:
  * [Make a Block Dialog](screenshots/snap_make_block_shapes.png)
  * [Block Editor & Parameters](screenshots/snap_block_editor.png)
  * [Variadic Operators](screenshots/snap_operators_variadic.png)
  * [Higher-Order Function Rings](screenshots/snap_higher_order_rings.png)
  * [Control Lambdas (`run`, `call`, `report`)](screenshots/snap_control_lambdas.png)
  * [Data Pipeline (`pipe ->`)](screenshots/snap_call_pipeline.png)

### A. Block Extension ("Make a Block")
* Allows creating all four block archetypes:
  1. **Command** (puzzle notch stack).
  2. **Reporter** (oval expression returning a value).
  3. **Predicate** (hexagonal boolean returning true/false).
  4. **Hat** (event trigger).
* **Scope Control**: Offers *"for all sprites"* (global) or *"for this sprite only"* (encapsulated instance method).

### B. Functional Programming & Grey Rings (Lambdas)
* Treats procedures, lists, and scripts as first-class data.
* **Grey Ring Slots**: Acts as an unevaluated lambda expression.
* **Core Higher-Order Blocks**:
  * `map ( ring ) over ( list )`: Applies a transformation function to list items.
  * `keep items such that < predicate ring > from ( list )`: Filters a list.
  * `combine using ( reporter ring ) items of ( list )`: Folds/reduces a list.
  * `call ( ring ) with inputs [ ]`: Evaluates a reporter lambda.
  * `report [ ]`: Explicit return statement that halts execution and pushes values up the call stack.
* **Variadic Slots (`◀ ▶`)**: Interactive arrow buttons to dynamically expand or contract the number of input parameters.

---

## 💡 Practical Implications for Our School Platform

1. **Block Geometry for the 10 Starting Blocks**:
   * Use **oval input slots** for math expressions and string text.
   * Use **hexagonal sockets** for conditionals (`if`, `if/else`, `repeat until`) to prevent students from placing non-boolean expressions in test conditions.
   * Use **C-blocks** for loops and conditionals with clear indentation guidelines.
2. **Pyodide Code Generation & Block Mapping**:
   * Follow Blockly's generator model: attach an ID attribute to each generated Python line (e.g. via comments `# block_id:xyz` or source-map metadata) so runtime Pyodide tracebacks immediately illuminate the failing block in red.
3. **Student Error Scaffolding (Code.org Model)**:
   * Suppress raw Python tracebacks (`NameError: name 'x' is not defined`).
   * Translate them into student-friendly guidance: *"It looks like variable 'x' hasn't been set yet. Try using a `set [x] to [...]` block first."*
4. **Dual Output Strategy**:
   * For Phase 1: Pure text console panel (capturing `sys.stdout` and `input()`).
   * Provide a clean "Run" / "Reset" toggle with execution timeout limits (3000ms) to safely halt infinite `repeat until` loops.
