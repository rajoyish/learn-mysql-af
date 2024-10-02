## 1.7 Enums

### The Power of Enums in MySQL

When working with string data in MySQL, it’s essential to store valid data in your table columns. One way to ensure this is by using **ENUMs**, a special data type that allows you to define a predefined list of allowable values for a column. Enums offer compact data storage, readability, and data validation, making them a powerful feature in MySQL.

---

### What are Enums?

**ENUMs** are a special data type in MySQL that store one of a predefined list of values. While enums look like strings, they are actually stored as **integers** behind the scenes. This provides both the readability of strings and the storage efficiency of integers.

- **How Enums Work:** When you declare an ENUM in a column, you define a set of possible values. Any value outside this list will result in an error when trying to insert data.
- **Storage:** MySQL stores enums as integers, but they behave like strings when interacting with them.

#### Example: Creating an Orders Table with Enums

Here’s how you can create a table with an enum column:

```sql
CREATE TABLE orders (
  id INT AUTO_INCREMENT PRIMARY KEY,
  size ENUM('extra small', 'small', 'medium', 'large', 'extra large')
);
```

Inserting values into the `size` column:

```sql
INSERT INTO orders (size) VALUES ('small'), ('medium'), ('large');
```

In this example, we've inserted string values into the enum column, but these are stored as integers under the hood. To demonstrate this:

```sql
SELECT size, size+0 FROM orders;
```

The output will show how the enum values are stored internally as integers:

```
+--------+--------+
| size   | size+0 |
+--------+--------+
| small  |      2 |
| medium |      3 |
| large  |      4 |
+--------+--------+
```

---

### Benefits of Enums

Using ENUMs in MySQL has several advantages:

#### 1. Data Validation

Enums enforce data validation by ensuring that only predefined values are entered into a column. For example, if someone tries to insert a value outside of the predefined options, an error will be thrown, preventing the invalid data from being stored.

#### 2. Readability

Enums allow you to store readable string values in the database, making it easier to understand the stored data at a glance. For instance, instead of using integers like 1, 2, or 3 to represent sizes, you can use 'small', 'medium', and 'large', which are more intuitive.

#### 3. Compact Data Type

Enums are stored as compact integers, making them more space-efficient than full string types. This is useful when working with large datasets where storage efficiency is important.

---

### Downsides of Enums

While enums are powerful, there are a few limitations you should be aware of:

#### 1. Changes to the Schema

If you need to add new allowable values to an enum, you will need to **alter the schema** of the table. While this isn’t difficult, it can be inconvenient, especially if your database design needs to evolve frequently.

#### 2. Sorting Behavior

MySQL sorts enum values based on their underlying integer values, not their actual string values. This can lead to unexpected results when ordering data, especially if you’re not aware of how enums are stored internally.

---

### Using Integer Enums

Using integer enums can lead to confusion, especially when you try to reference values using their integer equivalents. A **TINYINT** column is generally a better choice if you need integer-based enums, as it provides more clarity and flexibility.

---

### Conclusion

**Enums in MySQL** are a powerful tool that can simplify database design and improve data validation and readability. However, it’s important to be aware of their limitations, such as needing to alter the schema to add new values and their integer-based sorting behavior.

If used correctly, enums can make your database more efficient and easier to work with, but careful consideration should be given when choosing to use them in production environments.