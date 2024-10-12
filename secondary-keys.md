## Secondary keys

#### What Are Secondary Keys in MySQL?

A **secondary key** in MySQL is any index that is not the primary key of a table. While every MySQL table has one primary key, it can have multiple secondary keys. These keys, often referred to as **indexes**, play a crucial role in optimizing query performance by allowing MySQL to retrieve data more efficiently. Understanding how secondary keys relate to primary keys is essential for working with MySQL databases effectively.

#### Example: Creating and Querying a Table with a Primary and Secondary Key

Let’s start with an example where we create a `people` table, insert some data, and then add a secondary key on the `name` column.

##### Create the `people` Table:

```sql
CREATE TABLE people (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

This command creates a table called `people` with three columns:

- `id` – an **auto-incrementing primary key** that uniquely identifies each person.
- `name` – a string (VARCHAR) field for the person's name, which cannot be null.
- `email` – a string field for the email address, which also cannot be null.

##### Insert Data Into the Table:

```sql
INSERT INTO people (name, email)
VALUES
    ('John Doe', 'johndoe@example.com'),
    ('Jane Smith', 'janesmith@example.com'),
    ('Robert Brown', 'robertbrown@example.com'),
    ('Emily Davis', 'emilydavis@example.com'),
    ('Michael Wilson', 'michaelwilson@example.com'),
    ('Sarah Johnson', 'sarahjohnson@example.com'),
    ('David White', 'davidwhite@example.com'),
    ('Laura Martinez', 'lauramartinez@example.com'),
    ('James Anderson', 'jamesanderson@example.com'),
    ('Sophia Clark', 'sophiaclark@example.com');
```

Here, we insert 10 rows of data, with each row containing a `name` and `email` for different people.

##### View the Data:

```sql
SELECT * FROM people;
```

This query will return all the rows from the `people` table, displaying the `id`, `name`, and `email` of each person.

#### Creating a Secondary Key

Next, we can create a **secondary key** (index) on the `name` column to optimize queries that search for people by their names.

```sql
ALTER TABLE people ADD INDEX(`name`);
```

This command creates an index on the `name` column, allowing us to query the table more efficiently when filtering by `name`.

#### Querying the `people` Table Using a Secondary Key

Now, with the index in place, we can perform a query to find all rows where the `name` is 'Sarah Johnson':

```sql
SELECT * FROM people WHERE `name` = 'Sarah Johnson';
```

This query retrieves all the rows where the `name` matches 'Sarah Johnson', returning the `id`, `name`, and `email` of the matching person.

#### How the Query Works

When you run the query:

```sql
SELECT * FROM people WHERE `name` = 'Sarah Johnson';
```

MySQL uses the **B-tree index** that was created on the `name` column to speed up the search. Here's how it works:

1. MySQL begins at the **root node** of the B-tree index for the `name` column.
2. It navigates through the branches to find the leaf node corresponding to 'Sarah Johnson'.
3. The leaf node contains a **pointer** to the rest of the row's data. MySQL then performs a lookup using this pointer to retrieve the full row with the `id`, `name`, and `email` fields.

This two-step lookup is much faster than scanning the entire table, especially when dealing with large datasets.

#### Relationship Between Secondary and Primary Keys

Every **secondary key** (such as the index on `name`) stores a pointer to the corresponding **primary key** (`id`) of each row. This is because:

- The secondary key only stores the indexed column (`name`) and not all the data.
- MySQL uses the secondary key to find the **primary key** of the matching row.
- It then performs a lookup using the **primary key** to retrieve the full row's data.

#### Importance of Compact Primary Keys

Since each leaf node in the secondary key contains a reference to the **primary key**, it is crucial to use a **compact primary key**. Smaller primary keys require less storage space and allow for faster lookups. If your primary key is too large, it can slow down queries and use more disk space.

### Conclusion

Secondary keys (indexes) are essential for optimizing query performance in MySQL. They allow you to retrieve data efficiently, especially when dealing with large tables. Understanding the relationship between primary and secondary keys helps you design better databases that can handle complex queries while maintaining high performance.

In this example, we created a `people` table, added data, and used a secondary key to perform efficient queries based on the `name` column. By using both primary and secondary keys effectively, you can build well-optimized databases.
