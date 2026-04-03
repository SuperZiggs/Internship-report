---
title: "Worklog Tuần 7"
date: 2026-02-23
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* **Backend**: Xây dựng module Media — entity `Image` với polymorphic association, tích hợp AWS S3.
* **Frontend**: Xây dựng `SessionDetailScreen`, `SessionCalendarScreen`, và `mediaService` với image caching.
* Hiển thị hình ảnh cho bài tập và kế hoạch tập luyện trong toàn ứng dụng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Xây dựng entity **Image** (`module/media`) <br>&emsp; + `url (≤500)`, `isThumbnail`, `food (nullable)`, `workoutPlan (nullable)`, `exercise (nullable)` <br>&emsp; + Constraint `chk_image_exclusive`: chính xác 1 trong 3 FK có giá trị (polymorphic association) <br>&emsp; + Flyway **V3**: tạo bảng `image` với constraint | 24/02/2026 | 24/02/2026 | |
| 3   | - Xây dựng **ImageController** (`/api/images`) <br>&emsp; + `POST /` (register URL), `GET /{id}` <br>&emsp; + `GET /food/{foodId}`, `GET /workout-plan/{id}`, `GET /exercise/{id}` <br>&emsp; + `PUT /{id}`, `DELETE /{id}` <br> - Cấu hình **AWS S3** (`AwsS3Config.java`): `AmazonS3` bean, region `ap-southeast-1`, bucket từ env | 25/02/2026 | 25/02/2026 | <https://docs.aws.amazon.com/sdk-for-java/> |
| 4   | - Xây dựng **mediaService** (Frontend) <br>&emsp; + `getImageUrl(owner, id)` — gọi `GET /api/images/{owner}/{id}` <br>&emsp; + Cache URL trong bộ nhớ + deduplication in-flight <br>&emsp; + Bulk helpers: `getFoodImageUrlMap`, `getWorkoutPlanImageUrlMap`, `getExerciseImageUrlMap` | 26/02/2026 | 26/02/2026 | |
| 5   | - Xây dựng **SessionDetailScreen** (Frontend) <br>&emsp; + Tóm tắt buổi tập: nhóm bài tập theo `exerciseId` <br>&emsp; + Hiển thị set × rep × cân nặng cho từng bài <br>&emsp; + Format ngày giờ qua `utils/date.ts` | 27/02/2026 | 27/02/2026 | |
| 6   | - Xây dựng **SessionCalendarScreen** (Frontend) <br>&emsp; + Lịch tháng với chấm trên ngày có buổi tập <br>&emsp; + Di chuyển tháng (mũi tên trái/phải) <br>&emsp; + Nhấn ngày để hiển thị log buổi tập bên dưới <br>&emsp; + Nhãn tiếng Việt (Thứ 2–CN, Tháng 1–12) | 28/02/2026 | 28/02/2026 | |

### Kết quả đạt được tuần 7:

* **Backend — Module Media**:
  * Flyway V3 được apply với exclusive FK constraint — toàn vẹn dữ liệu polymorphic ở cấp DB.
  * `ImageController` đăng ký URL hình ảnh và trả về đúng theo entity sở hữu.
  * AWS S3 bean cấu hình local qua biến môi trường.
* **Frontend — Lịch sử buổi tập**:
  * `SessionDetailScreen` nhóm log bài tập đúng và hiển thị rõ ràng.
  * `SessionCalendarScreen` đánh dấu ngày tập; nhấn ngày hiển log chi tiết.
* **Frontend — Media Service**:
  * URL hình ảnh được cache trong bộ nhớ — không gọi API trung lặp.
  * Tile kế hoạch và danh sách bài tập hiển thị hình ảnh từ S3.

### Kiến thức AWS đã học:

* Hiểu cách dùng presigned URL của S3 để client upload hoặc download file an toàn mà không cần cầm credential AWS trực tiếp.
* Biết cách chọn thời hạn presigned URL ngắn cho tài nguyên nhạy cảm nhưng vẫn cân bằng được trải nghiệm người dùng.
* Nắm các yêu cầu validate upload như giới hạn content-type, kích thước file và key path để giảm rủi ro lạm dụng.
* Thiết kế lifecycle policy cho ảnh cũ hoặc file tạm nhằm tối ưu chi phí bằng cách chuyển storage class hoặc xóa theo thời hạn.
* Hiểu cơ chế phân phối nội dung có kiểm soát để hạn chế hotlink và việc tái sử dụng media ngoài ý muốn.
* Nắm mối liên hệ giữa object ownership và bucket policy để tránh lỗi quyền truy cập khó chẩn đoán.
* Liên kết chiến lược cache media với mục tiêu giảm chi phí cloud và tăng tốc độ tải giao diện.

Tóm lại, tuần 7 chuyển kiến thức về S3 từ mức khái niệm sang mô hình phục vụ media an toàn và tối ưu hơn cho dự án.

### Kế hoạch tuần tiếp theo:

* **Backend**: Xây dựng module Food & Nutrition — `Food`, `Meal`, `MealFood`, `DailyNutrition` với CRUD đầy đủ và tính toán dinh dưỡng.
* **Frontend**: Xây dựng `DietScreen` với quản lý bữa ăn, tìm kiếm thực phẩm, thêm thực phẩm vào bữa.
