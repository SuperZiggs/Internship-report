---
title : "3.1 Tạo EMR cluster"
date : 2026-03-25
weight : 1
chapter : false
pre : " <b> 5.5.1 </b> "
---

Nếu bạn đang chạy workshop này trên tài khoản của riêng mình, bạn cần làm theo các bước bên dưới để tạo EMR Cluster.

1.  Điều hướng đến [Amazon EMR Console](https://console.aws.amazon.com/emr/home). Trong thanh điểu hướng bên trái, trong mục **EMR on EC2**, chọn **Clusters**.

---

2.  Nhấp vào nút **Create cluster** ở góc trên cùng bên phải.

![click-create-cluster](/images/5-Workshops/5.5/5.5.1/1.png)

---

3.  Trên trang **Create cluster**, trong mục **Name and applications**, nhập tên mô tả cho cluster ở phần **Cluster name**.

![go-to-advance-option](/images/5-Workshops/5.5/5.5.1/3.png)

4.  Trong mục **Amazon EMR release**, chọn `emr-7.5.0`. Trong phần **Application bundle**, chọn **Custom** để chọn thủ công các ứng dụng sau:
    
    1.  Hadoop
    2.  Hive
    3.  JupyterEnterpriseGateway
    4.  Spark
    5.  Livy

5.  Mở rộng phần **Software settings**. Từ thanh thả xuống **Edit software settings**, chọn **Enter configuration** và dán đoạn JSON sau để bật hỗ trợ Iceberg:

```json
[{
  "Classification": "iceberg-defaults",
  "Properties": {
    "iceberg.enabled": "true"
  }
}]
```

> **Lưu ý:** Bài lab này đã được kiểm tra với `EMR 7.5.0`. Các bản phát hành EMR 7.x (7.5.0 trở lên) cũng tương thích. Sử dụng phiên bản EMR cũ hơn có thể dẫn đến hành vi không mong muốn.

![Create-Cluster-Advanced-Options](/images/5-Workshops/5.5/5.5.1/2.png)

6.  Cuộn xuống phần **Cluster termination**. Theo mặc định, tùy chọn **Automatically terminate cluster after idle time** (Tự động chấm dứt cluster sau thời gian chờ) được bật. **Vô hiệu hóa** (Disable) tùy chọn này để giữ cho cluster hoạt động trong suốt thời gian diễn ra workshop này.

![Create-Cluster-Advanced-Options](/images/5-Workshops/5.5/5.5.1/4.png)

7.  Giữ nguyên tất cả các cài đặt khác ở mức mặc định của chúng. Nhấp vào **Create cluster** để khởi chạy EMR cluster. Cluster sẽ đi qua các trạng thái `Starting` (Đang khởi động) → `Bootstrapping` (Thiết lập cấu hình khởi động) → `Waiting` (Đang chờ). **Đợi cho đến khi trạng thái cluster hiển thị là `Waiting`** (khoảng 10–15 phút) trước khi chuyển sang phần tiếp theo.

---