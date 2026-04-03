---
title : "2.2 Tiến hóa schema bảng Iceberg (Evolving Iceberg table schema)"
date : 2026-03-25
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Các bản cập nhật schema bên trong Iceberg chỉ là những thay đổi liên quan đến siêu dữ liệu (metadata-only). Không có tệp dữ liệu (data files) nào bị thay đổi khi bạn thực hiện cập nhật schema.

Định dạng Iceberg hỗ trợ các thay đổi tiến hóa schema sau:

-   Add (Thêm) – Thêm một cột mới vào bảng hoặc vào một cấu trúc lồng nhau (nested struct).
-   Drop (Xóa) – Xóa một cột hiện có khỏi bảng hoặc cấu trúc lồng nhau.
-   Rename (Đổi tên) – Đổi tên một cột hoặc trường dữ liệu hiện có thành một cấu trúc lồng nhau.
-   Reorder (Sắp xếp lại) – Thay đổi thứ tự các cột.
-   Type promotion (Mở rộng kiểu dữ liệu) – Mở rộng kiểu dữ liệu của cột, trường của struct, khóa map, giá trị map, hoặc phần tử của biến danh sách. Hiện tại, các trường hợp sau đây được hỗ trợ cho bảng Iceberg: \* integer to big integer \* float to double \* tăng độ chính xác của kiểu số thập phân

---

1.  Trong ví dụ này, chúng ta sẽ thêm một cột vào bảng Iceberg mà chúng ta vừa tạo. Chúng ta sẽ thêm cột `comment` vào bảng.

Sao chép rồi dán mã bên dưới vào trình soạn thảo truy vấn và nhấp vào **Run**

```sql
ALTER TABLE iceberg_database.amazon_reviews_iceberg ADD COLUMNS (comment string)
```

![add-column](/images/5-Workshops/5.4/5.4.2/17.png)

---

2.  Chúng ta sẽ thêm cờ `High rated` vào cột `comment` ở những dòng có xếp hạng (rating) lớn hơn hoặc bằng 4. Sao chép dán mã bên dưới vào trình soạn thảo truy vấn và nhấp vào **Run**

```sql
UPDATE iceberg_database.amazon_reviews_iceberg
SET comment = 'High rated'
Where star_rating >=4;
```

Sẽ mất vài phút để hoàn thành câu truy vấn này

![update-column](/images/5-Workshops/5.4/5.4.2/18.png)

---

3.  Cùng kiểm tra xem cột đã được thêm thành công hay chưa bằng cách truy vấn dữ liệu từ bảng. Sao chép và dán lệnh bên dưới vào Query Editor và chọn **Run**

```sql
SELECT * FROM iceberg_database.amazon_reviews_iceberg
Where star_rating >=4 limit 10
```

Cuộn màn hình kết quả sang bên phải để xem cột `comment` mới cùng với các giá trị bên trong.

![add-column](/images/5-Workshops/5.4/5.4.2/19.png)