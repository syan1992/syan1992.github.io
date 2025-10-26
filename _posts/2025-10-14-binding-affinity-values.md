---
layout: single
title: "How to Understand and Compare Binding Affinity Values: Kd, Ki, IC₅₀, and FEP"
date: 2025-10-14
excerpt: "Binding affinity metrics like Kd, Ki, IC₅₀, and ΔG are often used interchangeably — but they measure different things. This post explains how they relate, when they differ, and how to interpret them in both experimental and virtual screening settings."
tags: [binding affinity, drug discovery, biophysics, IC50, Ki, FEP, Kd]
---

## What Is Binding Affinity?

Binding affinity quantifies how strongly a ligand (typically a small molecule drug) binds to its target, often a protein. It reflects the free energy gain upon complex formation — that is, how favorable it is for the ligand and protein to form a stable complex.

The canonical equilibrium reaction is:

$$
P + L \rightleftharpoons PL
$$

This leads to the definition of the **dissociation constant**:

$$
K_d = \frac{[P][L]}{[PL]}
$$

Lower values of \( K_d \) indicate tighter binding (higher affinity), while higher values indicate weaker binding.

---

## Common Affinity Metrics: Definitions and Use Cases

| Metric   | Full Name                     | Units | Meaning                                 | Context                   |
|----------|-------------------------------|-------|------------------------------------------|---------------------------|
| **K<sub>d</sub>** | Dissociation constant          | M     | Thermodynamic binding strength            | Biophysics, binding assays |
| **K<sub>i</sub>** | Inhibition constant            | M     | Inhibitor affinity in enzyme systems      | Enzyme inhibition         |
| **IC₅₀**   | Half-maximal inhibitory conc. | M     | Concentration that inhibits 50% activity | Cell-based or enzymatic assays |
| **pK<sub>d</sub>** / pK<sub>i</sub> | –log₁₀(K<sub>d</sub> or K<sub>i</sub>) | –     | Log-transformed affinity values           | ML modeling, data normalization |
| **ΔG**    | Gibbs free energy              | kcal/mol | Energetic favorability                   | Thermodynamic modeling    |
| **FEP**   | Free energy perturbation       | kcal/mol | Simulated ΔG from alchemical transformations | Molecular dynamics        |

---

## Relationships Between Kd, Ki, IC₅₀, and ΔG

### K<sub>d</sub> and ΔG

The binding free energy is directly linked to \( K_d \) via:

$$
\Delta G = RT \ln K_d
$$

At room temperature (298K):

$$
\Delta G \approx 1.36 \times \log_{10}(K_d)
$$

| Kd (M)   | pKd | ΔG (kcal/mol) |
|---------|------|----------------|
| 1e–3    | 3.0  | –4.1           |
| 1e–6    | 6.0  | –8.2           |
| 1e–9    | 9.0  | –12.3          |

---

### K<sub>i</sub> vs K<sub>d</sub>

The inhibition constant \( K_i \) is a kinetic analog of \( K_d \) for enzyme–inhibitor interactions. In ideal 1:1 binding systems (and under competitive inhibition), \( K_i \approx K_d \). However, the two can differ depending on enzyme mechanisms and assay conditions.

---

### IC₅₀ and K<sub>i</sub>

IC₅₀ measures the inhibitor concentration needed to reduce enzyme activity by 50%. It depends on the substrate concentration and does not directly reflect true affinity. The **Cheng–Prusoff equation** corrects for this:

$$
K_i = \frac{IC_{50}}{1 + \frac{[S]}{K_m}}
$$

Where:
- \( [S] \) is the substrate concentration
- \( K_m \) is the Michaelis constant

Only when \( [S] \ll K_m \) does IC₅₀ approximate \( K_i \).

---

## Docking Score vs Binding Affinity

In virtual screening, docking scores (e.g., from AutoDock Vina or Glide) are often interpreted as proxies for binding free energy. However, docking scores:

- Are **not** true ΔG values  
- Often lack entropy, solvent effects, or protein flexibility  
- Are **ranking tools**, not accurate predictors of real \( K_d \) or ΔG

| Metric         | Units       | Interpretation         |
|----------------|-------------|-------------------------|
| Docking Score  | kcal/mol    | Lower ≈ stronger (heuristic) |
| ΔG             | kcal/mol    | Thermodynamic quantity |
| pKd / pKi      | unitless    | Higher = stronger affinity |
| Kd, Ki         | M           | Lower = stronger binding |

---

## FEP: Free Energy Perturbation

FEP is a physics-based method used in molecular simulations to estimate binding free energy changes.

### Absolute FEP

Computes:

$$
\Delta G_{\text{binding}} = G_{\text{complex}} - G_{\text{ligand}} - G_{\text{protein}}
$$

This provides an absolute prediction of ΔG for a ligand–protein pair. It requires extensive sampling and is sensitive to force field parameters and system setup.

### Relative FEP

Estimates the **ΔΔG** between two ligands:

$$
\Delta \Delta G = \Delta G_B - \Delta G_A
$$

This is highly useful in lead optimization, where one wants to compare binding strengths of similar compounds. Relative FEP is often more accurate and computationally efficient than absolute FEP, especially when ligands are structurally close.

---

## When to Use Each Value

| Scenario                          | Preferred Metric        |
|----------------------------------|--------------------------|
| Thermodynamic modeling           | ΔG, Kd                  |
| Enzyme inhibition assays         | Ki                      |
| Cell-based inhibition assays     | IC₅₀                    |
| ML regression models             | pKd, pKi                |
| Docking or structure-based VS    | Docking score (ranking) |
| Physics-based prediction         | ΔG from FEP             |
