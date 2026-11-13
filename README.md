# Drag-and-Drop Block Coding Platform

A Scratch-like, browser-based visual programming environment where students drag and drop blocks to build programs. The platform translates visual block structures into Python code, runs them inside an in-browser sandbox (Pyodide), and presents live output with visual block-level error mapping.

This project is built iteratively in collaboration with the teacher.

---

## 📌 Project Overview & Goals

* **Visual to Text Bridge**: Helps beginner students transition smoothly from visual block programming (Scratch paradigm) to text-based Python syntax.
* **Client-Side Execution**: Runs Python in-browser via WebAssembly (Pyodide), ensuring fast execution without requiring server-side compute.
* **Targeted Feedback**: Maps runtime errors directly to individual blocks, highlighting issues visually to guide learner debugging.
* **Pedagogical Scaffolding**: Built incrementally in short review sessions with the teacher, implementing minimal complete versions of each requirement.

---

## 🏗️ Proposed Architecture

```
┌────────────────────────┐      ┌───────────────────────────┐      ┌─────────────────────────┐
│     Google Blockly     │ ───► │ Blockly Python Generator  │ ───► │    Pyodide Sandbox      │
│  (Drag-and-Drop Canvas)│      │  (Block AST to Python)    │      │  (In-Browser WebWorker) │
└────────────────────────┘      └───────────────────────────┘      └────────────┬────────────┘
                                                                                │
                                                                   ┌────────────▼────────────┐
                                                                   │   Output Panel & Logs   │
                                                                   │   (stdout, input, error)│
                                                                   └────────────┬────────────┘
                                                                                │
                                                                   ┌────────────▼────────────┐
                                                                   │ Line-to-Block Highlighting
                                                                   │ (Pinpoint failing block)│
                                                                   └─────────────────────────┘
```

---

## 🧱 Proposed Starting Blocks (Initial 10)

| Category | Block Visual Signature | Python Translation | Notes |
| :--- | :--- | :--- | :--- |
| **Output** | `say [text]` | `print(text)` | Writes text to the output console |
| **Variables** | `set [name] to [value]` | `name = value` | Stores value in named variable |
| **Variables** | `[name]` (reporter) | `name` | Yields current value of variable |
| **Math** | `[a] [+ − × ÷] [b]` | `(a + b)` / `(a - b)` | Mathematical evaluation |
| **Logic** | `[a] [= < >] [b]` | `(a == b)` / `(a < b)` | Relational comparison |
| **Control** | `if [condition] then [blocks]` | `if condition:` | Conditional execution |
| **Control** | `if [condition] then [blocks] else [blocks]` | `if condition:`<br>`else:` | Branching control flow |
| **Loops** | `repeat [N] times [blocks]` | `for _ in range(N):` | Fixed iteration loop |
| **Loops** | `repeat until [condition] [blocks]` | `while not condition:` | Conditional loop with safety limit |
| **Input** | `ask [question]` | `input(question)` | Interactive user text prompt |

---

## 📂 Repository Contents

```
.
├── docs/
│   ├── PROJECT_CONTEXT.md    # Complete specification, processing rules, and scope
│   └── TEACHER_SESSIONS.md   # Decision log and notes from teacher working sessions
├── vendor/
│   └── blockly/              # Google Blockly v13.4.0 standalone distribution
│       ├── blockly.min.js    # Core Blockly engine
│       ├── blocks_compressed.js # Standard block definitions
│       ├── python_compressed.js # Python code generator
│       ├── media/            # Icons, cursors, sound effects
│       └── msg/en.js         # English localization dictionary
├── .gitignore                # Node modules, logs, and OS files ignored
├── package.json              # Project metadata and Blockly npm dependency
├── package-lock.json         # Pinned dependency tree
└── README.md                 # Project documentation
```

---

## 📥 Blockly Setup

Google Blockly (v13.4.0) is configured in two convenient formats:

1. **Vendor Bundle (`vendor/blockly/`)**: Pre-bundled scripts ready for direct browser use via `<script>` tags without a build step or package manager.
2. **NPM Package (`package.json`)**: Configured for modern web bundlers (e.g., Vite, React, Webpack) via `npm install`.

---

## ❓ Next Steps & Open Teacher Questions

Requirements are clarified one question at a time during sessions:
1. **Target Student Audience**: Age, grade, and prior programming experience.
2. **Program Outputs**: Text-only console vs. graphics/canvas vs. interactive games.
3. **Execution Runtime**: Python via Pyodide confirmed.
4. **AI Scaffolding**: Permission for server-proxied explanatory hints (no code generation).
5. **Timeline**: Final submission deadline and milestones.
