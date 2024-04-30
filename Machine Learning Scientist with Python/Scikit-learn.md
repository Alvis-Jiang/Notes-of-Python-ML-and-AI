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