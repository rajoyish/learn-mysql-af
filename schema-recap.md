## Schema recap

### Schema Recap: Overview of MySQL Schema Design

Designing a schema is a crucial part of developing a database. It serves as the **foundation** upon which all other database operations depend, making it a key component of the overall development process. A well-structured schema ensures that the database is efficient, scalable, and easy to maintain.

This section will recap the key points learned about schema design in MySQL.

---

### Understanding MySQL Data Types

The first step in designing a schema is selecting the appropriate **data types** for each column. MySQL provides a variety of data types to handle different types of data, such as:

- **Strings:** Used for textual data (e.g., `VARCHAR`, `TEXT`).
- **Numbers:** For storing numerical values, including integers and decimals (e.g., `INT`, `DECIMAL`).
- **Dates:** For storing date and time data (e.g., `DATE`, `DATETIME`, `TIMESTAMP`).
- **JSON:** For storing JSON objects, which is useful when working with semi-structured data.

Choosing the right data type is essential because it affects both how the data is stored and how efficiently queries can be run. Properly chosen data types optimize the database for **performance** and ensure that each column stores the correct kind of data.

### Choosing the Right Column Attributes

After determining the appropriate data type for each column, it is important to assign the correct **column attributes**. These attributes provide additional rules that define how the data can be stored in that column. Some key attributes include:

- **Nullable:** Defines whether a column can store `NULL` values (i.e., missing or unknown data).
- **Unsigned:** Used for numeric columns, meaning the value cannot be negative. This is useful for fields like `ID` or `Quantity` where negative numbers don't make sense.

  Example:

  ```sql
  CREATE TABLE users (
      id INT UNSIGNED AUTO_INCREMENT,
      age INT UNSIGNED,
      name VARCHAR(100) NOT NULL
  );
  ```

  - **Auto-increment:** Automatically increases the value for a column (usually used for primary keys like `id`).

Choosing the right attributes ensures that the database stores data accurately and reflects the business logic of your application.

### Keeping Schemas Small and Simple

A well-designed schema is **simple** and **minimalistic**. This means:

- Avoiding unnecessary columns and tables.
- Ensuring that the schema remains flexible enough to handle changes over time without becoming bloated.

Keeping the schema small makes it easier to manage, maintain, and **optimize**. Simpler schemas are less error-prone and reduce the complexity of queries, improving performance as the database grows.

### Example: Designing a Simple User Table

Here’s an example of a schema that uses proper data types and attributes to create an efficient user table:

```sql
CREATE TABLE users (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

- `id` is an **unsigned integer** because user IDs should never be negative.
- `username`, `email`, and `password_hash` are **strings** with appropriate length limits and set to **NOT NULL** because they are required fields.
- `created_at` is a **timestamp** column that captures when the user was created, with a default value of the current timestamp.

### Conclusion

Designing a schema is a **critical step** in database development. While requirements may evolve, careful planning and selecting the correct data types and attributes early on can set your project up for success. By following best practices—such as using the appropriate data types, setting column attributes correctly, and keeping the schema simple—you can ensure that your database is efficient, maintainable, and scalable over time.
