# BetaFold: Learning AlphaFold2 from Scratch

A hands-on educational series of 8 Jupyter notebooks that teach every component of AlphaFold2, from the protein folding problem to confidence interpretation. Each notebook is mathematically rigorous, visually rich, and self-contained.

No external data or GPU required. All examples are generated synthetically with NumPy so you can focus on the concepts.

## Who is this for?

Graduate students and researchers in computational biology, machine learning, or structural biology who want to deeply understand how AlphaFold2 works, not just use it.

## The Curriculum

| # | Notebook | What you will learn |
|---|----------|-------------------|
| 1 | [The Protein Folding Problem](notebooks/01_the_protein_folding_problem.ipynb) | Amino acids, backbone geometry, torsion angles, Ramachandran plots, Levinthal's paradox, energy landscapes |
| 2 | [MSAs and Evolutionary Information](notebooks/02_msas_and_evolutionary_information.ipynb) | Multiple sequence alignments, sequence conservation, coevolution, mutual information, direct coupling analysis, contact maps |
| 3 | [Input Representations and Embeddings](notebooks/03_input_representations_and_embeddings.ipynb) | One-hot encoding, MSA representation tensor, pair representation, positional encoding, template embedding, recycling |
| 4 | [Attention Mechanisms for Proteins](notebooks/04_attention_mechanisms_for_proteins.ipynb) | Self-attention from scratch, multi-head attention, row/column (axial) attention, gated attention, triangular self-attention |
| 5 | [The Evoformer](notebooks/05_the_evoformer.ipynb) | Full Evoformer block, outer product mean, triangular multiplicative updates, information flow through 48 blocks |
| 6 | [Structure Module and IPA](notebooks/06_structure_module_and_ipa.ipynb) | Rigid body frames, SE(3) equivariance, Invariant Point Attention, backbone updates, torsion angles, all-atom generation |
| 7 | [Loss Functions and Training](notebooks/07_loss_functions_and_training.ipynb) | FAPE loss, distogram loss, masked MSA loss, pLDDT loss, self-distillation, training regime |
| 8 | [Confidence Metrics and Interpretation](notebooks/08_confidence_metrics_and_interpretation.ipynb) | pLDDT, PAE maps, pTM/ipTM scores, model ranking, disorder prediction, domain boundaries, common pitfalls |

## How to use

### Install dependencies

```bash
pip install numpy matplotlib seaborn scipy jupyter ipywidgets
```

### Launch

```bash
jupyter notebook notebooks/
```

Work through the notebooks in order. Each one builds on concepts from the previous.

### What each notebook contains

- A clear learning objective at the top
- Mathematical derivations in LaTeX
- Runnable code cells that generate visualizations from scratch
- Intuitive explanations connecting the math to biological meaning
- A summary linking to the next notebook

## Key equations you will learn

| Concept | Equation |
|---------|----------|
| Attention | $\text{Attn}(Q,K,V) = \text{softmax}(QK^T / \sqrt{d_k}) V$ |
| Outer product mean | $o_{ij} = \frac{1}{N_s} \sum_s a_{si}^T b_{sj}$ |
| Triangular update | $z_{ij} \mathrel{+}= \sum_k g_{ik} \odot g_{jk}$ |
| IPA point attention | $a_{ij}^{\text{point}} = -\gamma \sum_p \lVert T_i \circ q_p - T_j \circ k_p \rVert^2$ |
| FAPE loss | $\frac{1}{N_f N_a} \sum_{i,j} \lVert T_i^{-1} \hat{x}_j - T_i^{\text{true}^{-1}} x_j^{\text{true}} \rVert_{\text{clamp}}$ |
| pLDDT | $\frac{1}{4\lvert S_i \rvert} \sum_{j \in S_i} \sum_t \mathbb{1}[\lvert d_{ij}^{\text{pred}} - d_{ij}^{\text{true}} \rvert < t]$ |
| TM-score | $\frac{1}{L} \sum_i \frac{1}{1 + (d_i / d_0)^2}$ |

## Architecture overview

```
Sequence
   |
   v
MSA Search + Template Search
   |                |
   v                v
MSA Embedding    Template Embedding
   |                |
   v                v
+------ Evoformer (x48) ------+
|  MSA Row Attention           |    <-- Notebook 4, 5
|  MSA Column Attention        |
|  Outer Product Mean -------> |
|  Triangular Multiplicative   |    <-- Notebook 5
|  Triangular Self-Attention   |
+-----------------------------+
   |              |
   v              v
Single repr    Pair repr
   |              |
   v              v
+-- Structure Module (x8) ----+
|  Invariant Point Attention   |    <-- Notebook 6
|  Backbone Update             |
|  Torsion Angle Prediction    |
+-----------------------------+
   |
   v
3D Coordinates + Confidence       <-- Notebook 7, 8
```

## References

- Jumper, J. et al. "Highly accurate protein structure prediction with AlphaFold." Nature 596, 583-589 (2021).
- AlphaFold2 Supplementary Information (the primary technical reference for all equations).
- Evoformer details: Algorithm 6-21 in the AF2 supplement.
- CASP14 results: Lupas, A. et al. "Critical assessment of methods of protein structure prediction (CASP) -- Round XIV." Proteins (2021).

## License

MIT

## Author

Sergio Mares, University of California, Berkeley
