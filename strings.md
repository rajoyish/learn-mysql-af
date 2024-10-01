## 1.4 Strings

### Understanding MySQL String Data Types

When it comes to storing strings in MySQL, there are various types available. This guide will provide an overview of these types and focus on two specific categories: fixed-length and variable-length columns.

### Types of MySQL String Columns

MySQL provides multiple types for storing string data. Here’s a summary of the different types:

- **CHAR**
- **VARCHAR**
- **TINYTEXT**
- **TEXT**
- **MEDIUMTEXT**
- **LONGTEXT**
- **BINARY**
- **VARBINARY**
- **TINYBLOB**
- **BLOB**
- **MEDIUMBLOB**
- **LONGBLOB**
- **ENUM**
- **SET**

Understanding the distinction between fixed-length and variable-length columns is crucial.

### Fixed-Length Columns

Fixed-length columns are best for storing data that maintains a consistent size. Examples include two-digit prefixes, MD5 hashes, or alphanumeric serial numbers. 

- **Data Type:** `CHAR`
- **Definition:** You must specify the column size.

**Example:**

```sql
CREATE TABLE strings (
  fixed_five CHAR(5),
  fixed_32 CHAR(32)
);
```

In this example:
- `fixed_five` can store up to **5** characters.
- `fixed_32` can store up to **32** characters.

**Key Point:** Regardless of how many characters you actually store, fixed-length columns always occupy the full specified size.

### Variable-Length Columns

Variable-length columns are not confined to a fixed size. The space used depends on the actual data stored.

- **Data Type:** `VARCHAR`
- **Definition:** You must specify the maximum column size.

**Example:**

```sql
CREATE TABLE strings (
  variable_length VARCHAR(100)
);
```

In this case:
- The column `variable_length` can store up to **100** characters.

**Key Point:** Variable-length columns can be more storage-efficient as they do not occupy the full specified size.

### Character Sets and Collations

#### Character Sets

A character set defines which characters can be stored in a column. MySQL supports a variety of character sets, with **utf8** and **utf8mb4** being the most commonly used (with **utf8mb4** as the default in MySQL 8).

#### Collations

Collations determine how strings are compared and sorted. A collation is a set of rules that defines string equivalence. 

- **Default collation for MySQL 8:** `utf8mb4_0900_ai_ci`
  - **ai**: Accent-insensitive
  - **ci**: Case-insensitive

**Example:**

```sql
CREATE TABLE strings (
  variable_length VARCHAR(100) CHARSET utf8mb4 COLLATE utf8mb4_general_ci
);
```

In this example, we define:
- A `VARCHAR` column with the character set `utf8mb4` and the collation `utf8mb4_general_ci`.

### Conclusion

Understanding the differences between fixed-length and variable-length columns is essential when storing strings in MySQL. Additionally, knowing about character sets and collations is vital for determining what data can be stored in a column and how strings are compared or sorted. 

**Important Note:** MySQL allocates memory based on the maximum column size, so it's crucial to choose the smallest possible data type for the data you plan to store.