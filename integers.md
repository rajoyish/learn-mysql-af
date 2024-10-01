## **1.2 Integers**

### Understanding Integers: Types and Ranges in MySQL

When designing a database, selecting the right data type is crucial to optimize storage and performance. One of the most common types is **integer**, which stores whole numbers. Let's break down the types of integers, their ranges, and how to apply them effectively in MySQL.

---

### Getting Started: A Simple Example

To illustrate the use of integers, imagine a table that stores information about books. It has two columns:

- `id` (to uniquely identify each book),
- `title` (the name of the book),
- `number_of_pages` (total pages in the book).

The column `number_of_pages` needs to store an integer, but we need to decide what type of integer to use based on the expected values and storage requirements.

---

### Types of Integer Columns

MySQL provides five primary data types for storing integers:

- **TINYINT**: Smallest storage size.
- **SMALLINT**: Used for moderately small numbers.
- **MEDIUMINT**: Middle range for larger numbers.
- **INT**: General-purpose integer type.
- **BIGINT**: For very large numbers.

**Note**: Terms like `INTEGER` are just aliases (another name) for `INT`.

---

### Integer Ranges and Storage

Each integer type has a different storage size and can store a different range of values:

- **TINYINT**:
    - 1 byte of storage
    - Values: `0 to 255` (unsigned) or `128 to 127` (signed)
- **SMALLINT**:
    - 2 bytes of storage
    - Values: `0 to 65,535` (unsigned) or `32,768 to 32,767` (signed)
- **MEDIUMINT**:
    - 3 bytes of storage
    - Values: `0 to 16,777,215` (unsigned) or `8,388,608 to 8,388,607` (signed)
- **INT**:
    - 4 bytes of storage
    - Values: `0 to 4,294,967,295` (unsigned) or `2,147,483,648 to 2,147,483,647` (signed)
- **BIGINT**:
    - 8 bytes of storage
    - Values: `0 to 18,446,744,073,709,551,615` (unsigned) or `9,223,372,036,854,775,808 to 9,223,372,036,854,775,807` (signed)

---

### Understanding Signed and Unsigned Integers

In MySQL, an integer can be **signed** or **unsigned**:

- **Signed integers** can hold both positive and negative numbers.
- **Unsigned integers** can only store positive numbers, effectively doubling the positive range since there is no need to reserve space for negative values.

For example, a `TINYINT` with signed values uses one bit for the sign and seven bits for the number, allowing it to store values from `-128 to 127`. Without the sign (unsigned), it can store values from `0 to 255`.

---

### Example: Defining the Number of Pages

In our books table, if we know that the number of pages will never exceed 65,000, we can use the `SMALLINT` data type, which is more efficient than `INT`. Here’s how we define the column:

```sql
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    number_of_pages SMALLINT UNSIGNED
);
```

By using the `UNSIGNED` keyword, we specify that negative numbers are not allowed, thus increasing the positive range.

---

### Common Misconception: `INT(11)`

Sometimes you may see `INT(11)` and think that the number in parentheses affects the range of the column. However, this is a **display width**, not a storage limit. It does **not** influence the actual range of numbers that can be stored.

**Important**: This feature is deprecated and will be removed in future versions of MySQL.

---

### Conclusion

Choosing the right integer type for each column helps:

- Save storage space,
- Improve query performance,
- Make your database more efficient.

By understanding the ranges and sizes of integer types, you can design a well-structured and scalable database.

---

This guide provides an overview of integers in MySQL and demonstrates how to apply the appropriate data type based on real-world scenarios like our books table example.