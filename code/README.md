# Code

This directory contains the re-implementation notebook for the DistilBERT vs. BERT-base comparison.

## File

- **`distilbert_comparison.ipynb`** — Main notebook. Fine-tunes both models on IMDb, runs inference benchmarks, and generates all result figures.

## Dependencies

```bash
pip install transformers datasets evaluate accelerate scikit-learn matplotlib seaborn
```

Requires Python 3.8+. A CUDA-capable GPU is strongly recommended (see root README for timing estimates).

## Configuration

Key constants at the top of the notebook (cell 1):

| Variable | Default | Description |
|---|---|---|
| `EPOCHS` | 3 | Fine-tuning epochs |
| `BATCH_SIZE` | 32 | Per-device batch size |
| `MAX_LEN` | 512 | Max token sequence length |
| `SUBSET_TRAIN` | `None` | Set an integer (e.g. `500`) to use a subset for quick testing |
| `SUBSET_TEST` | `None` | Set an integer for a smaller eval set |

## Output

Figures are saved to `../results/figures/` upon completion.
