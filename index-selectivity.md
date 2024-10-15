## Index selectivity

### Understanding Index Cardinality and Selectivity in MySQL

Indexing is a vital part of optimizing query performance in MySQL. By adding indexes to key columns, MySQL can significantly speed up data retrieval. However, selecting the right columns to index requires an understanding of **cardinality** and **selectivity**. In this guide, we’ll explain these concepts, break down useful queries, and cover how to manage indexes in MySQL.

---

### Defining Cardinality and Selectivity

#### **Cardinality**

Cardinality refers to the number of distinct (unique) values in a column. This is important because columns with high cardinality (many unique values) often benefit more from indexing than columns with low cardinality (few unique values).

For example, a column storing people's birthdays would have a cardinality based on how many unique birth dates exist in that table.

#### **Selectivity**

Selectivity measures how effectively an index can filter records. It is calculated as the ratio of unique values in a column to the total number of rows. A higher selectivity means that the index is more efficient at narrowing down the result set.

The formula for **selectivity** is:

```plaintext
Selectivity = (Number of distinct values) / (Total number of rows)
```

Columns with high selectivity are better candidates for indexing.

---

### Index Management and Queries in MySQL

#### Adding an Index

To add an index to a column, we use the `ALTER TABLE` statement. For example:

```sql
ALTER TABLE people ADD INDEX(birthday);
```

This query adds an index on the `birthday` column of the `people` table. This index can improve query performance when filtering by birthdays.

Similarly, you can add an index to the `state` column:

```sql
ALTER TABLE people ADD INDEX(state);
```

This creates an index on the `state` column, which may be useful if you frequently query based on a person's state of residence.

#### Dropping an Index

If an index is no longer useful or needed, you can drop it using the following query:

```sql
ALTER TABLE people DROP INDEX(birthday);
```

This removes the index on the `birthday` column, which can free up system resources if the index is not improving query performance.

#### Showing Indexes

To view all indexes on a table, you can use the following query:

```sql
SHOW INDEXES FROM people;
```

This will list all the indexes on the `people` table, including details such as the column, index name, and cardinality (the number of unique values the index covers).

---

### Queries to Analyze Cardinality and Selectivity

To make informed decisions about indexing, you can calculate the cardinality and selectivity of specific columns.

#### Counting Distinct Values (Cardinality)

To find out how many unique values a column has, use the `COUNT(DISTINCT column_name)` query. For example:

```sql
SELECT COUNT(DISTINCT birthday) FROM people;
```

This query counts the number of unique birthdates in the `people` table, giving you the cardinality of the `birthday` column.

Similarly, to check the cardinality of the `state` column:

```sql
SELECT COUNT(DISTINCT state) FROM people;
```

This query counts how many unique states are stored in the `state` column.

#### Calculating Selectivity

Selectivity can be calculated by dividing the number of distinct values by the total number of rows in the table. For example:

```sql
SELECT COUNT(DISTINCT birthday) / COUNT(*) FROM people;
```

This query gives you the selectivity of the `birthday` column. If the result is close to 1, the column is highly selective (many unique values). A value close to 0 means the column is not very selective (many repeated values).

To calculate the selectivity of the `state` column:

```sql
SELECT COUNT(DISTINCT state) / COUNT(*) FROM people;
```

This will show how unique the values in the `state` column are relative to the total number of rows.

#### Selectivity of the Primary Key

The primary key, often a column like `id`, typically has the highest selectivity because each row has a unique identifier. You can calculate it with:

```sql
SELECT COUNT(DISTINCT id) / COUNT(*) FROM people;
```

This will likely return `1`, as the `id` column should be completely unique.

To check the number of unique values in the `id` column:

```sql
SELECT COUNT(DISTINCT id) FROM people;
```

This will return the total number of rows, as every row should have a unique `id`.

---

### Practical Implications of Cardinality and Selectivity

#### **Low Selectivity Example**

If a column has low selectivity, indexing may not improve performance. For example, if the `type` column has only two possible values (`user` and `admin`), and most rows have the value `user`, adding an index on this column might not be beneficial:

```sql
SELECT * FROM people WHERE type = 'user';
```

Because the majority of the table contains `user`, the index would not be selective enough to improve performance. However, the index could be more useful for queries like:

```sql
SELECT * FROM people WHERE type = 'admin';
```

In this case, the index is more selective for `admin` because fewer rows have this value.

#### When an Index Isn’t Used

If MySQL isn’t using an index as expected, it may be because the index has low selectivity. MySQL may choose a full table scan over using an inefficient index. Calculating the selectivity of the index can help diagnose this problem.

---

### Conclusion

**Cardinality** and **selectivity** are crucial when optimizing MySQL databases with indexes:

- **Cardinality** refers to the number of unique values in a column.
- **Selectivity** is the ratio of unique values to total rows and measures how effectively an index filters data.

By using these concepts, along with the right queries, you can optimize the indexing strategy in your MySQL databases, improving query performance and making your application more efficient.
