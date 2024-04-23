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