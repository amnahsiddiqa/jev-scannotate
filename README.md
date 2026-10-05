# jev-scannotate

Experiments using [TypeSafe](https://docs.typesafe.ai)'s Jev model to annotate cell types in single-cell RNA-seq clusters, compared against Claude.

## Layout

```
Tests/
  Baseline/
    jev_vs_claude_baseline.ipynb   Jev vs Claude Opus 5.5 vs Claude Sonnet 5.5: time and answers
    inputs/state.json              cluster state: ranked marker genes + tissue
    inputs/questions.json          broad type, cell type, coherence (JevCellType defaults)
    outputs/                       written when the notebook runs
```

## Baseline: Jev vs Claude on one cluster

The notebook sends the same cluster to three models:
- **Jev** (`jev-1.13.0`);
- **Claude Opus 5.5**;
- **Claude Sonnet 5.5**.

It runs two cases:
- **`base`**: markers CD3D, CD3E, TRAC, IL7R, LTB;
- **`base + LST1`**: the same list with the monocyte gene LST1 appended.

It records wall-clock time per call and each model's broad type, cell type, and coherence answers. The input format follows [JevCellType](https://github.com/yw-Hua/JevCellType)'s default payload.

### Run it

```bash
pip install -r requirements.txt
```

**Keys.** Provide them as environment variables. Alternatively, save each key alone in a file under `keys/` at the repo root; that folder is git-ignored.

| Service | Variable / file |
|---|---|
| Jev | `TYPESAFE_API_KEY` (TypeSafe API) or `OPENROUTER_API_KEY` (OpenRouter Decisions endpoint) |
| Claude | `ANTHROPIC_API_KEY`, or a profile from `ant auth login` |

Open `Tests/Baseline/jev_vs_claude_baseline.ipynb` with `Tests/Baseline/` as the working directory and run all cells. The run makes 60 calls (3 models × 2 cases × 10 repeats), all real and billed. Results are written to `Tests/Baseline/outputs/`.
