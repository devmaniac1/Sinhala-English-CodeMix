# Sinhala-English Code-Mixed Cyberbullying Dataset

## Project Overview

This project creates a **Sinhala-English code-mixed cyberbullying dataset** by converting an existing Hindi-English code-mixed cyberbullying dataset.

## Dataset Pipeline

| Step | Notebook                     | Description                                         |
| ---- | ---------------------------- | --------------------------------------------------- |
| 1    | `01_translation.ipynb`       | Hindi → Sinhala translation using IndicTrans2       |
| 2    | `02_code_mixing.ipynb`       | Preserve English tokens, ensure natural code-mixing |
| 3    | `03_quality_filtering.ipynb` | Filter noisy translations using XLM-RoBERTa         |
| 4    | `04_validation_prep.ipynb`   | Prepare samples for human annotation                |
| 5    | `05_finalization.ipynb`      | Create final dataset with documentation             |

## Quick Start

### 1. Setup (Google Colab)

```python
# Mount Google Drive
from google.colab import drive
drive.mount('/content/drive')

# Upload this project to your Drive
# /content/drive/MyDrive/HIN_SIN/
```

### 2. Run Pipeline

Execute notebooks in order (01 → 05) on Google Colab with GPU runtime.

## Project Structure

```
HIN_SIN/
├── dataset/
│   ├── HindiEnglish.csv           # Source dataset
│   ├── translated_raw.csv         # After Step 1
│   ├── code_mixed_preserved.csv   # After Step 2
│   ├── quality_filtered.csv       # After Step 3
│   └── SinhalaEnglish_final.csv   # Final output
├── notebooks/
│   ├── 01_translation.ipynb
│   ├── 02_code_mixing.ipynb
│   ├── 03_quality_filtering.ipynb
│   ├── 04_validation_prep.ipynb
│   └── 05_finalization.ipynb
├── annotations/                    # Human validation files
├── outputs/                        # Logs and checkpoints
└── README.md
```

## Labels

- **0**: Non-bullying (positive/neutral content)
- **1**: Bullying (toxic/offensive content)

## Requirements

- Python 3.8+
- Google Colab (recommended for GPU)
- Key libraries: transformers, torch, pandas, langdetect

## Citation

[Add citation information]

## License

[Add license information]
