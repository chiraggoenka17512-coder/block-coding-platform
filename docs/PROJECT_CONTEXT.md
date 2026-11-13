# Project Context: Drag-and-Drop Block Coding Platform

## 1. Project Overview
A Scratch-like, website-based platform where students drag and drop blocks to build programs. The blocks are converted to Python code, executed inside a secure client-side sandbox, and program output is displayed in real time.

This is a school project built collaboratively with a teacher who provides requirements incrementally in short working sessions.

---

## 2. Current Status & Working Principles
* **Status**: Proposed draft awaiting progressive confirmation from the teacher. Not a finalized specification.
* **Build Approach**: Work with the teacher in short iterations, log every architectural and pedagogical decision, and build the smallest functional version of each new requirement first (Minimalist / YAGNI approach).
* **Key Milestones**: Review date and final deadline to be confirmed with the teacher.

---

## 3. Proposed Architecture

| Component | Technology / Mechanism | Description |
| :--- | :--- | :--- |
| **Block Editor** | Google Blockly | Drag-and-drop visual block canvas (plain HTML/CSS/JS or React + Vite). |
| **Code Generation** | Blockly Python Generator | Converts the AST (Abstract Syntax Tree) of blocks into clean Python code. |
| **Execution Engine** | Pyodide (WebAssembly) | Runs Python directly in the browser sandbox without requiring remote servers. |
| **Execution Guard** | Web Worker / Step Counter | Timeout and step-limit safeguards to interrupt infinite loops gracefully. |
| **Output Console** | Dedicated UI Panel | Displays standard output (`stdout`), input prompts, runtime logs, timeout alerts, and errors. |
| **Error Highlighting** | Block-to-Line Mapping | Maps generated Python lines to Blockly Block IDs to highlight failing blocks visually. |
| **Persistence** | JSON Workspace Serialization | Export and import project state via Blockly's standard JSON serialization format. |

---

## 4. Proposed Starting Blocks (Initial 10 Blocks)

| # | Block Signature | Category | Generated Python | Purpose / Notes |
| :-: | :--- | :--- | :--- | :--- |
| 1 | `say [text]` | Output | `print(text)` | Prints output to the console panel. |
| 2 | `set [name] to [value]` | Variables | `name = value` | Assigns or updates a variable. |
| 3 | `[name]` (reporter) | Variables | `name` | Yields the stored variable value. |
| 4 | `[a] [+ − × ÷] [b]` | Math | `(a + b)` / `(a - b)` etc. | Arithmetic expression evaluation. |
| 5 | `[a] [= < >] [b]` | Logic | `(a == b)` / `(a < b)` etc. | Boolean comparison operator. |
| 6 | `if [condition] then [blocks]` | Control | `if condition:` | Conditional execution block. |
| 7 | `if [condition] then [blocks] else [blocks]` | Control | `if condition:`<br>`else:` | Branching conditional block. |
| 8 | `repeat [N] times [blocks]` | Loops | `for _ in range(N):` | Fixed-count iteration loop. |
| 9 | `repeat until [condition] [blocks]` | Loops | `while not condition:` | Conditional loop with safety step-limit. |
| 10 | `ask [question]` | Input | `input(question)` | Prompts student and captures text input. |

---

## 5. Processing Rules Under Discussion With Teacher

1. **Type System & Coercion**:
   * Can numbers be dropped into text slots automatically?
   * How should answers from `ask` (`input()`) be typed (auto-cast to int/float if numeric, or always string)?
2. **Variable Scoping**:
   * Global project-wide variables vs. scoped variables.
   * Are variables declared upfront or dynamically created on first assignment?
3. **Error Reporting & Block Highlighting**:
   * What error message level is appropriate for students (raw Python traceback vs. friendly explanatory message)?
   * How should the failing block be styled (red outline, shake, tooltip)?
4. **Execution Safeguards & Resource Limits**:
   * Maximum loop execution count (e.g., 10,000 steps).
   * Timeout duration (e.g., 3000ms – 5000ms).
   * Maximum block count / program size constraints.
5. **Output Display Format**:
   * Text styling, line wrapping, clear button, execution history.

---

## 6. AI Feature Scope (Under Consideration)

> [!IMPORTANT]
> The AI feature is strictly limited to explanatory and scaffolding assistance. **No AI code generation** is permitted.

* **Guiding Principles**:
  * Must never solve assignments or generate solutions.
  * Hints must prompt student thinking and self-debugging.
* **Approved Scenarios**:
  1. **Explain Code**: Explaining the generated Python code in plain, accessible language.
  2. **Error Hinting**: Providing targeted clues when a runtime or syntax error occurs.
* **Security & Architecture**:
  * Browser $\rightarrow$ Custom Backend Proxy (`POST /explain`) $\rightarrow$ Hosted AI API.
  * Server-side API key only — secrets never exposed to the client.
  * Rate limiting (per-session/IP), capped prompt lengths, and token thresholds.
  * School privacy approval required prior to transmitting student code externally.

---

## 7. Open Questions Tracker for Teacher

| # | Question | Decision Status | Decision / Notes |
| :-: | :--- | :--- | :--- |
| 1 | Who are the target students (target grade, age, prior coding exposure)? | Pending | |
| 2 | What output should student programs produce (pure text console, 2D canvas/turtle, mini-games)? | Pending | |
| 3 | Target language: Confirm Python via Pyodide or alternative? | Proposed (Python) | |
| 4 | AI Feature: Is external AI scaffolding approved under school policies? | Pending | |
| 5 | What is the project deadline and schedule of review sessions? | Pending | |
| 6 | What are the rubric and evaluation criteria for this project? | Pending | |
| 7 | Interface requirements: Specific responsive layout, high contrast, or accessibility needs? | Pending | |
