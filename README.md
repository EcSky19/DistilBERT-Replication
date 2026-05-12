# DistilBERT vs. BERT-base: Reproducing Key Efficiency Results

A CS 4782 re-implementation project. We reproduce the core efficiency claims from the DistilBERT paper — *DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter* (Sanh et al., 2019) — by fine-tuning both DistilBERT and BERT-base on IMDb sentiment classification and comparing accuracy, parameter count, model size, and inference speed.

---

## Chosen Result

We reproduce **Table 2 & 3** of the original paper, which claim that DistilBERT retains 97% of BERT's accuracy while using 40% fewer parameters and running 60% faster at inference.

| Metric | DistilBERT | BERT-base | Ratio |
|---|---|---|---|
| IMDb Test Accuracy | 93.06% | 94.11% | 98.9% retained |
| Parameters | 67.0M | 109.5M | 38.8% fewer |
| Model Size | 255 MB | 418 MB | 38.8% smaller |
| Inference Latency (GPU) | 6.58 ms | 12.46 ms | 1.9× faster |
| Training Time | 19.5 min | 38.6 min | 2.0× faster |

Our results exceed the paper's accuracy retention claim (98.9% vs. 97%) and closely match the parameter reduction claim (38.8% vs. 40%). Inference speedup on GPU (1.9×) is lower than the paper's CPU-measured 60% (1.6×), which is expected given different hardware.

---

## Repository Contents

```
.
├── code/                  # Re-implementation notebook and scripts
├── data/                  # Dataset instructions
├── results/               # Generated figures and logs
├── poster/                # In-class poster (PDF)
├── report/                # 2-page project summary report (PDF)
├── LICENSE
└── .gitignore
```

---

## Re-implementation Details

- **Models**: `distilbert-base-uncased` and `bert-base-uncased` via HuggingFace Transformers
- **Dataset**: IMDb (25,000 train / 25,000 test) via HuggingFace Datasets
- **Training**: 3 epochs, lr=2e-5, batch size=32, max sequence length=512, weight decay=0.01, warmup ratio=0.1
- **Evaluation metric**: Accuracy
- **Hardware**: CUDA GPU (A100 / V100 recommended); also tested on CPU

Key modification: the paper evaluates inference speed on CPU. We benchmark on GPU (batch size=1) and additionally measure CPU latency (DistilBERT: 481.9 ms, BERT-base: 953.5 ms — a 1.98× speedup consistent with the paper's claim).

---

## Reproduction Steps

### Requirements

```bash
pip install transformers datasets evaluate accelerate scikit-learn matplotlib seaborn
```

### Run

Open and execute [`code/distilbert_comparison.ipynb`](code/distilbert_comparison.ipynb) top-to-bottom. The notebook will:

1. Download the IMDb dataset automatically via HuggingFace
2. Fine-tune DistilBERT and BERT-base sequentially
3. Benchmark inference latency
4. Save all figures to `results/figures/`

**Compute**: ~20 min (DistilBERT) + ~39 min (BERT-base) on a single GPU. CPU-only will take significantly longer (~8× slower per model).

---

## Results / Insights

Running the notebook produces 8 figures in `results/figures/` including accuracy comparison, parameter count, model size, inference latency, training time, and a 4-panel summary. Key takeaways:

- DistilBERT's accuracy retention (98.9%) slightly *exceeds* the paper's 97% claim on IMDb
- Parameter and size reduction (38.8%) closely matches the paper's 40% claim
- GPU speedup (1.9×) aligns with — and slightly exceeds — the paper's CPU speedup (1.6×)

---

## Conclusion

Our re-implementation successfully validates the core DistilBERT claims. The knowledge-distillation approach preserves nearly all of BERT's downstream task performance while delivering meaningful efficiency gains. The main discrepancy (GPU vs. CPU benchmarking) explains the modest difference in reported speedup.

---

## References

- Sanh, V., Debut, L., Chaumond, J., & Wolf, T. (2019). *DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter.* arXiv:1910.01108.
- Wolf, T., et al. (2020). *HuggingFace's Transformers: State-of-the-art Natural Language Processing.* EMNLP 2020.
- Maas, A., et al. (2011). *Learning Word Vectors for Sentiment Analysis.* ACL 2011. (IMDb dataset)

---

## Acknowledgements

This project was completed as part of **CS 4782: Introduction to Deep Learning** at Cornell University. We thank the course staff for their guidance and feedback.
