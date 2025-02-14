Linear Regression Analysis with Scikit-Learn
1. Introduction
The notebook aims to demonstrate the application of linear regression and its variants (Ridge and Lasso regression) on a dataset. It covers data preprocessing, model training, evaluation, and comparison.
2. Importing Libraries
Essential libraries are imported at the beginning, including pandas for data manipulation, numpy for numerical operations, matplotlib for plotting, and sklearn for machine learning models.
python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression, Ridge, Lasso
from sklearn.metrics import mean_squared_error, r2_score
3. Loading the Data
The dataset is loaded into a pandas DataFrame. The dataset seems to include advertising spend on TV, radio, and newspapers, along with sales figures.
python
data = pd.read_csv('advertising.csv')
data.head()
4. Data Preprocessing
Features (TV, radio, newspaper) and the target variable (sales) are defined. The data is split into training and testing sets.
python
feature_cols = ['TV', 'radio', 'newspaper']
X = data[feature_cols]
y = data['sales']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1)
5. Model Training and Evaluation
Linear Regression
A linear regression model is trained and evaluated. The coefficients, intercept, and evaluation metrics (R², Adjusted R², RMSE) are calculated.
python
lm = LinearRegression()
lm.fit(X_train, y_train)
y_pred = lm.predict(X_test)

print("Intercept:", lm.intercept_)
print("Coefficients:", lm.coef_)

r2 = r2_score(y_test, y_pred)
print("R square:", r2)

adjusted_r_squared = 1 - (1-r2)*(len(y)-1)/(len(y)-X.shape[1]-1)
print("Adjusted R Square:", adjusted_r_squared)

rmse = np.sqrt(mean_squared_error(y_test, y_pred))
print("RMSE:", rmse)
Ridge Regression
Ridge regression is applied to the data. The model's performance and coefficients are recorded.
python
lm_ridge = Ridge(alpha=1.0)
lm_ridge.fit(X_train, y_train)
y_pred_ridge = lm_ridge.predict(X_test)

print(lm_ridge.intercept_)
print(lm_ridge.coef_)
Lasso Regression
Lasso regression is performed with a specific alpha value. The results, including coefficients and performance metrics, are noted.
python
lm_lasso = Lasso(alpha=0.2)
lm_lasso.fit(X_train, y_train)
y_pred_lasso = lm_lasso.predict(X_test)

print(lm_lasso.intercept_)
print(lm_lasso.coef_)
6. Comparison of Models
The coefficients of each model (Linear, Ridge, Lasso) are compared. This comparison highlights how regularization affects the feature coefficients.
python
print("Linear Reg:", list(zip(feature_cols, lm.coef_)))
print("Ridge Reg:", list(zip(feature_cols, lm_ridge.coef_)))
print("Lasso Reg:", list(zip(feature_cols, lm_lasso.coef_)))
7. Performance Metrics
For each model, the R², Adjusted R², and RMSE are computed to evaluate and compare their performance.
python
# Calculate metrics for Lasso Regression
r2 = r2_score(y_test, y_pred_lasso)
print("R square:", r2)

adjusted_r_squared = 1 - (1-r2)*(len(y)-1)/(len(y)-X.shape[1]-1)
print("Adjusted R Square:", adjusted_r_squared)

rmse = np.sqrt(mean_squared_error(y_test, y_pred_lasso))
print("RMSE:", rmse)
8. Conclusion
The notebook demonstrates how to implement and compare linear regression, Ridge, and Lasso regression using scikit-learn. It highlights the importance of regularization in regression models and provides insights 
into how different models can be used and evaluated in the context of a real dataset. Each model's coefficients and performance metrics offer a clear picture of their respective strengths and weaknesses, especially in terms of handling multicollinearity and overfitting.
Detailed Breakdown of Key Sections
Data Exploration
Before diving into model training, it is essential to explore the dataset to understand its structure and contents.
python
data.info()
data.describe()
Data Visualization
Visualizations help in understanding the relationships between features and the target variable.
python
import seaborn as sns

# Pairplot to visualize relationships
sns.pairplot(data, x_vars=['TV', 'radio', 'newspaper'], y_vars='sales', height=7, aspect=0.7)
plt.show()
Feature Scaling
For Ridge and Lasso regression, scaling features can be crucial as these models are sensitive to the scale of input data.
python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scale = scaler.fit_transform(X_train)
X_test_scale = scaler.transform(X_test)
Training Ridge and Lasso with Different Alphas
Examining the impact of different alpha values on the Ridge and Lasso regression models.
python
alphas = [0.1, 1.0, 10.0]
for alpha in alphas:
    ridge = Ridge(alpha=alpha)
    ridge.fit(X_train_scale, y_train)
    print(f"Alpha: {alpha}, Coefficients: {ridge.coef_}")

    lasso = Lasso(alpha=alpha)
    lasso.fit(X_train_scale, y_train)
    print(f"Alpha: {alpha}, Coefficients: {lasso.coef_}")
Conclusion
Summarizing the findings and implications of the different models on the dataset. The analysis provides a comprehensive understanding of linear regression and its variants, and how they can be applied to real-world data for predictive modeling.
By documenting each step, including data preprocessing, model training, and evaluation, this notebook serves as a practical guide for implementing linear regression models using Python and scikit-learn. The comparisons between the models offer valuable insights into the importance of regularization techniques in predictive modeling.

