# SQL (Structured Query Language)

### Tables
#### - Table rows and columns are referred to as records and fields:
* be lowercase
* have no spaces—use underscores instead
* refer to a collective group or be plural
* be singular
#### - Fields are set at database creation; there is no limit to the number of records:
![[WeChat Screenshot_20240421225923.png]]
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
![[1713756423505 1.png]]
#### Select distinct records
![[1713756477615.png]]
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
![[1713831556276.png]]

#### - Keyword errors
![[1713831749198.png]]

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
![[1713835002151.png]]
- #### NOT LIKE
- ![[1713835065221.png]]
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

![[1713922184030.png]]


## Sorting results

### - ORDER BY, DESC (Text values are ordered alphabetically by default) 

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

## Joining data
### - inner join (look for records in both tables which match a given field)

![[1714002253654.png]]
```
-- the table.column_name format must be used when selecting columns taht exist in both tables to avoid a SQL error.

SELECT left_table.id, left_Val, right_val
FROM right_table
INNER JOIN left_table
ON right_table.id = left_table.id;

--Aliasing tables (Aliases can be used in the table.column_name syntax in SELECT and ON clauses)

SELECT P1.id, left_Val, right_val
FROM right_table AS P1
INNER JOIN left_table AS P2
ON P1.id = P2.id;

--Using USING

SELECT left_table.id, left_Val, right_val
FROM right_table AS P1
INNER JOIN left_table AS P2
USING(id);
```

### Defining relationships
#### - One-to-many relationships
![[1714004071376.png]]

#### - One-to-one relationships
![[1714004098818.png]]

#### - Many-to-many relationships
![[1714004185583.png]]

### Multiple joins

#### - Joins on joins
```
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id = right_table.id
INNER JOIN another_table
ON left_table.id = another_table.id;
```

#### - Joining on multiple keys
```
SELECT *
FROM left_table
INNER JOIN right_table
ON left_table.id1 = right_table.id1
	AND left_table.id2 = right_table.id2
```

## LEFT and RIGHT JOINs
#### - Left join will return all records in the left table, and those records in the right table that match on the joining field provided
![[1714095063194.png]]

```
-- LEFT JOIN can also be written as LEFT OUTER JOIN

SELECT left_val, right_val
FROM right_table as r
LEFT JOIN left_table as l
USING(id)
```

#### - Right join is less common than left join
![[1714181895593.png]]

```
-- RIGHT JOIN can also be written as RIGHT OUTER JOIN

SELECT *
FROM left_table 
RIGHT JOIN right_table 
ON right_table.id = left_table.id
```

## Full join
![[1714183543937.png]]

```
-- Order matters

SELECT left_table.id AS L_id, 
	   right_table.id AS R_id, 
	   left_table.val AS L_val, 
	   right_table.val AS R_val
FROM left_table
FULL JOIN right_table
USING (id);
```

#### - CROSS JOIN (creates all possible combinations of two tables)
![[1714184491653.png]]
```
SELECT id1, id2
FROM table1
CROSS JOIN table2;
```

#### - Self JOIN
-- Input table
![[1714186225726.png]]

```
SELECT p1.country AS country1, 
	   p2.country AS country2, 
	   p1.continent
FROM prime_ministers AS p1
INNERJOIN prime_ministers AS p2
ON p1.continent = p2.continent
LIMIT 10;
```
-- output
![[1714186299054.png]]

-- remove the same country 
```
SELECT p1.country AS country1, 
	   p2.country AS country2, 
	   p1.continent
FROM prime_ministers AS p1
INNERJOIN prime_ministers AS p2
ON p1.continent = p2.continent
	AND p1.country <> p2.country;
```

-- output 
![[1714186388068.png]]


## Set theory for SQL Joins

#### - UNION diagram -- same number of _fields_
* UNION takes two tables as input, and returns all records from both tables

![[1714245738820.png]]

```
SELECT * 
FROM left_table
UNION
SELECT *
FROM right_table;
```

* UNION ALL takes two tables and returns all records from both tables, including duplicates
![[1714245984300.png]]
```
SELECT *
FROM left_table
UNION ALL
SELECT *
FROM right_table;
```

#### - INTERSECT DIAGRAM
![[1714246660107.png]]

```
SELECT *
FROM left_table
INTERCET
SELECT *
FROM right_table;
```

Comparsion between INTERSECT and INNER JOIN
![[1714247195887.png]]

#### - EXCEPT
![[1714247671211.png]]

```
SELECT *
FROM left_table
EXCEPT
SELECT *
FROM right_table;
```

## Subquerying with semi joins and anti joins

#### - Semi join
![[1714248086108.png]]

```
SELECT col1 
FROM left_table
WHERE col1 IN
	(SELECT col2
	 FROM right_table
	 WHERE col2 BWETEEN 2 AND 3);
```

#### - Anti join (useful for identifying whether an incorrect number of records appears in a join)
![[1714248184319.png]]

```
SELECT col1 
FROM left_table
WHERE col1 NOT IN
	(SELECT col2
	 FROM right_table
	 WHERE col2 = B AND col2 = 3);
```

#### - Subqueries inside SELECT
```
SELECT DISTINCT field1, 
	(SELE CTCOUNT(*)
	 FROM table2
	 WHERE table1.field2 = table2.field2) AS var2
FROM table1;
```

#### - Subqueries inside FROM
```
- Query to return continents with monarchs and the year the most recent country gained independence

SELECT DISTINCT monarchs.continent, sub.most_recent
FROM monarchs, 
	(SELECT continent, 
		    MAX(indep_year) AS most_recent
	 FROM states
	 GROUPBY continent) AS sub 
WHERE monarchs.continent = sub.continent 
ORDERBY continent;
```

