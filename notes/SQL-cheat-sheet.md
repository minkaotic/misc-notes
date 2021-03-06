# SQL Cheat Sheet
- [`WHERE` clauses](#where-clauses)
- [Modifying data](#modifying-data)
- [Transactions](#transactions)
- [String manipulation](#string-manipulation)
- [Aggregate and numeric functions](#aggregate-and-numeric-functions)
- [Date and time functions](#date-and-time-functions)
- [Table Relationships](#table-relationships)
- [Join Queries](#join-queries)
- [Set Operations](#set-operations)
- [Subqueries](#subqueries)
- [Common Table Expressions](#common-table-expressions)


## `WHERE` clauses
Negative condition
```sql
WHERE <column> != <value>
-- or
WHERE <column> <> <value>
```

Searching with a set of values
```sql
WHERE <column> IN (<value1>, <value2>, <value3>)
WHERE <column> NOT IN (<value1>, <value2>, <value3>)
```

Searching in a range (rather than using `>= <value1> AND <= <value2>`)
```sql
WHERE <column> BETWEEN <minimum> AND <maximum>
WHERE <column> NOT BETWEEN <minimum> AND <maximum>
```

Searching for a pattern (unlike `=`, it's case insensitive!)
```sql
WHERE title LIKE "Harry Potter%Fire"
WHERE title NOT LIKE "Harry Potter%Fire"
```

Searching for null values
```sql
WHERE <column> IS NULL
WHERE <column> IS NOT NULL
```

Searching for null or empty values, using null coalescing operator
```sql
WHERE ISNULL(<column>, '') = ''
```

## Modifying data
**Insert** values in the order of the columns prescribed in the schema
```sql
INSERT INTO <table> VALUES (<value 1>, <value 2>)
```

Insert values - flexible column/value order
- NB: specifying the columns like this allows us to leave out the values for columns that are nullable (by dropping them from both the list of columns and the list of values)
```sql
INSERT INTO <table> (<column 1>, <column 2>) VALUES (<value 1>, <value 2>);
```

Insert multiple rows
```sql
INSERT INTO <table> (<column 1>, <column 2>, ...) 
             VALUES 
                    (<value 1>, <value 2>, ...),
                    (<value 1>, <value 2>, ...),
                    (<value 1>, <value 2>, ...);
```

Insert results of a `SELECT` statement
```sql
INSERT INTO <table> (<column 1>, <column 2>, ...) 
SELECT <column 1>, <column 2>, ... FROM <otherTable>
-- e.g. any SELECT query that returns data matching the columns to be inserted into
```

**Update** values in a row
```sql
UPDATE <table> SET <column 1> = <value 1>, <column 2> = <value 2>;
```

**Delete** all rows from a table
```sql
DELETE FROM <table>;
```

## Transactions
- **Autocommit** is default: every statement you write gets saved to disk
- Switch autocommit off and begin a transaction: `BEGIN TRANSACTION;`
- To save all results of the statements executed since the start of the transaction to disk: `COMMIT;`
- To reset the state of the database to before the begining of the transaction: `ROLLBACK;`


## Ordering and Paging
Ordering by multiple column criteria (ordering is prioritized left to right):
```sql
SELECT * FROM <table> ORDER BY <column> [ASC|DESC], <column2> [ASC|DESC];
```

To page through results, use the `OFFSET` keyword in conjunction with the `FETCH` keyword (`FETCH NEXT x ROWS ONLY` fulfils a similar purpose to `TOP` here) -
```sql
SELECT <columns> FROM <table> OFFSET <skipped rows> ROWS FETCH NEXT <# of rows> ROWS ONLY;
```

## String manipulation
Concatenating columns or values - either use the concatenation operator `+`:
```sql
SELECT <value or column> + <value or column> + <value or column>  FROM <table>;  
```
...or use the `CONCAT()` function.
```sql
SELECT CONCAT(<value or column>, <value or column>, <value or column>) FROM <table>;

-- for example:
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM users
```

Uppercase or lowercase text
```SQL
SELECT UPPER(<value or column>) FROM <table>;
SELECT LOWER(<value or column>) FROM <table>;
```

Substring to shorten string values
```sql
SELECT SUBSTR(<value or column>, <start>, <length>) FROM <table>;
-- e.g.
SELECT name, SUBSTR(description, 1, 30) + '...' AS short_description, price FROM products;
```

Replace pieces of strings of text in a larger body of text
```sql
SELECT REPLACE(<original value or column>, <target string>, <replacement string>) FROM <table>;
-- e.g.
SELECT REPLACE(email, '@', '<at>') AS obfuscated_email FROM customers
```


## Aggregate and numeric functions
Obtain the **length** of a value using the `LENGTH()` function:
```sql
SELECT LENGTH(<value or column>) FROM <tables>;
SELECT * FROM <table> WHERE LENGTH(<value or column>) > <value or column>

-- e.g. get the 5 longest product names:
SELECT TOP 5 name, LENGTH(name) AS name_length FROM products ORDER BY name_length DESC
```

To count rows you can use the `COUNT()` function:
```sql
SELECT COUNT(*) FROM <table>;

-- e.g. get all the product rows in the book category
SELECT COUNT(*) as product_count FROM products WHERE category = 'Books';
```

Use the `DISTINCT` keyword:
```sql
-- with COUNT() to count unique entries:
SELECT COUNT(DISTINCT <column>) FROM <table>;

-- to show distinct values in a column:
SELECT DISTINCT <column> FROM <table>;
```

To count aggregated rows with common values, use the **`GROUP BY`** keywords:
```sql
SELECT COUNT(<column>) FROM <table> GROUP BY <column with common value>;

-- e.g.:
SELECT category, count (*) as product_count FROM products GROUP BY category;
-- returns results in the below format!
```
|category   |product_count|
|-----------|-------------|
|Books      |20           |
|Clothing   |6            |
|Electronics|3            |

> :point_right: You can `GROUP BY` multiple columns too, [more information here](https://stackoverflow.com/questions/2421388/using-group-by-on-multiple-columns)

To total up numeric columns use the `SUM()` function:
```sql
SELECT SUM(<numeric column) FROM <table>;

-- e.g. get top 5 highest-spending customers
SELECT TOP 5 SUM(cost) as total_spent, user_id FROM orders
  GROUP BY user_id
  ORDER BY total_spent DESC
```

> :point_right: **NB: When using `GROUP BY`, use `HAVING` to specify conditions applying to the resulting groups**

A `HAVING` clause is like a `WHERE` clause, but applies only to groups as a whole (that is, to the rows in the result set representing groups), whereas the `WHERE` clause applies to individual rows:

```sql
SELECT SUM(<numeric column) AS <alias> FROM <table>
  GROUP BY <another column>
  HAVING <alias> <operator> <value>;

-- e.g. find all users who have spent over £250
SELECT SUM(cost) as total_spent, user_id FROM orders
  GROUP BY user_id
  HAVING total_spent > 250
  ORDER BY total_spent DESC
```

To get the average value of a numeric column use the `AVG()` function:

```sql
SELECT AVG(<numeric column>) FROM <table>;

-- e.g. get the average order value per customer
SELECT user_id, AVG(cost) AS average_cost FROM orders GROUP BY user_id;
```

To get the maximum or minimum value of a numeric column use the `MAX()` and `MIN()` functions:
```sql
SELECT MAX(<numeric column>) FROM <table>;
SELECT MIN(<numeric column>) FROM <table>;
```

You can also perform basic maths operations on numeric columns, using mathematical operators (/ * + -):
```sql
SELECT <numeric column> <mathematical operator> <numeric value> FROM <table>;
```

To round the results to the desired decimal point, use `ROUND(<value>, <no of decimal points>)`.


## Date and time functions
To get the current date, time, or date time (MSSQL; other DBs have varying syntax), use:
```sql
-- current date time
GETDATE()

-- current time
CONVERT(time, GETDATE())

-- current date
CONVERT(date, GETDATE())
```

To modify a date or date time with various time intervals, use the [`DATEADD()` function](https://docs.microsoft.com/en-us/sql/t-sql/functions/dateadd-transact-sql): 
```sql
-- add one month to a date
SELECT DATEADD(month, 1, '2020-08-30');

-- go 2 days back from a date
SELECT DATEADD(day, -2, '2020-08-30');
```

[Additional date and time functions are documented here.](https://docs.microsoft.com/en-us/sql/t-sql/functions/date-and-time-data-types-and-functions-transact-sql)


## Table Relationships
### Database Keys
Database keys are crucial for linking database tables together. Three main types of DB keys are:

**1. Unique Keys**
- column set up with a constraint that all new entries must be unique
- can be null
- a table can have multiple unique key constraints
- can be modified (as long as no conflict with any existing values in the column)

**2. Primary Keys**
- all keys must be unique
- can never be null
- a table can only have one primary key constraint, although it can be a composite of more than one column)
- cannot be modified

**3. Foreign Keys**
- any columns that relate records back to the primary key in another table
- foreign key constraints can be set up that will only allow values to be added to the table if that key does exist in the primary key column of the table it relates back to
- appropriate foreign key constraints are important for referential integrity

### Different types of relationships
- Most pairs of linkable tables in a database have a **one-to-many** relationship.
- Where two entities have a **many-to-many** relationship, these will need to be resolved by creating a 'juncture table' that links up the various foreign key combinations of both tables (essentially creating 2 one-to-many relationships).
- If two entities have a **one-to-one** relationship, it often makes sense for these to be combined into the same table. Exceptions are if certain columns need to be split out for performance reasons, or if needing to extend a 3rd party table that cannot be modified.


## Join Queries
### Inner Joins
```sql
SELECT mk.MakeName, md.ModelName FROM Make AS mk
  INNER JOIN Model AS md ON mk.MakeId = md.MakeId
  WHERE mk.MakeName = 'Chevy';
```

If multiple tables are inner-joined together, the resulting data set will contain only rows that match on the specified relationships throughout all tables.

![inner join venn diagram](https://image-proxy-cdn.teamtreehouse.com/8e4d3514a77b2bf0f9f8db0f065f86bfa136919a/687474703a2f2f74726565686f7573652d70726f6a6563742d646f776e6c6f6164732e73332e616d617a6f6e6177732e636f6d2f5175657279696e6752656c6174696f6e616c4461746162617365732f636f6d706c65785f76656e6e2e706e67)

### Outer Joins
There are 3 types of outer joins:
- **Left:** Includes *all* rows from the table specified first (regardless of whether they match the join condition), and all rows from the table specified 2nd which do match the join condition
- **Right:** As left outer join, but written in reverse order
- **Full:** Includes rows from all tables joined on, regardless of whether they match the join condition

Left outer join example:
```sql
SELECT mk.MakeName, COUNT(md.ModelID) AS NumberOfModels FROM make AS mk 
  LEFT JOIN model AS md ON mk.MakeID = md.MakeID
  GROUP BY mk.MakeName;
```
![left outer join venn diagram](https://uploads.teamtreehouse.com/production/quiz-assets/77522/web_venn2.png)

### Joins with filters
You can use additional `ON` conditions in your `JOIN` statements to filter down results as well:
```sql
SELECT p.*, s.NAME AS SUBJECT FROM PERIODS AS p
LEFT JOIN CLASSES AS c ON p.ID = c.PERIOD_ID AND c.TEACHER_ID = 391
LEFT JOIN SUBJECTS AS s ON s.ID = c.SUBJECT_ID
```

This can be useful when you want the result set of this outer join to include all rows from the `PERIODS` table, even ones that don't match the `TEACHER_ID` condition, as those rows would be excluded if a `WHERE c.TEACHER_ID = 391` clause was used instead. (Alternatively you could use a CTE or subquery to achieve similar results, but this would involve a more complex query.)


## Set Operations
> **Whilst `JOIN`s combine different *columns* together in the resulting data set, Set Operations combine different *rows* into the new data set.**

When looking at two result sets with some linkable data attributes, the following set operations can be performed:

### UNION
The `UNION` and `UNION ALL` operators are used to combine the result set of two or more `SELECT` statements.
- The `UNION` operator selects only distinct values by default. To allow duplicate values, use `UNION ALL`
- Each `SELECT` statement within `UNION` must have the same number of columns
- The columns must have similar data types
- The columns must be in the same order

```sql
-- get all distinct cities from multiple tables
SELECT City FROM Customers

UNION

SELECT City FROM Suppliers
ORDER BY City;
```
![union diagram](https://www.studytonight.com/dbms/images/sql-union.jpg)

### INTERSECT
The `INTERSECT`operator is used to combine two `SELECT` statements, and return only *common rows* returned by both `SELECT` statements.
- the same rules about matching number, data type and order of columns in each data set apply

```sql
-- get supplier ids from multiple tables based on certain conditions
SELECT supplier_id
FROM suppliers
WHERE supplier_id > 78

INTERSECT

SELECT supplier_id
FROM orders
WHERE quantity <> 0;
```
![intersect diagram](https://www.studytonight.com/dbms/images/sql-intersect.jpg)

### EXCEPT
The `EXCEPT` operator combines two `SELECT` statements and returns rows from the first `SELECT` statement that are *not returned by* the second `SELECT` statement.
- the same rules about matching number, data type and order of columns in each data set apply

```sql
-- return all product ids that don't have any inventory rows
SELECT product_id
FROM products

EXCEPT

SELECT product_id
FROM inventory;
```
![except diagram](https://www.studytonight.com/dbms/images/minus.jpg)

### Ordering the results of Set Operations
:exclamation: Whilst `WHERE` clauses only apply to each specific `SELECT` statement, `ORDER BY` at the end of the 2nd query applies to the whole result set returned.

```sql
SELECT MakeName FROM Make
WHERE MakeName < 'D'

UNION

SELECT MakeName FROM ForeignMake
WHERE MakeName < 'D'
ORDER BY MakeName;
```


## Subqueries
Subqueries (also known as inner queries or nested queries) are a tool for performing operations in multiple steps. They are useful when...
- criteria for a `WHERE` clause are not specifically known, so you filter using the result of a subquery instead
- you need a temporary data set to join with other tables in your database

### Using `IN` with Subqueries to Filter Data
NB: only 1 column may be selected in a filter subquery!

```sql
SELECT * FROM Sale WHERE CarID IN (
  SELECT CarId FROM Car WHERE ModelYear = 2015
);

```

### Using a Subquery to Create a Temporary/Derived Table

**`JOIN`ing on a Subquery**
- In order to have join criteria, the data set returned by the subquery needs to be aliased
- Multiple columns may be selected in the subquery

Previous example, rewritten to use a temporary table instead:
```sql
SELECT s.*, i.ModelYear FROM Sale AS s
  INNER JOIN (
    SELECT CarID FROM Car WHERE ModelYear = 2015
   ) AS i ON i.CarID = s.CarId;
```

:bulb: Subqueries creating a derived table can be particularly useful *if the data isn't stored in the structure we want for our query.*
For example, if we want to pivot sales data by location for each sales rep:
```sql
SELECT sr.LastName, stl.SaleAmount AS StLouisAmount, col.SaleAmount AS ColumbiaAmount
  FROM SalesRep AS sr
  LEFT JOIN (
    SELECT SalesRepID, SaleAmount FROM Sale
    WHERE LocationID = 1
    GROUP BY SalesRepID
  ) AS stl ON sr.SalesRepID = stl.SalesRepID
  LEFT JOIN (
    SELECT SalesRepID, SaleAmount FROM Sale
    WHERE LocationID = 2
    GROUP BY SalesRepID
  ) AS col ON sr.SalesRepID = col.SalesRepID;
```
Will generate a result set of this shape:

| LastName | StLouisAmount | ColumbiaAmount |
|----------|---------------|----------------|
| Jones    | 49382         |                |
| Gupta    | 30031         |                |
| Williams |               | 89023          |
| Jackson  |               | 56339          |
| Santiago |               |                |


**Subquery in the `FROM` to Perform Multiple Aggregations**
For example, to group + calculate the average based on the previous aggregation results of a group + count:
```sql
SELECT AVG(sub.Count) AS AveragePerMonth, sub.SalesRepID FROM (
    SELECT SalesRepID, DATEPART('month', SaleDate) AS Month, COUNT(*) as Count from Sale
    GROUP BY SalesRepID, Month
  ) AS sub
GROUP BY sub.SalesRepID
```

### Subqueries vs Joins
Example 1 could also have been written as an `INNER JOIN` query:
```sql
SELECT s.* FROM Sale AS s
JOIN Car AS c ON c.CarID=s.CarID
WHERE c.ModelYear = 2015;
```

**:exclamation: This section still needs more figuring out! Incl. reading the following:**
- https://www.essentialsql.com/what-is-the-difference-between-a-join-and-subquery/
- https://mode.com/sql-tutorial/sql-sub-queries/
- https://www.scarydba.com/2016/10/24/sub-query-not-hurt-performance/

 
## Common Table Expressions
A CTE is a SQL query which has been named, and is then used within a longer query. CTEs are particularly useful for organising long and complex SQL queries:
- Represents a temporary result set
- Makes long queries more readable by giving descriptive names to its building blocks
- Organises queries into reusable modules
- Helps mentally break down complex data analysis problems into manageable parts
- Can be used to refactor subqueries into more readable SQL

Create CTEs using the `WITH` statement (don't use a semicolon at the end!):
```sql
WITH cte_name AS (
  -- select statement here
)
```

Then use CTE like a table:
```sql
SELECT * FROM cte_name;
```

:bulb: Separate multiple CTEs with commas!
```sql
WITH all_orders AS (
  -- select all orders
),
late_orders AS (
  -- select late queries
)

-- then use CTEs in the main query like a table (joining on it, selecting from it etc.)
```

:bulb: CTEs can also be used inside other CTEs, as long as all CTEs are declared before they are referenced.



