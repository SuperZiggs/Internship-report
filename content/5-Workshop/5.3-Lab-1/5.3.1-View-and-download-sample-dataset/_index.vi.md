---
title : "1.1 Xem và tải xuống tập dữ liệu mẫu (sample dataset)"
date : 2026-03-25
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Chúng ta sẽ sử dụng phiên bản mô phỏng của bộ dữ liệu Đánh giá Sản phẩm trên Amazon (Amazon Product Reviews dataset) cho các bài lab Athena và EMR. Hãy dành chút thời gian để làm quen với bộ dữ liệu này.

**Tập dữ liệu Đánh giá của khách hàng Amazon**

Đánh giá của khách hàng (hay còn gọi là Đánh giá sản phẩm) trên Amazon là một trong những sản phẩm mang tính biểu tượng của Amazon. Trong khoảng thời gian hơn hai thập kỷ kể từ bài đánh giá đầu tiên vào năm 1995, hàng triệu khách hàng của Amazon đã đóng góp hơn một trăm triệu bài đánh giá để bày tỏ ý kiến ​​và mô tả trải nghiệm của họ về các sản phẩm trên trang web Amazon.com. Điều này làm cho Đánh giá của khách hàng trên Amazon trở thành một nguồn thông tin phong phú cho các nhà nghiên cứu học thuyết trong các lĩnh vực Xử lý ngôn ngữ tự nhiên (NLP), Truy xuất thông tin (IR) và Machine Learning (ML), v.v. Theo đó, chúng tôi đang phát hành dữ liệu này để nghiên cứu sâu hơn về nhiều quy tắc liên quan đến việc hiểu trải nghiệm sản phẩm của khách hàng. Cụ thể, bộ dữ liệu này được xây dựng để thể hiện một mẫu đánh giá và ý kiến ​​của khách hàng, sự thay đổi trong nhận thức về sản phẩm giữa các khu vực địa lý, cũng như ý định quảng cáo hoặc sự thiên lệch trong các bài đánh giá.

LƯU Ý: Bộ dữ liệu mà chúng ta sử dụng là dạng mô phỏng và chứa các số liệu, tên và từ ngữ ngẫu nhiên.

**Tải xuống bộ dữ liệu mẫu vào máy tính cục bộ của bạn và tải nó lên S3**

1.  Tải xuống bộ dữ liệu từ một trong các liên kết bên dưới
    
    -   [Dữ liệu mẫu (Sample Data) - us-east-1](https://ws-assets-prod-iad-r-iad-ed304a55c2ca1aee.s3.us-east-1.amazonaws.com/e57af944-04b9-41f5-8285-90295ed32bdc/simulatedproductreviews.parquet)
        
    -   [Dữ liệu mẫu (Sample Data) - us-west-2](https://ws-assets-prod-iad-r-pdx-f3b3f9f1a7d6a3d0.s3.us-west-2.amazonaws.com/e57af944-04b9-41f5-8285-90295ed32bdc/simulatedproductreviews.parquet)
        
2.  Điều hướng tới [bảng điều khiển giao diện AWS S3](https://s3.console.aws.amazon.com/s3/buckets) của bạn.
    
3.  Nhấp vào tên của S3 bucket được tạo cho bạn.
    
4.  Nhấp **Create folder**.
    
![create folder](/images/5-Workshops/5.3/5.3.1/3.png)

5.  Nhập tên thư mục là **productreviews** sau đó nhấp vào nút **Create folder**.
    
6.  Di chuyển vào thư mục mới bằng cách chọn **productreviews**, sau đó nhấp vào nút **Upload**.
    
7.  Nhấp vào **Add files**, chọn tệp dữ liệu mà bạn đã tải về ở bước 1 bên trên, sau đó nhấp **Upload**.

![upload file](/images/5-Workshops/5.3/5.3.1/4.png)