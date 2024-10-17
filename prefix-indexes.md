## Prefix indexes

### Prefix Indexing in MySQL

In this section, we’ll explore **prefix indexing** in MySQL, a technique that helps optimize performance for long string columns. Prefix indexing is especially useful when you can't index an entire column due to size, but indexing a portion of the string can provide good performance improvements.

We'll also refresh key concepts like **cardinality** and **selectivity** to help you understand how prefix indexing works in practice.

---

### Cardinality and Selectivity

Before diving into prefix indexing, it’s important to understand two related concepts:

- **Cardinality**: This refers to the number of unique values in a column. High cardinality means many unique values, and low cardinality means fewer unique values.
- **Selectivity**: This measures the uniqueness of a column's data. It’s calculated as the ratio of unique values to the total number of rows. High selectivity means that most values are unique, while low selectivity indicates many duplicate values.

Indexes work best when they have high selectivity because this allows MySQL to efficiently narrow down search results.

---

### What is Prefix Indexing?

Prefix indexing allows you to create an index on only a portion of a string column, rather than indexing the entire string. This is especially useful when working with columns containing long strings, such as URLs, UUIDs, or hashes.

By indexing just the beginning part of the string (a prefix), you can:

- **Reduce the index size**, making it more efficient.
- **Speed up queries** that rely on the indexed column without needing to process the entire string.

---

### Advantages of Prefix Indexing

1. **Smaller Index Size**: By indexing a portion of the string, the size of the index is reduced, which can lead to faster query performance.
2. **Good for Long Strings**: It works well for long string columns (e.g., UUIDs or hashes) where full indexing would be too costly.
3. **Improved Query Performance**: Even though only part of the string is indexed, queries using that column can still be optimized.

---

### Creating a Prefix Index

To create a prefix index, you specify the number of characters from the start of the string that should be included in the index. For example, to index the first 5 characters of the `first_name` column in the `people` table, you would run:

```sql
ALTER TABLE people ADD INDEX(first_name(5)); -- Sub_part: 5
```

This command adds an index to the `first_name` column using only the first 5 characters.

You can verify the creation of the index using the following query:

```sql
SHOW INDEXES FROM people;
```

This query displays all indexes on the `people` table, including details like the `sub_part` column, which shows the number of indexed characters (in this case, 5).

---

### Understanding Prefix Index Selectivity

The effectiveness of a prefix index depends on how selective the prefix is. You want to choose a prefix length that maintains high selectivity while keeping the index size manageable.

Let’s calculate the selectivity for the `first_name` column at different prefix lengths. First, run the following query to measure the selectivity of the full column and various prefix lengths:

```sql
SELECT
	COUNT(DISTINCT first_name) / COUNT(*) AS original,
	COUNT(DISTINCT LEFT(first_name, 1)) / COUNT(*) AS left1, -- less selective
	COUNT(DISTINCT LEFT(first_name, 4)) / COUNT(*) AS left4,
	COUNT(DISTINCT LEFT(first_name, 5)) / COUNT(*) AS left5,
	COUNT(DISTINCT LEFT(first_name, 6)) / COUNT(*) AS left6,
	COUNT(DISTINCT LEFT(first_name, 7)) / COUNT(*) AS left7
FROM people;
```

Explanation:

- **`original`**: Shows the selectivity of the entire `first_name` column.
- **`left1`**: Shows the selectivity if we only indexed the first character of the name (likely to have very low selectivity).
- **`left4`** through **`left7`**: Show how selectivity improves as you index more characters. The goal is to find the smallest prefix length that achieves high selectivity (close to the original).

In this case, a higher prefix length will increase selectivity, but the optimal length is the shortest one that provides enough selectivity for your queries.

---

### Drawbacks of Prefix Indexing

While prefix indexing has many benefits, it also has some limitations:

1. **Sorting and Grouping**:
   - Prefix indexes cannot be used for sorting or grouping because they do not contain the full string value. You can still sort by the column, but the index will not be used, which may lead to slower query performance.
2. **Not Suitable for Full Matches**:
   - If your queries require matching the entire string (for example, if you need to search for exact URLs), prefix indexing might not be appropriate. In such cases, a full index might be necessary.

---

### Conclusion

**Prefix indexing** is a valuable technique for optimizing MySQL database performance, particularly when dealing with long string columns. By indexing only the first part of a string, you can create smaller, faster indexes that improve query performance while maintaining selectivity.

However, it's essential to find the optimal prefix length by analyzing the selectivity of your data. Also, keep in mind the limitations, such as its unsuitability for sorting or grouping.

Prefix indexing is a great tool for improving database performance in the right scenarios, but be sure to assess whether it fits your specific use case.
