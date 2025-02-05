# Investigating the Determinants of University Rankings

## Overview
University rankings influence global education choices, funding, and institutional strategies. This project explores the relationship between a university’s overall score and key factors: research score, student-to-staff ratio, and percentage of international students. Using data from the [World University Rankings 2023](https://www.kaggle.com/datasets/alitaqi000/world-university-rankings-2023), we conducted an Exploratory Data Analysis (EDA) and built a Multiple Linear Regression (MLR) model to quantify these associations. Our findings reveal that research score and international student percentage positively impact overall score, while a higher student-to-staff ratio negatively correlates. The model explains 89.9% of score variability, providing valuable insights into university ranking dynamics.

## Methods

### Data Preprocessing
- Renamed columns for consistency.
- Filtered null values and converted relevant columns to numeric types.
- Limited dataset to universities ranked within the top 200 to manage processing time.

```r
library(tidyverse)
uni_data <- read_csv("university.csv") %>%
  rename_with(~ gsub(" ", "_", .)) %>%
  filter(!is.na(University_Rank)) %>%
  mutate(
    Research_Score = as.numeric(Research_Score),
    OverAll_Score = as.numeric(OverAll_Score),
    University_Rank = as.numeric(University_Rank),
    International_Student = as.numeric(gsub("%", "", International_Student))
  ) %>%
  filter(University_Rank < 200)
```

### Statistical Modeling
- **Multiple Linear Regression (MLR)**: Modeled `OverAll_Score` using `Research_Score`, `No_of_student_per_staff`, and `International_Student`.
- **Model Fit & Validation**:
  - p-values < 0.05 confirmed statistical significance of predictors.
  - R² = 0.899 indicated strong explanatory power.
  - Residuals met normality and homoscedasticity assumptions with minor deviations.
  - Variance Inflation Factors (VIF < 5) confirmed no multicollinearity.

```r
MLR <- lm(OverAll_Score ~ Research_Score + No_of_student_per_staff + International_Student, data = uni_data)
summary(MLR)
vif(MLR)
plot(MLR, 1) # Residuals vs. Fitted
plot(MLR, 2) # Q-Q Plot
```

## Findings & Future Work
- Research score is the strongest predictor of overall university ranking.
- Higher student-to-staff ratios negatively impact rankings, while internationalization plays a key role.
- Future work could explore interaction models, non-linear transformations, and alternative regression techniques (e.g., LASSO) to refine predictions.

## References
- Docampo, D., & Cram, L. (2014). On the informativeness of university rankings.
- Hazelkorn, E. (2015). Rankings and the reshaping of higher education.
- Marginson, S. (2007). Global university rankings: Implications for higher education.
- Kaggle: World University Rankings 2023.
