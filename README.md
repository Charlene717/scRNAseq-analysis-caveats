# scRNAseq-analysis-caveats

A curated knowledge base of **common caveats, methodological nuances, and interpretation guidelines** for single-cell RNA-seq (scRNA-seq) data analysis.

This repository collects **analysis pitfalls, tool-specific behaviors, and statistical interpretation issues** that are frequently encountered across a wide range of scRNA-seq workflows and software packages.


---

## 🎯 Purpose

Modern scRNA-seq analysis pipelines can generate visually compelling and statistically complex results. However, many analytical outputs are **easy to overinterpret or misinterpret** if underlying assumptions, scaling strategies, or statistical tests are not fully understood.

This repository aims to:

* Document **common pitfalls and caveats** in scRNA-seq data analysis
* Clarify **what frequently used metrics actually measure**
* Explain **tool-specific default behaviors** that are often overlooked
* Highlight **statistical assumptions and limitations** in downstream analyses
* Provide **practical guidance for correct result interpretation**

The focus is on *analysis understanding and interpretation*, not on pipeline execution.

---

## 🧩 Scope

Topics covered in this repository may include (but are not limited to):

* Cell–cell communication analysis (e.g., CellChat, CellPhoneDB)
* Clustering resolution, stability, and biological plausibility
* Cell type annotation and over-annotation issues
* Differential expression and pseudobulk analysis
* Trajectory and lineage inference
* Data integration and batch-effect correction
* Scaling, normalization, and transformation choices
* Statistical testing strategies in scRNA-seq contexts
* Visualization choices that may lead to misleading conclusions

Each note focuses on **why a specific issue arises**, **how to recognize it**, and **how to interpret results appropriately**.

---

## 📂 Repository Organization (Planned)

```
scRNAseq-analysis-caveats/
│
├── cellchat/
│   └── README.md
│
├── clustering/
│   └── README.md
│
├── statistics/
│   └── README.md
│
├── integration/
│   └── README.md
│
├── trajectory/
│   └── README.md
│
├── visualization/
│   └── README.md
│
└── README.md
```

The structure may evolve as new analysis topics and tools are added.

---

## 🧠 Intended Audience

This repository is intended for:

* Bioinformaticians performing scRNA-seq analysis
* Experimental researchers interpreting single-cell results
* Trainees learning how to critically evaluate scRNA-seq outputs
* Researchers preparing figures, methods, or responses for peer review

Basic familiarity with scRNA-seq analysis concepts and tools (e.g., Seurat or Scanpy) is assumed.

---

## 🚫 What This Repository Is *Not*

* ❌ A step-by-step tutorial for running scRNA-seq pipelines
* ❌ A benchmark comparison of analysis tools
* ❌ A replacement for official software documentation

Instead, this repository complements existing resources by focusing on **interpretation, assumptions, and analytical caveats**.

---

## 🧾 Versioning and Reproducibility Notes

### Why Version Information Matters

Many scRNA-seq analysis tools evolve rapidly. Changes in **default parameters, internal data structures, scaling strategies, or statistical implementations** can lead to different results even when the same code is executed.

To reduce ambiguity and improve reproducibility, notes in this repository explicitly consider **software version–specific behaviors** whenever relevant.

---

### Version Annotation Policy

When applicable, each note should clearly state:

* The **package name** (e.g., Seurat, CellChat, Scanpy)
* The **exact version(s)** under which the behavior was observed
* Whether the issue is:

  * version-specific
  * persistent across versions
  * known to have changed in newer releases

If behavior differs across versions, this will be explicitly highlighted.

---

### Limitations

* Not all historical versions can be exhaustively tested
* Observations are based on practical usage and documented behavior
* Users are encouraged to verify critical behaviors under their own software environment

---

### Recommended Citation Practice

When referencing notes from this repository in manuscripts or reports, it is recommended to also specify:

* The software version used in the original analysis
* The commit or release version of this repository, if applicable

This helps ensure clarity when analytical behavior changes over time.

---

## 🤝 Contributions

Contributions are welcome, including:

* Additional caveats or methodological clarifications
* Notes on tool-specific behaviors or defaults
* Explanations motivated by reviewer comments or analysis debugging

Please ensure contributions are **technically accurate, concise, and well-scoped**.

---

## 📖 Citation

If you find this repository useful in your research or teaching, you are welcome to cite or link to it.

Maintained by **Chia-Jun Chang**.

---

## 📬 Contact

For questions, discussions, or suggestions, please open an issue or discussion in this repository.
