---
title : "Giới thiệu về Iceberg trên AWS"
date : 2026-03-25 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

[Apache Iceberg](https://iceberg.apache.org/) là một định dạng bảng mở dành cho các tập dữ liệu cực lớn và hỗ trợ các hoạt động data lake hiện đại như chèn (insert), cập nhật (update), xóa (delete) ở cấp độ bản ghi và các truy vấn du hành thời gian (time travel). Iceberg quản lý các tập hợp tệp lớn dưới dạng bảng và cho phép tiến hóa bảng liền mạch như tiến hóa schema và phân vùng (partition). Thiết kế của Iceberg được tối ưu hóa cho việc sử dụng trên Amazon S3 và giúp đảm bảo tính đúng đắn của dữ liệu trong các kịch bản ghi đồng thời.

---

Một số thuật ngữ phổ biến trong các bảng Apache Iceberg như sau:

-   Schema – Tên và kiểu của các trường trong một bảng.
-   Partition spec – Định nghĩa về cách các giá trị phân vùng được xuất ra từ các trường dữ liệu.
-   Snapshot – Trạng thái của một bảng tại một thời điểm, bao gồm tập hợp tất cả các tệp dữ liệu.
-   Manifest list – Một tệp liệt kê các tệp manifest; mỗi snapshot có một danh sách.
-   Manifest – Một tệp liệt kê dữ liệu hoặc các tệp đã xóa; một phần con của snapshot.
-   Data file – Một tệp chứa các hàng của một bảng.
-   Delete file – Một tệp mã hóa các hàng của một bảng bị xóa theo vị trí hoặc giá trị dữ liệu.

---

Apache Iceberg theo dõi các tệp dữ liệu riêng lẻ trong một bảng thay vì các thư mục. Điều này cho phép user tạo các tệp dữ liệu tại chỗ (in-place) và chỉ thêm tệp vào bảng với một commit rõ ràng. Trạng thái bản được lưu trong file metadata. Tất cả thay đổi tạo ra file metadata mới, thay cho cái cũ. File metadata của bảng chứa schema, cấu hình phân vùng và các snapshot nội dung của bảng. Snapshot cho phép truy xuất dữ liệu hoàn chỉnh của bảng.

Mỗi snapshot là một tập hợp đầy đủ dữ liệu tại một thời điểm. Snapshot lưu trong file metadata, nhưng dữ liệu lại nằm rải rác ở các file manifest. Chuyển đổi metadata mới tạo ra sự cách ly snapshot. Người xem sẽ thấy snapshot ở thời điểm tải metadata lên và không bị thay đổi cho đến khi refresh. Đường dẫn tới tệp dữ liệu nằm ở các manifest. Snapshot bao gồm nhiều manifest, và chúng có thể chia sẻ tệp để tránh làm nặng hệ thống.

---

**Apache Iceberg trên Amazon Athena**

Bảng Iceberg hỗ trợ AWS Glue catalog, định dạng Parquet. Cú pháp:

```sql
CREATE TABLE
  [db_name.]table_name (col_name data_type [COMMENT col_comment] [, ...] )
  [PARTITIONED BY (col_name | transform, ... )]
  LOCATION 's3://DOC-EXAMPLE-BUCKET/your-folder/'
  TBLPROPERTIES ( 'table_type' ='ICEBERG' [, property_name=property_value] )
```

---

**Apache Iceberg trên Amazon EMR**

Bắt đầu với Amazon EMR 6.5.0, hỗ trợ Apache Spark 3 và Iceberg định dạng. Thiết lập cluster như sau:

```java
[{ "Classification":"iceberg-defaults",
    "Properties":{"iceberg.enabled":"true"}
}]
```

Hoặc thêm tệp '/usr/share/aws/iceberg/lib/iceberg-spark3-runtime.jar'.

Lưu ý: Workshop mất 2 giờ, sử dụng `us-east-1` region.