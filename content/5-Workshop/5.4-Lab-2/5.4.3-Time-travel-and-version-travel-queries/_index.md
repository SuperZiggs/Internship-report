---
title : "2.3 Time travel and version travel queries"
date : 2026-03-25
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

1.  Let's retrieve snapshot of iceberg using below table. Copy paste below code in query editor and click on **Run**. Save/copy the older "snapshot\_id" as you will need this for the next step.

```sql
SELECT * FROM "iceberg_database"."amazon_reviews_iceberg$history"
```

![snapshot-timetravel](/images/5-Workshops/5.4/5.4.3/20.png)

---

2.  Now query the snapshot before we added the `comment` column. Copy paste below code in query editor, replace the snapshot\_id before you **Run** below query and then click on **Run**.

Once you run the query with older snapshot\_id, verify that the results displayed doesn’t have the `comment` column.

```sql
select * from iceberg_database.amazon_reviews_iceberg FOR VERSION AS OF  <<replace snapshot_id>>
where marketplace ='UK'
```

![retrive-snapshot](/images/5-Workshops/5.4/5.4.3/21.png)

---

3.  We can use the made\_current\_at time to query the snapshot. Copy paste below code in query editor, replace the `made_current_at time` in the query and then click on **Run**.

**Alternately** we can use the `made_current_at` **column** time to query the snapshot.

```sql
select * from iceberg_database.amazon_reviews_iceberg for TIMESTAMP AS OF TIMESTAMP '<<replace with made_current_at time>>' 
where marketplace ='UK'
```


