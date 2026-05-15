# Hidden Markov Model — Gene Splice Site Prediction

A Python implementation of the **Viterbi algorithm** applied to a **Hidden Markov Model (HMM)** for predicting gene splice sites (exon–intron boundaries) in a DNA sequence.

---

## Overview

This project models a simplified gene structure using an HMM with five hidden states representing different regions of a gene. The **Viterbi algorithm** is used to decode the most probable sequence of hidden states (i.e., which part of the gene each nucleotide belongs to) given an observed DNA sequence.

### Hidden States

| State | Description |
|:-----:|:------------|
| `s`   | **Start** — initial state |
| `E`   | **Exon** — protein-coding region |
| `5`   | **5′ Splice Site** — boundary between exon and intron |
| `I`   | **Intron** — non-coding region |
| `e`   | **End** — terminal state |

### Target Sequence

```
CTTCATGTGAAAGCAGACGTAAGTCA
```

---

## Methodology

1. **HMM Definition** — Transition and emission probability matrices are defined for the five hidden states over a four-nucleotide alphabet (`A`, `C`, `G`, `T`).
2. **Path Log-Probability Calculation** — A helper function `get_log_prob_for_state_path()` computes the total log-probability for any candidate state path, enabling manual comparison of different splice site hypotheses.
3. **Viterbi Matrix Initialisation** — Two matrices are created: `viterbi_value_matrix` to store the best log-probability at each (state, position) cell, and `viterbi_trace_matrix` to store backpointers to the best previous state.
4. **Viterbi Algorithm (Dynamic Programming)** — The function `calculate_prob_for_a_node()` fills the matrices column-by-column (one column per nucleotide), tracking the maximum log-probability of arriving at each state.
5. **Traceback** — The optimal state path is reconstructed by following backpointers from the highest-scoring final state back to the start.

---

## Results

### Candidate Path Comparison

Several candidate paths with different exon–intron boundaries were manually evaluated:

| Path | Splice Position | Log-Probability |
|:----:|:---------------:|:---------------:|
| 1    | 6               | −43.897         |
| 2    | 8               | −43.451         |
| 3    | 12              | −43.945         |
| 4    | 15              | −42.582         |
| 5    | 18              | −41.220         |
| 6    | 22              | −41.713         |
| 7    | All Exon        | −40.980         |

### Viterbi Optimal Path

```
Sequence:      CTTCATGTGAAAGCAGACGTAAGTCA
Optimal Path:  EEEEEEEEEEEEEEEEEEEEEEEEEE
Max Log Prob:  -38.678
```

The Viterbi algorithm determines that the most probable state path assigns all positions to the **Exon (E)** state for this particular sequence and model parameterisation. Notably, the Viterbi result (`-38.678`) is higher (less negative) than all manually tested paths, confirming it found the globally optimal solution.

---

## Tech Stack

- **Python 3**
- **NumPy** — matrix operations and dynamic programming
- **math** — log-probability computations
- **Jupyter Notebook** — interactive development

---

## Getting Started

### Prerequisites

```bash
pip install numpy jupyter
```

### Running the Notebook

```bash
jupyter notebook Assignment_HMM_and_Viterbi.ipynb
```

Run all cells sequentially to reproduce the analysis.

---

## Project Structure

```
.
├── Assignment_HMM_and_Viterbi.ipynb   # Main Jupyter Notebook with full implementation
└── README.md                          # Project documentation (this file)
```

---

## Model Parameters

### Transition Matrix

```
       s     E     5     I     e
  s [ 0.0   1.0   0.0   0.0   0.0 ]
  E [ 0.0   0.9   0.1   0.0   0.0 ]
  5 [ 0.0   0.0   0.0   1.0   0.0 ]
  I [ 0.0   0.0   0.0   0.9   0.1 ]
  e [ 0.0   0.0   0.0   0.0   0.0 ]
```

### Emission Matrix

```
       A     C     G     T
  s [ 0.00  0.00  0.00  0.00 ]
  E [ 0.25  0.25  0.25  0.25 ]
  5 [ 0.05  0.00  0.95  0.00 ]
  I [ 0.40  0.10  0.10  0.40 ]
  e [ 0.00  0.00  0.00  0.00 ]
```

---

## Key Concepts

- **Hidden Markov Models (HMMs)** — Statistical models where the system transitions between hidden states, each emitting observable symbols with certain probabilities. The "hidden" part means we cannot directly observe which state generated each nucleotide — we infer it.
- **Viterbi Algorithm** — A dynamic programming algorithm that efficiently finds the most likely sequence of hidden states given a sequence of observations, without checking every possible path.
- **Log-Space Computation** — All probability calculations use logarithms to prevent numerical underflow when multiplying many small probabilities together.
- **Gene Splice Site Prediction** — Identifying the boundaries between exons (coding regions) and introns (non-coding regions) in pre-mRNA sequences, which is a fundamental problem in computational genomics.

---

<p align="center">
  Made for Computational Biology & Bioinformatics
</p>
