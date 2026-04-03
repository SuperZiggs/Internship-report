---
title : "3.2 Tạo EMR Studio và gắn nó vào EMR cluster"
date : 2026-03-25
weight : 2
chapter : false
pre : " <b> 5.5.2 </b> "
---

**A. Lấy VPC và Subnet ID của EMR cluster của bạn**

1.  Đi tới [Amazon EMR Console](https://console.aws.amazon.com/emr/home). Trong thanh điều hướng bên trái, trong phần **EMR on EC2**, chọn **Clusters**.

2.  Nhấp vào **Cluster ID** của cluster của bạn để mở trang chi tiết của nó. Trên trang chi tiết cluster, đi tới thẻ **Summary** và cuộn xuống mục **Network and security**. Sao chép cả **VPC ID** và **Subnet ID** — bạn sẽ cần chúng khi tạo Studio.

---

**B. Tạo EMR Studio và Workspace**

1.  Trong ngăn điều hướng bên trái, vẫn nằm trong mục **EMR on EC2**, chọn **Studios**. Sau đó bấm **Create Studio**.

    ![EMRStudioPage](/images/5-Workshops/5.5/5.5.2/1.png)
    ![EMRStudioPage2](/images/5-Workshops/5.5/5.5.2/2.png)

2.  Trên trang **Create a Studio**:
    - Chọn **Custom setup**.
    - Bấm **Browse S3** để chọn S3 bucket của bạn làm vị trí lưu trữ cho notebooks.
    - Trong mục **Service role**, chọn **EMR\_Iceberg\_Notebook\_Role**.

    ![Create Studio Bucket/Role](/images/5-Workshops/5.5/5.5.2/3.png)

3.  Cuộn xuống phần **Networking and security**. Chọn **VPC ID** và **Subnet ID** mà bạn đã sao chép ở Bước A.2. Sau đó nhấp vào **Create Studio**.

    ![Create Studio Bucket/Role](/images/5-Workshops/5.5/5.5.2/4.png)
    ![Create Studio Bucket/Role2](/images/5-Workshops/5.5/5.5.2/5.png)   

4.  Sau khi Studio được tạo, hãy nhấp vào **Launch Studio** để mở nó trong tab trình duyệt mới. Bạn sẽ thấy giao diện JupyterLab.

    > **Lưu ý:** Nếu tab mới không tự động mở, hãy kiểm tra trình chặn cửa sổ bật lên (popup blocker) của trình duyệt và cho phép cửa sổ bật lên đối với các tên miền (domain) của AWS Console.

5.  Bên trong giao diện JupyterLab, nhấp vào biểu tượng **Cluster** (hoặc biểu tượng **Compute**) ở thanh bên trái. Trong phần **Compute type**, chọn **EMR on EC2 cluster**, sau đó chọn cluster của bạn từ danh sách thả xuống. Bấm **Attach** ở phía cuối bảng điều khiển để kết nối cluster với workspace của bạn.

    ![Create Studio Bucket/Role](https://static.us-east-1.prod.workshops.aws/public/ebc00129-fa83-4c1d-931e-1f301fc04542/static/AttachCluster.png)


**Chúc mừng! Bạn đã hoàn thành thiết lập EMR Studio/Workspace, bây giờ là lúc để khám phá các tính năng của Iceberg trên EMR.**