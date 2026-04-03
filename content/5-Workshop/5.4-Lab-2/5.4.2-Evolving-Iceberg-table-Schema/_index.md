---
title : "2.2 Evolving Iceberg table schema"
date : 2026-03-25
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Iceberg schema updates are metadata-only changes. No data files are changed when you perform a schema update.

The Iceberg format supports the following schema evolution changes:

-   Add – Adds a new column to a table or to a nested struct.
-   Drop – Removes an existing column from a table or nested struct.
-   Rename – Renames an existing column or field in a nested struct.
-   Reorder – Changes the order of columns.
-   Type promotion – Widens the type of a column, struct field, map key, map value, or list element. Currently, the following cases are supported for Iceberg tables: \* integer to big integer \* float to double \* increasing the precision of a decimal type

---

1.  In this example we will add a column to the Iceberg table which we just created. We will add `comment` column to the table.

Copy paste below code in query editor and click on **Run**

```sql
ALTER TABLE iceberg_database.amazon_reviews_iceberg ADD COLUMNS (comment string)
```

![add-column](/images/5-Workshops/5.4/5.4.2/17.png)

---

2.  We will add `High rated` flag to the `comment` column where rating is greater or equal to 4. Copy paste below code in query editor and click on **Run**

```sql
UPDATE iceberg_database.amazon_reviews_iceberg
SET comment = 'High rated'
Where star_rating >=4;
```

It will take couple of minutes to finish this query

![update-column](/images/5-Workshops/5.4/5.4.2/18.png)

---

3.  Lets test if column is added successfully by querying the table. Copy paste below code in query editor and click on **Run**

```sql
SELECT * FROM iceberg_database.amazon_reviews_iceberg
Where star_rating >=4 limit 10
```

Scroll the result window to right you will see new column `comment` and the values.

![add-column](/images/5-Workshops/5.4/5.4.2/19.png)

