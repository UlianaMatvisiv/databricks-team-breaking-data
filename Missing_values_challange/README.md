# Missing values challenge - Team Breaking data

## Setup
The code was created on the Databricks platform in a GitHub repo, so some of the functions may not run correctly simply using Python. 

### How to run the code, file guide
1. Run "setup" to create schemas 
2. Run load_athens_airbnb_data to get the data our team worked with during the assignment
3. Run Missing_data_visualisation and/or review_analysis. The first file contains a universal module that will analyse missing values in the data frame, while the other is a short research on review values in the Athens Airbnb data

# Report
Our team developed a comprehensive multi-dimensional visualization system to diagnose missing values in Spark DataFrames, combining quantitative summaries with visual pattern detection across columns, rows, types, and geographic categories.
## Motivations
Missing data patterns reveal critical insights about data quality, collection processes, and potential modeling challenges. Our visualization suite addresses three key diagnostic needs:
* Volume Assessment - Quickly identify which columns have substantial missingness that may require imputation or exclusion
* Pattern Detection - Discover whether values are missing completely at random (MCAR), at random (MAR), or not at random (MNAR) through co-missingness analysis
* Context-Specific Analysis - Understand how missingness varies by data type, geographic region, and business context to inform targeted data quality improvements
## Key Ideas
  **1. Missingness Heatmap** \
A row-by-column binary visualization where green indicates present values and red indicates missing values. You can enter specific columns or the entire data frame into the code, and the code will filter out the columns that are fine. This pattern-based view reveals:
- Systematic missingness across rows (e.g. if horizontal red stripes are visible -> several variables are missing in these lines,
perhaps these entries have a common cause. The Airbnb data contained entire records with missing fields related to reviews)
- Column-level completeness at a glance (In our data there are a few completely empty columns, but they don't carry much meaning: calendar_updated, bathrooms, neighbourhood_cleansed)
- Clustering of missing values that suggests related fields
![](https://drive.google.com/file/d/1VXxCP6PQG1kTbl5OXkF_aWzoFRr50nNp/view?usp=sharing)

 **2. Null Percentage Bar Chart**\
Sorted bar chart showing percentage of missing values per column, enabling quick prioritization:
- Identifies high-missingness fields (calendar_updated, bathrooms, neighbourhood_group_cleansed at 100%)
- Reveals moderate missingness in review-related fields (~15-48%)
  
**3. Co-Missingness Correlation Matrix**\
Correlation heatmap of missing value indicators across top-N columns with highest missingness. Shows which pairs of features have gaps at the same time (high correlation → structural dependence):
- Strong yellow diagonal reveals perfect correlation (same field)
- Off-diagonal patterns expose fields that tend to be missing together
- Review-related fields show high co-missingness (correlation ~0.6-0.8), suggesting systematic data collection patterns

**4. Missing Rate by Data Type**\
Aggregated missingness statistics grouped by column types (numeric, string, boolean, date/timestamp). Visualizing missingness by data type provides a clear overview of where data quality issues are concentrated across numeric, string, and date fields. It helps identify which data categories require targeted cleaning or imputation strategies and reveals systematic gaps specific to certain data types.:
- Numeric fields: 11.1% average missing rate
- String fields: 7.7% average missing rate
- Date/timestamp fields: 0% missing rate

**5. Geographic Missing Rate Analysis**\
Missing rate for specific fields (license, review scores) by geographic area. Geographic missing rate analysis highlights how data completeness varies across locations, helping detect regional reporting inconsistencies or local data collection issues. It supports targeted data quality improvements by revealing areas, such as Athens, with higher missing rates in specific fields like licenses or reviews:
- License missing rate in Athens: 5.3%
- Review missing rate in Athens: 15.1%

**6. More Extensive Analysis Of A Specific Field & Hypotheses Testing (Reviews Analysis)**\
We investigated the reasons for the absence of values in reviews_per_month:
- Hypothesis I: new hosts have not yet received reviews — refuted → most hosts have been active for several years.
- Hypothesis II: the type of accommodation affects the availability of reviews — partially confirmed → “Private room in boat” and “Camper/RV” have up to 100% omissions (less popular). (MAR, missingness depends on another variable).
- Hypothesis III: Availability affects reviews — not confirmed. (MCAR).
- Hypothesis IV: Price affects the number of reviews — confirmed → Average price without reviews = €128, with reviews = €107. (MAR pattern)
## Limitations
**Technical Constraints**
- Scalability: Heatmap limited to sample of rows (default 2,000-15,000) due to visualization constraints; extremely wide datasets may become illegible
- Co-missingness Matrix: Limited to top-N columns (default 30) and uses sampling (20%) for computational efficiency; may miss correlations in tail columns
- Performance: Correlation computation on large datasets requires sampling; full dataset analysis could be prohibitively expensive
- Missing values are not always random (MAR, MCAR, MNAR) — additional analysis is required before imputation or modeling.

**Data-Specific Challenges**
- Airbnb Dataset: Some columns (calendar_updated, bathrooms, neighbourhood_group_cleansed) show 100% missingness, indicating structural data collection issues rather than random missingness
- Empty Strings: Treatment of empty strings as missing values is configurable but may not align with domain-specific definitions of missingness
- Temporal Patterns: Current implementation doesn't capture time-based missingness trends

**Interpretation Pitfalls**
- High co-missingness correlation doesn't necessarily imply causation; may reflect data collection workflows
- Geographic analysis requires sufficient sample size per region; rare categories may show unreliable rates
- Hypothesis testing (as performed on reviews data) is exploratory and doesn't establish causal mechanisms

## Design Decisions
**Color Choices**
- Heatmap: Green (#3ec245) for present, red (#f21818) for missing - universally recognizable traffic light metaphor
- Correlation Matrix: Yellow-to-purple diverging colormap emphasizing high correlation (yellow) vs. low correlation (purple)
- Bar Charts: Single blue color for clarity and consistency

**Scale Decisions**

- Percentage Scale: 0-100% for intuitive interpretation of missingness severity
- Sorted Display: Descending order by missingness to prioritize attention on problematic fields
- Top-N Filtering: Focus on most relevant columns/regions to avoid overwhelming users

**Interactivity**
- Non-Interactive Static Plots: Matplotlib-based visualizations prioritize reproducibility and ease of sharing
- Configurable Parameters: Functions accept max_rows, include_empty_str, and column selection for flexibility
- DataFrame Display: Summary statistics returned as Spark DataFrame for further programmatic analysis

**Reproducibility Instructions**\
Dependencies: pythonpyspark, pandas, numpy, matplotlib, delta-spark, requests

**Screenshot Summary**\
Our visualizations on Airbnb Athens dataset (12,955 listings) reveal:

- 3 columns with complete missingness (data collection gaps)
- Strong co-missingness patterns in review-related fields suggesting systematic missing data mechanism
- Review missingness correlates with property type and higher pricing (€128 vs €107 average)
- Geographic variation in license compliance and review coverage

These patterns inform imputation strategies (e.g., domain-specific defaults for correlated missing values) and feature engineering decisions (e.g., creating "has_reviews" indicator features).
