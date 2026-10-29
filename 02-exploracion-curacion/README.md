# 02 · Data Exploration and Curation

Graded assignments for the **Exploración y Curación de Datos** course (Data Science Diploma, FaMAF-UNC, 2026).

> **Group work (Group 5):** Constanza Bencharski, Gerónimo Castro, Daniela Riego and Gerónimo Sequeiros.
> The notebooks are written in Spanish.

## Problem

Prepare a dataset of **property sales in Melbourne, Australia** to predict sale prices. The first assignment cleans, encodes and imputes the data. The second one loads it into a SQL database, queries it and enriches it with Airbnb prices from the same area.

| Assignment | Topics | Notebook |
|---|---|---|
| 1. Data curation | Missing values, impossible values, one-hot encoding, KNN imputation, PCA | [entregable1_curacion_datos.ipynb](./entregable1_curacion_datos.ipynb) |
| 2. SQL and data integration | SQLite with SQLAlchemy, SQL queries, JOINs, data-quality checks, merging external data | [entregable2_sql_airbnb.ipynb](./entregable2_sql_airbnb.ipynb) |

## Data

- **[Melbourne Housing Snapshot](https://www.kaggle.com/dansbecker/melbourne-housing-snapshot)** (Kaggle): 13,580 sales with 21 attributes (rooms, bathrooms, land and building size, year built, location, price, etc.). The notebooks download it from a UNC server.
- **`airbnb_price_by_zipcode.csv`**: mean and median prices of [Melbourne Airbnb listings](https://www.kaggle.com/tylerx/melbourne-airbnb-open-data), aggregated by postcode (247 postcodes). Built during the course.
- **`melb_processed.csv`**: the curated dataset produced by Assignment 1 and used by Assignment 2.

## Methods

### Assignment 1: data curation

1. **Missing and impossible values.** Besides `NaN`s, we looked for zeros with no physical meaning:
   - `Landsize` values below 20 m² (including 1,939 zeros) and properties with rooms but no bathrooms (34) were set to missing.
   - `Distance = 0` (6 properties in the CBD) and `Car = 0` (1,026 properties without parking) were kept as real values.
   - `YearBuilt` before 1839 (Melbourne's founding) and `BuildingArea` below 8 m² or above the 95th percentile were set to missing.
2. **Redundant features.** `Bedroom2` was dropped: it has a 0.95 correlation with `Rooms` and comes from a less consistent source.
3. **Categorical features.** `Type`, `Regionname` and `CouncilArea` were one-hot encoded (45 columns). `Suburb`, `Address` and `SellerG` were dropped because of their high cardinality.
4. **Imputation.** Data was standardized and numeric missing values were imputed with **`IterativeImputer` + `KNeighborsRegressor`** (k = 5). `CouncilArea` was imputed with its mode.
5. **Dimensionality reduction.** **PCA** was applied and the first two components (`PC1`, `PC2`) were added as new features. 14 components explain 90% of the variance.

### Assignment 2: SQL and data integration

1. **SQLite database.** Tables `melbourne_housing` and `airbnb` were created with `SQLAlchemy` and `DataFrame.to_sql()`, after converting `Date` to a datetime type.
2. **SQL queries:** record counts by region and suburb (`GROUP BY`, `COUNT`), filtering (`WHERE`), average price by type and region (`AVG`, `HAVING`) and the top 5 most expensive suburbs (`ORDER BY`, `LIMIT`).
3. **JOIN and validation.** Both tables were joined by postcode with a `LEFT JOIN`. We checked that the row count did not change (13,580), that new nulls (0.15%) came only from the 7 postcodes with no Airbnb data, and that there were no duplicates or negative prices.
4. **Feature selection** for price prediction: `Rooms`, `Bathroom`, `BuildingArea` and `YearBuilt` (correlation with price above 0.3), plus `Type` and `Suburb`. `PC1` and `PC2` were excluded because they were computed using the price itself (data leakage).
5. **Airbnb enrichment.** The **median** Airbnb price per postcode was added, keeping only postcodes with at least 10 listings.
6. **Discussion** of other ways to link the datasets: composite keys (suburb + property type), geographic coordinates, and questions to validate neighborhood mappings with a real-estate expert.

## Results

- The curated dataset has **13,580 rows and no missing values**.
- `PC1` is mainly related to property size and value; `PC2` to distance from the city center.
- **Southern Metropolitan** is the region with the most sales (4,695), and its houses are the most expensive on average (**AUD 1.8 M**).
- The most expensive suburbs (at least 10 sales) are **Canterbury, Middle Park, Albert Park, Brighton and Balwyn**.
- Filtering outliers and dropping rows with missing values would keep only **41%** of the data, so Assignment 2 uses the imputed dataset from Assignment 1.

## Takeaways

- A `0` or an out-of-range value can be a **disguised missing value**; deciding which is which requires domain knowledge.
- **Standardize before KNN and PCA**: both depend on distances or variances, so large-scale features like `Price` would otherwise dominate.
- **Multivariate imputation** (KNN) preserves the original distributions better than filling with the mean.
- **Always validate a JOIN**: a bad join can silently duplicate or drop rows.
- **Median over mean** for aggregated prices: it is robust to a few luxury listings.
- Watch out for **data leakage**: features derived from the target look great during training but are unavailable in practice.

## How to run

```bash
pip install numpy pandas scikit-learn matplotlib seaborn missingno sqlalchemy jupyter
jupyter notebook
```

Run **Assignment 1** first: it creates `melb_processed.csv`. Assignment 2 expects `airbnb_price_by_zipcode.csv` and `melb_processed.csv` in the same folder as the notebook.

Study notes (in Spanish) are in [NOTAS.md](./NOTAS.md).
