# SQL Cheat Sheet
- [`WHERE` clauses](#where-clauses)
- [Modifying data](#modifying-data)
- [Transactions](#transactions)
- [String manipulation](#string-manipulation)


## `WHERE` clauses
Negative condition
```sql
WHERE <column> != <value>
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
INSERT INTO <table> (<column 1>, <column 2>) VALUES (<value 1>, <value 2>)
```

Insert multiple rows
```sql
INSERT INTO <table> (<column 1>, <column 2>, ...) 
             VALUES 
                    (<value 1>, <value 2>, ...),
                    (<value 1>, <value 2>, ...),
                    (<value 1>, <value 2>, ...)
```

**Update** values in a row
```sql
UPDATE <table> SET <column 1> = <value 1>, <column 2> = <value 2>
```

**Delete** all rows from a table
```sql
DELETE FROM <table>;
```

## Transactions
- **Autocommit** is default: every statement you write gets saved to disk
- Switch autocommit off and begin a transaction: `BEGIN TRANSACTION;`
  - Or simply: `BEGIN;`
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


## Length
Obtain the length of a value or column use the LENGTH() function:
```sql
SELECT LENGTH(<value or column>) FROM <tables>;
SELECT * FROM <table> WHERE LENGTH(<value or column>) > <value or column>
```