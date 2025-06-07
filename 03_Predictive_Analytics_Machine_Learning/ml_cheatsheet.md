# Machine Learning Cheat Sheet

## Algorithm Selection Guide

### Classification Problems

| Algorithm | Best For | Pros | Cons |
|-----------|----------|------|------|
| **Logistic Regression** | Linear relationships, baseline | Fast, interpretable, no overfitting | Only linear boundaries |
| **Decision Tree** | Interpretability needed | Easy to understand, handles mixed data | Prone to overfitting |
| **Random Forest** | Most cases, good performance | High accuracy, handles overfitting | Less interpretable, slower |
| **SVM** | High-dimensional data | Works well with many features | Slow on large datasets |
| **k-NN** | Simple patterns, small datasets | Simple, no assumptions | Sensitive to irrelevant features |

### Regression Problems

| Algorithm | Best For | Pros | Cons |
|-----------|----------|------|------|
| **Linear Regression** | Linear relationships, baseline | Fast, interpretable, simple | Only linear relationships |
| **Polynomial Regression** | Non-linear patterns | Captures curves | Easy to overfit |
| **Ridge Regression** | Multicollinearity | Prevents overfitting | Less interpretable |
| **Lasso Regression** | Feature selection needed | Automatic feature selection | Can be unstable |
| **Random Forest** | Most cases, complex patterns | High accuracy, robust | Less interpretable |

## Evaluation Metrics

### Classification Metrics

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

# Basic metrics
accuracy = accuracy_score(y_true, y_pred)
precision = precision_score(y_true, y_pred)
recall = recall_score(y_true, y_pred)
f1 = f1_score(y_true, y_pred)
```

- **Accuracy**: Overall correctness
- **Precision**: True positives / (True positives + False positives)
- **Recall**: True positives / (True positives + False negatives)
- **F1-Score**: Harmonic mean of precision and recall

### Regression Metrics

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score

# Basic metrics
mae = mean_absolute_error(y_true, y_pred)
mse = mean_squared_error(y_true, y_pred)
rmse = np.sqrt(mse)
r2 = r2_score(y_true, y_pred)
```

- **MAE**: Mean Absolute Error (robust to outliers)
- **MSE**: Mean Squared Error (penalizes large errors)
- **RMSE**: Root Mean Squared Error (same units as target)
- **R²**: Coefficient of determination (explained variance)

## Data Preprocessing

### Handling Missing Data

```python
# Drop missing values
df.dropna()

# Fill with mean/median/mode
df.fillna(df.mean())
df.fillna(df.median())
df.fillna(df.mode().iloc[0])

# Forward/backward fill
df.fillna(method='ffill')
df.fillna(method='bfill')
```

### Feature Scaling

```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler

# Standardization (mean=0, std=1)
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Normalization (min=0, max=1)
scaler = MinMaxScaler()
X_scaled = scaler.fit_transform(X)
```

### Encoding Categorical Variables

```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder

# Label Encoding (ordinal)
le = LabelEncoder()
df['category_encoded'] = le.fit_transform(df['category'])

# One-Hot Encoding (nominal)
df_encoded = pd.get_dummies(df, columns=['category'])
```

## Model Selection and Validation

### Train-Test Split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y
)
```

### Cross-Validation

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# K-Fold CV
cv_scores = cross_val_score(model, X, y, cv=5, scoring='accuracy')

# Stratified K-Fold (for classification)
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
cv_scores = cross_val_score(model, X, y, cv=skf, scoring='accuracy')
```

### Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV

# Grid Search
param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [3, 5, 10, None]
}

grid_search = GridSearchCV(
    RandomForestClassifier(random_state=42),
    param_grid, cv=5, scoring='accuracy'
)

grid_search.fit(X_train, y_train)
best_model = grid_search.best_estimator_
```

## Common Pitfalls

### Data Leakage
- **Problem**: Using future information to predict the past
- **Solution**: Careful feature engineering, proper time splits

### Overfitting
- **Signs**: High training score, low validation score
- **Solutions**: Cross-validation, regularization, more data

### Underfitting
- **Signs**: Low training and validation scores
- **Solutions**: More complex model, better features

### Imbalanced Data
- **Problem**: Unequal class distribution
- **Solutions**: Stratified sampling, class weights, SMOTE

## Python Code Templates

### Basic Classification Pipeline

```python
# Import libraries
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score, classification_report

# Load and prepare data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate
y_pred = model.predict(X_test)
accuracy = accuracy_score(y_test, y_pred)
print(f"Accuracy: {accuracy:.3f}")
print(classification_report(y_test, y_pred))
```

### Basic Regression Pipeline

```python
# Import libraries
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error, r2_score

# Load and prepare data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train, y_train)

# Evaluate
y_pred = model.predict(X_test)
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
print(f"MSE: {mse:.3f}")
print(f"R²: {r2:.3f}")
```

## Quick Reference

### When to Use Each Algorithm

1. **Start Simple**: Begin with Logistic/Linear Regression
2. **Need Interpretability**: Decision Trees, Linear models
3. **Want Best Performance**: Random Forest, Gradient Boosting
4. **Large Dataset**: Linear models, SGD
5. **Small Dataset**: k-NN, SVM
6. **High Dimensions**: Linear models, SVM

### Typical ML Workflow

1. **Understand Problem** → Define goals and metrics
2. **Explore Data** → EDA, visualizations
3. **Clean Data** → Handle missing values, outliers
4. **Feature Engineering** → Create, select, scale features
5. **Split Data** → Train/validation/test sets
6. **Train Models** → Start simple, try multiple algorithms
7. **Evaluate** → Cross-validation, multiple metrics
8. **Tune** → Hyperparameter optimization
9. **Test** → Final evaluation on holdout set
10. **Deploy** → Production considerations
