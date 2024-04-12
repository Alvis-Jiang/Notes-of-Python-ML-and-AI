# Introduction to Data Preprocessing
```
# Exploring missing data
df.isna().sum()

# Dropping columns form datafram --Drop the Latitude and Longitude columns from volunteer
volunteer_cols = volunteer.drop(['Latitude', 'Longitude'], axis=1)

# Dropping missing data --Drop rows with missing category_desc values from volunteer_cols
volunteer_subset = volunteer_cols.dropna(subset = ['category_desc'])
```

#### Data types are important 
##### * object: string/mixed types
##### * int64: integer
##### * float64: float
##### * datetime64: dates and times

####  Converting a column type
```
# check datafram info
volunteer.info()
volunteer.describe()

# Convert the hits column to type int
volunteer["hits"] = volunteer["hits"].astype("int")
```

## Training and test sets

```
# count frequency of elements in a column
volunteer['category_desc'].value_counts()
```

## Stratified sampling
```
# Create a DataFrame with all columns except category_desc
X = volunteer.drop(['category_desc'], axis=1)

# Create a category_desc labels dataset
y = volunteer[['category_desc']]

# Use stratified sampling to split up the dataset according to the y dataset
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)

# Print the category_desc counts from y_train
print(y_train['category_desc'].value_counts())
```



# Standardization (transform continuous data to appear normally distributed)

*  scikit-learn models assume normally distributed data
*  Using non-normal training data can induce bias 
## Practice: k-nearest neighbors model(knn) on non-scaled data
```
# Split the dataset into training and test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)
knn = KNeighborsClassifier()
  
# Fit the knn model to the training data -- x_tain and y_train
knn.fit(X_train, y_train)

# Score the model on the test data-- predict y from the X-test
print(knn.score(X_test, y_test))
```

## Log normalization 
```
# Print out the variance of the Proline column
print(wine['Proline'].var())

# Apply the log normalization function to the Proline column
wine['Proline_log'] = np.log(wine['Proline'])

# Check the variance of the normalized Proline column
print(wine['Proline_log'].var())
```


## Scaling data (substrate the mean and scaling each feature to have a variance of 1)

```
# Ash, Alcalinity of ash, and Magnesiumcolumns in the wine dataset are all on different scales
# Print out the variance of the Proline column
print(wine['Proline'].var())

# Import StandardScaler
from sklearn.preprocessing import StandardScaler

# Create the scaler
scaler = StandardScaler()

# Subset the DataFrame you want to scale
wine_subset = wine[['Ash', 'Alcalinity of ash', 'Magnesium']]

# Apply the scaler to wine_subset
wine_subset_scaled = scaler.fit_transform(wine_subset)
```

## KNN on scaled data
```
# fit_transform() is used on the training data so that we can scale the training data and also learn the scaling parameters of that data. Here, the model built will learn the mean and variance of the features of the training set
# Using the transform() method we can use the same mean and variance as it is calculated from our training data to transform our test data. Thus, the parameters learned by our model using the training data will help us to transform our test data.

X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)
knn = KNeighborsClassifier()

# Instantiate a StandardScaler
scaler = StandardScaler()

# Scale the training and test features
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Fit the k-nearest neighbors model to the training data
knn.fit(X_train_scaled, y_train)

# Score the model on the test data
print(knn.score(X_test_scaled, y_test))
```


# Feature engineering(Creation of new features from existing ones)

## Encode categorical variable -- scikit-learn
```
# Set up the LabelEncoder object
from sklearn.preprocessing import LabelEncoder
enc = LabelEncoder()

# Apply the encoding to the "column1" column
df['column1_enc'] = enc.fit_transform(df['column1'])
```

## Encode categorical variable -- one-hot
```
# Transform the category_desc column in the df
category_enc = pd.get_dummies(df["category_desc"])

# Take a look at the encoded columns
print(category_enc.head())
```

## Engineering numerical features

### Aggregating numerical features

```
# Use .loc to create a mean column in the dafaframe df
# .loc() attribute accesses a group of rows and columns by label(s) or a boolean array
df["mean"] = df.loc[:, :].mean(axis=1)
```

  
### Extracting datetime components -- pd.to_datetime()
```
# First, convert string column to date column
volunteer["start_date_converted"] = pd.to_datetime(volunteer['start_date_date'])

# Extract just the month from the converted column
volunteer["start_date_month"] = volunteer['start_date_converted'].dt.month

# Take a look at the converted and new month columns
print(volunteer[['start_date_converted', 'start_date_month']].head())
```

### Engineering text features
#### Extracting string patterns--Regular expression:
- \\d :  grab digits
- + : grab as many as possible
- \\. :   grab the decimal point
- \\d+(right side): grab digits on the right side of decimal points 
- example data:
  Length 
0    0.8 miles 
1    1.0 mile 
2     0.75 miles 
3     0.5 miles 
4     0.5 miles 

```
# Write a pattern to extract numbers and decimals
# use group(0) for the result of re.search() -- returns the complete matched subgroup by default or a tuple of matched subgroups depending on the number of arguments
def return_mileage(length):
    # Search the text for matches
    mile = re.search("\d+\.\d+", length)
    # If a value is returned, use group(0) to return the found value
    if mile is not None:
        return float(mile.group(0))

# Apply the function to the Length column and take a look at both columns
hiking["Length_num"] = hiking["Length"].apply(return_mileage)
print(hiking[["Length", "Length_num"]].head())
```


#### Vectorizing text (**a methodology in NLP to map words or phrases from vocabulary to a corresponding vector of real numbers which used to find word predictions, word similarities/semantics**.) 

#### [bag-of-words](https://en.wikipedia.org/wiki/Bag-of-words_model) (BoW): A BoW vector has the length of the entire vocabulary — that is, the set of unique words in the corpus. The vector’s values represent the frequency with which each word appears in a given text passage.

####  [TF-IDF](https://haystack.deepset.ai/components/retriever#tf-idf): Give higher relevance scores to words that occur in fewer documents within the corpus.

```
# Take the column1 text
column1_text = df['column1']

# Create the vectorizer method
tfidf_vec = TfidfVectorizer()
  
# Transform the text into tf-idf vectors
text_tfidf = tfidf_vec.fit_transform(column1_text)
```


# Feature selection

## Remove redundant features: 
- remove noisy features
- remove correlated features: features move together directionally
- remove duplicated features
#### Checking for correlated features-- Pearson correlation coefficients
```
df.corr()
```


#### Selecting features using text vectors
zip() --The `zip()` function returns a zip object, which is an iterator of tuples where the first item in each passed iterator is paired together, and then the second item in each passed iterator are paired together etc.

```
# Looking at word weights
example: 
vocab = {v:k for k,v intfidf_vec.vocabulary_.items()

def return_weights(vocab, original_vocab, vector, vector_index, top_n):
    zipped = dict(zip(vector[vector_index].indices, vector[vector_index].data))
    # Transform that zipped dict into a series
    zipped_series = pd.Series({vocab[i]:zipped[i] for i in vector[vector_index].indices})
    # Sort the series to pull out the top n weighted words
    zipped_index = zipped_series.sort_values(ascending=False)[:top_n].index
    return [original_vocab[i] for i in zipped_index]

# Print out the weighted words
print(return_weights(vocab, tfidf_vec.vocabulary_, text_tfidf, 8, 3))
```

```
def words_to_filter(vocab, original_vocab, vector, top_n):
    filter_list = []
    for i in range(0, vector.shape[0]):
        # Call the return_weights function and extend filter_list
        filtered = return_weights(vocab, original_vocab, vector, i, top_n)
        filter_list.extend(filtered)
    # Return the list in a set, so we don't get duplicate word indices
    return set(filter_list)

# Call the function to get the list of word indices
filtered_words = words_to_filter(vocab, tfidf_vec.vocabulary_, text_tfidf, 3)

# Filter the columns in text_tfidf to only those in filtered_words
filtered_text = text_tfidf[:, list(filtered_words)]
```


#### Dimensionality reduction -- PCA
###### # Difficult to interpret component
###### # End of preprocessing journey

```
#Using PCA
# Instantiate a PCA object
pca = PCA()

# Define the features and labels from the wine dataset
X = wine.drop("Type", axis=1)
y = wine["Type"]

X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y, random_state=42)

# Apply PCA to the wine dataset X vector
pca_X_train = pca.fit_transform(X_train)
pca_X_test = pca.transform(X_test)

# Look at the percentage of variance explained by the different components
print(pca.explained_variance_ratio_)
```

```
# Training a model with PCA
# Fit knn to the training data
knn = KNeighborsClassifier()
knn.fit(pca_X_train, y_train)

# Score knn on the test data and print it out
print(knn.score(pca_X_test, y_test))
```

# UFOs and preprocessing -- exercise
![[WeChat Screenshot_20240411224941.png]]

```
# Checking column types
# Print the DataFrame info
print(ufo.info())

# Change the type of seconds to float
ufo["seconds"] = ufo["seconds"].astype('float')

# Change the date column to type datetime
ufo["date"] = pd.to_datetime(ufo["date"])

# Check the column types
print(ufo.info())
```

```
# Dropping missing data
# Count the missing values in the length_of_time, state, and type columns, in that order
print(ufo[['length_of_time', 'state', 'type']].isna().sum())

# Drop rows where length_of_time, state, or type are missing
# To remove rows with missing values in specific columns, pass a list of those columns to the subset argument of .dropna().
# rows with NaN in the columns specified by `subset` are removed.
ufo_no_missing =  ufo.dropna(subset=['length_of_time', 'state', 'type'])

  

# Print out the shape of the new dataset
print(ufo_no_missing.shape)
```

```
# Categorical variables and standardization

def return_minutes(time_string):
    # Search for numbers in time_string
    num = re.search("\d+", time_string)
    if num is not None:
        return int(num.group(0))

# Apply the extraction to the length_of_time column
ufo["minutes"] = ufo["length_of_time"].apply(return_minutes)

# Take a look at the head of both of the columns
print(ufo[['minutes', 'length_of_time']].head())
```

```
# Identifying features for standardization
# Check the variance -- var()
# Check the variance of the seconds and minutes columns
print(ufo[['seconds', 'minutes']].var())

# Log normalize the seconds column
ufo["seconds_log"] = np.log(ufo["seconds"])

# Print out the variance of just the seconds_log column
print(ufo["seconds_log"].var())
```

```
# Encoding categorical variables
# Use pandas to encode us values as 1 and others as 0
ufo["country_enc"] = ufo["country"].apply(lambda x: 1 if x == "us" else 0)

# Print the number of unique type values
print(len(ufo['type'].unique()))

# Create a one-hot encoded set of the type values
type_set = pd.get_dummies(ufo['type'])

# Concatenate this set back to the ufo DataFrame
ufo = pd.concat([ufo, type_set], axis=1)
```

```
# Features from dates
# Look at the first 5 rows of the date column
print(ufo['date'].head())

# Extract the month from the date column
ufo["month"] = ufo["date"].dt.month
ufo["month"]

# Extract the year from the date column
ufo["year"] = ufo["date"].dt.year

# Take a look at the head of all three columns
print(ufo[['date', "month", "year" ]].head())
```

```
# Text vectorization
#Take a look at the head of the desc field
print(ufo['desc'].head())

# Instantiate the tfidf vectorizer object
vec = TfidfVectorizer()

# Fit and transform desc using vec
desc_tfidf = vec.fit_transform(ufo['desc'])

# Look at the number of columns and rows
print(desc_tfidf.shape)
```

```
# Selecting the ideal dataset
# Now to get rid of some of the unnecessary features in the `ufo` dataset. Because the `country` column has been encoded as `country_enc`, you can select it and drop the other columns related to location: `city`, `country`, `lat`, `long`, and `state`.

# You've engineered the `month` and `year` columns, so you no longer need the `date` or `recorded` columns. You also standardized the `seconds` column as `seconds_log`, so you can drop `seconds` and `minutes`.

#Y ou vectorized `desc`, so it can be removed. For now you'll keep `type`.

# You can also get rid of the `length_of_time` column, which is unnecessary after extracting `minutes`.

# Make a list of features to drop
to_drop = ['country','city','lat','long','state','date','recorded','seconds','minutes','length_of_time','desc',]

# Drop those features
ufo_dropped = ufo.drop(to_drop, axis=1)

# Let's also filter some words out of the text vector we created
filtered_words = words_to_filter(vocab, vec.vocabulary_, desc_tfidf, 4)
```

```
# Modeling the UFO dataset --  build a k-nearest neighbor model to predict which country the UFO sighting took place in
# Take a look at the features in the X set of data
#  The `X` dataset contains the log-normalized seconds column, the one-hot encoded type columns, as well as the month and year when the sighting took place. 
# The `y` labels are the encoded country column, where `1` is `"us"` and `0` is `"ca"` .
print(X.columns)


# Split the X and y sets
X_train, X_test, y_train, y_test = train_test_split(X, y, stratify=y,random_state=42)
 
# Fit knn to the training sets
knn = KNeighborsClassifier()
knn.fit(X_train, y_train)

# Print the score of knn on the test sets
pint(knn.score(X_test,y_test))
```

```
# Modeling the UFO dataset, part 2 -- build a model using the text vector we created, `desc_tfidf`

# Use the list of filtered words we created to filter the text vector
filtered_text = desc_tfidf[:, list(filtered_words)]

# Split the X and y sets using train_test_split, setting stratify=y 
# y: `type` of the sighting based on the text
X_train, X_test, y_train, y_test = train_test_split(filtered_text.toarray(), y, stratify=y, random_state=42)

# Fit nb to the training sets
nb.fit(X_train, y_train)

# Print the score of nb on the test sets
print(nb.score(X_test, y_test))
```
