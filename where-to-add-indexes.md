## Where to add indexes

In this guide, we will dive into how to use indexes effectively in MySQL to improve query performance. Adding the right indexes can dramatically speed up certain queries, but over-indexing can actually slow down your database. Let’s break down how and when to use indexes, explain some MySQL commands, and walk through code examples that show how indexes are applied in practice.

---

### Start with Your Queries

The first step in indexing is understanding which queries are commonly run. Your **queries should drive your indexes**, meaning that you need to analyze the access patterns and decide which columns need indexes based on how the queries interact with the data.

- **Which queries are run frequently?**
- **Which tables and columns do these queries access?**

These questions will help you decide where to place indexes. Keep in mind that not every query or column needs an index, and adding too many can negatively impact performance (especially for inserts, updates, and deletes).

### When to Use Indexes

Here are some key principles to keep in mind:

- **Don’t index every column in the `WHERE` clause**: Not all conditions need indexes. Analyze your access patterns and consider the whole query.
- **Avoid over-indexing**: Indexing every column can slow down data modification queries (like `INSERT`, `UPDATE`, `DELETE`) because MySQL has to update the indexes as well.
- **Think beyond the `WHERE` clause**: Indexes can also improve queries that involve sorting, grouping, and joining data.
- **Rework queries, if necessary**: Sometimes, it’s better to adjust the query rather than create more indexes.

### Adding an Index in MySQL

Let’s take a look at some practical examples of adding indexes and analyzing their impact on queries.

```sql
ALTER TABLE people ADD INDEX(birthday);
```

This command adds an index on the `birthday` column in the `people` table. This allows MySQL to find rows with specific birthdays or ranges of birthdays more efficiently.

### Showing Indexes in a Table

You can see the indexes currently present on a table by using the following command:

```sql
SHOW INDEXES FROM people;
```

This will show you all the indexes associated with the `people` table, including the columns being indexed and other details like uniqueness.

### Understanding Query Optimization with `EXPLAIN`

The `EXPLAIN` keyword is used to analyze how MySQL executes a query. It provides information about which indexes are used, or if none are used at all.

#### Example: Query Without an Index

```sql
EXPLAIN SELECT * FROM people WHERE first_name = 'Jeanne';
```

In this query, we are searching for people with the first name "Jeanne". If the `first_name` column is not indexed, the result from `EXPLAIN` will show `key: NULL`. This indicates that MySQL is not using any index and is likely performing a full table scan to find the matching rows.

#### Example: Query With an Indexed Column

```sql
EXPLAIN SELECT * FROM people WHERE birthday >= '1952-05-12';
```

Here, we are searching for people born on or after May 12, 1952. Since we added an index on the `birthday` column earlier, the `EXPLAIN` output will show `key: birthday`. This means MySQL is using the `birthday` index to efficiently retrieve the rows.

### Query Examples and How Indexes Affect Them

#### Example 1: Simple Search on Unindexed Column

```sql
SELECT * FROM people WHERE first_name = 'Neha';
```

In this query, we are searching for rows where `first_name = 'Neha'`. If `first_name` is not indexed, this query will likely result in a full table scan, which is slow for large datasets. Adding an index on `first_name` could improve performance for such queries.

#### Example 2: Range Query with Indexed Column

```sql
SELECT * FROM people WHERE birthday >= '1952-05-12';
```

Because we have an index on `birthday`, this query will perform much faster. MySQL will use the index to locate the rows that match the condition without scanning the entire table.

#### Example 3: Grouping and Aggregating Data with Indexes

```sql
SELECT COUNT(*) FROM people GROUP BY birthday LIMIT 10;
```

This query groups people by their birthdays and counts how many people were born on the same day. Since we have an index on `birthday`, MySQL can group the rows more efficiently. Without the index, MySQL would have to scan the entire table to group the rows.

You can also analyze this query’s execution plan using `EXPLAIN`:

```sql
EXPLAIN SELECT COUNT(*) FROM people GROUP BY birthday LIMIT 10;
```

The output will show `key: birthday`, indicating that MySQL is using the `birthday` index to optimize the grouping operation.

### Conclusion

In this guide, we learned how to use indexes to optimize MySQL queries. We covered:

- **How to let queries guide your indexing decisions**: Focus on the queries you run most frequently and analyze their access patterns.
- **Rules for adding indexes**: Avoid over-indexing, and think about how sorting, grouping, and joining also benefit from indexes.
- **How to add and view indexes**: Use `ALTER TABLE` to add indexes and `SHOW INDEXES` to view them.
- **Using `EXPLAIN` to analyze query performance**: This helps you understand whether MySQL is using an index or performing a full table scan.

By applying these best practices and analyzing your query performance, you can optimize your MySQL database while avoiding the pitfalls of over-indexing. Indexing is both a science and an art that requires ongoing analysis to maintain the right balance for your queries and workloads.
