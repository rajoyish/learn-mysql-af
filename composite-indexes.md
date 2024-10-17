## Composite indexes

### Mastering Composite Indexes in MySQL

#### Introduction to Composite Indexes

In MySQL, an **index** is used to quickly retrieve data from a database table. A **composite index** (or multi-column index) spans multiple columns, which can significantly optimize query performance when queries frequently use multiple columns together. Composite indexes allow for efficient searching, ordering, and grouping operations.

#### What is a Composite Index?

Instead of creating individual indexes for each column, a composite index combines multiple columns into a single index. This is particularly useful when a query filters or sorts data by multiple columns, as it enables MySQL to use the combined index efficiently.

#### Basic SQL Operations

Before diving into composite indexes, here's an example query to retrieve the first 10 records from the `people` table:

```sql
SELECT * FROM people LIMIT 10;
```

#### Creating and Managing Composite Indexes

##### 1. Adding a Composite Index

To improve query performance that filters by multiple columns, you can create a composite index. The following SQL creates a composite index for the `first_name`, `last_name`, and `birthday` columns:

```sql
ALTER TABLE people ADD INDEX MULTI(first_name, last_name, birthday);
```

##### 2. Dropping an Index

If an individual index on a column is no longer needed, especially after creating a composite index, it’s good practice to remove it to save space and avoid redundant indexing. You can drop an individual index on the `birthday` column like this:

```sql
ALTER TABLE people DROP INDEX birthday;
```

##### 3. Viewing Existing Indexes

You can inspect the current indexes on the `people` table by using:

```sql
SHOW INDEXES FROM people;
```

This displays all the indexes and their column sequences. For composite indexes, the sequence in which columns are indexed matters for query optimization.

#### Understanding Index Usage with `EXPLAIN`

The `EXPLAIN` statement helps you understand how MySQL uses indexes in queries. Let’s look at several examples using the composite index `MULTI` we created.

##### 1. Basic Query Using the First Column in the Composite Index

In this query, MySQL will use the `MULTI` composite index because the query filters by `first_name`, the first column in the composite index:

```sql
EXPLAIN SELECT * FROM people WHERE first_name = 'Neha' LIMIT 10;
```

Output shows the `MULTI` index is used as `key`, indicating that MySQL has utilized the composite index for this query.

##### 2. Query Without the First Column

If the query only filters by the second column (`last_name`), MySQL cannot use the composite index, because composite indexes are used from left to right. Here, the result will show `key: NULL`, indicating no index was used:

```sql
EXPLAIN SELECT * FROM people WHERE last_name = 'Neha' LIMIT 10;
```

##### 3. Using Two Columns in the Composite Index

In this query, both `first_name` and `last_name` are included, so MySQL will use the composite index:

```sql
EXPLAIN SELECT * FROM people WHERE first_name = 'Neha' AND last_name = 'Collier';
```

The output will show the `MULTI` index being used because both columns are included in the composite index.

##### 4. Using All Three Columns in the Composite Index

When all three columns (`first_name`, `last_name`, and `birthday`) are used in the query, MySQL can take full advantage of the composite index:

```sql
EXPLAIN SELECT * FROM people WHERE first_name = 'Neha' AND last_name = 'Collier' AND birthday = '1973-08-20';
```

Here, MySQL uses the full composite index, resulting in an optimized query.

##### 5. Range Condition in the Query

When a range condition (e.g., `<`) is used on one of the indexed columns, MySQL stops using the composite index after the range condition:

```sql
EXPLAIN SELECT * FROM people WHERE first_name = 'Neha' AND last_name < 'Collier' AND birthday = '1973-08-20';
```

In this case, MySQL only uses the index for the `first_name` and `last_name` columns, and stops using the index after the range condition on `last_name`.

#### Rules for Using Composite Indexes

1. **Left-to-right, no skipping**: MySQL can only use a composite index if the query filters from the leftmost column of the index. If you skip a column, MySQL will not use the index efficiently.
2. **Stops at the first range condition**: MySQL stops using the composite index once it encounters a range condition (like `<`, `>`, `BETWEEN`). After that, the index is not used for further columns.

#### Tips for Defining Composite Indexes

- **Order matters**: When creating a composite index, place the columns most frequently used in equality conditions (e.g., `=`) at the beginning, and less frequently used columns or columns with range conditions at the end.
- **Avoid redundant indexes**: If you create a composite index that covers multiple columns, remove individual indexes on those same columns unless they serve different query patterns.

#### Conclusion

Composite indexes are powerful tools for optimizing queries that involve multiple columns. By understanding how MySQL uses them and by following best practices for defining them, you can greatly enhance the performance of your queries. Use the `EXPLAIN` statement regularly to analyze how MySQL is utilizing your indexes and to ensure optimal query performance.
