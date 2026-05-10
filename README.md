# News Sentiment Analysis & Stock Price Correlation

> **10 Academy — KAIM 9 | Week 1 Challenge**
> Quantifying the relationship between financial news sentiment and stock price movements for Nova Financial Solutions.

---

## Project Overview

Financial news is published at enormous volume every day. Some headlines drive meaningful stock price reactions — others are noise. This project builds a rigorous, data-driven pipeline that:

- Analyses **1.4 million financial news headlines** using Natural Language Processing (NLP)
- Computes **technical indicators** (SMA, EMA, RSI, MACD) from 15 years of historical stock data
- Measures the **statistical correlation** between news sentiment scores and daily stock returns
- Produces **investment strategy recommendations** for Nova Financial Solutions

---

## Target Stocks

| Ticker | Company |
|--------|---------|
| AAPL | Apple Inc. |
| AMZN | Amazon.com Inc. |
| GOOG | Alphabet Inc. (Google) |
| META | Meta Platforms Inc. |
| NVDA | NVIDIA Corporation |

---

## Repository Structure

```
news-sentiment-analysis/
├── .github/
│   └── workflows/
│       └── unittests.yml        # CI/CD — runs pytest on every push
├── data/
│   └── raw/                     # Raw datasets (not tracked by Git)
├── notebooks/
│   ├── __init__.py
│   ├── task1_eda.ipynb           # Exploratory Data Analysis ✅
│   ├── task2_indicators.ipynb    # Technical Indicator Analysis ✅
│   └── task3_correlation.ipynb   # Sentiment–Return Correlation 🔄
├── src/
│   ├── __init__.py
│   ├── data_loader.py            # Data loading utilities
│   ├── indicators.py             # Technical indicator functions
│   └── sentiment.py              # Sentiment analysis functions
├── tests/
│   └── __init__.py
├── scripts/
│   └── README.md
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Task Progress

| Task | Description | Status |
|------|-------------|--------|
| Setup | GitHub repo, CI/CD, virtual environment, branching | ✅ Complete |
| Task 1 | Exploratory Data Analysis on 1.4M news articles | ✅ Complete |
| Task 2 | Technical indicators — SMA, EMA, RSI, MACD on 5 stocks | ✅ Complete |
| Task 3 | Sentiment scoring + stock return correlation | 🔄 In Progress |

---

## Key Findings So Far

### Task 1 — EDA
- **1,407,328 articles** spanning 11 years (2009–2020) with **zero missing values**
- **1,034 unique publishers** — heavily concentrated around Benzinga-affiliated authors
- **Top keyword:** `eps` (140,604 occurrences) — confirming analyst ratings focus
- **Biggest news day:** 2020-03-12 with 973 articles — the COVID-19 market crash
- 8 of the top 10 busiest news days occurred in 2020 (March–June)

### Task 2 — Technical Indicators
- SMA crossover signals visible on all 5 stocks — META shows a clear bearish crossover in 2022
- RSI confirms NVDA overbought conditions in 2023 driven by the AI rally
- MACD captures the COVID crash (March 2020) as the most extreme negative signal across all stocks
- NVDA shows the strongest post-2020 bullish MACD histogram of all five stocks

---

## Datasets

### Financial News — FNSPID (raw_analyst_ratings.csv)

| Column | Description |
|--------|-------------|
| `headline` | News article title |
| `url` | Link to the full article |
| `publisher` | Author or news organisation |
| `date` | Publication datetime (UTC-4) |
| `stock` | Stock ticker symbol |

### Historical Stock Prices (AAPL.csv, AMZN.csv, GOOG.csv, META.csv, NVDA.csv)

| Column | Description |
|--------|-------------|
| `Date` | Trading day |
| `Open` | Opening price |
| `High` | Daily high |
| `Low` | Daily low |
| `Close` | Closing price |
| `Volume` | Shares traded |

> **Note:** Raw data files are excluded from version control. Download datasets separately and place in `data/raw/`.

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/lielina-pro/news-sentiment-analysis.git
cd news-sentiment-analysis
```

### 2. Create and activate virtual environment

```bash
# Create
python -m venv venv

# Activate — Windows
venv\Scripts\activate

# Activate — Mac/Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

### 5. Fix import path in notebooks

Paste this at the top of every notebook before importing from `src/`:

```python
import sys, os
sys.path.insert(0, os.path.abspath('..'))
```

---

## Dependencies

```
pandas
numpy
matplotlib
seaborn
nltk
textblob
vaderSentiment
scikit-learn
yfinance
jupyter
pytest
```

---

## Methodology

### Task 1 — Exploratory Data Analysis
- Loaded and cleaned 1.4M news articles
- Computed headline length and word count distributions
- Identified top publishers and email domain analysis
- Time series analysis of daily publication volume
- Keyword extraction using CountVectorizer (unigrams + bigrams)

### Task 2 — Technical Indicators
All indicators computed using pure pandas — no TA-Lib required:

```python
# SMA
df['SMA_20'] = df['Close'].rolling(window=20).mean()

# EMA
df['EMA_12'] = df['Close'].ewm(span=12, adjust=False).mean()

# MACD
df['MACD'] = df['EMA_12'] - df['EMA_26']

# RSI
delta = df['Close'].diff()
gain = delta.clip(lower=0).rolling(14).mean()
loss = (-delta.clip(upper=0)).rolling(14).mean()
df['RSI'] = 100 - (100 / (1 + gain / loss))
```

### Task 3 — Sentiment & Correlation (In Progress)
- VADER sentiment scoring on all headlines
- Date alignment between news and trading days
- Daily sentiment aggregation per stock
- Pearson correlation with next-day returns
- Visualisation and investment strategy recommendations

---

## CI/CD

GitHub Actions runs automatically on every push and pull request:

```yaml
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with: { python-version: '3.10' }
      - run: pip install -r requirements.txt
      - run: python -m pytest tests/
```

---

## Branching Strategy

```
main          ← clean base, stable at all times
└── task-1    ← all current work (EDA + indicators + correlation)
```

Pull request raised to merge `task-1` into `main` at final submission.

---

## Commit Convention

All commits follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add RSI computation for all 5 stocks
fix: handle timezone parsing in news dataset
docs: update README with Task 2 findings
```

---

## Author

**Lielina Fekadu Zenebe**
Cohort: KAIM 9 — 10 Academy
GitHub: [github.com/lielina-pro](https://github.com/lielina-pro)

---

## License

Submitted as part of the 10 Academy Artificial Intelligence Mastery (KAIM) program — Week 1 Challenge. For academic use only.
