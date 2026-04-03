---
title : "2.5 Khôi phục dữ liệu đã xóa từ snapshot của bảng Iceberg"
date : 2026-03-25
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

1.  Hãy lấy snapshot của iceberg bằng cách sử dụng bảng bên dưới. Sao chép và dán truy vấn bên dưới vào trình soạn thảo và nhấp vào **Run**

```sql
SELECT * FROM "iceberg_database"."amazon_reviews_iceberg$history"
```

![retrive-rows](/images/5-Workshops/5.4/5.4.5/23.png)

---

2.  Sử dụng truy vấn bên dưới để lấy ra các hàng đã xóa từ snapshot và chèn vào bảng chính, bạn sẽ cần cập nhật đoạn `<<Enter Snapshot ID>>` trước khi chạy truy vấn. Sao chép và Dán truy vấn bên dưới vào trình soạn thảo và nhấp vào **Run**

```sql
insert into iceberg_database.amazon_reviews_iceberg
select * from iceberg_database.amazon_reviews_iceberg FOR VERSION AS OF  <<Enter Snapshot ID>>
where product_category = 'Software' limit 10
```

![retrive-rows](/images/5-Workshops/5.4/5.4.5/24.png)

---

3.  Truy vấn bảng chính để xác nhận rằng các dòng đã xóa được truy xuất thành công. Sao chép và Dán truy vấn bên dưới vào trình soạn thảo và nhấp vào **Run**

```sql
Select * from iceberg_database.amazon_reviews_iceberg
where product_category = 'Software' limit 10
```

![rows-retrived](/images/5-Workshops/5.4/5.4.5/25.png)