# articles_stats

Using this repository to write articles backed with stats.

---

## Notebooks

| # | Notebook | Description |
|---|----------|-------------|
| 01 | [`key_matches_analysis.ipynb`](key_matches_analysis.ipynb) | Manually entered data for key matches and historical comparisons. Establishes the analytical structure before automating data collection. |

---

## Setup

Install the required Python packages:

```bash
pip install -r requirements.txt
```

Then launch Jupyter:

```bash
jupyter notebook
```

---

## Structure

Each notebook follows this standard layout:

1. **Setup** — imports and display configuration  
2. **Data** — manually entered or loaded data  
3. **Analysis** — derived metrics and aggregations  
4. **Visualisations** — charts to support the article narrative  
5. **Summary** — key stats ready for the article write-up
