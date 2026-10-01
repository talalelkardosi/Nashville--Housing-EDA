# Nashville Housing — Exploratory Data Analysis

Exploratory analysis of Nashville housing transactions using Python to examine sale prices, property characteristics, geographic differences, and trends from January 2013 to October 2016.

## Main Question

How do housing transaction prices vary across property types, cities, and recorded property features, and how do those differences change after accounting for other characteristics?

## Repository Files

| File | Description |
|---|---|
| [eda.ipynb](eda.ipynb) | Analysis notebook containing code, visualizations, findings, and interpretation |
| [Nashville_Housing_Cleaned.xlsx](Nashville_Housing_Cleaned.xlsx) | Cleaned dataset with data-quality review flags |

This repository contains the exploratory analysis stage. Data cleaning was completed separately.

## Dataset and Sample Definitions

The notebook reads the `Cleaned_Data` worksheet from the cleaned workbook.

| Sample | Records |
|---|---:|
| Cleaned dataset | 56,374 |
| Study period, excluding date-review flags | 56,372 |
| Primary price sample, additionally excluding potential multi-parcel records | 51,605 |
| Single-family records in the primary price sample | 33,376 |
| Supported complete-case sample used for model comparisons | 20,491 |

The study period runs from **January 2013 to October 2016**.

Sample sizes differ across analyses because property features are not available for every record. Potential multi-parcel flags identify records requiring review, not confirmed errors.

## Analytical Approach

The analysis covers three themes:

1. **Market structure and geography:** transaction counts and price differences across land-use categories and cities.
2. **Property characteristics:** relationships between sale prices and recorded bedrooms, bathrooms, acreage, and property age.
3. **Time and robustness:** annual and monthly patterns, missing-data coverage, multi-parcel sensitivity, and explanatory model comparisons.

### Methodological Rules

- Use medians and quartiles for descriptive price comparisons.
- Require at least 30 records for reported group comparisons.
- Use January–October in every year for annual comparisons.
- Do not automatically remove price outliers.
- Do not impute missing property features.
- Report record counts alongside statistical results.
- Interpret associations without claiming causation.

## Key Findings

### 1. Nominal transaction medians increased across major property types

Using the primary price sample and the same January–October window:

| Land Use | 2013 Median | 2016 Median | Change |
|---|---:|---:|---:|
| Single Family | 185,237 | 235,000 | +26.9% |
| Residential Condo | 185,000 | 240,775 | +30.1% |
| Vacant Residential Land | 58,226.5 | 75,000 | +28.8% |

These figures describe changes in nominal transaction medians. They are not inflation-adjusted returns or a repeat-sales appreciation index. Changes in the types of properties sold can influence the results.

### 2. Recorded property features are associated with sale prices

Within the single-family sample, higher bedroom and bathroom counts were generally associated with higher transaction prices.

Pairwise Spearman correlations with sale price were:

| Feature | Spearman Correlation |
|---|---:|
| Full Bathrooms | 0.541 |
| Bedrooms | 0.408 |
| Acreage | 0.236 |
| Half Bathrooms | 0.219 |

Each correlation uses the records available for that pair of variables, so sample sizes differ.

Property age showed a non-monotonic pattern. Some newer and older property groups had higher medians than middle-aged groups, while city-level comparisons revealed differences hidden by pooled summaries.

### 3. Adjusted bedroom comparisons differ from simple descriptive comparisons

Explanatory OLS specifications use `log(SalePrice)` and compare results on the same supported complete-case sample.

After accounting for city, time, recorded bathrooms, acreage, age, and vacancy status, the categorical-count specification estimated a **9.4% price difference between four-bedroom and three-bedroom properties**, with a reported interval of approximately **7.0%–11.8%**.

The categorical specification had an adjusted R² of approximately **0.571** on the log-price scale.

This is an adjusted association within the available model sample. It does not establish the causal value of adding a bedroom or constitute a validated property valuation tool.

### 4. Vacant-land results depend strongly on multi-parcel handling

For January–October vacant residential land transactions:

| Year | Median Excluding Flagged Records | Median Including Flagged Records | Flagged Share |
|---|---:|---:|---:|
| 2013 | 58,226.5 | 149,984 | 45.07% |
| 2014 | 68,000 | 157,500 | 51.20% |
| 2015 | 72,500 | 210,000 | 61.58% |
| 2016 | 75,000 | 124,900 | 53.20% |

From 2015 to 2016, the median increased by approximately **3.4%** when flagged records were excluded, but decreased by approximately **40.5%** when they were included.

This segment therefore requires an explicit transaction definition and transparent treatment of potential multi-parcel records.

## Visualizations and Diagnostics

The notebook includes:

- Feature-availability comparisons by land use.
- City-by-year median-price heatmaps.
- Median and interquartile-range plots by bedroom and bathroom counts.
- Three-bedroom versus four-bedroom comparisons within supported groups.
- Bathroom-composition comparisons between bedroom groups.
- Property-age summaries and city-by-age heatmaps.
- Acreage-versus-price scatter plots.
- Vacancy-status comparisons within land-use categories.
- Annual and monthly transaction-median trends.
- Multi-parcel sensitivity comparisons.
- Pairwise correlation heatmaps with record counts.
- Regression residual and influence diagnostics.

## Limitations

- Bedroom and bathroom information is missing for many transactions.
- Complete-case coverage differs substantially across cities, limiting the geographic representativeness of the model sample.
- Recorded features may not describe a property's exact condition at the transaction date.
- Potential multi-parcel flags do not prove that a record is incorrect.
- Very low prices and influential transactions remain in the analysis and are examined through diagnostics.
- Group medians can change because the composition of transactions changes.
- Regression controls cannot account for every unobserved property or neighborhood characteristic.

## Tools

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- Plotly
- SciPy
- statsmodels
- openpyxl
- Jupyter Notebook

## How to Run

1. Download or clone this repository.
2. Keep `eda.ipynb` and `Nashville_Housing_Cleaned.xlsx` in the same directory.
3. Install the required packages:

   ```bash
   python -m pip install pandas numpy matplotlib seaborn plotly scipy statsmodels openpyxl jupyter
   ```

4. Launch Jupyter Notebook:

   ```bash
   jupyter notebook
   ```

5. Open `eda.ipynb` and run the cells from top to bottom.

The workbook must contain a worksheet named `Cleaned_Data`.

## Conclusion

Nashville housing transactions show substantial price differences across property types, locations, and recorded features. Interpreting those differences requires consistent comparison periods, clear sample definitions, and attention to missing data and potential multi-parcel transactions.

The notebook documents the findings, their supporting calculations, and their limitations.
