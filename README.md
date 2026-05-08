# News Sentiment Analysis & Stock Price Correlation

> Predicting price moves with financial news sentiment — a rigorous analytical pipeline built for Nova Financial Solutions as part of the 10 Academy KAIM Week 1 Challenge.

---

## Overview

Financial news is relentless — thousands of headlines are published daily. Some drive meaningful stock price reactions; others are noise. This project builds a data-driven pipeline that separates signal from noise by:

- Quantifying the **sentiment** expressed in financial news headlines using NLP
- Computing **technical indicators** from historical stock price data
- Measuring the **statistical correlation** between news sentiment and daily stock returns

The ultimate goal is to produce actionable investment strategy recommendations that leverage the relationship between news narratives and market price action.

---

## Project Structure

```
news-sentiment-analysis/
├── .github/
│   └── workflows/
│       └── unittests.yml        # CI/CD pipeline
├── .gitignore
├── requirements.txt
├── README.md
├── data/
│   └── raw/                     # Raw datasets (not tracked by Git)
├── notebooks/
│   ├── __init__.py
│   ├── task1_eda.ipynb           # Exploratory Data Analysis
│   ├── task2_indicators.ipynb    # Technical Indicator Analysis
│   ├── task3_correlation.ipynb   # Sentiment–Return Correlation
│   └── README.md
├── src/
│   ├── __init__.py
│   ├── data_loader.py            # Data loading utilities
│   ├── indicators.py             # Technical indicator functions
│   └── sentiment.py              # Sentiment analysis functions
├── tests/
│   └── __init__.py
└── scripts/
    ├── __init__.py
    └── README.md
```

---

## Tasks

### Task 1 — Git, GitHub & Exploratory Data Analysis
- Repository and CI/CD setup
- Descriptive statistics on headline text (length distribution, publisher counts)
- Keyword and topic extraction using TF-IDF / CountVectorizer
- Time series analysis of news publication frequency
- Publisher domain analysis

### Task 2 — Quantitative Analysis with TA-Lib & PyNance
- Load and clean historical stock price data (OHLCV)
- Compute Simple Moving Average (SMA) and Exponential Moving Average (EMA)
- Compute Relative Strength Index (RSI)
- Compute MACD and signal line
- Visualize indicators against price action

### Task 3 — Sentiment–Price Correlation
- Normalize timestamps across news and stock datasets
- Assign sentiment scores to headlines using VADER / TextBlob
- Calculate daily percentage stock returns
- Aggregate daily sentiment and compute Pearson correlation
- Visualize correlation with scatter plots and bar charts by sentiment category

---

## Dataset

### Financial News and Stock Price Integration Dataset (FNSPID)

| Field | Description |
|---|---|
| `headline` | News article title |
| `url` | Link to the full article |
| `publisher` | Author or news organization |
| `date` | Publication datetime (UTC-4) |
| `stock` | Stock ticker symbol (e.g., AAPL) |

### Historical Stock Price Dataset (via YFinance)

| Field | Description |
|---|---|
| `Date` | Trading day timestamp |
| `Open` | Opening price |
| `High` | Daily high |
| `Low` | Daily low |
| `Close` | Closing price |
| `Adj Close` | Adjusted closing price (used for return calculations) |
| `Volume` | Total shares traded |

---

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/news-sentiment-analysis.git
cd news-sentiment-analysis
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

---

## Requirements

Key packages used in this project:

```
pandas
numpy
matplotlib
seaborn
nltk
textblob
vaderSentiment
scikit-learn
ta-lib
pynance
yfinance
jupyter
pytest
```

See `requirements.txt` for the full pinned list.

---

## Key Findings

> This section will be updated progressively as analysis is completed.

- **EDA**: Publication volume spikes correlate with major market events (earnings seasons, Fed announcements)
- **Indicators**: SMA crossovers on AAPL align with several significant price inflections in the dataset
- **Correlation**: Initial Pearson correlation between average daily sentiment and next-day returns shows [to be updated after Task 3]

---

## Methodology

### Sentiment Analysis
Headlines are scored using **VADER** (Valence Aware Dictionary and sEntiment Reasoner), which is specifically tuned for short, financial-style text. Each headline receives a compound score ranging from -1 (most negative) to +1 (most positive). Daily sentiment is aggregated as the mean score across all articles published for a given stock on a given trading day.

### Technical Indicators
Computed using **TA-Lib** with default parameters unless otherwise noted:
- SMA (20-day, 50-day windows)
- EMA (12-day, 26-day windows)
- RSI (14-day window)
- MACD (12/26/9 configuration)

### Correlation Analysis
Pearson correlation is computed between average daily sentiment scores and the percentage daily return `((Close_t - Close_{t-1}) / Close_{t-1}) * 100`. Weekend and holiday articles are aligned to the next valid trading day before matching.

---

## Limitations

- Correlation does not imply causation — a statistical relationship between sentiment and returns does not guarantee predictive power
- Lag effects are not fully accounted for in this version; same-day alignment may underestimate next-day impact
- VADER was designed for social media text and may misclassify highly domain-specific financial language
- Dataset coverage is limited to a fixed historical window and a small set of tickers

---

## Contributing

Contributions are welcome. Please open an issue first to discuss what you would like to change. All commits should follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) standard:

```
feat: add RSI computation module
fix: handle missing dates in news dataset
docs: update correlation findings in README
```

---

## License

This project is submitted as part of the 10 Academy Artificial Intelligence Mastery (KAIM) program — Week 1 Challenge. For academic use only.

---

## Author

**[Your Name]**  
Cohort: KAIM 9 — 10 Academy  
Contact: [your email or GitHub profile link]
