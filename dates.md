## 1.8 Dates

### Understanding MySQL Data Types: Storing Temporal Data

As your MySQL database grows, understanding the various data types used to store information becomes crucial. When it comes to temporal (time-related) data, MySQL offers several data types to handle different use cases. In this guide, we will explore how to store and work with temporal data, using examples to make concepts clear.

---

### The Importance of Time Data

Before deciding on which data type to use, consider **what kind of time data** you need to store. Common scenarios include:

- **Dates only**: When only the date is needed (e.g., birthdates, holidays).
- **Date and time**: When both date and time are essential (e.g., event logs, timestamps).

Choosing the right temporal data type can improve efficiency and ensure accuracy in your database.

---

### The Five Temporal Data Types in MySQL

1. **DATE**
   - **Use Case**: Storing only dates.
   - **Storage**: 3 bytes.
   - **Range**: From the year 1000 to 9999.
   - **Format**: `'YYYY-MM-DD'`
   
   Example:
   ```sql
   CREATE TABLE events (
     event_date DATE
   );
   ```

2. **DATETIME**
   - **Use Case**: Storing both date and time.
   - **Storage**: 8 bytes.
   - **Range**: From the year 1000 to 9999.
   - **Format**: `'YYYY-MM-DD HH:MM:SS'`

   **Example**: Using `DATETIME` along with `TIMESTAMP` to understand their differences.
   
   ```sql
   CREATE TABLE timezone_test (
     `timestamp` TIMESTAMP NULL,
     `datetime` DATETIME NULL
   );

   INSERT INTO timezone_test 
   VALUES ('2024-01-01 00:00:00', '2024-01-01 00:00:00');

   SELECT * FROM timezone_test;
   ```

   In this example, both `TIMESTAMP` and `DATETIME` store the same values initially. However, if we change the time zone, you will see how `TIMESTAMP` behaves differently from `DATETIME`.

   ```sql
   SET SESSION time_zone = '+05:00';
   SELECT * FROM timezone_test;
   ```

   After changing the session’s time zone to `+05:00`, the `TIMESTAMP` value will be adjusted to `2023-12-31 19:00:00`, whereas the `DATETIME` value remains unchanged.

3. **TIMESTAMP**
   - **Use Case**: Storing both date and time, with automatic time zone handling.
   - **Storage**: 4 bytes (more efficient than `DATETIME`).
   - **Range**: From 1970-01-01 to 2038-01-19.
   - **Format**: `'YYYY-MM-DD HH:MM:SS'`
   - **Time Zones**: Automatically converts values to UTC when stored and converts back to the session’s time zone when retrieved.

   **Example**: Creating a `users` table with automatic timestamp handling.
   
   ```sql
   CREATE TABLE users (
     `id` INT AUTO_INCREMENT PRIMARY KEY,
     `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     `updated_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
   );

   INSERT INTO users VALUES ();

   SELECT * FROM users;
   ```

   In this table:
   - The `created_at` column automatically stores the timestamp when the record is created.
   - The `updated_at` column updates the timestamp each time the record is modified.

4. **YEAR**
   - **Use Case**: Storing only years.
   - **Storage**: 1 byte.
   - **Range**: From 1901 to 2155.
   - **Format**: `'YYYY'`

   Example:
   ```sql
   CREATE TABLE historical_data (
     year YEAR
   );
   ```

5. **TIME**
   - **Use Case**: Storing hours, minutes, and seconds (can store intervals longer than 24 hours).
   - **Storage**: 3 bytes.
   - **Range**: Can store negative or positive time intervals.
   - **Format**: `'HH:MM:SS'`

   Example:
   ```sql
   CREATE TABLE work_hours (
     duration TIME
   );
   ```

---

### Choosing Between `DATETIME` and `TIMESTAMP`

When you need to store both date and time, you’ll typically choose between `DATETIME` and `TIMESTAMP`. Here's how to decide:

1. **Storage Size**:
   - `DATETIME`: 8 bytes.
   - `TIMESTAMP`: 4 bytes, making it more efficient in terms of storage.

2. **Range of Data**:
   - `DATETIME`: Can store data up to the year 9999.
   - `TIMESTAMP`: Limited to the range 1970-01-01 to 2038-01-19 (due to the **2038 problem**).

3. **Time Zones**:
   - **DATETIME**: Does not handle time zones; what you store is what you get.
   - **TIMESTAMP**: Automatically adjusts for time zones, converting data to UTC when stored and back to the session's time zone when retrieved.

---

### Conclusion

Understanding and correctly using MySQL's temporal data types is essential for efficient database design. The most commonly used types include `DATE`, `DATETIME`, and `TIMESTAMP`. When choosing between `DATETIME` and `TIMESTAMP`, consider the range of data, storage size, and whether your application needs time zone management.

By utilizing these data types appropriately, you can ensure that your database handles temporal data efficiently and accurately.