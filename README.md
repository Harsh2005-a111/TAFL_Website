Here is a comprehensive and professionally structured `README.md` file tailored for your Regex to Automata Converter project. You can copy this directly into your GitHub repository.

```markdown
# 🌟 Regex to Automata Visualizer

An interactive, web-based educational tool and physics-driven visualizer that converts Regular Expressions into Finite Automata. Built to help students, educators, and developers intuitively understand the underlying mechanics of formal language theory and compiler design.

![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![D3.js](https://img.shields.io/badge/d3.js-F9A03C?style=for-the-badge&logo=d3.js&logoColor=white)

---

## ✨ Features

### 🔄 Core Conversions
* **Regex to ε-NFA:** Implements **Thompson's Construction** algorithm to parse regular expressions and build Non-deterministic Finite Automata with epsilon moves.
* **ε-NFA to DFA:** Uses the **Subset (Powerset) Construction** algorithm to convert the NFA into a Deterministic Finite Automaton.
* **DFA Minimization:** Implements state minimization to reduce the DFA to its most optimal form (based on the Myhill-Nerode theorem).

### 🎨 Interactive Visualization
* **D3.js Physics Engine:** Renders states and transitions using a force-directed graph. 
* **Draggable Nodes:** Interact with the graph in real-time. Turn the physics engine on or off.
* **State Highlighting:** Visually distinguishes between Start states, Accept states, and normal states.
* **Curved/Straight Edges:** Toggle between curved edges or straight lines for better visual clarity.

### 🛠️ Advanced Tooling
* **String Simulator:** Test input strings against the generated automaton to see if they are **Accepted** or **Rejected**, complete with a step-by-step transition trace.
* **Transition Tables:** Automatically generates tabular representations of both the ε-NFA and the DFA.
* **Export Options:** Download the visual graph as an **SVG** or copy the **DOT notation** to your clipboard for use in Graphviz.
* **Dark/Light Mode:** Seamlessly switch between themes for optimal viewing.

### 📚 Built-in Theory Reference
Includes a comprehensive "Theory" tab that explains:
* Core concepts (Regex, FA, ε-NFA, DFA).
* Step-by-step breakdowns of Thompson's and Subset Construction algorithms.
* Regex Operator Precedence.
* Formal Language Properties (Kleene's Theorem, Pumping Lemma, Chomsky Hierarchy).

---

## 🚀 Getting Started

This project is entirely client-side and runs completely in the browser. **No build tools, package managers, or server setups are required.**

### Prerequisites
* A modern web browser (Chrome, Firefox, Safari, Edge).
* An active internet connection (to load the D3.js library via CDN).

### Installation
1. Clone the repository:
   ```bash
   git clone [https://github.com/yourusername/regex-to-automata.git](https://github.com/yourusername/regex-to-automata.git)

```

2. Navigate to the project directory:
```bash
cd regex-to-automata

```


3. Open `index.html` directly in your web browser:
* **Windows/Linux:** Double-click the file or right-click -> "Open with Chrome".
* **Mac:** `open index.html`



---

## 📖 How to Use

1. **Enter a Regex:** On the left panel, type a regular expression in the input field.
* Supported operators: `*` (Kleene Star), `+` (One or more), `?` (Optional), `|` (Union), `()` (Grouping).
* Concatenation is implicit (e.g., `ab` means `a` followed by `b`).


2. **Convert:** Click the "Convert" button to generate the automata.
3. **Switch Views:** Use the top navigation bar or the canvas toolbar to switch between the **ε-NFA** graph, the **DFA** graph, the Transition **Table**, and the **Theory** guide.
4. **Simulate:** Enter a test string in the "Simulate Input" section to see if your automata accepts or rejects the string.
5. **Adjust Visuals:** Use the Options panel to toggle ε-moves, physics simulations, labels, and curved edges.

---

## 🧠 Algorithms Under the Hood

### 1. Parsing and Postfix Conversion

The raw regex is tokenized and converted from infix notation to postfix notation using the **Shunting-yard algorithm** to handle operator precedence gracefully.

### 2. Thompson's Construction (Regex → ε-NFA)

The postfix expression is evaluated using a stack. Each character and operator is mapped to a predefined NFA fragment, which are then linked together using epsilon (ε) transitions.

### 3. Subset Construction (ε-NFA → DFA)

The algorithm calculates the **ε-closure** of the starting state. It then simulates reading every possible input symbol to generate new sets of states, effectively removing non-determinism.

### 4. DFA Minimization

Removes unreachable states and merges equivalent states by tracking distinguishable state pairs, yielding the smallest possible DFA that recognizes the same language.

---

## 💻 Tech Stack

* **HTML5 / CSS3:** Semantic markup with modern CSS Grid/Flexbox layouts and custom CSS variables for theme management.
* **Vanilla JavaScript (ES6+):** Handles the mathematical algorithms, parsing, state management, and DOM manipulation.
* **D3.js (v7):** Powers the robust force-directed physics graph and SVG rendering.

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
If you want to add new operators, improve the layout algorithm, or add step-by-step animation, feel free to fork the repository and submit a pull request.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

---

*Built with ❤️ for students traversing the complexities of Automata Theory.*

```
