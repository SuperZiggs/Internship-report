---
title : "1.2 Tạo Athena tables"
date : 2026-03-25 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

1.  Điều hướng tới [Athena Console](https://console.aws.amazon.com/athena/home) 
    
    ---
    
2.  Mở trình soạn thảo truy vấn **Query Editor** (không phải Notebook editor)
    
    ---
![open query editor](/images/5-Workshops/5.3/5.3.2/5.png)

3.  Khi đã vào trong trình soạn thảo truy vấn của Athena console, hãy nhấp vào '**Edit Settings**' như hình bên dưới:
    

![Athena console](/images/5-Workshops/5.3/5.3.2/6.png)

---

1.  Nhấp vào Settings và sau đó nhấp vào **Manage**.

![Athena Settings](/images/5-Workshops/5.3/5.3.2/7.png)

---

4.  Tại phần Manage Settings, nhấp vào **Browse S3**.

![Athena Settings](/images/5-Workshops/5.3/5.3.2/8.png)

---

5.  Chọn S3 bucket bằng cách nhấp vào nút radio (hình tròn) và sau đó nhấp chọn **Choose**. Đối với các bài lab tự thực hành (self paced labs), hãy chọn S3 bucket mà bạn đã tạo trước đó. Đối với AWS Events, hãy chọn S3 bucket có tên bắt đầu bằng iceberg-workshop-ACCOUNTID (ví dụ: iceberg-workshop-123456789).

![Choose-S3-data-set](/images/5-Workshops/5.3/5.3.2/9.png)

---

6.  Nhấn chọn **Save** để lưu lại S3 location này.

![Save-s3-location](/images/5-Workshops/5.3/5.3.2/10.png)

---

7.  Nhấp vào **Editor** và copy paste câu truy vấn bên dưới. Cập nhật **YOURBUCKET** thành tên S3 bucket của bạn sau đó nhấn nút **Run**.

```sql
CREATE EXTERNAL TABLE default.amazon_reviews_parquet(
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
ROW FORMAT SERDE 
'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe' 
STORED AS INPUTFORMAT 
'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat' 
OUTPUTFORMAT 
'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION
's3://YOURBUCKET/productreviews/'; 
```

![Run-athena-create-table](/images/5-Workshops/5.3/5.3.2/11.png)

---

8.  Sao chép và dán (copy paste) mã lệnh bên dưới trong trình chỉnh sửa truy vấn và nhấn vào **Run**.

Lệnh `MSCK REPAIR TABLE` sẽ quét hệ thống tệp tin (chẳng hạn như Amazon S3) để tìm các phân vùng tương thích với Hive đã được thêm vào hệ thống tệp sau khi bảng được tạo. Lệnh `MSCK REPAIR TABLE` so sánh các phân vùng trong metadata của bảng và các phân vùng trong S3. Nếu có các phân vùng hệ thống mới xuất hiện ở cùng vị trí Amazon S3 mà bạn đã chỉ định lúc bạn tạo bảng, nó sẽ tự động thêm các phân vùng đó cho metadata và cho bảng Athena.

```sql
MSCK REPAIR TABLE default.amazon_reviews_parquet;
```
![Run-athena-repair-table](/images/5-Workshops/5.3/5.3.2/12.png)
---

**Bạn đã tạo bảng Athena thành công. Bảng Athena này (`default.amazon_reviews_parquet`) sẽ được dùng để đưa dữ liệu vào những bảng Iceberg mà chúng ta sẽ tạo trong Bài lab (Lab) tiếp theo.**