# Reinforcement-Learning-in-Custom-Grid-World-Environment

**Author:** Sourabh Raja

---

## Overview

This assignment implements and compares reinforcement learning algorithms on a custom **Alien Planet Grid World** environment. The agent must navigate a 6×6 grid, collect items, avoid hazards, and reach the goal with maximum cumulative reward.

---

## Grid World Environment

```
S  .  C  X  .  .
.  X  .  A  .  .
.  .  .  .  X  .
.  .  X  X  X  .
.  A  .  .  .  .
.  .  .  G  .  C
```

| Symbol | Meaning      | Reward |
|--------|--------------|--------|
| `S`    | Start        | —      |
| `.`    | Empty cell   | −1     |
| `X`    | Obstacle     | −5     |
| `A`    | Alien        | −10    |
| `C`    | Collectible  | +4     |
| `G`    | Goal         | +20    |
| Wall   | Out of bounds| −5     |

**States:** 36 states (S1–S36), indexed as `row × 6 + col`  
**Actions:** Up (0), Down (1), Left (2), Right (3)

---

## Repository Structure

```
├── a3_part_1_sourabhr.ipynb   # Environment definition & random agent
├── a3_part_2_sourabhr.ipynb   # SARSA algorithm & hyperparameter tuning
├── a3_part_3_sourabhr.ipynb   # n-step Double Q-Learning & comparison
└── README.md
```

---

## Part 1 – Environment (`environment.ipynb`)

- Implements the `AlienPlanetEnv` class with `reset()`, `step()`, and `render()` methods
- Handles boundary checking (wall penalty), collectible consumption, and episode termination at goal
- Runs a random agent for 10 steps to demonstrate environment interaction

---

## Part 2 – SARSA (`SARSA.ipynb`)

- Implements the **SARSA (on-policy TD)** algorithm with ε-greedy exploration
- Trains for 500 episodes; plots reward per episode and epsilon decay
- Prints initial and trained Q-tables
- Hyperparameter sweep over **gamma** (γ) and **epsilon** values (3 values each, 9 combinations)
- Key findings:
  - Low γ (0.90) → shortsighted but faster convergence
  - High γ → better long-term planning but slower learning
  - Q-table shows state (5,2) learns `Right` → 19.99, directly pointing to the goal

---

## Part 3 – n-step Double Q-Learning (`Q-Learning.ipynb`)

- Implements **n-step Double Q-Learning** with two Q-tables to reduce overestimation bias
- Tests n ∈ {1, 2, 3, 4, 5}; best performance observed at **n = 3**
- Best hyperparameters found: `γ=0.90`, `eps_decay=0.99`, `eps_min=0.01`, `episodes=800`, `max_steps=80`
- Compares SARSA vs Double Q-Learning on the same environment:
  - Double Q-Learning converges faster and to a higher reward
  - SARSA is more sensitive to epsilon; Double Q is more stable

---

## Key Results

| Algorithm           | Avg Greedy Return | Notes                        |
|---------------------|-------------------|------------------------------|
| SARSA               | ~13               | Stable but slower            |
| Double Q (n=1)      | 13.0              | Noisy, slow convergence      |
| Double Q (n=2,3,4)  | 14.0              | Smoother, faster convergence |
| Double Q (n=5)      | ~4                | Unstable with long n         |

---

## Requirements

```bash
pip install numpy pandas matplotlib
```

All notebooks run in **Python 3** (tested on Google Colab).

---

## How to Run

1. Open any notebook in [Google Colab](https://colab.research.google.com/) or Jupyter
2. Run all cells in order
3. Part 2 and Part 3 depend on the `AlienPlanetEnv` class defined in Part 1 (included at the top of each notebook)
