## Functional indexes

### Function-Based Indexes in MySQL

In MySQL, an **index** is a tool that optimizes data retrieval speed. Here, we’ll focus on **function-based indexes**, which allow indexing on expressions or functions applied to columns, rather than on the raw column values.

---

### What Are Function-Based Indexes?

- **Function-Based Indexes** enable you to index based on the result of a function applied to one or more columns.
- This is useful when you need to filter or search data based on calculated values, such as extracting a specific month from a date.

#### Real-World Example

Imagine a scenario where you want to quickly find all records where the birth month is **February**. If you have a `birthday` column in your table, you might apply the `MONTH()` function to extract the month. However, using `MONTH(birthday)` directly without indexing will be slower for large tables. Creating a function-based index on `MONTH(birthday)` improves this performance.

### How Function-Based Indexes Work

A function-based index works by:

1. Applying a function (like `MONTH()`) to a column's data.
2. Creating an index based on the results of that function.

This allows MySQL to quickly search records by using the indexed function result, benefiting from faster retrieval times.

---

### Example: Creating and Using a Function-Based Index

Let’s see how to create and use a function-based index in MySQL.

#### Step 1: Create a Sample Table

First, we’ll create a `people` table:

```sql
CREATE TABLE people (
  id INT(11) NOT NULL AUTO_INCREMENT,
  first_name VARCHAR(50) NOT NULL,
  last_name VARCHAR(50) NOT NULL,
  birthday DATE NOT NULL,
  PRIMARY KEY (id)
);
```

#### Step 2: Add a Function-Based Index

To retrieve records where the birth month is February, we create a function-based index on `MONTH(birthday)`:

```sql
ALTER TABLE people
ADD INDEX m((MONTH(birthday)));
```

In this statement:

- `m` is the name of the index.
- `(MONTH(birthday))` creates an index on the result of the `MONTH()` function applied to the `birthday` column.
- Double parentheses `((...))` specify that this is a **function-based index**.

#### Step 3: Query Using the Function-Based Index

Now, you can run a query to retrieve records where the birth month is February, using the function-based index:

```sql
SELECT *
FROM people
WHERE MONTH(birthday) = 2
LIMIT 10;
```

Here:

- The query retrieves all columns from `people` where the birth month is February.
- The **function-based index** `m` will be used in this query, making the retrieval faster.

> **Explanation**: In the query, MySQL will recognize `MONTH(birthday)` as a **possible key** due to the function-based index. You can check if the index is being used by running `EXPLAIN` on the query.

---

### Conclusion

**Function-Based Indexes** are highly useful when you need to filter based on calculated values or expressions rather than a raw column. They:

- Enable optimized search on function results.
- Improve performance in cases where traditional indexes do not work.

Consider using function-based indexes for columns where filtering, sorting, or joining is done on derived values rather than raw data.
