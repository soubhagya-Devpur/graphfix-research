# GraphFix: Adaptive Graph-Guided Context Selection for Local Software Engineering Agents

> Combining semantic retrieval and repository structure for compact code localization.

[![Kaggle Paper Track](https://img.shields.io/badge/Kaggle-Paper%20Track-20BEFF)](https://www.kaggle.com/competitions/gemma-4-developer-agent-paper)
[![Kaggle Benchmark](https://img.shields.io/badge/Kaggle-Gemma%204%20Developer%20Agent-20BEFF)](https://www.kaggle.com/competitions/gemma-4-developer-agent)

## Overview

GraphFix is a research prototype for selecting compact code context for software-engineering agents.

**Research question:** Given a small set of code symbols identified from a software-engineering issue, does adaptive graph expansion improve localization of the files modified by the reference patch while controlling context size?

GraphFix combines:
- issue-derived code anchors;
- precomputed code embeddings;
- repository graph structure;
- adaptive ranking with a bounded context budget.

## Results

The experiment covers **127 unique task base commits** from four repositories.

| Method | Mean Coverage | Mean Precision | Mean Context |
|---|---:|---:|---:|
| Issue-derived anchors | 0.4177 | 0.1074 | 27.65 |
| Embedding retrieval | 0.4073 | 0.0879 | 29.76 |
| Fixed graph expansion | 0.4071 | 0.1054 | 29.02 |
| **GraphFix** | **0.4141** | 0.0934 | 29.76 |

GraphFix does not universally outperform the baselines. It exceeds embedding retrieval and fixed graph expansion on mean coverage, while issue-derived anchors remain slightly higher on aggregate. The analysis indicates that **anchor quality is a major bottleneck**.

## Project structure

```text
graphfix-research/
├── README.md
├── LICENSE
├── .gitignore
├── notebook/
│   └── GraphFix.ipynb
├── presentation/
│   └── GraphFix-Presentation.pdf
├── figures/
│   ├── graphfix-workflow.png
│   └── results-coverage.png
├── results/
│   └── aggregate-results.csv
└── references/
    └── benchmark.md
```

## Reproducibility

Primary executed notebook:

**https://www.kaggle.com/code/soubhagyadevpur/graphfix-adaptive-graph-guided-context-selection**

The benchmark's graph and embedding resources are matched to each task using its repository and exact base commit.

The competition dataset, repository snapshots, embeddings, and other large benchmark resources are **not redistributed in this repository**.

## Evaluation

**Coverage** measures the fraction of reference-patch files represented by selected code nodes.

**Precision** measures the fraction of selected nodes belonging to reference-patch files.

**Context size** is the number of selected code nodes; GraphFix targets a budget of 30 nodes.

## Diagnostic: `rich_4079`

The problem statement is:

> Inline table code

The reference patch modifies `rich/markdown.py`.

The original anchor extraction produced generic terms (`github.com`, `Textualize`), which led retrieval toward generic test nodes. A diagnostic using `Inline` and `table` surfaced relevant symbols including `rich.table.Table` and `rich.markdown.Markdown.__init__`.

This supports the conclusion that **issue-to-symbol grounding is a critical dependency of graph-guided retrieval**.

## Limitations

- Evaluation is file-localization level rather than end-to-end patch resolution.
- Embeddings are precomputed benchmark resources rather than generated directly from issue text.
- GraphFix depends on initial anchor quality.
- Only a fixed context budget of 30 nodes was evaluated.
- The benchmark contains four repositories, with many tasks concentrated in FastAPI and Rich.

## Future work

- Improve issue-to-symbol grounding.
- Evaluate multiple graph depths and context budgets.
- Measure end-to-end patch generation and test resolution.
- Study whether improved localization translates into higher agent resolution.

## References

1. Google. *Gemma 4 Developer Agent Benchmark*. Kaggle, 2026.
2. Google. *Gemma 4 Developer Agent — Paper Track*. Kaggle, 2026.
3. Yang, J., Jimenez, C. E., Wettig, A., et al. *SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering*. NeurIPS, 2024.
4. Zhang, F., Chen, G., et al. *RepoCoder: Repository-Level Code Completion Through Iterative Retrieval and Generation*. EMNLP, 2023.
5. Guo, D., Ren, S., Lu, S., et al. *GraphCodeBERT: Pre-training Code Representations with Data Flow*. ICLR, 2021.
6. Feng, Z., Guo, D., Tang, D., et al. *CodeBERT: A Pre-Trained Model for Programming and Natural Languages*. EMNLP, 2020.

## Author

**Soubhagya Devpur**

Research project prepared for the Google Gemma 4 Developer Agent Paper Track.
