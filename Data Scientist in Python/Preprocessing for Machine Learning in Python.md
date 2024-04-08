`# Exploring missing data`
`df.isna().sum()`

 # `# Dropping columns form datafram` --`Drop the Latitude and Longitude columns from volunteer`
 `volunteer_cols = volunteer.drop(['Latitude', 'Longitude'], axis=1)`

 # `# Dropping missing data --Drop rows with missing category_desc values from volunteer_cols
`volunteer_subset = volunteer_cols.dropna(subset = ['category_desc'])`

## Data types are important 
##### object: string/mixed types
##### int64: integer
##### float64: float
##### datetime64: dates and times

####  Converting a column type
`# check datafram info`
`volunteer.info()`

`# Convert the hits column to type int`
`volunteer["hits"] = volunteer["hits"].astype("int")`

## Training and test sets

`# count frequency of elements in a column`
`volunteer['category_desc'].value_counts()`

### Stratified sampling
`# Create a DataFrame with all columns except category_desc`
`X = volunteer.drop(['category_desc'], axis=1)`

`# Create a category_desc labels dataset`
`y = volunteer[['category_desc']]`

`# Use stratified sampling to split up the dataset according to the y dataset`
`X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)`

`# Print the category_desc counts from y_train`
`print(y_train['category_desc'].value_counts())`

Standardization (transform continuous data to appear normally distributed)

*  scikit-learn models assume normally distributed data
*  Using non-normal training data can induce bias 
## Practice: k-nearest neighbors model(knn) on non-scaled data
`# Split the dataset into training and test sets`
`X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)`
`knn = KNeighborsClassifier()`
  
`# Fit the knn model to the training data -- x_tain and y_train`
`knn.fit(X_train, y_train)`

`# Score the model on the test data-- predict y from the X-test`
`print(knn.score(X_test, y_test))`

## Log normalization 
`# Print out the variance of the Proline column`
`print(wine['Proline'].var())`

`# Apply the log normalization function to the Proline column`
`wine['Proline_log'] = np.log(wine['Proline'])`

`# Check the variance of the normalized Proline column````
`print(wine['Proline_log'].var())`


## Scaling data (substrate the mean and scaling each feature to have a variance of 1)

`# Ash, Alcalinity of ash, and Magnesiumcolumns in the wine dataset are all on different scales
`# Print out the variance of the Proline column`
`print(wine['Proline'].var())`
`# Import StandardScaler`
`from sklearn.preprocessing import StandardScaler`

`# Create the scaler`
`scaler = StandardScaler()`

`# Subset the DataFrame you want to scale`
`wine_subset = wine[['Ash', 'Alcalinity of ash', 'Magnesium']]`

`# Apply the scaler to wine_subset`
`wine_subset_scaled = scaler.fit_transform(wine_subset)`

## KNN on scaled data
`# fit_transform() is used on the training data so that we can scale the training data and also learn the scaling parameters of that data. Here, the model built will learn the mean and variance of the features of the training set`
`# Using the transform() method we can use the same mean and variance as it is calculated from our training data to transform our test data. Thus, the parameters learned by our model using the training data will help us to transform our test data.`

`# X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)`
`knn = KNeighborsClassifier()`

`# Instantiate a StandardScaler`
`scaler = StandardScaler()`

`# Scale the training and test features`
`X_train_scaled = scaler.fit_transform(X_train)`
`X_test_scaled = scaler.transform(X_test)`

`# Fit the k-nearest neighbors model to the training data`
`knn.fit(X_train_scaled, y_train)`

`# Score the model on the test data`

`print(knn.score(X_test_scaled, y_test))`


# Feature engineering(Creation of new features from existing ones)

### Encode categorical variable

