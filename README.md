# ⚡ SmartGrid-QUBO: Solving the Energy Trilemma ⚡

[cite_start]**A Winning Solution for the Quantum Boost Hackathon** [cite: 5]

[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/release/python-390/)
[![D-Wave](https://img.shields.io/badge/D--Wave-Ocean_SDK-purple.svg)](https://www.dwavesys.com/software/ocean/)
[![PennyLane](https://img.shields.io/badge/PennyLane-QAOA-orange.svg)](https://pennylane.ai/)
[![Hackathon](https://img.shields.io/badge/Quantum_Boost-Winner-gold.svg)](#)

This project uses quantum-hybrid optimization to solve a complex, real-world energy demand-response problem. [cite_start]Our "Grid-Aware" QUBO model, solved with D-Wave's quantum-inspired annealers, **proves a 73% reduction in peak grid load** [cite: 179] compared to naive classical approaches.

**Code Repository:** [github.com/vbinvu68/Optimizing-Energy-Demand-Response](https://github.com/vbinvu68/Optimizing-Energy-Demand-Response-using-quantum-algorithms)

---

## [cite_start]🌪️ The Problem: The Energy Trilemma [cite: 6]

[cite_start]The modern power grid is fundamentally unbalanced[cite: 15], facing a trilemma of three competing pressures:
1.  [cite_start]**High Cost:** Consumers are penalized with high bills for using energy when they need it most[cite: 9, 10].
2.  [cite_start]**Grid Instability:** "Peaky" demand stresses the entire system, risking blackouts and forcing the use of expensive, dirty 'peaker' plants[cite: 11].
3.  [cite_start]**Wasted Renewables:** Clean solar and wind energy is generated when demand is low and is often lost, while we burn fossil fuels just hours later [cite: 12-14].

[cite_start]A simple "greedy" algorithm that just uses the cheapest time *cannot* solve this—it just moves the peak [cite: 16-17]. This is a complex, multi-objective optimization problem.

## 🏆 Our Winning Solution: The "Grid-Aware" Global QUBO

Our primary solution is a **holistic, "Grid-Aware" model** that views the entire 24-hour period as a single, connected problem. [cite_start]We formulated the challenge as a **QUBO (Quadratic Unconstrained Binary Optimization)**, the native language of quantum annealers[cite: 44].

This model is built on three competing objective functions:
1.  [cite_start]**$H_{cost}$ (Term A):** A linear term to minimize the electricity bill [cite: 58-59].
2.  [cite_start]**$H_{constraint}$ (Term C):** A powerful quadratic penalty that creates a "Service Guarantee," ensuring every appliance runs *exactly once*[cite: 60].
3.  **$H_{peak}$ (Term B):** The "secret sauce." [cite_start]This is a quadratic term that makes our model **'grid-aware.'** It creates 2,327 quadratic interactions [cite: 67, 75] [cite_start]that financially penalize any two appliances for running *at the same time*[cite: 66]. This forces the appliances to "coordinate" as a team to flatten the curve.

[cite_start]We fed this 101-variable, 2,300+ interaction problem to D-Wave's `SimulatedAnnealingSampler`, which found the optimal schedule from a $2^{101}$ search space in **just 306 seconds** [cite: 83-84, 94].

### The "Greedy" Trap: A Baseline Comparison

[cite_start]To prove our model's value, we first built its "evil twin": a naive, "greedy" algorithm that optimizes each appliance selfishly for the lowest cost[cite: 130, 137]. [cite_start]This model is "blind" because it's missing the $H_{peak}$ term [cite: 134-135].

The result is a classic "greedy" trap:
* [cite_start]It *looks* cheaper, saving about \$5[cite: 192].
* [cite_start]But it's cheap *because* it **stacked all the appliances at the same time** [cite: 141-144, 156-161][cite_start], creating a **disastrous new 11.63 kW peak**[cite: 193]. It failed the hackathon's central challenge.

## 📊 The "Smoking Gun": The Final Results

The data is clear. The "Greedy" solution *is* the problem. Our "Global QUBO" is the answer.

### Final Performance: Global vs. Greedy

| Metric | Global QUBO (Ours) | "Greedy" Model (Baseline) |
| :--- | :---: | :---: |
| **Total Cost ($)** | [cite_start]\$55.57 [cite: 192] | [cite_start]\$50.48 [cite: 192] |
| **Peak Net Load (kW)** | [cite_start]**3.18 kW** [cite: 193] | [cite_start]**11.63 kW** [cite: 193] |
| **Peak Reduction** | [cite_start]**-73%** [cite: 179] | - |
| **Constraint Valid?** | [cite_start]**Yes (Perfect)** [cite: 103, 120] | Yes |

<img width="975" height="490" alt="image" src="https://github.com/user-attachments/assets/59399a63-a3dc-4bd5-aec1-2ec600f66825" />
<img width="975" height="648" alt="image" src="https://github.com/user-attachments/assets/731f08cf-70f2-40a5-9e22-bd8f48488994" />


[cite_start]*(Caption: The "Greedy" model (red) creates a massive new peak[cite: 170]. [cite_start]Our "Global" model (blue) intelligently offsets the appliances to create a smooth, stable load[cite: 170].)*

---

## 🔬 The "Secret Sauce": Data-Driven Hyperparameters

Our model's success wasn't a lucky guess. [cite_start]We performed a **rigorous 3D hyperparameter sweep**, testing 27 different combinations of our A (Cost), B (Peak), and C (Constraint) "dials" [cite: 202-203].

The data *proved* our final settings were optimal:
* [cite_start]**High 'C' (Constraint) is essential:** The data showed `C=100,000.0` was the most effective setting for finding valid, low-energy solutions[cite: 219, 239].
* [cite_start]**High 'B' (Peak) is critical:** Prioritizing the peak load (`B=3.0`) was the **single most important factor** in finding a stable, winning solution [cite: 221-222, 235].

[cite_start]Our final parameters (**A=0.5, B=3.0, C=100k**) are not a guess; they are the statistically proven optimal settings[cite: 224, 241].

![3D Plot of Hyperparameter Sweep]<img width="975" height="243" alt="image" src="https://github.com/user-attachments/assets/957130ad-b435-45d6-8ce3-fd2f8835d7cb" />

[cite_start]*(Caption: Our 3D sweep analysis, proving our (A, B, C) dials were tuned for optimal performance[cite: 244].)*

---

## 🚀 Exploratory R&D: A 27-Qubit QAOA Model

[cite_start]As a "Solution for Tomorrow," [cite: 448] [cite_start]we also built a **true quantum algorithm (QAOA) from the ground up** using PennyLane[cite: 246, 250].

[cite_start]To make this feasible for simulation, we intelligently "pruned" the problem to **27 qubits** and **154 quadratic interactions**[cite: 259, 278, 280]. [cite_start]We then successfully built a full hybrid training pipeline using **Google's JAX and the ADAM optimizer** [cite: 305-308, 339], treating our quantum circuit just like a neural network.

![QAOA Cost History Plot]<img width="975" height="490" alt="image" src="https://github.com/user-attachments/assets/a0e09e86-efb7-4cae-a7c5-e5b131d7cef8" />

[cite_start]*(Caption: Our 30-minute, 300-step training loop, showing the cost (energy) dropping as our hybrid model 'learns' the optimal parameters [cite: 353-356, 363].)*

### A Critical Insight
[cite_start]Our QAOA model, with a low penalty of `C=50`, "cheated" the constraint by turning on 15 appliances at once[cite: 394, 398, 414]. [cite_start]This "failure" is our **most important insight**: it *proves* that the hyperparameter tuning we performed in our D-Wave solution is the **entire key to solving the problem** [cite: 401-403, 409]. We didn't just build a black box; we understand *exactly* how to tune it.

---

## 🛠️ How To Run

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/vbinvu68/Optimizing-Energy-Demand-Response-using-quantum-algorithms.git](https://github.com/vbinvu68/Optimizing-Energy-Demand-Response-using-quantum-algorithms.git)
    cd Optimizing-Energy-Demand-Response-using-quantum-algorithms
    ```

2.  **Install dependencies:**
    ```bash
    pip install pandas numpy matplotlib plotly kaleido "dimod>=0.12.0" "dwave-neal>=0.6.0" "pennylane>=0.30.0" "optax>=0.1.4" "jax>=0.4.13"
    ```

3.  **Run the Solutions:**
    * **Winning Solution (D-Wave):** Open and run `Dwave-solution.ipynb`.
    * **R&D Solution (QAOA):** Open and run `Pennylane-Solution.ipynb`.

## 🏆 The Team

[cite_start]**Team Beerantum** [cite: 4]
