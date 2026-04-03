---
title : "2.4 Xóa các hàng khỏi bảng (Delete rows from table)"
date : 2026-03-25
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Bây giờ hãy xóa một số bản ghi khỏi bảng và lấy lại dữ liệu đã xóa từ Snapshot của bảng Iceberg

1.  Xóa tất cả các bản ghi khỏi bảng có product\_category = 'Software'. Sao chép dán lệnh bên dưới vào trình soạn thảo truy vấn và nhấp **Run**

```sql
delete from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software'
```

![delete-rows](/images/5-Workshops/5.4/5.4.4/21.png)

---

2.  Bạn sẽ nhận được không có bản ghi nào (zero records) đối với truy vấn bên dưới, xác minh rằng các hàng đã bị xóa. Sao chép dán lệnh bên dưới vào trình soạn thảo truy vấn và nhấp vào **Run**

```sql
Select * from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software'
```
![delete-rows](/images/5-Workshops/5.4/5.4.4/22.png)