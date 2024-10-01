## 1.5 Binary strings

### The Power of Binary and Varbinary Columns in MySQL

When discussing MySQL, **CHAR** and **VARCHAR** columns are the most well-known ways to store string values. However, MySQL also provides **BINARY** and **VARBINARY** columns, which store **bytes of data** rather than characters. This makes them ideal for storing raw binary data or information that doesn't need to be represented as strings, like hashes or UUIDs.

---

### Understanding BINARY and VARBINARY Columns

The **BINARY** and **VARBINARY** columns in MySQL are similar to **CHAR** and **VARCHAR**, but with a key distinction:

- **CHAR** and **VARCHAR** store **characters** and follow character sets and collations.
- **BINARY** and **VARBINARY** store **raw bytes** and do **not** involve character sets or collations.

#### Key Differences:

- **BINARY**:
  - Fixed-length column.
  - Useful for storing a specific number of bytes.
- **VARBINARY**:
  - Variable-length column.
  - Allows storage of binary data up to a defined number of bytes, providing more flexibility than BINARY.

These columns are less common but very efficient for storing binary data such as hashes, UUIDs, or any information that doesn't need character encoding.

---

### How to Use Binary Columns in MySQL

Let’s demonstrate how to use **BINARY** and **VARBINARY** columns in MySQL by storing hashes.

#### Step 1: Create a Table with Binary Columns

We will create a table called `bins` with two columns:

- `bin`: A **BINARY** column of 16 bytes.
- `varbin`: A **VARBINARY** column that can store up to 100 bytes.

```sql
CREATE TABLE bins (
  bin BINARY(16),
  varbin VARBINARY(100)
);
```

#### Step 2: Generate Binary Data Using MD5 and UNHEX

We will generate a hash using the **MD5** function. MD5 returns a hash in hexadecimal format. To store it as binary data, we use the **UNHEX** function, which converts a hexadecimal string into binary.

```sql
SELECT UNHEX(MD5('hello'));
```

The MD5 hash of "hello" is `5d41402abc4b2a76b9719d911017c592`. Using `UNHEX`, this string is converted into binary format.

#### Step 3: View Binary Data in the MySQL Command Line

To see the raw binary data, use the MySQL command-line client with specific flags to skip displaying binary data as hexadecimal.

```bash
mysql --skip-column-names --skip-binary-as-hex -e "SELECT UNHEX(MD5('hello'))"
```

The output will show the binary data like this:  
`"\x5d\x41\x40\x2a\xbc\x4b\x2a\x76\x b9\x71\x9d\x91\x10\x17\xc5\x92"`

#### Step 4: Insert Binary Data into the Table

Now that we have the binary data, we can insert it into the `bins` table.

```sql
INSERT INTO bins (bin, varbin) VALUES (UNHEX(MD5('hello')), UNHEX(MD5('hello')));
```

#### Step 5: Retrieve the Binary Data

To retrieve the binary data, simply query the table:

```sql
SELECT * FROM bins;
```

This will display the raw binary data.

#### Step 6: Convert Binary Data Back to a Readable Format

To convert the binary data back into a human-readable format, use the **HEX** function:

```sql
SELECT HEX(bin), HEX(varbin) FROM bins;
```

This will output the binary data as a string of hexadecimal characters.

---

### Conclusion

**BINARY** and **VARBINARY** columns in MySQL are powerful tools for storing binary data that doesn’t need to be represented as strings. They are particularly useful for storing:

- Hashes
- UUIDs
- Any raw binary data

By using these column types, you can store binary data more efficiently, without the need for character sets or collations. While they may not be used as frequently as **CHAR** or **VARCHAR**, they can be crucial in certain use cases where binary data needs to be stored compactly on disk.

---

These notes break down the concept of **BINARY** and **VARBINARY** columns, providing an easy-to-follow example and practical steps on how to use them in MySQL.
