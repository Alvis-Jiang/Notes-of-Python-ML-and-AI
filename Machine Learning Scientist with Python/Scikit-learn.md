#### - Unsupervised learning
- Uncovering hidden pattern from unlabeled data
#### - Supervised learning
- The predicted values are known
- Aim: Predict the target values of unseen data, given the features
- Feature = predictor variable = independent variable
- Target variable = dependent variable = response variable
### Types of supervised learning 
Classification: Target variable consists of categories;
Regression: Target variable is continuous

## scikit-learn syntax
```
from sklearn.module import Model
model = Model()
model.fit(X, y)
predictions = model.predict(X_new)
print(predictions)
```

#### Classifying labels of unseen data
Example: K-Nearest Neighbors
- Predict the label of a data point by:
   - Looking at the k closest labeled data points
   - Taking a majority vote

If k = 3, then the black point will be classified as Red:
![1714703600122](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/8885fc15-601f-4332-9617-0061eab6491e)


If k = 5, then the black point will be classified as Blue:
![1714703679164](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/bd1a3ea5-b296-4ad3-8f1e-b5e180605718)


### Code

```
# using scikit-learn to fit a classifier
from sklearn.neighbors import KNeighborsClassifier
X = churn_df[["total_day_charge", "total_eve_charge"]].values
y = churn_df["churn"].values
knn = KNeighborsClassifier(n_neighbors=15)
knn.fit(X, y)

# Predicting on unlabeled data
X_new = np.array([[56.8, 17.5], 
				 [24.4, 24.1], 
				 [50.1, 10.9]])

predictions = knn.predict(X_new)
```

### Measuring model performance

#### Split train/test split
```
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, 
													random_state=21, stratify=y)
knn = KNeighborsClassifier(n_neighbors=6)
knn.fit(X_train, y_train)
```
#### Model complexity and over/underfitting

##### Model complexity
![1714789216454](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/61762a39-93cf-42f3-91e8-c12300ae4f6c)


##### Check over/underfitting
```
train_accuracies = {}
test_accuracies = {}
neighbors = np.arange(1, 26)
for neighbor in neighbors: 
	knn = KNeighborsClassifier(n_neighbors=neighbor) 
	knn.fit(X_train, y_train) 
	train_accuracies[neighbor] = knn.score(X_train, y_train)    
	test_accuracies[neighbor] = knn.score(X_test, y_test)

# plot
plt.figure(figsize=(8, 6))
plt.title("KNN: Varying Number of Neighbors")
plt.plot(neighbors, train_accuracies.values(), label="Training Accuracy")
plt.plot(neighbors, test_accuracies.values(), label="Testing Accuracy")
plt.legend()
plt.xlabel("Number of Neighbors")
plt.ylabel("Accuracy")
plt.show()
```
![1714789393683](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/cf9db55f-36bd-49e7-b4f9-ad00ae76b50f)


# Regression

```
# example: predicting blood glucose levels
# Creating feature and target arrays
X = diabetes_df.drop("glucose", axis=1).values
y = diabetes_df["glucose"].values

# Making predictions from a single feature
X_bmi = X[:, 3]
# convert values into the format of column
X_bmi = X_bmi.reshape(-1, 1)
```
![1714790770000 1](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/275d2509-43f3-4ce1-af2f-73daf3db90b7)

```
# Plotting glucose vs. body mass index
import matplotlib.pyplot as plt
plt.scatter(X_bmi, y)
plt.ylabel("Blood Glucose (mg/dl)")
plt.xlabel("Body Mass Index")
plt.show()
```
![1714790951179](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/185315f0-bf61-4a1f-b2e4-f7ccd59846ba)


```
# Fitting a regression model
from sklearn.linear_model import LinearRegression
reg = LinearRegression()
reg.fit(X_bmi, y)
predictions = reg.predict(X_bmi)
plt.scatter(X_bmi, y)
plt.plot(X_bmi, predictions)
plt.ylabel("Blood Glucose (mg/dl)")
plt.xlabel("Body Mass Index")
plt.show()
```

![1714791054253](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/5db297f3-2966-4daf-a9e9-bee6419a037b)

## The basics of linear regression

### Regression mechanics

![1714791617629](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/ff686aaa-cdf5-40e0-9dbf-640c2b792746)


RSS: Residual sum of squares

![1714791805716](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/fd0e75bd-bded-43fd-8253-006539fa8b5b)

```
# Linear regression using all features
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, 
													random_state=42)

reg_all = LinearRegression()
reg_all.fit(X_train, y_train)
y_pred = reg_all.predict(X_test)
```

### R-squared (quantifies the variance in target values explained by the features, values range from 0 to 1)

![1714792775847](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/b792cb97-7824-4391-a61f-feb8933dec0a)

#### Mean squared error and root mean squared error
![1714792861218](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/7ff42572-0c8d-4fcb-93d6-1c3fda1a7ed4)


```
# Compute R-squared
r_squared = reg.score(X_test, y_test)

# RMSE in scikit-learn
from sklearn.metrics import mean_squared_error
mean_squared_error(y_test, y_pred, squared=False)
```

### Cross-validation basics
##### 5-fold cross-validation *metric  = R-squared*
More folds = More computationally expensive
![1714829171759](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/42c3dba8-e326-479a-987c-118d8069df4e)


```
from sklearn.model_selection import cross_val_score, KFold
kf = KFold(n_splits=6, shuffle=True, random_state=42)
reg = LinearRegression()
cv_results = cross_val_score(reg, X, y, cv=kf)

# Print the 95% confidence interval
print(np.quantile(cv_results, [0.025, 0.975]))
```

### Regularized regression--dealing with overfitting
- Recall: Linear regression minimizes a loss function
- It chooses a coefficient, *a*, for each feature variable, plus *b*
- Large coefficients can lead to overfitting
- Regularization: Penalize large coefficients
##### Ridge regression
- Ridge penalizes large positive or negative coefficients
- α: parameter we need to choose, picking α is similar to picking k in KNN
- α controls model complexity:
	- α = 0 = OLS (Can lead to overfitting)
	- Very high α: Can lead to underfitting
![1714831327679](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/89db8af2-13c2-4d4b-8b82-b8512d30688c)


```
from sklearn.linear_model import Ridge
scores = []
for alpha in [0.1, 1.0, 10.0, 100.0, 1000.0]: 
	ridge = Ridge(alpha=alpha) 
	ridge.fit(X_train, y_train) 
	y_pred = ridge.predict(X_test) 
	scores.append(ridge.score(X_test, y_test))
print(scores)
	

[0.2828466623222221, 0.28320633574804777, 0.2853000732200006, 0.26423984812668133, 0.19292424694100963]
```

##### Lasso regression
- Lasso can select important features of a dataset
- Shrinks the coefficients of less important features to zero
- Features not shrunk to zero are selected by lasso

![1714831512168](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/6fc5bec0-d1c9-44de-95e8-a2b1d37c029b)

```
from sklearn.linear_model import Lasso
scores = []
for alpha in [0.01, 1.0, 10.0, 20.0, 50.0]:
	lasso = Lasso(alpha=alpha)
	lasso.fit(X_train, y_train)
	lasso_pred = lasso.predict(X_test)
	scores.append(lasso.score(X_test, y_test))
print(scores)

[0.99991649071123, 0.99961700284223, 0.93882227671069, 0.74855318676232, -0.05741034640016]


```

```
# Lasso for feature selection in scikit-learn
from sklearn.linear_model import Lasso
X = diabetes_df.drop("glucose", axis=1).values
y = diabetes_df["glucose"].values
names = diabetes_df.drop("glucose", axis=1).columns
lasso = Lasso(alpha=0.1)
lasso_coef = lasso.fit(X, y).coef_
plt.bar(names, lasso_coef)
plt.xticks(rotation=45)
plt.show()
```
![1714831757221](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/6f5131fd-e460-41ff-810a-c07c6a88b86d)


## Evaluate performance model

### classification models
![1714832680827](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/435b62f5-c405-47fd-9743-3a1e1ffbff13)


#### -- Precision
- High precision = lower false positive rate
- High precision: Not many legitimate transactions are predicted to be fraudulent
![1714832750303](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/c24176f6-095e-4f9b-a663-8a07d8760a32)


#### -- Recall
- High recall = lower false negative rate
- High recall: Predicted most fraudulent transactions correctly

![1714832806387](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/2a337da9-c471-4940-95ef-e7f511827585)


#### -- FA score
![1714832883838 1](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/c995a0b2-5e07-41ad-b82b-8e2432083410)

```
# Confusion matrix in scikit-learn
from sklearn.metrics import classification_report, confusion_matrix
knn = KNeighborsClassifier(n_neighbors=7)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.4, 
													random_state=42)
knn.fit(X_train, y_train)
y_pred = knn.predict(X_test)

print(confusion_matrix(y_test, y_pred))

[[1106 11] 
 [ 183 34]]
```

```
# Classification report in scikit-learn
print(classification_report(y_test, y_pred))
```
![1714833057437](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/4be3376c-cae5-47a6-93cf-41a6996572e5)


## Logistic regression
- Logistic regression is used for classification problems
- Logistic regression outputs probabilities
- If the probability, p>0.5: data is labeled 1
- If the probability, p<0.5: data is labeled 0
- Probability thresholds: By default, logistic regression threshold = 0.5
```
from sklearn.linear_model import LogisticRegression
logreg = LogisticRegression()
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, 
													random_state=42)
logreg.fit(X_train, y_train)
y_pred = logreg.predict_proba(X_test)
```


#### ROC (Receiver operating characteristic) curve -- **performance of a classification model at all classification thresholds**

The ROC curve is above the dotted line, so the model performs better than randomly guessing the class of each observation.

![1714834213425](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/503cbbdc-f7e4-4780-a8da-359ddca971f9)

```
from sklearn.metrics import roc_curve
fpr, tpr, thresholds = roc_curve(y_test, y_pred_probs)
plt.plot([0, 1], [0, 1], 'k--')
plt.plot(fpr, tpr)
plt.xlabel('False Positive Rate')
plt.ylabel('True Positive Rate')
plt.title('Logistic Regression ROC Curve')
plt.show()
```

![1714834293796](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/3aae67c1-d680-4fd7-a144-bf6cd59bbb41)


#### ROC AUC (**Area under the ROC Curve**)
![1714834593808](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/7c2943e6-5301-4729-a055-3781929325ea)


```
from sklearn.metrics import roc_auc_score
print(roc_auc_score(y_test, y_pred_probs))

0.6700964152663693
```

###  Hyperparameter tuning
#### Hyperparameters: Parameters we specify before fitting the model
	- Ridge/lasso regression: Choosing alpha
	- KNN: Choosing n_neighbors

#### Choosing the correct hyperparameters
- Try lots of different hyperparameter values
- Fit all of them separately
- See how well they perform
- Choose the best performing values
- We can still split the data and perform cross-validation on the training set

![1714838234069](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/cb929b75-05a0-467e-a77d-8529054c19b6)

```
from sklearn.model_selection import GridSearchCV
kf = KFold(n_splits=5, shuffle=True, random_state=42)
param_grid = {"alpha": np.arange(0.0001, 1, 10),"solver": ["sag", "lsqr"]}
ridge = Ridge()
ridge_cv = GridSearchCV(ridge, param_grid, cv=kf)
ridge_cv.fit(X_train, y_train)
print(ridge_cv.best_params_, ridge_cv.best_score_)
```
![1714838337516](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/286f400c-d1c2-4b62-aebb-05dc4e6d60fd)


##### RandomizedSearchCV
```
from sklearn.model_selection import RandomizedSearchCV
kf = KFold(n_splits=5, shuffle=True, random_state=42)
aram_grid = {'alpha': np.arange(0.0001, 1, 10),"solver": ['sag', 'lsqr']}
ridge = Ridge()
ridge_cv = RandomizedSearchCV(ridge, param_grid, cv=kf, n_iter=2)
ridge_cv.fit(X_train, y_train)
print(ridge_cv.best_params_, ridge_cv.best_score_)
```
![1714918231581](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/1799834e-649c-4e24-8ace-7bfbfea3dd78)


# Preprocessing data

#### scikit-learn requirements:
- Numeric data
- No missing values

## Dealing with categorical features
- scikit-learn will not accept categorical features by default
- Need to convert categorical features into numeric values
- Convert to binary features called dummy variables: 
	- 0: Observation was NOT that category
	- 1: Observation was that category

If a category has 0 in all rows, then it implies that it is Rock, which will have the modelconstryction
![1714919740565](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/a0487edf-ba90-41b4-8c96-393d3fe59fd9)


### pandas: get_dummies()

![1714919876022](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/412b16b3-3b48-4d4c-89e1-9b93f65a0cb4)


```
import pandas as pd
music_df = pd.read_csv('music.csv')
music_dummies = pd.get_dummies(music_df["genre"], drop_first=True)
print(music_dummies.head())
```
![1714919990680](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/bf719d03-305a-4216-a63c-5cf88511cdf8)

```
music_dummies = pd.concat([music_df, music_dummies], axis=1)
music_dummies = music_dummies.drop("genre", axis=1)

# Encoding dummy variables
music_dummies = pd.get_dummies(music_df, drop_first=True)
print(music_dummies.columns)
```
![1714920144249](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/b6c51b81-9c93-47b5-b9f7-2ed9a8723190)


```
# Linear regression with dummy variables
from sklearn.model_selection import cross_val_score, KFold
from sklearn.linear_model import LinearRegression
X = music_dummies.drop("popularity", axis=1).values
y = music_dummies["popularity"].values
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, 
													random_state=42)
kf = KFold(n_splits=5, shuffle=True, random_state=42)
linreg = LinearRegression()
linreg_cv = cross_val_score(linreg, X_train, y_train, cv=kf, 
							scoring="neg_mean_squared_error")
print(np.sqrt(-linreg_cv))
```

## Handling missing data

#### - Drop missing data(No value for a feature in a particular row)
##### Drop missing observations account for less then 5% of all data
##### if there are missing values in a subset column, the entire row will be removed 
Example
![1714921557369](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/0873bcf5-a33d-456b-b6ad-5cf1eb338e37)

```
music_df = music_df.dropna(subset=["genre", "popularity", "loudness", "liveness", "tempo"])
print(music_df.isna().sum().sort_values())
```
![1714921606355](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/2f0b2ea3-4f59-4af9-8816-3906a3083193)


#### - Impute missing data
- Imputation - use subject-matter expertise to replace missing data with educated guesses
- Common to use the mean, can also use the median, or another value
- For categorical values, we typically use the most frequent value - the mod
- **Must split our data first, to avoid data leakage**

```
from sklearn.impute import SimpleImputer
X_cat = music_df["genre"].values.reshape(-1, 1)
X_num = music_df.drop(["genre", "popularity"], axis=1).values
y = music_df["popularity"].values
X_train_cat, X_test_cat, y_train, y_test = train_test_split(X_cat, y, 
										   test_size=0.2, random_state=12)
X_train_num, X_test_num, y_train, y_test = train_test_split(X_num, y, 
															test_size=0.2, 
															random_state=12)

# impute categoritical values
imp_cat = SimpleImputer(strategy="most_frequent")
X_train_cat = imp_cat.fit_transform(X_train_cat)
X_test_cat = imp_cat.transform(X_test_cat)

# Impute numeric values
# Instantiate an imputer
imp_num = SimpleImputer()
X_train_num = imp_num.fit_transform(X_train_num)
X_test_num = imp_num.transform(X_test_num)
X_train = np.append(X_train_num, X_train_cat, axis=1)
X_test = np.append(X_test_num, X_test_cat, axis=1)
```
 
```
# impute within a pipeline
from sklearn.pipeline import Pipeline
music_df = music_df.dropna(subset=["genre", "popularity", "loudness", "liveness", 
						   "tempo"])


# Convert music_df["genre"] to values of `1` if the row contains "Rock", otherwise change the value to 0

music_df["genre"] = np.where(music_df["genre"] == "Rock", 1, 0)
X = music_df.drop("genre", axis=1).values
y = music_df["genre"].value

steps = [("imputation", SimpleImputer()), 
		 ("logistic_regression", LogisticRegression())]

pipeline = Pipeline(steps)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, 
													random_state=42)
pipeline.fit(X_train, y_train)
pipeline.score(X_test, y_test)
```

## Centering and scaling

### How to scale data:



