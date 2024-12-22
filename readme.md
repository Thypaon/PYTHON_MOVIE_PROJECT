# Movie Data Analysis Project

## Overview
This project demonstrates a step-by-step approach to analyzing a movie dataset. Using Python and popular data analysis libraries, we explore and clean the dataset, handle missing values, convert categorical data to numerical formats, and generate insights using correlation analysis and visualizations. The primary objective is to uncover relationships between various features of the movies, such as budget, gross revenue, and release year.

---

## Key Learning Objectives
1. **Data Cleaning and Preprocessing:** Learn how to handle missing values, deal with infinite values, and properly format data types.
2. **Feature Engineering:** Extract meaningful information, such as release year, from raw data.
3. **Categorical Data Encoding:** Convert non-numeric (categorical) data into numeric formats for analysis.
4. **Correlation Analysis:** Identify relationships between features using correlation matrices and heatmaps.
5. **Data Visualization:** Present findings visually to improve interpretability and insights.

---

## Project Steps

### 1. Importing Required Libraries
We start by importing essential Python libraries such as:
- **pandas**: For data manipulation and analysis.
- **numpy**: For numerical computations.
- **seaborn** and **matplotlib**: For data visualization.

These libraries provide the foundation for working with the dataset and generating insights.

---

### 2. Checking for Missing Data
We iterate through all columns in the dataset to calculate the percentage of missing values. Missing data can significantly affect analysis, so this step helps us identify which features need attention.

```python
for col in df.columns:
    pct_missing = np.mean(df[col].isnull())
    print('{} - {}%'.format(col, pct_missing))
```

---

### 3. Handling Missing and Infinite Values
To ensure the dataset is clean and analysis-ready:
- Replace missing values (NaN) in the `budget` and `gross` columns with `0`.
- Replace infinite values in these columns with `0`.
- Convert the `budget` and `gross` columns to integer data types for consistency.

```python
df['budget'] = df['budget'].fillna(0)
df['gross'] = df['gross'].fillna(0)
df['budget'] = df['budget'].replace([float('inf'), -float('inf')], 0)
df['gross'] = df['gross'].replace([float('inf'), -float('inf')], 0)
```

---

### 4. Extracting Year Information
The `released` column contains the movie release date. To make this data more usable, we extract the year as a separate column (`yearcorrect`).

```python
df['yearcorrect'] = df['released'].astype(str).str[:4]
```

---

### 5. Sorting Data by Gross Revenue
We sort the dataset by the `gross` column in descending order to focus on the highest-grossing movies.

```python
df = df.sort_values(by=['gross'], inplace=False, ascending=False)
```

---

### 6. Displaying All Rows
We configure pandas to display all rows of the dataset to inspect the data thoroughly.

```python
pd.set_option('display.max_rows', None)
```

---

### 7. Encoding Categorical Data
Categorical columns (e.g., genres, studios) must be converted to numeric codes for analysis. Using pandas' `astype('category')` and `.cat.codes`, we encode these columns into numeric representations.

```python
for col_name in df_numarized.columns:
    if df_numarized[col_name].dtype == 'object':
        df_numarized[col_name] = df_numarized[col_name].astype('category')
        df_numarized[col_name] = df_numarized[col_name].cat.codes
```

---

### 8. Correlation Analysis
We compute the Pearson correlation matrix to identify relationships between numeric features. A heatmap visualizes these correlations, highlighting the strength and direction of relationships.

```python
correlation_matrix = df.corr(method='pearson')
sns.heatmap(correlation_matrix, annot=True)
plt.title('Correlation Matrix for Numeric Features')
plt.xlabel('Movie Features')
plt.ylabel('Movie Features')
plt.show()
```

---

### 9. Analyzing Correlation Pairs
To dig deeper into feature relationships:
- Unstack the correlation matrix into long format.
- Sort correlation pairs by their values.
- Filter out redundant and trivial correlations (e.g., feature correlations with themselves).

```python
correlation_mat = df_numarized.corr()
corr_pairs = correlation_mat.unstack()
corr_pairs = corr_pairs.sort_values(ascending=False)
filtered_corr_pairs = corr_pairs[corr_pairs < 1.0]
unique_corr_pairs = filtered_corr_pairs.loc[
    ~filtered_corr_pairs.index.duplicated(keep='first')
]
```

---

### 10. Filtering High Correlations
We extract correlation pairs with values greater than `0.5` to focus on strong relationships. This helps identify key drivers of movie success, such as the correlation between budget and gross revenue.

```python
high_corr = sorted_pairs[(sorted_pairs) > 0.5]
```

---

## Key Insights
From this analysis, we can draw the following insights:
1. **Budget vs. Gross Revenue:** A strong positive correlation indicates that higher budgets generally lead to higher gross revenues.
2. **Feature Relationships:** The heatmap and correlation pairs highlight relationships between other features, such as release year and revenue trends.
3. **Categorical Feature Encoding:** Converting categorical data into numeric codes enables a more comprehensive analysis of non-numeric features.

---

## Conclusion
This project showcases the essential steps for data cleaning, preprocessing, and exploratory data analysis. The insights gained from correlation analysis and visualizations can guide further investigations into the dataset and help identify factors contributing to a movie's success. The techniques demonstrated here can be applied to other datasets, making this workflow a valuable template for data analysis projects.

---

## How to Run the Code
1. Install the required libraries: `pandas`, `numpy`, `seaborn`, and `matplotlib`.
2. Download the `movies.csv` dataset and place it in the specified directory.
3. Run the code step by step to replicate the analysis.

---

## Future Improvements
- Include additional visualizations (e.g., scatter plots, bar charts) to present findings more effectively.
- Perform advanced feature engineering to explore hidden patterns.
- Use machine learning models to predict movie success based on features like budget, genre, and release year.

---

Feel free to contribute to this project or extend the analysis by integrating new datasets or methodologies!
