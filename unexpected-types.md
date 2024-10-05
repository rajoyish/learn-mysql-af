## 1.10 Unexpected types

### MySQL Booleans

- **MySQL doesn't have a native boolean data type.**
  - Instead, MySQL uses the `TINYINT` type to represent boolean values.
  - The `BOOLEAN` keyword in MySQL is simply a shorthand for creating a `TINYINT` column.
  - In MySQL:
    - `0` is treated as `FALSE`.
    - Any non-zero value (usually `1`) is treated as `TRUE`.

```sql
CREATE TABLE users (
  is_active BOOLEAN DEFAULT NULL
);
```

- **Alternative Method**: You can also simulate boolean logic using a zero-length character column, but this is not recommended because it can confuse the representation of your data.

---

### Zip Codes

- **Storing Zip Codes:**
  - Zip codes in the U.S. are typically 5 digits long but sometimes have leading zeros.
  - If you store zip codes as integers, leading zeros will be stripped. For example, `02134` will be stored as `2134`, which could cause issues.
  - **Best Practice**: Store zip codes as character strings to preserve the leading zeros.

```sql
CREATE TABLE addresses (
  zip_code CHAR(5)
);
```

- **Extended Zip Codes**: Some zip codes use a format like `12345-6789` (5 digits, a dash, and 4 digits). In this case, use a string column that can hold 10 characters.

```sql
CREATE TABLE addresses (
  zip_code CHAR(10)
);
```

- **Exception Example**: Some zip codes may contain letters. For instance, Saks Fifth Avenue in New York has the zip code `"10022-SHOE"`. Ensure your design handles such exceptions.

---

### IP Addresses

- **Storing IPv4 Addresses:**
  - IP addresses are typically stored as strings in their dot-decimal format (e.g., `192.168.0.1`), but storing them as integers can sometimes be more efficient for querying and analysis.
  - MySQL provides functions to convert between string and integer representations:
    - `INET_ATON()` converts an IPv4 address to an integer.
    - `INET_NTOA()` converts an integer back to an IPv4 address.

```sql
-- Storing an IP address as an integer
SELECT INET_ATON('192.168.0.1'); -- Output: 3232235521
```

- **IPv6 Addresses**: If you need to store IPv6 addresses (which are much longer than IPv4 addresses), you can use a `BINARY(16)` column to store the 128-bit IPv6 address.

```sql
CREATE TABLE network_data (
  ip_address BINARY(16)
);
```

---

### Conclusion

Understanding these unconventional data types and how to store information efficiently will help you in optimizing both your database storage and querying speed.

- **Booleans** are handled using `TINYINT`.
- **Zip codes** should be stored as strings to handle special cases like leading zeros and alphanumeric codes.
- **IP addresses** can be stored as integers for efficiency using MySQL's conversion functions.

By carefully choosing the correct data types for your needs, you can ensure that your data is not only stored efficiently but is also easy to query and manage.
