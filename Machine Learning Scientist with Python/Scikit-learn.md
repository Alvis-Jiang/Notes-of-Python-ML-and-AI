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
# RMSE in scikit-learn
from sklearn.metrics import mean_squared_error
mean_squared_error(y_test, y_pred, squared=False)
```
