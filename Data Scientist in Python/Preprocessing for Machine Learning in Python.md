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
