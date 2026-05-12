# Data

## Dataset: IMDb Sentiment Classification

**Source**: [HuggingFace Datasets — `imdb`](https://huggingface.co/datasets/imdb)

| Split | Samples | Labels |
|---|---|---|
| Train | 25,000 | 50% positive / 50% negative |
| Test | 25,000 | 50% positive / 50% negative |
| Unsupervised | 50,000 | Unlabeled |

## How to Obtain

The dataset is downloaded automatically by the notebook via HuggingFace:

```python
from datasets import load_dataset
raw_dataset = load_dataset("imdb")
```

No manual download required. The dataset will be cached locally by HuggingFace at `~/.cache/huggingface/datasets/` on first run (~84 MB).

## Citation

Maas, A., Daly, R., Pham, P., Huang, D., Ng, A., & Potts, C. (2011). *Learning Word Vectors for Sentiment Analysis.* ACL 2011.
