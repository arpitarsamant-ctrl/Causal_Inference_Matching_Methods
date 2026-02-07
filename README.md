## Project 1 — Causal Inference: RCT vs Matching (Healthcare Policy)

### Overview

This project compares **randomized experiments (RCT designs)** versus **observational causal methods (matching + propensity scores)** using a simulated healthcare-policy setting for **CMS (Centers for Medicare & Medicaid Services)**. The goal is to show how **selection bias** distorts naïve estimates—and how different methods recover the **true causal effect**. 

### Client + Research Question

**Client:** CMS, because they pay for both pacemakers and downstream long-term care costs. 
**Research question:** *Does having a pacemaker lower MoCA cognitive scores 3 years later due to restricted access to MRI/diagnostic imaging (“diagnostic penalty”)?* 

### Data (Simulated)

* **Treatment (D):** Pacemaker implanted (1/0) 
* **Outcome (Y):** MoCA score (0–30; higher is better) 
* **Confounders (X):**

  * **Age** (affects both pacemaker probability and MoCA) 
  * **Socioeconomic Status (Esri SEI)** (captures access + health “buffer”) 

Treatment assignment is **not random**: pacemaker probability is generated via a logistic model that depends on age and SES. 

### Methods Implemented

**1) RCT designs (simulated assignment):**

* Simple random assignment
* Complete randomization
* Block randomization (by Age)
* Cluster assignment (Age × SES clusters)
  Estimated TEs are compared to the known **true effect**. 

**2) Observational causal methods:**

* Naïve Difference-in-Means (DiM)
* **Exact matching** on Age
* **Coarsened Exact Matching (CEM)** on Age + SES
* **Propensity Score Matching (PSM)** + DiM
* PSM + **G-computation** (ATT-style)
* **IPTW** (ATE) via propensity scores

Selection bias is summarized as:
[
\text{Selection Bias} = \widehat{TE} - TE_{true}
]


### Key Results Snapshot

**True treatment effect:** **−2.5 MoCA points** (diagnostic penalty). 

#### RCT estimates (close to truth, mostly)

* Simple: **−2.523**
* Complete: **−2.627**
* Block (Age): **−2.483**
* Cluster (Age × SES): **−3.669** *(more off in this run)*
* True: **−2.500** 

#### Selection bias comparison (observational methods)

* Naive DiM: **−3.642** (bias **−1.142**)
* Exact match (Age): **−3.171** (bias **−0.671**)
* CEM (Age + SES): **−2.353** (bias **+0.147**)
* PS Matching (DiM): **−3.574** (bias **−1.074**)
* PS Matching (G-Comp ATT): **−2.398** (bias **+0.102**)
* IPTW (ATE): **−2.528** (bias **−0.028**) 

**Takeaway:** naive comparisons seriously overstate the harm because pacemaker patients are systematically different; **CEM / G-comp / IPTW** get much closer to the true causal effect in this simulated setup. 

### How to Run

1. Open the `.qmd` in RStudio (or Quarto CLI).
2. Render:

```bash
quarto render Project1_fixed_v2.qmd --to html
```


### Tools / Libraries

R + Quarto, using packages including: `tidyverse`, `dagitty`, `ggdag`, `MatchIt`, `marginaleffects`, `cem`, `survey`, `broom`, `knitr` (as used throughout the report).

