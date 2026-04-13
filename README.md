# Regex → Automata Converter

> A zero-dependency, single-file browser tool that transforms any regular expression into a fully visualised **ε-NFA** and **DFA** — built on Thompson's Construction and the Subset Construction algorithm.

<br>

## Table of Contents

- [Overview](#overview)
- [Live Demo](#live-demo)
- [Screenshots](#screenshots)
- [Features](#features)
- [How to Use](#how-to-use)
- [Supported Regex Syntax](#supported-regex-syntax)
- [Algorithms](#algorithms)
  - [Tokenizer & Explicit Concatenation](#tokenizer--explicit-concatenation)
  - [Shunting-Yard (Infix → Postfix)](#shunting-yard-infix--postfix)
  - [Thompson's Construction (Postfix → ε-NFA)](#thompsons-construction-postfix--ε-nfa)
  - [Subset Construction (ε-NFA → DFA)](#subset-construction-ε-nfa--dfa)
  - [Layout Engine](#layout-engine)
- [Canvas Interaction](#canvas-interaction)
- [Project Structure](#project-structure)
- [Running Locally](#running-locally)
- [Technology Stack](#technology-stack)
- [Known Limitations](#known-limitations)
- [Contributing](#contributing)
- [Academic Context](#academic-context)
- [License](#license)

<br>

---

## Overview

This tool was built as an interactive study aid and demonstration for the **Theory of Automata and Formal Languages** course. It takes a regular expression typed by the user and performs the full formal-language pipeline in the browser — no server, no build step, no dependencies.

The conversion chain is:

```
Regular Expression
       │
       ▼  Tokenize + insert explicit concatenation operator (·)
   Token Stream
       │
       ▼  Shunting-Yard algorithm
   Postfix (RPN) expression
       │
       ▼  Thompson's Construction
   ε-NFA (Non-deterministic Finite Automaton with epsilon moves)
       │
       ▼  Subset Construction (Powerset Construction)
   DFA (Deterministic Finite Automaton)
```

All three artefacts — the ε-NFA diagram, the DFA diagram, and both transition tables — are available for inspection interactively.

<br>

---

## Live Demo

Because the entire application is a single self-contained HTML file, you can run it directly:

1. Download `regex-automata-converter.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. No internet connection required after the font loads

Alternatively, hosted it for free on **GitHub Pages**:

```
https://harsh2005-a111.github.io/TAFL_Website/
```

<br>

---

## Features

### Core Engine
- **Full Thompson's Construction** — correctly handles union (`|`), concatenation, Kleene star (`*`), one-or-more (`+`), optional (`?`), and arbitrary grouping with `( )`
- **ε-closure computation** — proper epsilon-closure used throughout NFA simulation and DFA construction
- **Subset (Powerset) Construction** — converts any ε-NFA to an equivalent minimal DFA; dead states (∅) are omitted from the diagram
- **String simulation** — run any input string against the NFA and see step-by-step state-set trace

### Visualisation
- **Interactive canvas** rendered with the HTML5 Canvas API
- **Layered BFS layout** — states are automatically positioned by BFS depth from the start state for a clean left-to-right flow
- **Node dragging** — drag any state to any position; all edges and arrowheads update in real time, resolving overlapping transitions
- **Curved edges** with automatic collision avoidance: bidirectional edges curve to opposite sides, parallel edges get progressive curvature offsets
- **Self-loop placement** — collision-aware angle picker ensures self-loops land on the least-occupied side of a node
- **Pan & zoom** — pan by dragging empty canvas space or using the dedicated Pan tool; zoom via scroll wheel or ± buttons; Fit View restores full diagram
- **Highlighted simulation** — accept/reject states are colour-coded after string simulation

### Views
| View | Contents |
|------|----------|
| **ε-NFA** | Interactive graph of the non-deterministic automaton with ε-transitions rendered as dashed purple arcs |
| **DFA** | Interactive graph of the deterministic automaton with all ε-moves eliminated |
| **Transition Table** | Full δ-function tables for both the ε-NFA and DFA side by side |

### Display Options
- **Show ε-moves** — toggle epsilon transitions on/off in the NFA diagram
- **Show labels** — toggle transition labels on/off for a cleaner view
- **Auto-layout** — enable/disable the automatic BFS-based positioning on each new conversion
- **Animate** — reserved hook for animated step-through (extensible)

### Export
- **DOT notation** — copies Graphviz-compatible `.dot` source to the clipboard; paste into Graphviz, `dot -Tpng`, or [Graphviz Online](https://dreampuf.github.io/GraphvizOnline/)
- **SVG export** — placeholder directing the user to print-to-PDF / browser screenshot

### UX Details
- **Instruction modal** — full guided walkthrough shown on first open; dismisses only when the user ticks the "I Understood" checkbox
- **Quick-example chips** — six pre-loaded regexes loadable with a single click
- **Live state table** — left panel shows every state with its type (start / accept / normal) and its outgoing transitions
- **Status bar** — live counts of states, transitions, and alphabet symbols
- **Toast notifications** — non-blocking feedback on success, errors, and clipboard operations
- **Responsive grid background** — purely decorative CSS grid gives the tool a circuit-board aesthetic

<br>

---

## How to Use

### 1. Enter a Regular Expression
Type your regex into the input field in the left panel. The syntax follows standard formal-language convention (see [Supported Regex Syntax](#supported-regex-syntax) below). You can also click any of the example chips:

| Chip | Matches |
|------|---------|
| `(a\|b)*abb` | All strings over {a,b} ending in `abb` |
| `a*b+` | Zero or more `a` followed by one or more `b` |
| `(a\|b)+` | One or more characters from {a,b} |
| `ab?c` | `ac` or `abc` |
| `(0\|1)*00` | Binary strings ending in `00` |
| `a(bc)*d` | `ad`, `abcd`, `abcbcd`, … |

### 2. Convert
Click **Convert** or press `Enter`. The engine runs synchronously and immediately renders both the ε-NFA and DFA.

### 3. Explore the Diagram
Use the toolbar at the top of the canvas:

| Button | Action |
|--------|--------|
| `+` | Zoom in |
| `−` | Zoom out |
| `⊡` | Fit entire automaton to the viewport |
| `✥` | Switch to Pan tool (drag to move viewport) |
| `◈` | Switch to Select tool (drag nodes to reposition) |
| `SVG` | Export hint |
| `DOT` | Copy DOT notation to clipboard |

Switch between **ε-NFA**, **DFA**, and **Table** views using the tabs in the header or the view-mode buttons inside the toolbar.

### 4. Rearrange Nodes
If transitions overlap, select the **◈ Select** tool (active by default) and drag any state node to a new position. Edges are redrawn live. This is especially useful for large NFAs generated from complex expressions.

### 5. Simulate a String
In the **Simulate Input** section of the left panel, type any string and click **Run** (or press `Enter`). The result shows:
- **ACCEPTED** (green) or **REJECTED** (red)
- A step-by-step trace showing the active state-set after each input symbol, e.g.:  
  `{q0} ─a→ {q0,q1} ─b→ {q0,q2} ─b→ {q0,q3}`

The accepted/rejected states are also highlighted on the canvas.

### 6. Inspect the Transition Table
Switch to the **Table** view to see the full δ-function for both the ε-NFA and DFA. ε-columns can be toggled with the **Show ε-moves** option.

<br>

---

## Supported Regex Syntax

| Operator | Symbol | Example | Meaning |
|----------|--------|---------|---------|
| Concatenation | *(implicit)* | `ab` | `a` followed by `b` |
| Union / Alternation | `\|` | `a\|b` | `a` or `b` |
| Kleene Star | `*` | `a*` | Zero or more `a` |
| One-or-More | `+` | `a+` | One or more `a` (desugared to `aa*`) |
| Optional | `?` | `a?` | Zero or one `a` (desugared to `a\|ε`) |
| Grouping | `( )` | `(ab)+` | Treat `ab` as a unit |

**Operator precedence** (highest to lowest):
1. `*`, `+`, `?` (postfix, unary)
2. Concatenation (implicit `·`)
3. `|` (lowest)

All symbols other than the operators above are treated as **literal alphabet characters**. Multi-character alphabets are fully supported — e.g. `(0|1)*00` uses `{0, 1}`.

> **Note:** Escape sequences, character classes (`[a-z]`), anchors (`^`, `$`), and backreferences are not supported. This is a formal-automata tool, not a production regex engine.

<br>

---

## Algorithms

### Tokenizer & Explicit Concatenation

The input string is first scanned character by character into a flat token stream. Operators `( ) * + ? |` get their own token types; everything else becomes a `SYM` token.

A second pass inserts an explicit concatenation operator `·` between any two adjacent tokens where the left token is a symbol / `)` / postfix-op **and** the right token is a symbol / `(`. This converts the implicit juxtaposition convention into an explicit binary operator so the Shunting-Yard algorithm can handle it uniformly.

```
Input:   a b c
Tokens:  SYM(a)  SYM(b)  SYM(c)
After:   SYM(a)  ·  SYM(b)  ·  SYM(c)
```

### Shunting-Yard (Infix → Postfix)

Dijkstra's Shunting-Yard algorithm converts the infix token stream to **Reverse Polish Notation (postfix)**. Precedence table used:

| Operator | Precedence |
|----------|-----------|
| `*` `+` `?` | 3 |
| `·` (concat) | 2 |
| `\|` | 1 |

All operators are left-associative. Parentheses are handled by the standard push/pop rule.

### Thompson's Construction (Postfix → ε-NFA)

The postfix expression is evaluated using an **NFA fragment stack**. Each operand and operator pops its operands from the stack, builds a small NFA fragment, and pushes the result back. The fragments for each operation are:

**Symbol `a`**
```
→ (q0) ──a──► (q1)
```

**Union `a|b`**
```
         ε    ┌─ NFA_a ─┐    ε
→ (q_new) ──►─┤         ├──►─ (q_accept)
              └─ NFA_b ─┘
```

**Concatenation `a·b`**
```
→ NFA_a ──ε──► NFA_b
```
*(the accept state of `a` merges into the start state of `b` via an ε-transition)*

**Kleene Star `a*`**
```
→ (q_new) ──ε──► NFA_a ──ε──► (q_accept)
     └──────────────ε──────────────┘   (loop)
     └──────────────ε──────────────►   (skip)
```

`+` is desugared to `a·a*`; `?` to `a|ε`.

### Subset Construction (ε-NFA → DFA)

The algorithm maintains a **work-list** of sets of NFA states, each set representing one DFA state.

1. Compute the ε-closure of the NFA start state → DFA start state `D0`
2. For each unprocessed DFA state `S` and each alphabet symbol `c`:
   - Compute `move(S, c)` — all NFA states reachable from any state in `S` via `c`
   - Compute the ε-closure of that set
   - If the result is a new set, create a new DFA state and add to work-list
3. A DFA state is an **accept state** if any of its NFA states includes the NFA accept state
4. Dead transitions (to the empty set) are omitted — partial DFA

The resulting DFA may have up to 2^n states for an n-state NFA but is typically much smaller in practice.

### Layout Engine

Both the NFA and DFA use a **BFS layered layout**:

1. BFS from the start state assigns a **depth level** to every state
2. States at the same level are stacked vertically, centred at `y = 0`
3. Horizontal spacing: 120 px (NFA) / 130 px (DFA) per level
4. Vertical spacing: 90 px (NFA) / 100 px (DFA) between same-level states

After the initial layout, nodes can be freely repositioned by dragging. The layout is recomputed fresh on each new conversion (if Auto-layout is enabled).

**Edge drawing** uses quadratic Bézier curves. The control point is offset perpendicular to the straight line between the two nodes. For bidirectional edges, the first edge curves to one side and the reverse to the other. For multiple parallel edges (same direction), curvature is progressively increased.

**Self-loop placement** evaluates eight candidate angles (top, bottom, left, right, and four diagonals) and picks the one with the greatest minimum angular distance from all existing outgoing/incoming edges at that node.

<br>

---

## Canvas Interaction

| Action | Tool | How |
|--------|------|-----|
| Drag a node | ◈ Select | Click and drag any state circle |
| Pan viewport | ✥ Pan | Click and drag empty canvas space |
| Pan viewport (quick) | ◈ Select | Click and drag empty canvas space |
| Zoom in / out | Either | Scroll wheel, or `+` / `−` buttons |
| Fit to view | Either | Click `⊡` button |
| Select a node | ◈ Select | Single click — highlights the node with a cyan ring |
| Hover feedback | Either | Hovering a node shows four directional dot indicators and changes the cursor to `grab` |

<br>

---

## Project Structure

```
regex-automata-converter.html     ← Entire application (single file)
README.md
```

The single HTML file is internally organised into clearly commented sections:

```
<style>                      CSS custom properties, layout, modal, canvas styles
<body>
  #modal-backdrop            Instruction modal (shown on first load)
  <header>                   Logo, view tabs, NFA/DFA state-count badges
  <main>
    .left-panel              Regex input, options toggles, simulator, states table
    .canvas-area             Toolbar, HTML5 canvas, legend, status bar
<script>
  // Thompson's Construction
  tokenize()                 Lexer + explicit concatenation insertion
  toPostfix()                Shunting-Yard
  buildNFA()                 Stack-based NFA fragment builder
  epsClosure()               ε-closure via BFS

  // Subset Construction
  subsetConstruction()       ε-NFA → DFA

  // Layout
  layoutNFA() / layoutDFA()  BFS layered positioning

  // Canvas Drawing
  redraw()                   Main render loop
  drawNode()                 State circles, labels, start arrow, accept ring
  drawEdge()                 Quadratic Bézier edges with arrowheads
  drawSelfLoop()             Collision-aware self-loop arcs
  drawArrowHead()            Generic arrowhead helper

  // Interaction
  mousedown / mousemove      Node drag + canvas pan
  mouseup / mouseleave       Drag release
  wheel                      Zoom

  // Views & Controls
  setView()                  Switch NFA / DFA / Table
  zoomIn/Out/fitView()        Viewport controls
  setTool()                  Pan ↔ Select

  // Simulation
  simulate()                 NFA string acceptance + trace

  // Export
  copyDot()                  Graphviz DOT output
  buildTable()               HTML transition table renderer

  // Instruction Modal
  handleUnderstood()         Enables Get Started button on checkbox tick
  dismissModal()             Fades out and removes the backdrop

  // INIT
  resizeCanvas()             Keeps canvas pixel-perfect on resize
  loadExample()              Pre-fills input and runs conversion
```

<br>

---

## Running Locally

No build tools, package manager, or server required.

```bash
# Option 1 — just open the file
open regex-automata-converter.html       # macOS
start regex-automata-converter.html      # Windows
xdg-open regex-automata-converter.html  # Linux

# Option 2 — serve with Python for clean URLs
python3 -m http.server 8080
# then visit http://localhost:8080/regex-automata-converter.html

# Option 3 — serve with Node
npx serve .
```

The only external resource loaded at runtime is the **Google Fonts** stylesheet (Space Mono + DM Sans). The tool is fully functional offline if the fonts have been cached, or if you replace them with system fonts by removing the `<link>` tag in the `<head>`.

<br>

---

## Technology Stack

| Concern | Technology |
|---------|-----------|
| Language | Vanilla JavaScript (ES2020) |
| Rendering | HTML5 Canvas 2D API |
| Styling | Plain CSS with custom properties (no framework) |
| Fonts | Space Mono (monospace labels), DM Sans (UI) via Google Fonts |
| Build | None — single `.html` file |
| Dependencies | None |

<br>

---

## Known Limitations

- **No character classes** — `[a-z]`, `\d`, `\w` and similar shorthands are not supported. Each character is treated as a literal symbol.
- **No anchors** — `^` and `$` have no special meaning.
- **No backreferences** — the tool models regular languages only; backreferences require context-sensitive power and are out of scope.
- **Single-character symbols only** — multi-character tokens like `ab` as a single symbol are not supported; write them as concatenation `a·b`.
- **ε is reserved** — the epsilon character cannot be used as a literal alphabet symbol.
- **SVG export** — currently redirects the user to use browser print-to-PDF. A proper programmatic SVG serialiser is a planned improvement.
- **No minimisation** — the DFA produced by subset construction is not automatically minimised (Hopcroft's algorithm is not implemented). The DFA may therefore have redundant states for some inputs.
- **Large automata** — very long or deeply nested expressions (e.g. `((a|b|c|d)*xyz)+`) can generate large NFAs. The canvas handles them but the automatic layout may be cramped; use node dragging to rearrange.

<br>

---

## Contributing

Contributions are welcome. Some areas where improvements would be valuable:

- **DFA minimisation** via Hopcroft's algorithm
- **Proper SVG export** — serialise the current canvas drawing to a downloadable `.svg` file
- **Animated step-through** — step the NFA/DFA simulation one symbol at a time with highlighted active states
- **ε-closure table** — show the ε-closure of each state in the Table view
- **Regex ↔ grammar** — generate a right-linear grammar from the NFA
- **Mobile touch support** — touch event handlers for drag/pan/pinch-zoom on tablets

To contribute:

```bash
git clone https://github.com/<your-username>/regex-automata-converter.git
cd regex-automata-converter
# Edit regex-automata-converter.html directly — no build step needed
# Open in browser to test
```

Please keep all changes in the single-file format to preserve the zero-dependency, open-and-run nature of the project.

<br>

---

## Academic Context

This project was developed as part of coursework for the **Theory of Automata and Formal Languages** course. It demonstrates the following theoretical concepts from the course:

- Regular expressions and their equivalence to finite automata (Kleene's theorem)
- Thompson's construction for converting a regex to an ε-NFA
- ε-closure and its role in NFA computation
- Subset (powerset) construction for NFA-to-DFA conversion
- The relationship between NFA state sets and DFA states
- Transition function representation (δ-tables)
- Acceptance conditions for finite automata

The implementation follows the definitions as presented in:
> Hopcroft, J. E., Motwani, R., & Ullman, J. D. (2006). *Introduction to Automata Theory, Languages, and Computation* (3rd ed.). Pearson.

<br>

---
