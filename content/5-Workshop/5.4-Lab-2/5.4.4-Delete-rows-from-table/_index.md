---
title : "2.4 Delete rows from table"
date : 2026-03-25
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Now delete some records from the table and retrieve deleted data from the Iceberg table Snapshot

1.  Delete all records from the table for product\_category = 'Software'. Copy paste below code in query editor and click on **Run**

```sql
delete from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software'
```

![delete-rows](/images/5-Workshops/5.4/5.4.4/21.png)

---

2.  You should get zero records for below query, verifying that the rows are deleted. Copy paste below code in query editor and click on **Run**

```sql
Select * from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software'
```
![delete-rows](/images/5-Workshops/5.4/5.4.4/22.png)
