---
title : "2.1 Tạo các bảng Iceberg (Iceberg tables)"
date : 2026-03-25
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---


1.  Để tạo Cơ sở dữ liệu (Database), sao chép và dán (copy paste) đoạn mã bên dưới vào trình soạn thảo truy vấn và nhấp vào **Run**. Bạn cần ở trong Athena Query Editor để chạy các lệnh bên dưới.

```sql
create database iceberg_database;
```

![create-iceberg-db](/images/5-Workshops/5.4/5.4.1/13.png)

---

2.  Để tạo bảng Iceberg, hãy sao chép dán đoạn mã bên dưới vào trình soạn thảo truy vấn, thay thế `<<update-s3-bucket>>` bằng S3 bucket được tạo trong phần điều kiện tiên quyết **Tạo S3 bucket** (Create S3 bucket) và nhấp vào **Run** .

Trong thuộc tính bảng mà chúng ta đã chỉ định, `'write_target_data_file_size_bytes'='536870912'`, thuộc tính này dùng để chỉ định kích thước mục tiêu tính bằng byte của các tệp được tạo bởi Athena. Trong ví dụ này, kích thước tệp mục tiêu (target file size) là 512MB.

```sql
CREATE TABLE iceberg_database.amazon_reviews_iceberg(
marketplace string,
customer_id string,
review_id string,
product_category string,
product_id string,
product_parent string,
product_title string,
star_rating int,
helpful_votes int,
total_votes int,
vine string,
verified_purchase string,
review_headline string,
review_body string,
review_date bigint)
LOCATION 's3://<<update-s3-bucket>>/amazon_reviews_iceberg/'
TBLPROPERTIES (
'table_type'='ICEBERG',
'format'='parquet',
'write_target_data_file_size_bytes'='536870912'
)
```

![Create-iceberg-table](/images/5-Workshops/5.4/5.4.1/14.png)

---

3.  Chèn (insert) dữ liệu vào bảng `iceberg_database.amazon_reviews_iceberg` lấy từ `amazon_reviews_parquet`

Sao chép rồi dán (Copy Paste) truy vấn bên dưới vào trình soạn thảo và nhấp vào **Run** . (Chúng ta chỉ tải một vài Danh mục Sản phẩm (Product Categories) cho nhanh.)

Bước này có thể mất tối đa 1 phút để hoàn thành.

```sql
insert into iceberg_database.amazon_reviews_iceberg
select *
from default.amazon_reviews_parquet
where product_category in ('Gift_Card', 'Apparel','Software')
```

![insert-into-iceberg-table](/images/5-Workshops/5.4/5.4.1/15.png)

---

4.  Kiểm tra xem dữ liệu đã được tải chưa bằng cách truy vấn bảng. Sao chép và dán đoạn mã bên dưới vào trình soạn thảo truy vấn sau đó nhấp vào **Run**

```sql
SELECT * FROM "iceberg_database"."amazon_reviews_iceberg" limit 10;
```

![iceberg-test-query](/images/5-Workshops/5.4/5.4.1/16.png)

---

**Chúc mừng bạn đã tạo thành công bảng Athena Iceberg! Bây giờ hãy cùng khám phá một số tính năng chính của Athena Iceberg nhé.**