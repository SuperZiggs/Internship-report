---
title : "2.3 Truy vấn du hành thời gian và du hành phiên bản (Time travel and version travel queries)"
date : 2026-03-25
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

1.  Hãy truy xuất snapshot của bảng iceberg bằng cách sử dụng bảng bên dưới. Sao chép và dán lệnh bên dưới vào trình soạn thảo truy vấn và nhấp vào **Run**. Lưu/sao chép "snapshot\_id" cũ hơn vì bạn sẽ cần nó cho bước tiếp theo.

```sql
SELECT * FROM "iceberg_database"."amazon_reviews_iceberg$history"
```

![snapshot-timetravel](/images/5-Workshops/5.4/5.4.3/20.png)

---

2.  Bây giờ hãy truy vấn snapshot tại thời điểm trước khi chúng ta thêm cột `comment`. Sao chép dán đoạn mã bên dưới vào trình soạn thảo truy vấn, thay thế `snapshot_id` trước khi bạn nhấp **Run** đối với truy vấn bên dưới rồi sau đó nhấp vào **Run**.

Sau khi bạn chạy truy vấn với `snapshot_id` cũ hơn, hãy xác minh rằng kết quả hiển thị không có cột `comment`.

```sql
select * from iceberg_database.amazon_reviews_iceberg FOR VERSION AS OF  <<replace snapshot_id>>
where marketplace ='UK'
```

![retrive-snapshot](/images/5-Workshops/5.4/5.4.3/21.png)

---

3.  Chúng ta có thể sử dụng thời gian tĩnh `made_current_at` để truy vấn snapshot. Sao chép dán lệnh bên dưới vào trình soạn thảo truy vấn, thay thế `made_current_at time` trong truy vấn và sau đó nhấp vào **Run**.

**Ngoài ra**, chúng ta cũng có thể sử dụng **cột** `made_current_at` làm thời gian để truy vấn snapshot.

```sql
select * from iceberg_database.amazon_reviews_iceberg for TIMESTAMP AS OF TIMESTAMP '<<replace with made_current_at time>>' 
where marketplace ='UK'
```
