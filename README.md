# ⚛️ Scalable Quantum ALU

### Ancilla-Aware Fault-Tolerant Synthesis of Quantum Arithmetic Logic Units

[![Quantum](https://img.shields.io/badge/Quantum-Computing-6f42c1)](#)
[![Qiskit](https://img.shields.io/badge/Qiskit-1.x-6929C4)](https://qiskit.org/)
[![Python](https://img.shields.io/badge/Python-3.x-3776AB)](https://www.python.org/)
[![IEEE Access](https://img.shields.io/badge/Paper-IEEE%20Access-red)](#)

> **A scalable reversible QALU framework comparing NCV, Clifford+T, TTK and CDKM arithmetic under resource and ancilla constraints.**

---

## 🔬 Project at a Glance

```text
             ┌─────────────────────┐
             │   Quantum Inputs    │
             │   |A⟩  |B⟩  |S⟩    │
             └──────────┬──────────┘
                        ↓
             ┌─────────────────────┐
             │     QALU CORE       │
             │                     │
             │ ADD │ SUB │ XOR     │
             │ AND │ OR            │
             └──────────┬──────────┘
                        ↓
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
       ┌───────┐   ┌───────────┐  ┌─────────┐
       │  NCV  │   │Clifford+T │  │  IBM    │
       │ Logic │   │   Logic   │  │Mapping  │
       └───┬───┘   └─────┬─────┘  └────┬────┘
           └─────────────┼──────────────┘
                         ↓
              ┌────────────────────┐
              │ Resource Analysis  │
              │ Qubits • Depth     │
              │ CNOT • T-count     │
              │ Routing • QD       │
              └────────────────────┘
```

---

## 🎯 Main Idea

The research studies **how ancilla availability and gate representation affect scalable quantum arithmetic**.

```text
        Quantum Arithmetic
               │
       ┌───────┴────────┐
       ↓                ↓
   Zero Ancilla      1 Clean Ancilla
       │                │
      TTK              CDKM
       │                │
       └───────┬────────┘
               ↓
        Resource Trade-off
               │
       ┌───────┼────────┐
       ↓       ↓        ↓
     Qubits   Depth     T-cost
```

---

## ⚙️ QALU Operations

| Opcode | Operation |
| :----: | :-------- |
|  `000` | ADD       |
|  `001` | SUB       |
|  `010` | XOR       |
|  `011` | AND       |
|  `100` | OR        |

The QALU uses a reversible result-register formulation:

```text
R ───────⊕──────→ R ⊕ f(A,B)
A ──────────────→ A
B ──────────────→ B
S ──────────────→ S
```

This preserves reversibility and unitarity.

---

## 🧩 Architecture

```text
       ┌─────────┐
       │   |A⟩   │
       └────┬────┘
            │
       ┌────▼────┐
       │         │
       │  QALU   │◄──── |S⟩
       │         │      Selector
       └────┬────┘
            │
       ┌────▼────┐
       │   |R⟩   │
       │ Result  │
       └─────────┘
            ▲
            │
       ┌────┴────┐
       │   |B⟩   │
       └─────────┘
```

---

# 🧮 Arithmetic Cores

```text
             RIPPLE-CARRY QALU
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
       TTK CORE            CDKM CORE
     Zero Ancilla        One Clean Ancilla
          │                   │
          ▼                   ▼
     Low Workspace       Lower Depth
```

### Resource Comparison

| Metric        |       TTK |      CDKM |
| ------------- | --------: | --------: |
| Clean ancilla |     **0** |     **1** |
| Depth         |  `5n − 3` |  `2n + 4` |
| Toffoli       |  `2n − 1` |  `2n − 1` |
| T-count       | `14n − 7` | `14n − 7` |
| Width         |  `3n + 4` |  `3n + 5` |

The paper models the two designs as a controlled **workspace-versus-depth trade-off**.

---

# 🔀 Gate-Level Comparison

```text
       Reversible QALU
              │
      ┌───────┴────────┐
      ▼                ▼
     NCV           Clifford + T
      │                │
      │                │
 Abstract          FT Resource
 Synthesis          Accounting
      │                │
      └───────┬────────┘
              ▼
       IBM-Oriented
        Compilation
```

NCV, Clifford+T and IBM-native representations are treated as **different abstraction layers**, not interchangeable physical gate sets.

---

# 📈 Scalability

```text
4-bit
  ↓
8-bit
  ↓
16-bit
  ↓
32-bit
  ↓
64-bit
  ↓
128-bit
```

### 16-bit Reference

```text
┌───────────────────┬───────────┐
│ Metric            │ Value     │
├───────────────────┼───────────┤
│ NCV Logical Qubits│ 65        │
│ NCV Depth         │ 167       │
│ Clifford+T Qubits │ 51        │
│ Clifford+T Depth  │ 1298      │
└───────────────────┴───────────┘
```

The larger-width values are resource projections under the reported synthesis models.

---

# ✅ Verification

```text
          QALU CIRCUIT
               │
               ▼
       ┌───────────────┐
       │ Exact Testing │
       └───────┬───────┘
               ↓
     ┌─────────────────────┐
     │ Basis-State Tests   │
     │ Superposition Tests │
     │ Fidelity Tests     │
     └──────────┬──────────┘
                ↓
          ┌──────────┐
          │  PASS ✓  │
          └──────────┘
```

### Reported Validation

| Test                         |       Result |
| ---------------------------- | -----------: |
| Basis-state cases            |  **254,752** |
| Mismatches                   |        **0** |
| Superposition fidelity       |    **≈ 1.0** |
| Restricted-isometry fidelity | **1.000000** |

---

# 🛠️ Tech Stack

```text
Python
  │
  ├── Qiskit
  ├── Qiskit Aer
  ├── NumPy
  ├── Matplotlib
  └── Jupyter
        │
        ▼
   Quantum Circuits
        │
        ▼
 Simulation / Compilation
```

---

# 📁 Repository Structure

```text
📦 QUANTUM-ALU
│
├── 📄 README.md
├── 📄 requirements.txt
│
├── 📁 src/
│   ├── qalu.py
│   ├── ttk_adder.py
│   ├── cdkm_adder.py
│   └── clifford_t.py
│
├── 📁 simulations/
│   ├── exhaustive_validation.py
│   ├── superposition.py
│   └── fidelity.py
│
├── 📁 analysis/
│   ├── scalability.py
│   ├── resource_analysis.py
│   └── generate_figures.py
│
├── 📁 notebooks/
│   ├── QALU.ipynb
│   └── Scalability.ipynb
│
├── 📁 results/
│   ├── figures/
│   └── tables/
│
└── 📁 paper/
    └── IEEE_Access_Manuscript.pdf
```

---

# 🚀 Quick Start

```bash
git clone <repository-url>

cd QUANTUM-ALU

pip install -r requirements.txt

python src/qalu.py
```

Run validation:

```bash
python simulations/exhaustive_validation.py
```

Run scalability analysis:

```bash
python analysis/scalability.py
```

---

# 📚 Paper

**Ancilla-Aware Fault-Tolerant Synthesis of Scalable Quantum ALUs:
Formal Reversible Operator Models, Clifford+T Complexity, and IBM Quantum-Oriented Compilation**

The manuscript develops the QALU framework, formal reversible model, ancilla-aware arithmetic comparison, Clifford+T accounting, and IBM-oriented compilation analysis.

---

# 👨‍💻 Authors

**Priyanjit Dutta · Himanshu Sharma · Indranil Karmakar · Abhik Mondal · Purba Mukhopadhyay · Gouranga Mandal · Laxmidhar Biswal · Debashri Roy · Malay Kule · Hafizur Rahaman · Chandan Bandyopadhyay**

---

### ⭐ If you use this work

```text
⭐ Star the repository
🍴 Fork the project
🧪 Reproduce the experiments
📖 Cite the paper
🚀 Extend the QALU
```

### 📜 Citation

```bibtex
@article{Dutta2026QuantumALU,
  title={Ancilla-Aware Fault-Tolerant Synthesis of Scalable Quantum ALUs:
  Formal Reversible Operator Models, Clifford+T Complexity,
  and IBM Quantum-Oriented Compilation},
  author={Dutta, Priyanjit and Sharma, Himanshu and Karmakar, Indranil
  and Mondal, Abhik and Mukhopadhyay, Purba and Mandal, Gouranga
  and Biswal, Laxmidhar and Roy, Debashri and Kule, Malay
  and Rahaman, Hafizur and Bandyopadhyay, Chandan},
  journal={IEEE Access},
  year={2026}
}
```

