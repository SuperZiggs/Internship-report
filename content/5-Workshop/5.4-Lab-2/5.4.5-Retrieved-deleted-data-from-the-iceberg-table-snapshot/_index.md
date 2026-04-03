---
title : "2.5 Retrieved deleted data from the Iceberg table snapshot"
date : 2026-03-25
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

1.  Let's retrieve snapshot of iceberg using below table. Copy Paste below query into editor and click on **Run**

```sql
SELECT * FROM "iceberg_database"."amazon_reviews_iceberg$history"
```

![retrive-rows](/images/5-Workshops/5.4/5.4.5/23.png)

---

2.  Use below query to retrieve deleted rows from snapshot and to insert into the main table, you will need to update the `Enter Snapshot ID` before running the query. Copy Paste below query into editor and click on **Run**

```sql
insert into iceberg_database.amazon_reviews_iceberg
select * from iceberg_database.amazon_reviews_iceberg FOR VERSION AS OF  <<Enter Snapshot ID>>
where product_category = 'Software' limit 10
```

![retrive-rows](/images/5-Workshops/5.4/5.4.5/24.png)

---

3.  Query the main table to verify that deleted rows are retrieved successfully. Copy Paste below query into editor and click on **Run**

```sql
Select * from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software' limit 10
```

![rows-retrived](/images/5-Workshops/5.4/5.4.5/25.png)

