# NEXUS: Multi-Agent Workflow for SysML v2 Generation from Industrial PDFs

> **Neural EXtraction for Unified Systems engineering**

NEXUS is a multi-agent agentic workflow for the semi-automated generation of formally valid SysML v2 models directly from unstructured technical documentation. It decomposes the modeling process into three specialized agents — **Extraction**, **Building**, and **Verification** — that collaboratively transform legacy industrial PDFs into compilable, traceable SysML v2 code.

This repository accompanies the paper:

**"NEXUS: A Multi-Agent Agentic Workflow for Valid SysML v2 Model Generation from Unstructured Industrial Documentation"**

---

## Demo

[![Watch the NEXUS demo on YouTube](https://img.youtube.com/vi/WREB_6u0pxM/maxresdefault.jpg)](https://www.youtube.com/watch?v=WREB_6u0pxM)

> **▶ Click the image above to watch the full pipeline demonstration on YouTube.**

---

## Architecture

```
┌──────────────┐     ┌──────────────────┐     ┌──────────────────┐
│  Technical   │     │  Extraction Agent │     │  Building Agent  │
│  PDF         ├────►│                  ├────►│                  │
│              │     │  PDF → Markdown  │     │  Phase 1: Structure
│              │     │  → 3× JSON:      │     │  Phase 2: Interfaces
│              │     │    • Components   │     │  Phase 3: Behavior
│              │     │    • Interfaces   │     │                  │
│              │     │    • Behavior     │     │  Compiler-in-the-│
└──────────────┘     └────────┬─────────┘     │  loop validation │
                              │               └────────┬─────────┘
                              ▼                        ▼
                     ┌──────────────────────────────────┐
                     │       Verification Agent          │
                     │  • Semantic consistency checks    │
                     │  • Compiler error analysis        │
                     │  • Structured diagnostic report   │
                     └──────────────────────────────────┘
                              │
                              ▼
                     ┌──────────────────┐
                     │  Human-in-the-   │
                     │  Loop Review     │
                     │  (every stage)   │
                     └──────────────────┘
```

---

## Key Results

Evaluated on **8 industrial-grade documents** with 5 independent runs per configuration, judged by Gemini 3.1 Pro:

| Configuration | Syntactic Validity | Semantic Coverage | Hallucination ↓ |
|---|---|---|---|
| **NEXUS (ours)** | **100 ± 0** | 81.6 ± 3.5 | **25.3 ± 3.9** |
| Direct Generation (Opus 4.6) | 35.3 ± 4.2 | 85.4 ± 4.6 | 65.1 ± 5.7 |

NEXUS achieves **100% syntactic validity** across all configurations, compared to 35% for the one-shot baseline, while reducing hallucination by a factor of **2.6×**.

### Ablation (on real-world H₂ fuel-cell spec)

| Configuration | s_syn | s_sem | s_hal ↓ |
|---|---|---|---|
| NEXUS (full) | 100 ± 0 | 68 ± 3.8 | 32 ± 3.7 |
| w/o Verification Agent | 82 ± 6.3 | 64 ± 4.7 | 36 ± 4.8 |
| w/o JSON intermediate | 58 ± 8.4 | 51 ± 6.9 | 67 ± 7.6 |

The **structured JSON intermediate** is the most impactful component for hallucination suppression, and the **Verification Agent** primarily enforces structural consistency on complex systems.

### Backbone Generality

All three tested backbones (DeepSeek-V3.2, Claude Sonnet 3.7, GPT o4-mini) achieve **100% syntactic validity** within the NEXUS architecture, confirming that performance gains are architecture-driven rather than model-specific.

---

## Pipeline Stages

### 1. Extraction Agent
Converts unstructured PDF content into three structured JSON representations using Docling for PDF→Markdown preprocessing, followed by chain-of-thought LLM extraction of components & attributes, interfaces & connections, and behavioral state machines.

### 2. Building Agent
Incrementally generates SysML v2 code in three compiler-validated phases: structural skeleton → interface wiring → behavioral state machine. Each phase includes a one-shot example and automatic compiler feedback.

### 3. Verification Agent
Performs semantic validation on both the extracted JSON and the generated SysML v2 code, producing structured diagnostic reports that guide iterative refinement.

### Human-in-the-Loop
Editable JSON and code artifacts are exposed at every stage. Domain experts can review, correct, and provide guidance between pipeline steps.

---

## License

This repository contains supplementary material for the accompanying paper. Please refer to the paper for full methodological details.
