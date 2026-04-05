---
title: "Worklog Tuần 4"
date: 2026-01-26
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* **Backend**: Thiết kế và xây dựng module System Workout — tập trung quản lý kho dữ liệu bài tập chuẩn và các kế hoạch tập luyện mẫu (System Plans) dưới quyền quản trị của Admin.
* **Frontend**: Phát triển giao diện duyệt và tương tác với bài tập — Wizard đề xuất kế hoạch tập luyện cá nhân hóa và Component hỗ trợ tìm kiếm, lựa chọn bài tập (`PlanExercisePicker`).
* Định hình và liên kết chặt chẽ mô hình dữ liệu quan hệ giữa `GoalType` ↔ `WorkoutPlan` ↔ `Exercise`, tạo nền tảng logic vững chắc cho thuật toán gợi ý kế hoạch tập luyện.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --------- | ------------ | --------------- | -------------- |
| 2   | - Khởi tạo thực thể **MuscleGroup** và hệ thống API CRUD dành cho Admin <br>&emsp; + Định nghĩa Entity: `name (UNIQUE, ≤100)`, `description (≤500)` <br>&emsp; + Triển khai `AdminMuscleGroupController` được bảo mật nghiêm ngặt bởi `@PreAuthorize("hasRole('ADMIN')")` <br>&emsp; + Cung cấp đầy đủ các thao tác CRUD: `POST`, `GET /`, `GET /{id}`, `PUT /{id}`, `DELETE /{id}` | 27/01/2026 | 27/01/2026 | |
| 3   | - Phát triển thực thể **Exercise** và API quản trị tương ứng <br>&emsp; + Định nghĩa Entity: `name (≤150)`, `description`, `equipment`, `muscleGroup (@ManyToOne)` <br>&emsp; + Xây dựng `AdminExerciseController`: hỗ trợ CRUD toàn diện kèm tính năng lọc (filter) theo nhóm cơ <br>&emsp; + Mở các Public Endpoints: `GET /api/workouts/exercises`, `GET /{id}`, `GET /by-muscle-group/{id}`, `POST /custom` | 28/01/2026 | 28/01/2026 | |
| 4   | - Thiết kế kiến trúc cho **WorkoutPlan** và **WorkoutPlanExercise** <br>&emsp; + `WorkoutPlan`: bao gồm `name`, `description`, `goalType (@ManyToOne)`, `difficultyLevel`, `estimatedDurationMinutes`, `isSystemPlan` <br>&emsp; + `WorkoutPlanExercise`: cấu trúc lịch tập chi tiết theo `dayOfWeek (1-7)`, `sets`, `reps`, `restSeconds`, `dayIndex`, `weekIndex`, `orderIndex` <br>&emsp; + Triển khai `AdminWorkoutPlanController`: CRUD toàn diện kết hợp bộ lọc theo `goalType` | 29/01/2026 | 29/01/2026 | |
| 4   | - Triển khai **Public Workout Endpoints** thông qua `WorkoutController` (`/api/workouts`) <br>&emsp; + Nhóm cơ: `GET /muscle-groups`, `GET /muscle-groups/{id}` <br>&emsp; + Bài tập: `GET /exercises`, `GET /exercises/{id}`, `GET /exercises/by-muscle-group/{id}` <br>&emsp; + Kế hoạch mẫu: `GET /plans`, `GET /plans/{id}`, `GET /plans/by-goal-type/{id}` | 29/01/2026 | 29/01/2026 | |
| 5   | - Xây dựng giao diện **SuggestedPlanScreen** (Wizard 3 bước) <br>&emsp; + Bước 1: Khảo sát loại mục tiêu (gọi API `GET /api/goal-types`) <br>&emsp; + Bước 2: Hiển thị danh sách kế hoạch mẫu kèm hình ảnh minh họa và mức độ khó <br>&emsp; + Bước 3: Xem trước chi tiết lịch trình theo ngày → hỗ trợ nhân bản dữ liệu qua `cloneFromSystemPlan` | 30/01/2026 | 30/01/2026 | |
| 6   | - Phát triển màn hình **PlanExercisePicker** <br>&emsp; + Hiển thị danh sách toàn bộ bài tập hệ thống, phân loại theo nhóm cơ cùng hình ảnh trực quan <br>&emsp; + Tích hợp công cụ tìm kiếm bài tập theo tên <br>&emsp; + Xử lý sự kiện lựa chọn: kích hoạt hàm `addExerciseToPlan(planId, dayOfWeek, exerciseId)` <br> - Xây dựng cơ chế **DatabaseSeeder** để khởi tạo dữ liệu mẫu (nhóm cơ, bài tập) ban đầu | 31/01/2026 | 31/01/2026 | |

### Kết quả đạt được tuần 4:

* **Backend — System Workout**:
  * module cốt lõi `MuscleGroup` cùng bộ API Admin CRUD đã vận hành trơn tru; hoàn tất seed các dữ liệu chuẩn: Ngực, Lưng, Chân, Vai, Tay, Cơ bụng.
  * Thực thể `Exercise` tích hợp thành công với cả Admin CRUD và các public read endpoints, đáp ứng tốt yêu cầu truy xuất từ phía client.
  * Cấu trúc phức hợp `WorkoutPlan` và `WorkoutPlanExercise` đã được thiết lập hoàn chỉnh, hỗ trợ phân rã lịch tập chi tiết theo từng ngày và mở rộng linh hoạt lịch tập nhiều tuần thông qua `weekIndex`.
  * Đảm bảo tính rạch ròi trong phân quyền: toàn bộ Admin endpoints được bảo vệ nghiêm ngặt bằng `ROLE_ADMIN`, trong khi các public endpoints cho phép đọc dữ liệu hoạt động độc lập không yêu cầu xác thực.
* **Frontend — Duyệt bài tập**:
  * Giao diện `SuggestedPlanScreen` dẫn dắt người dùng qua quy trình 3 bước mượt mà: từ việc xác định mục tiêu đến lúc chọn được kế hoạch phù hợp.
  * Component `PlanExercisePicker` liệt kê danh sách bài tập một cách trực quan theo ngữ cảnh nhóm cơ, cho phép thao tác chọn và thêm bài tập vào kế hoạch cá nhân một cách dễ dàng.
* Xây dựng thành công `DatabaseSeeder` tự động đọc file cấu hình JSON (`s3_images_upload.json`) để đồng bộ dữ liệu nhóm cơ và bài tập ban đầu. Đã xử lý triệt để các vấn đề mapping do ký tự đặc biệt hoặc dấu nháy kép, đảm bảo 100% URL hình ảnh được lưu trữ và hiển thị chính xác từ cơ sở dữ liệu.

### Kiến thức AWS đã học:

* Thấu hiểu nguyên lý thiết kế kiến trúc lưu trữ đa phương tiện (media architecture): sử dụng Amazon S3 cho dữ liệu nhị phân (binary files) và PostgreSQL cho siêu dữ liệu (metadata), đảm bảo mỗi thành phần đảm nhiệm đúng chức năng tối ưu của nó.
* Thiết lập các chuẩn quy ước định danh đối tượng (object key conventions) như `workouts/`, `exercises/`, `foods/` nhằm tối ưu hóa tổ chức không gian lưu trữ, đơn giản hóa quá trình dọn dẹp và áp dụng các chính sách vòng đời (lifecycle policies).
* Quán triệt nguyên tắc "private-by-default" cho các S3 buckets, nhận thức sâu sắc những rủi ro bảo mật tiềm ẩn khi lạm dụng Public ACL đối với tài nguyên ứng dụng nội bộ.
* Nắm bắt các phương thức mã hóa dữ liệu tại server (server-side encryption) như SSE-S3 và SSE-KMS, đồng thời phân biệt rõ các kịch bản thực tế đòi hỏi KMS để thắt chặt kiểm soát truy cập.
* Nắm vững vai trò chiến lược của S3 Versioning như một cơ chế phòng vệ tự động chống lại các rủi ro vô tình ghi đè hoặc xóa nhầm file media quan trọng.
* Phân tích chuyên sâu bài toán đánh đổi (trade-off) giữa chi phí lưu trữ S3 và băng thông truyền tải đối với hình ảnh, qua đó khẳng định tính cấp thiết của việc tối ưu hóa dung lượng file trước khi tải lên.
* Kiến tạo mô hình lưu trữ linh hoạt (cloud-ready), sẵn sàng cho việc tích hợp Amazon CloudFront làm CDN (mạng phân phối nội dung) phía trước S3 trong tương lai mà không cần can thiệp hay sửa đổi business logic hiện tại.

Nhìn chung, tuần 4 đã chuyển hóa những lý thuyết nền tảng về Amazon S3 thành một chiến lược kiến trúc quản lý media bài bản và thực chiến cho ứng dụng.

### Kế hoạch tuần tiếp theo:

* **Backend**: Triển khai module `UserWorkoutPlan` với các tính năng nâng cao: hỗ trợ nhân bản (clone) kế hoạch từ các template chuẩn của hệ thống, thiết lập cơ chế xóa mềm (soft-delete), và xây dựng logic kích hoạt (activate) lịch tập cá nhân.
* **Frontend**: Xây dựng nhóm giao diện quản lý kế hoạch cá nhân bao gồm `MyPlansScreen`, `CreatePlanScreen` và `PlanEditScreen` (tích hợp trình chỉnh sửa lịch tập chi tiết theo ngày).