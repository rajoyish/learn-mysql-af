## Primary keys
### Understanding Primary and Secondary Keys in MySQL

In MySQL, choosing the right **primary key** is crucial for optimizing data storage, retrieval, and overall database performance. Let's explore the key concepts of primary and secondary keys, and look at practical examples of how to create tables, define primary keys, and manage indexes.

---

### What is a Primary Key?

A **primary key** is a unique identifier for each record in a table. It ensures that each row can be uniquely identified and accessed efficiently. MySQL enforces several important rules:
- A primary key must be **unique**.
- A primary key **cannot be NULL**.
- MySQL automatically creates an **index** for the primary key, optimizing data retrieval.

#### Example: Creating a Table with a Primary Key

Let's create a simple `users` table, with an `id` column as the primary key and a `name` column:

```sql
CREATE TABLE users (
  id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255)
);
```

**Explanation**:
- `id`: The primary key for the table, using the `BIGINT UNSIGNED` datatype, which allows large, positive integers.
- `AUTO_INCREMENT`: Automatically generates a unique sequential number for each new row.
- `PRIMARY KEY`: Ensures that the `id` column is unique and indexed.
- `name`: A column to store user names, using the `VARCHAR(255)` datatype, which allows variable-length text strings up to 255 characters.

MySQL will automatically enforce uniqueness and create a **non-nullable** primary key on the `id` column. Additionally, MySQL generates a **clustered index** for the primary key, which means the data in the table is physically stored in the order of the `id` column.

---

### Viewing Table Structure and Indexes

After creating a table, it's essential to check its structure and see the indexes MySQL automatically creates.

#### Query: Display Table Structure
```sql
SHOW CREATE TABLE users;
```

This query shows the SQL statement used to create the table, including details about the columns, data types, and primary key.

#### Query: Display Indexes on a Table
```sql
SHOW INDEXES FROM users;
```

This query shows the **indexes** created by MySQL for the `users` table, including the one created for the primary key. Each row in the result will include the index name, the columns involved, and the uniqueness of the index.

---

### Primary Keys vs. Secondary Keys

#### **Primary Key**:
- Uniquely identifies each row in the table.
- MySQL uses it to determine the **physical order** of the data in the table (in InnoDB).
- It is a **clustered index**, meaning the data is stored in the order of the primary key values.

#### **Secondary Key (Index)**:
- Any key or index that is **not** the primary key.
- It is used to optimize searches on non-primary key columns.
- It does not affect the physical order of the data.

---

### Why Choosing the Right Primary Key is Important

In MySQL’s **InnoDB** engine, the primary key plays a key role in determining how the data is stored on disk. A good primary key helps improve performance, especially for data retrieval and insertion.

#### Key Points:
- **Clustered Index**: The primary key is the basis for the clustered index, determining the order in which rows are stored.
- **Auto-Incrementing IDs**: Using an **auto-incrementing integer** as the primary key, as shown in the example, is generally more efficient. New rows are added at the end of the index, requiring less rebalancing of the B+ tree.
- **Avoid Complex Keys**: Using complex keys like GUIDs or hashed values as primary keys can slow down insertions because they require more storage space and frequently rebalance the index.

---

### Example of Indexing a Non-Primary Key Column

To improve search performance on the `name` column in the `users` table, we can create a secondary index like this:

```sql
CREATE INDEX idx_name ON users (name);
```

This query creates a **secondary index** on the `name` column. Secondary indexes help speed up queries when searching by columns other than the primary key, but they do not affect the physical order of the data.

---

### Conclusion

Understanding and correctly defining **primary** and **secondary keys** is essential for efficient database design. Here are some key takeaways:
- The primary key uniquely identifies rows and determines the physical storage order in InnoDB.
- Secondary keys improve query performance by indexing non-primary key columns.
- Choosing the right primary key, typically an **auto-incrementing integer**, improves insertion efficiency and overall performance.
- MySQL automatically creates indexes for primary keys, so there's no need to manually create a duplicate index for the primary key column.

By learning how primary and secondary keys work and using the right data types, you can design scalable and highly performant MySQL databases.