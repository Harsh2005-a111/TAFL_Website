# ⚙️ Automata Lab: Regex to Automata Visualizer

![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E)
![D3.js](https://img.shields.io/badge/d3.js-F9A03C?style=for-the-badge&logo=d3.js&logoColor=white)

An interactive, web-based visualizer and simulation tool designed to bridge the gap between abstract formal languages and practical computation. This tool converts Regular Expressions into Finite Automata, featuring real-time physics-based graph rendering and step-by-step state transition analysis. 

## ✨ Key Features

* **Regex to ε-NFA Conversion:** Parses regular expressions and constructs Non-deterministic Finite Automata using **Thompson's Construction**.
* **Subset Construction (NFA to DFA):** Automatically converts the generated ε-NFA into a Deterministic Finite Automaton (DFA) by computing ε-closures and state moves.
* **DFA Minimization:** Implements state minimization based on the Myhill-Nerode theorem to reduce the DFA to its most optimal configuration.
* **D3.js Physics Engine:** Interactive, draggable nodes with dynamic force-directed physics.
* **Transition Tables:** Dynamically generates mathematical state transition tables for both NFA and DFA states.
* **Input Simulation:** Test strings against the generated automata to visualize the exact acceptance/rejection trace.

## 🚀 Getting Started

This project is completely client-side. No build processes, package managers, or local servers are required to run the core visualizer.

1. Clone this repository:
   ```bash
   git clone [https://github.com/yourusername/automata-lab.git](https://github.com/yourusername/automata-lab.git)
