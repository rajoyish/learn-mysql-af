## 1.6 Long strings

### Long Strings in MySQL

When managing large amounts of data in MySQL, you have several column types for storing text and binary data. **TEXT** and **BLOB** columns are specialized for handling large data, making them perfect for storing long strings or binary files. Here's a detailed breakdown of how these columns work and best practices for using them.

---

### TEXT Columns

**TEXT** columns are designed to store large amounts of character data, such as long strings of text. Unlike other string types like **VARCHAR**, which are limited in size, **TEXT** columns allow you to store significantly larger blocks of data.

However, there are a few limitations:

- **TEXT columns are not indexable** by default (without using full-text indexes).
- You **cannot sort by the full value** of a TEXT column directly. A common workaround is to index or sort only a **prefix** or subset of the text.

#### Variations of TEXT Columns:

MySQL provides four types of **TEXT** columns, each with different storage limits:

- **TINYTEXT**: Stores up to 255 characters.
- **TEXT**: Stores up to 65,535 characters (~64KB).
- **MEDIUMTEXT**: Stores up to 16,777,215 characters (~16MB).
- **LONGTEXT**: Stores up to 4,294,967,295 characters (~4GB).

Each type is designed for different storage needs, allowing flexibility depending on how much data you need to store.

---

### BLOB Columns

**BLOB** (Binary Large Object) columns are used to store **binary data**, such as images, videos, or audio files. While they are similar to **TEXT** columns in terms of size, they **do not have a character set or collation** since they store raw binary data.

#### Variations of BLOB Columns:

Like **TEXT** columns, there are four types of **BLOB** columns, each designed to hold different amounts of binary data:

- **TINYBLOB**: Stores up to 255 bytes.
- **BLOB**: Stores up to 65,535 bytes (~64KB).
- **MEDIUMBLOB**: Stores up to 16,777,215 bytes (~16MB).
- **LONGBLOB**: Stores up to 4,294,967,295 bytes (~4GB).

While **BLOB** columns can store large files, it's generally not recommended to store media files directly in the database. It's better to store such files in external storage and save a **pointer** (e.g., a file path) in the database instead.

---

### Storing Files in BLOB Columns

While **BLOB** columns can technically hold binary data such as images or audio files, it's considered a **best practice** to avoid storing large files directly in the database. Instead:

- Store files externally (e.g., in a file system or cloud storage).
- Use a **VARCHAR** column to store a reference (e.g., the file path or URL) to the actual location of the file.

This approach keeps the database efficient and improves performance.

---

### Best Practices for Using TEXT and BLOB Columns

When using **TEXT** or **BLOB** columns, it’s essential to plan how much data you need to store and how you’ll access it. Here are some best practices to follow:

1. **Only Select the Columns You Need**:

   - TEXT and BLOB data can take up a lot of storage. Therefore, you should only select these columns when necessary. For example:

   ```sql
   SELECT title, description FROM articles;
   ```

   - Avoid this:

   ```sql
   SELECT title, content FROM articles;
   ```

2. **Avoid Indexing or Sorting Entire Columns**:

   - Due to their large size, indexing or sorting a whole TEXT or BLOB column is inefficient. Instead, index or sort on a **prefix** of the column:

   ```sql
   ALTER TABLE articles ADD INDEX (content(100));
   ```

3. **Use VARCHAR for Smaller Text Data**:

   - If you only need to store a few hundred characters, **VARCHAR** is more efficient than using **TEXT** columns. **VARCHAR** columns support better indexing and sorting.

   ```sql
   CREATE TABLE users (
     username VARCHAR(255),
     bio TEXT
   );
   ```

---

### Conclusion

**TEXT** and **BLOB** columns in MySQL are powerful tools for storing large text and binary data. They provide various options depending on the amount of data you need to store, but they also come with certain limitations:

- **TEXT** columns are not easily indexed or sorted without additional steps.
- **BLOB** columns should generally be used for binary data, but it's often better to store large files externally and store references in the database.

By following best practices such as limiting your SELECT queries and avoiding indexing full columns, you can use these data types efficiently in your database design.

---

This provides a clear guide to understanding and using **TEXT** and **BLOB** columns in MySQL while following best practices for performance and efficiency.
