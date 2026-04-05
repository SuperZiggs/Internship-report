---
title: "Worklog Tuần 7"
date: 2026-02-23
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* **Backend**: Khởi tạo và phát triển module Media — thiết lập kiến trúc tích hợp sâu giữa **Amazon S3** và **Amazon CloudFront** nhằm tối ưu hóa khả năng lưu trữ cũng như tăng tốc độ phân phối hình ảnh tĩnh.
* **Frontend**: Xây dựng tổ hợp giao diện theo dõi tiến trình (`SessionDetailScreen`, `SessionCalendarScreen`) và phát triển `mediaService` chuyên biệt để ánh xạ URL hình ảnh qua CloudFront, đảm bảo hiệu suất tải trang tối ưu.
* Tối ưu hóa triệt để chi phí và độ trễ thông qua việc ứng dụng mạng phân phối nội dung (CDN) toàn cầu, thay thế hoàn toàn phương thức truy xuất trực tiếp vào S3 bucket.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo thực thể **Image** (`module/media`) <br>&emsp; + Định nghĩa schema: `url (≤500)`, `isThumbnail`, `food (nullable)`, `workoutPlan (nullable)`, `exercise (nullable)` <br>&emsp; + Áp dụng constraint `chk_image_exclusive`: đảm bảo tính toàn vẹn dữ liệu đa hình (polymorphic association), yêu cầu chính xác 1 trong 3 khóa ngoại (FK) có giá trị <br>&emsp; + Triển khai script Flyway **V3**: tạo bảng `image` kèm hệ thống constraint bảo vệ | 24/02/2026 | 24/02/2026 | |
| 3   | - Phát triển **ImageController** (`/api/images`) <br>&emsp; + Xây dựng các API cơ bản: `POST /` (đăng ký URL), `GET /{id}`, `PUT /{id}`, `DELETE /{id}` <br>&emsp; + Cung cấp các endpoint định tuyến chuyên biệt: `GET /food/{foodId}`, `GET /workout-plan/{id}`, `GET /exercise/{id}` <br> - Thiết lập cấu hình **AWS S3** (`AwsS3Config.java`): khởi tạo bean `AmazonS3`, ấn định region `ap-southeast-1`, và trích xuất tên bucket an toàn từ biến môi trường | 25/02/2026 | 25/02/2026 | <https://docs.aws.amazon.com/sdk-for-java/> |
| 4   | - Triển khai **mediaService** phía Frontend <br>&emsp; + Viết hàm `getImageUrl(owner, id)` — tích hợp gọi API `GET /api/images/{owner}/{id}` <br>&emsp; + Xây dựng cơ chế cache URL trên bộ nhớ (in-memory) kết hợp xử lý chống trùng lặp (deduplication in-flight) cho các request đang chờ <br>&emsp; + Bổ sung các bulk helpers tối ưu truy xuất danh sách: `getFoodImageUrlMap`, `getWorkoutPlanImageUrlMap`, `getExerciseImageUrlMap` | 26/02/2026 | 26/02/2026 | |
| 5   | - Xây dựng giao diện **SessionDetailScreen** (Frontend) <br>&emsp; + Trình bày dashboard tóm tắt hiệu suất buổi tập: tự động gom nhóm (grouping) dữ liệu log theo `exerciseId` <br>&emsp; + Kết xuất trực quan thông số chi tiết: hiệp (set) × số lần (rep) × khối lượng (weight) cho từng bài tập <br>&emsp; + Chuẩn hóa định dạng hiển thị thời gian thông qua module helper `utils/date.ts` | 27/02/2026 | 27/02/2026 | |
| 6   | - Phát triển giao diện **SessionCalendarScreen** (Frontend) <br>&emsp; + Trình bày lịch tháng trực quan, sử dụng hệ thống marker (chấm màu) để đánh dấu các ngày diễn ra buổi tập <br>&emsp; + Tích hợp tính năng vuốt/nhấn (mũi tên trái/phải) để điều hướng các tháng linh hoạt <br>&emsp; + Xử lý sự kiện chọn ngày: render lập tức danh sách log buổi tập chi tiết tại khung bên dưới <br>&emsp; + Bản địa hóa toàn bộ nhãn hiển thị sang chuẩn tiếng Việt (Thứ 2–CN, Tháng 1–12) | 28/02/2026 | 28/02/2026 | |

### Kết quả đạt được tuần 7:

* **Kiến trúc Cloud & Module Media**:
  * Thiết lập thành công Private S3 Bucket với chính sách bảo mật nghiêm ngặt (chặn hoàn toàn mọi truy cập Public).
  * Triển khai mạng phân phối nội dung **Amazon CloudFront**, áp dụng cấu hình bảo mật **Origin Access Control (OAC)** để thiết lập kết nối an toàn và độc quyền vào S3 bucket.
  * Cập nhật đồng bộ file dữ liệu gốc (`s3_images_upload.json`), điều hướng toàn bộ request ảnh tĩnh sang domain của CloudFront, giúp cắt giảm đáng kể chi phí truyền tải dữ liệu (Data Transfer Out).
* **Frontend — Lịch sử buổi tập**:
  * Giao diện `SessionDetailScreen` xử lý logic gom nhóm workout log một cách mượt mà, mang lại báo cáo tổng quan trực quan và dễ nắm bắt.
  * Màn hình `SessionCalendarScreen` vận hành chính xác; hệ thống marker hiển thị đúng ngày tập và thao tác tra cứu log diễn ra tức thì không có độ trễ.
* **Frontend — Media Service**:
  * Cơ chế lưu trữ đệm URL (in-memory caching) hoạt động xuất sắc — loại bỏ hoàn toàn các lời gọi API trùng lặp dư thừa.
  * Các thành phần giao diện phức tạp (tile kế hoạch, danh sách bài tập) tải hình ảnh từ S3/CloudFront với tốc độ cực nhanh, nâng tầm trải nghiệm người dùng.

### Kiến thức AWS đã học:

* Hiểu rõ cơ chế hoạt động của Presigned URL trên S3, trao quyền cho client thực hiện upload/download file một cách an toàn mà không cần cấp phát trực tiếp credential AWS.
* Làm chủ kỹ thuật tinh chỉnh thời hạn (expiration time) của Presigned URL, đảm bảo nguyên tắc bảo mật tối đa cho các tài nguyên nhạy cảm trong khi vẫn duy trì trải nghiệm người dùng liền mạch.
* Nắm vững các yêu cầu kiểm duyệt upload phía server: áp dụng các quy tắc khắt khe về `content-type`, giới hạn dung lượng file và chuẩn hóa `key path` để phòng chống triệt để rủi ro lạm dụng không gian lưu trữ.
* Thiết kế và áp dụng hiệu quả các chính sách vòng đời (Lifecycle Policies), tự động hóa quá trình dọn dẹp file tạm hoặc chuyển các ảnh ít truy cập sang các lớp lưu trữ (storage classes) tối ưu chi phí hơn.
* Nghiên cứu chuyên sâu cơ chế phân phối nội dung có kiểm soát, áp dụng các rào cản kỹ thuật ngăn chặn tình trạng ăn cắp băng thông (hotlinking) và tái sử dụng media trái phép.
* Thấu hiểu mối liên hệ mật thiết giữa Object Ownership và Bucket Policies, tạo tiền đề vững chắc để chẩn đoán và khắc phục nhanh các lỗi liên quan đến phân quyền truy cập.
* Liên kết chiến lược caching media ở phía client với định hướng kiến trúc vĩ mô: vừa giảm thiểu gánh nặng chi phí Cloud, vừa bứt phá tốc độ tải trang cho ứng dụng di động.

Tóm lại, tuần 7 đã hiện thực hóa các kiến thức lý thuyết về S3 thành một mô hình kiến trúc phân phối media thực chiến, an toàn và tối ưu hiệu suất cho dự án.

### Kế hoạch tuần tiếp theo:

* **Backend**: Xây dựng toàn diện module quản lý Dinh dưỡng (Food & Nutrition) — thiết kế các entity `Food`, `Meal`, `MealFood`, `DailyNutrition`, và phát triển hệ thống API CRUD hoàn chỉnh tích hợp thuật toán tính toán calo tự động.
* **Frontend**: Triển khai `DietScreen` đóng vai trò là trung tâm quản lý bữa ăn, bao gồm công cụ tìm kiếm thực phẩm thông minh và luồng thao tác gán thực phẩm vào thực đơn hàng ngày.