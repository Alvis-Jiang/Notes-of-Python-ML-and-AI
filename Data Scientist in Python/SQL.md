# SQL (Structured Query Language)

### Tables
#### - Table rows and columns are referred to as records and fields:
* be lowercase
* have no spaces—use underscores instead
* refer to a collective group or be plural
* be singular
#### - Fields are set at database creation; there is no limit to the number of records:
![WeChat Screenshot_20240421225923](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/501100e0-0010-40aa-b6bf-93ae98ef566f)

* be lowercase
* have no spaces
* be singular
* be different from other field names
* be different from the table name*


## Introducing queries

### keywords (reserved words for operation)

```
SELECT name # field names (have order)
FROM patrons; # table

SELECT *
FORM patrons
```

Query results often called result set

### Writing queries
#### Aliasing (rename columns)
```
SELECT name AS first_name, year_hired
FROM employees;

```
![1713756423505 1](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/c7f38bbd-67f0-403c-aefc-bc8426006c11)

#### Select distinct records
![1713756477615](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/7e662507-7c42-427a-aa20-136c9ca2aa55)

#### View (a virtual table that is the result of a saved SQL SELECT statement)
- When accessed, views automatically update in response to updates in the underlying data
```
CREATE VIEW employee_hire_years AS
SELECT id, name, year_hired
FROM employees;
```

### SQL flavors (Two popular SQL flavors)

PostgreSQL
- Free and open-source relational database system
- Created at the University of California, Berkeley
- "PostgreSQL" refers to both the PostgreSQL database system and its associated SQL flavor

SQL Server
- Has free and paid versions
- Created by Microsoft
- T-SQL is Microsoft's SQL flavor, used with SQL Server databases

# Query a database
### COUNT() -- counts the number of records with a value in a field
```
SELECT COUNT(fieldname) AS assigned_new_fieldname
FROM table_name;

# count multiple fields
SELECT COUNT(fieldname1) AS assigned_new_fieldname1, COUNT(fieldname2) AS assigned_new_fieldname2
FROM table_name;

# COUNT(*) -- counts records in a table
SELECT COUNT(*) AS fieldname
FROM table_name;
```

### DISTINCT -- removes duplicates to return only unique values
```

SELECT DISTINCT field_name
FROM table_name;

# combine COUNT() with DISTINCT
SELECT COUNT(DISTINCT field_name) AS assigned_new_fieldname
FROM table_name;
```

### Query execution
#### - SQL is not processed in its written order
```
-- Order of execution
SELECT name
FROM people
LIMIT 10;
```

#### - Comma errors
![1713831556276](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/5987ad67-6e2f-4be0-897d-37a4837bea5c)


#### - Keyword errors
![1713831749198](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/9f8cb6e5-92db-452b-a28b-b3d6acab134c)


### SQL style (Holywell's style guide: https://www.sqlstyle.guide/)
- Formatting is not required, but lack of formatting  can cause issues 
- Good practices: Capitalize keywords + Add new lines
- Dealing with non-standard field names: Put non-standard field names in double-quotes("asd asd")

# Filtering numbers
## WHERE 
#### - WHERE with comparison operators
```
--
> Greater than or after
< Less than or before
= Equal to
>= Greater than or equal to
<= Less than or equal to
<> Not equal to



SELECT field_name
FROM table_name
WHERE another_field_name = 'abc'
LIMIT 5;

```

## Multiple criteria

#### - OR
```
SELECT field_name
FROM table_name
WHERE another_field_name1 = 'abc' 
	OR another_field_name2 = 'DEF
```

#### - AND
```
SELECT field_name
FROM table_name
WHERE another_field_name1 = 'abc' 
	AND another_field_name2 = 'DEF
```

#### - BETWEEN(inclusive)
```
SELECT field_name
FROM table_name
WHERE another_field_name
	BETWEEN 1995 AND 1996
```

#### - AND, OR
```
SELECT field_name
FROM table_name
WHERE (another_field_name1 =1994 OR another_field_name2 =1995)
	AND (another_field_name3 ='PG'OR another_field_name4 ='R');
```

#### - BETWEEN, AND, OR
```
SELECT field_name
FROM table_name
WHERE (another_field_name1 =1994 OR another_field_name2 =1995)
	AND (another_field_name3 ='PG'OR another_field_name4 ='R');
```

## Filtering text
#### - Filter a pattern rather than specific text -- case sensitive
- #### LIKE(Used to search for a pattern in a field)
% match zero, one, or many characters
_ match a single character
![1713835002151](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/b4dc554a-92e7-4cba-9e1b-d4f2935d2b25)

- #### NOT LIKE
![1713835065221](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/ea3fe32b-7ab3-4027-929d-8fbfe8447ec9)

#### - WHERE, IN
```
SELECT field_name
FROM Table_name
WHERE another_field_name IN (1920, 1930, 1940);
```

### NULL values -- Missing values:
- Human error
- Information not available
- Unknown

#### IS NULL/IS NOT NULL
```
SELECT field_name
FROM Table_name
WHERE another_field_name IS NULL;


SELECT field_name
FROM Table_name
WHERE another_field_name IS NOT NULL;
```

# Summarizing data 
## Aggregate functions:
### - AVG(), SUM() -- Numerical fields only
```
SELECT AVG(field_name)
FROM table_name;

SELECT SUM(field_name)
FROM table_name;
```

### - MIN(), MAX(), COUNT() -- Various data types

```
SELECT MIN(field_name) AS MIN_field_name
FROM table_name;

SELECT MAX(field_name)
FROM table_name;
```

## Summarizing subsets (with WHERE)

```
SELECT AVG(field_name)
FROM table_name
WHERE another_field_name = 2024;

SELECT SUM(field_name)
FROM table_name;
```

### ROUND(number_to_round, decimal_places)
```
SELECT ROUND(AVG(field_name)) AS average
FROM table_name
WHERE another_field_name = 2024;

-- second parameter as 0
SELECT ROUND(AVG(field_name, 0)) AS average
FROM table_name
WHERE another_field_name = 2024;

-- second parameter as negative(decimal place parameter)
SELECT ROUND(AVG(field_name), -5) AS avg_budget
FROM table_name
WHERE another_field_name >=2010;
```

## Aliasing and arithmetic

### +, - , * and  /
```
SELECT (4 + 3)
|7|

SELECT (4 * 3)
|12|

SELECT (4 - 3)
|1|

SELECT (4 / 3)
|1|

SELECT (4.0 / 3.0)
|1.333333|

--example
SELECT (field_name1 - field_name2)
FROM table_name;
```
#### difference between aggregate function and arithmetic

![1713922184030](https://github.com/Alvis-Jiang/Notes-of-Python-ML-and-AI/assets/64271338/10d7060f-7d62-413d-afca-33563fad3c57)



## Sorting results

### - ORDER BY, DESC(Text values are ordered alphabetically by default) 

```
SELECT field_name1, field_name2
FROM table_name
ORDER BY field_name2 DESC, field_name2 DES;
```

## Grouping data
### - GROUP BY single filed name
```
SELECT certification, COUNT(title) AS title_count
FROM films
GROUP BY certification;
|certification|title_count|
|-------------|-----------|
|Unrated      |62 |
|M            |5 |
|G            |112 |
|NC-17        |7 |...
```

### - GROUP BY multiple fields
```
SELECT certification, language, COUNT(title) AS title_count
FROM films
GROUP BY certification, language;
|certification|language |title_count|
|-------------|---------|-----------|
|null         |null     |5 |
|Unrated      |Japanese |2 |
|R            |Norwegian|2 |...
```

### - GROUP BY with ORDER BY
```
SELECT certification, COUNT(title) AS title_count
FROM films
GROUP BY certification
ORDER BY title_count DESC;

|certification|title_count|
|-------------|-----------|
|R            |2118 |
|PG-13        |1462 |
...
```

### - Filtering grouped data
WHERE filters individual records, HAVING filters grouped records
```
SELECT release_year
FROM films
GROUPBY release_year
HAVINGAVG(duration) >120;
```