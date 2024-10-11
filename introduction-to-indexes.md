## Introduction to indexes

### Indexing: The Building Blocks of Database Performance

Indexing is a critical part of ensuring your queries perform efficiently. After setting up your database schema, indexing becomes the next most important step to optimize database performance. In fact, indexing is far more significant for performance than schema design—some say it's ten or even a hundred times more crucial!

This section will provide a brief overview of indexing, its characteristics, and key rules to follow when creating indexes.

---

### Characteristics of Indexes

Indexes are a separate data structure in your database that maintain a copy of part of your data. When you create an index, it creates a second data structure, distinct from the primary structure (your table). The key points to understand are:

- **Indexes maintain a copy of part of your data**: Each index represents a portion of the data from your table, and this can result in multiple copies of data if you create multiple indexes.
- **Indexes have pointers to the rows**: Every index contains pointers back to the corresponding rows in the original table, so it can retrieve the full data when needed.

---

### Rules for Creating Indexes

Creating indexes is essential for query performance, but it's also important to follow some best practices when doing so:

- **Create as many indexes as needed**: Indexes significantly improve query performance and, in turn, enhance the performance of your entire application.
- **Avoid creating too many indexes**: Too many indexes can slow down performance, especially during data modification operations (e.g., `INSERT`, `UPDATE`, `DELETE`).
- **Find the right balance**: Aim to create the minimum number of indexes needed to optimize performance. This balance is influenced by factors such as the size of your database, how often your data is updated, and the specific queries run on your database.
- **Understand access patterns**: Your decision on which indexes to create should be based on how the data will be accessed. Review the common queries in your application and design indexes around them.

---

### Creating Optimal Indexes

Unlike schema design, where you can visually inspect your data, indexing is **query-driven**. This means that indexes should be designed based on the queries your application runs, not on the structure of the data itself. Key points to consider:

- **Look at your queries**: Focus on the queries that your application runs frequently. Create indexes that can support these access patterns.
- **Developers play a key role**: Since application developers write the queries that define access patterns, they are often in the best position to decide which indexes will perform best.

---

### Wrapping Up

Indexes are essential for optimizing database performance. Once your schema is in place, focus on indexing to ensure your queries run efficiently. The key principles to remember:

- Indexes are separate data structures that improve performance.
- Balance the number of indexes you create.
- Indexing should be based on access patterns, so look at your queries closely.
- Developers are in a strong position to design effective indexes because they understand how the application accesses data.
