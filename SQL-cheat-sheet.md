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

Searching for empty values
```sql
WHERE <column> IS NULL
WHERE <column> IS NOT NULL
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

To count aggregated rows with common values, use the GROUP BY keywords:
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
- a table can have multiple unique keys
- can be modified (as long as they don't conflict with any existing values in the column)

**2. Primary Keys**
- all keys must be unique
- can never be null
- a table can only have one primary key
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


## Set Operations
> **Whilst `JOIN`s combine different *columns* together in the resulting data set, Set Operations combine different *rows* into the new data set.**

When looking at two result sets with some linkable data attributes, the following set operations can be performed:

The **UNION** and **UNION ALL** operators are used to combine the result set of two or more `SELECT` statements.
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

The **INTERSECT** operator is used to combine two `SELECT` statements, and return only common rows returned by both `SELECT` statements.
- the same rules about matching number, data type and order of columns in each data set apply
- `INTERSECT` is a way of applying multiple conditions to a result set via each `SELECT`, as only the rows matching both queries will be returned
- It can therefore often alternatively be expressed by a `JOIN` and multiple `WHERE/AND` clauses

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



- **Minus** - the results that have matches in *one* of the data sets, but not the other
  ![except diagram](https://www.studytonight.com/dbms/images/minus.jpg)







