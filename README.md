# NYC Airbnb Data Workflow

## Project Description

This project builds a complete, reproducible data workflow for exploring NYC Airbnb listings — loading, cleaning, exploring, and visualizing the data using Python, Pandas, and Matplotlib/Seaborn. It focuses on pricing patterns across boroughs and room types, and on the relationship between price and review activity. The goal is to establish clean, modular practices that later ML, Deep Learning, and Agentic AI projects in this program will build on.

## What Was Built

- A Jupyter Notebook (`data_workflow.ipynb`) with a full workflow: data ingestion, four cleaning functions, two exploratory analysis functions, three labeled visualizations, and a written summary of findings
- A written report (`module_summary.pdf`) with academic citations, covering the same work in more depth
- A `requirements.txt` for reproducing the environment
- A GitHub repository with commits and branches tracking progress task by task

## Dataset

**New York City Airbnb Open Data** (via Kaggle)
Link: https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data

## How to Run the Project

### 1. Install dependencies

```
pip install -r requirements.txt
```

### 2. Get the dataset

Download `AB_NYC_2019.csv` from the Kaggle link above and place it in the same folder as `data_workflow.ipynb`.

### 3. Open and run the notebook

```
jupyter notebook data_workflow.ipynb
```

Run all cells top to bottom. No additional setup is required.

### Reproducing `requirements.txt`

If dependencies change, regenerate it with:

```
pip freeze > requirements.txt
```

## Bias & Data Quality Considerations

A few cleaning choices in this workflow carry bias risk worth flagging. Rows missing `name` or `host_name` (37 rows) were dropped rather than imputed — a small change, but any row removal is a choice that can shift results slightly. More significantly, high-price listings (up to $10,000) and unusually long `minimum_nights` values (up to 1,250) were deliberately kept in the dataset rather than capped or removed, since most appear to be real listings rather than errors. This is a reasonable choice for descriptive analysis, but it means these outliers would need to be revisited before using this data for something like a price-prediction model, where they could disproportionately skew results. Small subgroups, like Staten Island (176 listings), are also more sensitive to individual outliers than larger boroughs — a single high-price listing was enough to noticeably shift that group's average. Full detail on these decisions is in `module_summary.pdf`.

## Future Integration Reflections

**How would this change for a Machine Learning project?**
The outliers currently kept as-is (high prices, long minimum-night stays) would need a real decision — cap, transform, or keep with explicit justification — since they'd affect a model far more than they affect a descriptive chart. Categorical columns (`neighbourhood_group`, `room_type`, `neighbourhood`) would need to be encoded into numeric form. A train/test split would also be needed, which this purely exploratory workflow doesn't require.

**What would change to prepare this data for a Neural Network?**
On top of the ML changes above, numeric features would need scaling or normalization — `price`, `minimum_nights`, and `availability_365` currently sit on very different numeric scales. Raw `latitude`/`longitude` would likely need to be turned into more useful derived features rather than used directly.

**Where could agentic automation fit in?**
Two possibilities: an agent that automatically re-runs this cleaning and EDA pipeline whenever new listing data arrives, and an agent that scans for new outliers or data-quality issues — like the ones found manually in this project — and flags them for review instead of requiring someone to go looking.
