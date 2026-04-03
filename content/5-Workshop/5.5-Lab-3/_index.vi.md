---
title : "Lab 3: EMR Iceberg - sử dụng bảng Iceberg"
date : 2026-03-16 
weight : 5 
chapter : false
pre : " <b> 5.5. </b> "
---


Trong bài lab này, đối với các bài lab tự thực hành (self paced labs), chúng ta sẽ tạo một EMR cluster với Phiên bản 7.5 có cài đặt Iceberg. Đối với AWS Events, EMR cluster đã được tạo sẵn. Bài lab này đã được kiểm tra với `EMR 7.5.0`. Do đó, chúng tôi khuyên bạn nên sử dụng cùng một phiên bản EMR cho workshop của mình.

Chúng ta sẽ sử dụng PySpark Jupyter Notebook để làm việc tương tác với các bảng Iceberg.

Đối với các bài lab tự thực hành, trước khi bắt đầu bài lab này, hãy đảm bảo rằng bạn đã hoàn thành Bài lab **Tạo S3 bucket** (Create S3 bucket). Đối với các sự kiện AWS, S3 bucket đã được tạo sẵn.