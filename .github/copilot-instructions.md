# Copilot Instructions

## Project Overview

This project focuses on **creating a Sinhala-English code-mixed cyberbullying dataset** by converting an existing Hindi-English code-mixed cyberbullying dataset. This is a low-resource NLP research project following established methodologies from multilingual dataset creation papers.

### Source Dataset

- **Input**: Hindi-English code-mixed cyberbullying dataset (`dataset/HindiEnglish.csv`)
- **Output**: Sinhala-English code-mixed cyberbullying dataset
- **Labels**: Binary classification (0 = Non-bullying, 1 = Bullying)

---

## Pipeline Overview

### Step 1: Automatic Translation (Hindi → Sinhala)

**Goal**: Translate Hindi tokens to Sinhala while preserving English tokens.

- Use **IndicTrans2** (recommended) or Google Translate API
- Translate at sentence/token level
- Keep English words untouched

### Step 2: Preserve Code-Mixing (Critical)

**Goal**: Maintain natural code-mixed structure, avoid "pure Sinhala" output.

**Do NOT translate**:

- English swear words
- Internet slang (bro, lol, fake, loser, yaar, etc.)
- Common English expressions

**Translate ONLY**:

- Hindi-origin words and phrases

**Techniques**:

- Regex + language detection per token
- Translate Hindi spans only using language identification

### Step 3: Automatic Quality Filtering

**Goal**: Remove noisy translations using ML classifiers.

**Keep samples only if**:

- Original label ≈ predicted label (via classifier)
- Toxicity score difference < threshold

**Models**:

- XLM-RoBERTa (zero-shot multilingual)
- mBERT for cross-lingual classification
- Sentiment + toxicity scoring

**Expected**: ~15-25% data loss (acceptable for quality)

### Step 4: Partial Human Validation

**Goal**: Validate subset for research paper credibility.

**Sample size**: 1,000–1,500 samples with 2 annotators

**Check**:

- Intent preserved?
- Code-mix natural?
- Bullying strength similar?

**Metrics to compute**:

- Accuracy
- Cohen's Kappa (inter-annotator agreement)
- Noise estimate

### Step 5: Dataset Finalization

- Remove flagged samples
- Freeze labels
- Document pipeline methodology

---

## Project Structure

```
HIN_SIN/
├── .github/
│   └── copilot-instructions.md
├── dataset/
│   ├── HindiEnglish.csv              # Original Hindi-English dataset
│   ├── translated_raw.csv            # Step 1 output
│   ├── code_mixed_preserved.csv      # Step 2 output
│   ├── quality_filtered.csv          # Step 3 output
│   ├── human_validated_sample.csv    # Step 4 annotations
│   └── SinhalaEnglish_final.csv      # Final dataset
├── notebooks/
│   ├── 01_translation.ipynb          # IndicTrans2 / API translation
│   ├── 02_code_mixing.ipynb          # Language detection & preservation
│   ├── 03_quality_filtering.ipynb    # XLM-RoBERTa / mBERT filtering
│   ├── 04_validation_prep.ipynb      # Prepare samples for annotation
│   └── 05_finalization.ipynb         # Final dataset creation
├── src/
│   ├── __init__.py
│   ├── translator.py                 # Translation utilities
│   ├── language_detector.py          # Token-level language ID
│   ├── quality_filter.py             # Filtering logic
│   └── utils.py                       # Common utilities
├── models/                           # Downloaded/cached models
├── annotations/                      # Human annotation files
├── outputs/                          # Intermediate outputs & logs
├── requirements.txt
└── README.md
```

---

## File Format Guidelines

### Use `.ipynb` (Jupyter Notebooks) when:

- Training or fine-tuning ML models
- Running inference with large models (IndicTrans2, XLM-RoBERTa)
- Code needs to run on **Google Colab** with GPU
- Interactive data exploration and visualization

### Use `.py` (Python scripts) when:

- Utility functions and reusable modules
- No GPU/training required
- Simple data preprocessing
- CLI tools

---

## Coding Guidelines

- Use **Python 3.8+** for all code
- Follow **PEP 8** style guidelines
- Include docstrings for all functions
- Handle encoding properly for **Sinhala (UTF-8)** and Hindi text
- Validate data integrity when processing CSV files
- Use type hints for function signatures

### Required Libraries

```python
# Core
pandas, numpy

# Translation
indicnlp, indic-nlp-library, googletrans (backup)

# Language Detection
langdetect, fasttext, polyglot

# ML Models
transformers, torch, sentencepiece
xlm-roberta-base, bert-base-multilingual-cased

# Evaluation
scikit-learn, scipy (for Cohen's Kappa)
```

---

## Best Practices

- Test code with **sample data (100 rows)** before full processing
- Use **pandas** for CSV manipulation with `encoding='utf-8'`
- Handle exceptions gracefully with proper logging
- Save intermediate outputs after each pipeline step
- Use **tqdm** for progress bars on long operations
- Document all hyperparameters and thresholds
- Version control dataset snapshots

---

## Colab-Specific Notes

- All notebooks should include `!pip install` cells at the top
- Mount Google Drive for persistent storage
- Use GPU runtime for translation and classification models
- Save checkpoints frequently to avoid losing progress

```python
# Standard Colab setup
from google.colab import drive
drive.mount('/content/drive')

# Install dependencies
!pip install transformers torch indic-nlp-library langdetect
```

---

## Avoid

- Hardcoded file paths (use `pathlib` or config files)
- Translating English tokens to Sinhala
- Ignoring encoding issues with non-ASCII text
- Running heavy models without GPU
- Skipping intermediate saves
- Over-translating (losing code-mix nature)
